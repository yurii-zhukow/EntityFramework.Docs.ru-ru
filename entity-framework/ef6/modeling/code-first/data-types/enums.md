---
title: Поддержка перечисления-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415793"
---
# <a name="enum-support---code-first"></a>Поддержка перечисления — Code First
> [!NOTE]
> **Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

В этом видео и пошаговом руководстве показано, как использовать типы Enum с Entity Framework Code First. В нем также показано, как использовать перечисления в запросе LINQ.

В этом пошаговом руководстве будет использоваться Code First для создания новой базы данных, но можно также использовать [Code First для сопоставлений с существующей базой данных](~/ef6/modeling/code-first/workflows/existing-database.md).

Поддержка перечисления появилась в Entity Framework 5. Чтобы использовать новые функции, такие как перечисления, пространственные типы данных и функции, возвращающие табличное значение, необходимо выбрать .NET Framework 4,5. Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.

В Entity Framework перечисление может иметь следующие базовые типы: **Byte**, **Int16**, **Int32**, **Int64** или **SByte**.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео показано, как использовать типы Enum с Entity Framework Code First. В нем также показано, как использовать перечисления в запросе LINQ.

**Представлено**: Julia Корнич

**Видео**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.

 

## <a name="set-up-the-project"></a>Настройка проекта

1.  Откройте Visual Studio 2012
2.  В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .
3.  В левой области щелкните **Visual C\#** , а затем выберите шаблон **консоли** .
4.  Введите **енумкодефирст** в качестве имени проекта и нажмите кнопку **ОК** .

## <a name="define-a-new-model-using-code-first"></a>Определение новой модели с помощью Code First

При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель. В приведенном ниже коде определяется класс Department.

Код также определяет перечисление Департментнамес. По умолчанию перечисление имеет тип **int** . Свойство Name класса Department имеет тип Департментнамес.

Откройте файл Program.cs и вставьте следующие определения классов.

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
 

## <a name="define-the-dbcontext-derived-type"></a>Определение производного типа DbContext

В дополнение к определению сущностей необходимо определить класс, производный от DbContext и предоставляющий DbSet&lt;&gt; свойств. Свойства DbSet&lt;ости&gt; позволяют контексту понять, какие типы необходимо включить в модель.

Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, включая заполнение объектов данными из базы данных, отслеживание изменений и сохранение данных в базе данных.

Типы DbContext и DbSet определены в сборке EntityFramework. Мы добавим ссылку на эту библиотеку DLL с помощью пакета NuGet EntityFramework.

1.  В обозреватель решений щелкните правой кнопкой мыши имя проекта.
2.  Выберите **Управление пакетами NuGet...**
3.  В диалоговом окне Управление пакетами NuGet выберите вкладку в **Интернете** и выберите пакет **EntityFramework** .
4.  Нажмите кнопку **Установить**.

Обратите внимание, что в дополнение к сборке EntityFramework также добавляются ссылки на System. ComponentModel. и сборки System. Data. Entity.

В верхней части файла Program.cs добавьте следующую инструкцию using:

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
 

## <a name="persist-and-retrieve-data"></a>Сохранение и получение данных

Откройте файл Program.cs, в котором определен метод Main. Добавьте следующий код в функцию main. Код добавляет новый объект Department в контекст. Затем данные сохраняются. Код также выполняет запрос LINQ, возвращающий отдел, имя которого — Департментнамес. English.

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
 

## <a name="view-the-generated-database"></a>Просмотр созданной базы данных

При первом запуске приложения Entity Framework создает базу данных. Поскольку на компьютере установлен Visual Studio 2012, база данных будет создана на экземпляре LocalDB. По умолчанию Entity Framework имя базы данных после полного имени производного контекста (для этого примера это **енумкодефирст. енумтестконтекст**). В последующих случаях будет использоваться существующая база данных.  

Обратите внимание, что при внесении изменений в модель после создания базы данных следует использовать Code First Migrations для обновления схемы базы данных. Пример использования миграций см. в разделе [Code First к новой базе данных](~/ef6/modeling/code-first/workflows/new-database.md) .

Чтобы просмотреть базу данных и данные, выполните следующие действия.

1.  В главном меню Visual Studio 2012 выберите **View** -&gt; **Обозреватель объектов SQL Server**.
2.  Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши на **SQL Server** и выберите **Добавить SQL Server** использовать **проверку подлинности Windows** по умолчанию для подключения к экземпляру LocalDB.
3.  Разверните узел LocalDB.
4.  Unfold папку **databases** , чтобы увидеть новую базу данных, и перейдите к таблице **Department** . Обратите внимание, что Code First не создает таблицу, сопоставленную с типом перечисления.
5.  Чтобы просмотреть данные, щелкните таблицу правой кнопкой мыши и выберите **Просмотр данных** .

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели, как использовать типы Enum с Entity Framework Code First. 
