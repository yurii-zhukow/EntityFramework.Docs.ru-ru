---
title: Конфигурация на основе кода - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 7c7da8992fd1b29f998e08c13d445c1d2d8cc2ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490822"
---
# <a name="code-based-configuration"></a>Конфигурация на основе кода
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

Конфигурация приложения Entity Framework можно указать в файле конфигурации (app.config/web.config) или с помощью кода. Последний известен как конфигурация на основе кода.  

Конфигурации в файле конфигурации описан в [отдельной статье](config-file.md). Файл конфигурации имеет приоритет над конфигурация на основе кода. Другими словами Если параметр конфигурации в коде и в файле конфигурации, используется параметр в файле конфигурации.  

## <a name="using-dbconfiguration"></a>С помощью DbConfiguration  

Конфигурация на основе кода в EF6 и выше достигается путем создания подкласса System.Data.Entity.Config.DbConfiguration. Следующие рекомендации необходимо соблюдать при подклассификация DbConfiguration:  

- Создайте только один класс DbConfiguration для вашего приложения. Этот класс задает параметры на уровне домена приложения.  
- Поместите класс DbConfiguration в ту же сборку класса DbContext. (См. в разделе *перемещение DbConfiguration* Если вы хотите изменить этот раздел.)  
- Присвойте классу DbConfiguration открытый конструктор без параметров.  
- Задать параметры конфигурации, вызвав защищенных методов DbConfiguration в этот конструктор.  

Следуя этим рекомендациям позволяет EF для обнаружения и автоматически использовать конфигурацию, оба средства, требуется доступ к модели и при запуске приложения.  

## <a name="example"></a>Пример  

Класс, производный от DbConfiguration может выглядеть следующим образом:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

Этот класс задает EF для использования стратегии выполнения SQL Azure — автоматический повтор операций поврежденной базы данных - и локальная база данных для базы данных, которые создаются в соответствии с соглашением с Code First.  

## <a name="moving-dbconfiguration"></a>Перемещение DbConfiguration  

Бывают случаи, когда не может поместить класс DbConfiguration в ту же сборку класса DbContext. Например в разных сборках имеется два класса DbContext. Существует два варианта обработки такой.  

Первый вариант — использовать файл конфигурации можно указать экземпляр DbConfiguration для использования. Чтобы сделать это, установите атрибут codeConfigurationType разделе entityFramework. Пример:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Значение codeConfigurationType должен быть сборки и полное имя пространства имен класса DbConfiguration.  

Второй вариант — разместить DbConfigurationTypeAttribute на класс контекста. Пример:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Значение, передаваемое в атрибут может быть ваш тип DbConfiguration — как показано выше - или сборки и пространства имен полное имя типа string. Пример:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Явно настроить параметр DbConfiguration  

Существуют ситуации, где конфигурации могут быть необходимы перед использует любого типа DbContext. Примеры включают:  

- С помощью DbModelBuilder для построения модели без контекста  
- С помощью другой служебная программа framework код, использующий DbContext, где используется этот контекст, прежде чем использовать контексте приложения  

В таких ситуациях не удается автоматически обнаружить конфигурации EF и вместо этого необходимо выполнить одно из следующих:  

- Задайте для типа DbConfiguration в файле конфигурации, как описано в разделе *перемещение DbConfiguration* выше
- Вызовите статический метод DbConfiguration.SetConfiguration во время запуска приложения  

## <a name="overriding-dbconfiguration"></a>Переопределение DbConfiguration  

Существуют ситуации, когда необходимо переопределяют конфигурацию, заданную в DbConfiguration. Обычно этого не сделать, разработчиками приложений, а скорее сторонних поставщиков и подключаемые модули, которые нельзя использовать в производном классе DbConfiguration.  

Для этого EntityFramework позволяет обработчик событий для регистрации, можно изменить текущую конфигурацию, непосредственно перед ее блокировкой.  Он также предоставляет метод sugar специально для замены любая служба, возвращенный указатель служб EF. Это, как он предназначен для использования:  

- При запуске приложения (до использования EF) подключаемого модуля или поставщик должен зарегистрировать метод обработчика событий для данного события. (Обратите внимание, что это должно произойти, прежде чем приложение использует EF).  
- Обработчик событий вызывает ReplaceService для каждой службы, необходимо заменить.  

Например repalce IDbConnectionFactory и DbProviderService зарегистрировать обработчик примерно так:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

В коде выше MyProviderServices и MyConnectionFactory представляют реализации службы.  

Можно также добавить обработчики дополнительную зависимость, чтобы получить тот же эффект.  

Обратите внимание, что можно также поместить DbProviderFactory таким образом, но это таким образом влияют только на EF и не использования DbProviderFactory вне EF. По этой причине, вероятно, стоит продолжить перенос DbProviderFactory, у вас есть до.  

Следует также помнить служб, выполняемых извне приложения. Например, при выполнении миграции из консоли диспетчера пакетов. При выполнении миграции из консоли, предпринимается попытка найти ваш DbConfiguration. Тем не менее, зависит от того ли он получит в оболочку службу о том, где он зарегистрирован обработчик событий. Если он зарегистрирован как часть конструкции вашего DbConfiguration код должен выполняться, и служба должны получить оболочку. Обычно это работать не будет, и это означает, что инструменты не получить службу оболочку.  
