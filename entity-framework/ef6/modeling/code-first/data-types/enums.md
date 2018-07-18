---
title: Поддержка перечислений — Code First — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
caps.latest.revision: 3
ms.openlocfilehash: 698c84a9f0679c67a086574de2f253f214fb8d0b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122566"
---
# <a name="enum-support---code-first"></a>Поддержка перечислений — Code First
> [!NOTE]
> **EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

Это видео и пошаговые пошаговом руководстве показано, как использовать типы перечисления с Entity Framework Code First. Также демонстрируется использование перечислений в запросе LINQ.

В этом пошаговом руководстве будет использовать Code First для создания новой базы данных, но можно также использовать [Code First для сопоставления существующей базы данных](~/ef6/modeling/code-first/workflows/existing-database.md).

В Entity Framework 5 добавлена поддержка перечисления. Чтобы использовать новые функции, например перечисления, Пространственные типы данных и функции, возвращающие табличные значения, необходимо ориентироваться .NET Framework 4.5. Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.

В Entity Framework, что перечисление может иметь следующие базовые типы: **байтов**, **Int16**, **Int32**, **Int64** , или **SByte**.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео показано, как использовать типы перечисления с Entity Framework Code First. Также демонстрируется использование перечислений в запросе LINQ.

**Представленный**: Юлия Корнич

**Видео**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Предварительные требования

Необходимо будет установлен выпуск Visual Studio 2012 Ultimate, Premium, Professional и Web Express для выполнения этого пошагового руководства.

 

## <a name="set-up-the-project"></a>Настройка проекта

1.  Откройте Visual Studio 2012
2.  На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**
3.  В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона
4.  Введите **EnumCodeFirst** как имя проекта и нажмите кнопку **ОК**

## <a name="define-a-new-model-using-code-first"></a>Определения новой модели с помощью Code First

При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен). Приведенный ниже код определяет класс отдела.

Код также определяет перечисление DepartmentNames. По умолчанию имеет перечисления **int** типа. Свойство Name класса Department имеет тип DepartmentNames.

Откройте файл Program.cs и вставьте следующие определения класса.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Определить тип, производный от DbContext

Кроме определения сущностей, необходимо определить класс, который является производным от DbContext и DbSet предоставляет&lt;TEntity&gt; свойства. DbSet&lt;TEntity&gt; свойства let контекст знать, какие типы, которые вы хотите включить в модель.

Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.

Типы DbContext и DbSet определенные в сборке EntityFramework. Мы добавим ссылку на эту библиотеку DLL с помощью пакета EntityFramework NuGet.

1.  В обозревателе решений щелкните правой кнопкой мыши имя проекта.
2.  Выберите **управление пакетами NuGet...**
3.  В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета.
4.  Нажмите кнопку **установки**

Обратите внимание, что помимо сборки EntityFramework, ссылки на сборки System.ComponentModel.DataAnnotations и System.Data.Entity добавляются также.

В верхней части файла Program.cs, добавьте следующий оператор using:

``` csharp
using System.Data.Entity;
```

В Program.cs добавьте определение контекста. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Сохранения и извлечения данных

Откройте файл Program.cs, в котором определен метод Main. Добавьте следующий код в функцию Main. Код добавляет новый объект отдела в контекст. Затем она сохраняет данные. Код также выполняется запрос LINQ, возвращающий подразделение, где имя задается DepartmentNames.English.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Скомпилируйте и запустите приложение. Программа выдает следующие результаты.

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Представление создаваемой базы данных

При запуске приложение в первый раз, Entity Framework создает базу данных. Поскольку у нас есть установленной Visual Studio 2012, базе данных создается на экземпляре LocalDB. По умолчанию Entity Framework содержит имя базы данных после полное имя производного контекста (например, **EnumCodeFirst.EnumTestContext**). Последующие случаи, когда будет использоваться существующая база данных.  

Обратите внимание, что если внести изменения в модель после создания базы данных, необходимо использовать Code First Migrations для обновления схемы базы данных. См. в разделе [Code First для новой базы данных](~/ef6/modeling/code-first/workflows/new-database.md) пример с помощью миграций.

Чтобы просмотреть базы данных и данных, сделайте следующее:

1.  В главном меню Visual Studio 2012 выберите **представление**  - &gt; **обозреватель объектов SQL Server**.
2.  Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши **SQL Server** и выберите **добавить SQL Server** используйте значение по умолчанию **проверки подлинности Windows** для подключения к Экземпляр LocalDB
3.  Разверните узел LocalDB
4.  Unfold **баз данных** папку для новой базы данных см. в разделе и перейдите к **отдел** таблицы Обратите внимание, что Code First не создает таблицу, которая сопоставляется с типом перечисления
5.  Чтобы просмотреть данные, щелкните правой кнопкой мыши по таблице и выберите **Просмотр данных**

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели способы использования типов перечисления с Entity Framework Code First. 
