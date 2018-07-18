---
title: Строки подключения и модели — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122757"
---
# <a name="connection-strings-and-models"></a>Строки подключения и моделей
В этом разделе рассматриваются как Entity Framework обнаруживает какие подключения к базе данных, и как его можно изменить. Обе модели, созданные с помощью Code First и конструкторе EF рассматриваются в этом разделе.  

Обычно в приложении Entity Framework использует класс, производный от DbContext. Этот производный класс будет вызовите один из конструкторов в базовом классе DbContext для элемента управления.  

- Как контекст будет подключаться к базе данных, то есть как строка подключения — обнаружении используется  
- Будет использовать контекст вычисления модели с помощью Code First или загрузить модели, созданной с помощью конструктора EF  
- Дополнительные параметры  

Следующие фрагменты Показать некоторые из способов конструкторы DbContext может использоваться.  

## <a name="use-code-first-with-connection-by-convention"></a>Использовать Code First с подключением по соглашению  

Если вы не сделали любую другую конфигурацию в приложении, затем вызов конструктора без параметров для DbContext приведет к DbContext для запуска в режиме Code First с помощью подключения к базе данных, созданные по соглашению. Пример:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

В этом примере DbContext использует полное имя пространства имен вашего производного контекста class—Demo.EF.BloggingContext—as имя базы данных и создает строку подключения для этой базы данных с помощью SQL Express или LocalDB. Если установлены оба, SQL Express будут использоваться.  

Visual Studio 2010 включает в себя SQL Express по умолчанию и Visual Studio 2012 и более поздних версий LocalDB. Во время установки пакета EntityFramework NuGet проверяет, какой сервер базы данных доступна. Пакет NuGet затем обновит файл конфигурации, задав сервер базы данных по умолчанию, использующий Code First, при создании подключения по соглашению. Если выполняется SQL Express, он будет использоваться. Если SQL Express не доступен затем LocalDB будет зарегистрировано по умолчанию вместо этого. Нет изменения вносятся в файл конфигурации, если он уже содержит параметр для фабрики подключения по умолчанию.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Использовать Code First с подключением, соглашение о вызовах и указанное имя базы данных  

Если вы не сделали любую другую конфигурацию в приложении, последующим вызовом конструктор строк на DbContext с именем базы данных, которую вы хотите использовать приведет к DbContext для запуска в режиме Code First с помощью подключения к базе данных, созданные по соглашению к базе данных Это имя. Пример:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

В этом примере DbContext использует «BloggingDatabase» в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express (устанавливается с Visual Studio 2010) или LocalDB (устанавливается с Visual Studio 2012). Если установлены оба, SQL Express будут использоваться.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Использование Code First, строкой подключения в файле app.config/web.config  

Вы можете поместить строку подключения в файле app.config или web.config. Пример:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Это простой способ сообщить DbContext, чтобы использовать сервер базы данных, отличных от SQL Express или LocalDB, приведенном выше примере указывает базу данных SQL Server Compact Edition.  

Если имя строки подключения совпадает с именем контекста (с или без квалификации пространства имен) затем он будет найден DbContext при использовании конструктора без параметров. Если имя строки подключения отличается от имени контекста можно сказать DbContext, чтобы использовать это подключение в режиме Code First, передавая имя строки подключения для конструктора DbContext. Пример:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Кроме того, можно использовать форму «имя =\<имя строки подключения\>"для строки, переданных конструктору DbContext. Пример:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Эта форма позволяет явные, что предполагается, что строка подключения должна находиться в файле конфигурации. Исключение возникает, если строку подключения с заданным именем не найден.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Database First или Model First со строкой подключения в файле app.config/web.config  

Модели, созданные в конструкторе EF, отличаются от Code First, в том, что модель уже существует и не создается из кода при запуске приложения. Модель обычно существует как файл EDMX в проекте.  

Конструктор добавляет строки подключения к EF в файл app.config или web.config. Эта строка подключения отличается тем, что он содержит сведения о том, как найти сведения в файле EDMX. Пример:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

Конструктор EF также создаст код, который сообщает DbContext, чтобы использовать это подключение, передав имя строки подключения конструктору DbContext. Пример:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext знал о необходимости загрузить существующей модели (а не с помощью Code First для вычисления его из кода), так как строка подключения имеет строки подключения к EF, содержащий подробные сведения о модели для использования.  

## <a name="other-dbcontext-constructor-options"></a>Другие параметры конструктора DbContext  

Класс DbContext содержит другие конструкторы и шаблоны использования, которые обеспечивают некоторые более расширенные сценарии. Ниже приведены некоторые из них.  

- Класс DbModelBuilder можно использовать для построения модели Code First без создания экземпляра экземпляра DbContext. В результате является объектом DbModel. Затем можно передать этот объект DbModel для одного или конструкторов DbContext, когда будете готовы для создания экземпляра DbContext.  
- Строка подключения целиком можно передать DbContext вместо базы данных или соединения строки имени. По умолчанию эта строка подключения используется с поставщиком System.Data.SqlClient; Это можно изменить, задав другой реализации IConnectionFactory в контекст. Database.DefaultConnectionFactory.  
- Можно использовать существующий объект DbConnection, передавая его конструктора DbContext. Если объект подключения — это экземпляр EntityConnection, модели, указанное в соединении будет использоваться, а не вычисление модель с помощью Code First. Если объект является экземпляром другого типа — например, SqlConnection, затем контекст будет использовать для режиме Code First.  
- Можно передать существующий ObjectContext DbContext конструктора для создания DbContext упаковки существующего контекста. Это можно использовать для существующих приложений, использующие ObjectContext, но которым нужно воспользоваться преимуществами DbContext в некоторых частях приложения.  
