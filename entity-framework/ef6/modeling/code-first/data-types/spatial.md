---
title: Пространственный-Code First — EF6
description: Пространственный Code First в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/code-first/data-types/spatial
ms.openlocfilehash: 5d18624cdcef8db3e604277aeef1c3a2d05bfcab
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073968"
---
# <a name="spatial---code-first"></a>Пространственный Code First
> [!NOTE]
> **Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

В видео и пошаговом руководстве показано, как сопоставлять пространственные типы с Entity Framework Code First. В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.

В этом пошаговом руководстве будет использоваться Code First для создания новой базы данных, но можно также использовать [Code First в существующей базе данных](xref:ef6/modeling/code-first/workflows/existing-database).

Поддержка пространственного типа появилась в Entity Framework 5. Обратите внимание, что для использования новых функций, таких как пространственный тип, перечисления и функции с табличным значением, необходимо выбрать .NET Framework 4,5. Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.

Чтобы использовать пространственные типы данных, необходимо также использовать поставщик Entity Framework с поддержкой пространственного доступа. Дополнительные сведения см. в разделе [Поддержка пространственных типов в поставщике](xref:ef6/fundamentals/providers/spatial-support) .

Существует два основных типа пространственных данных: geography и geometry. Тип данных geography хранит данные эллиптических (например, координаты широты и долготы GPS). Тип данных geometry представляет евклидово (плоскую) систему координат.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео показано, как сопоставлять пространственные типы с Entity Framework Code First. В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.

**Представлено**: Julia Корнич

**Видео**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv)  |  [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.

## <a name="set-up-the-project"></a>Настройка проекта

1.  Откройте Visual Studio 2012
2.  В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .
3.  В левой области щелкните **Visual C \# **, а затем выберите шаблон **консоли** .
4.  Введите **спатиалкодефирст** в качестве имени проекта и нажмите кнопку **ОК** .

## <a name="define-a-new-model-using-code-first"></a>Определение новой модели с помощью Code First

При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель. В приведенном ниже коде определяется класс университет.

Университет имеет свойство Location типа заданное DbGeography. Чтобы использовать тип заданное DbGeography, необходимо добавить ссылку на сборку System. Data. Entity, а также добавить инструкцию System. Data. пространственный using.

Откройте файл Program.cs и вставьте в начало файла следующие инструкции using:

``` csharp
using System.Data.Spatial;
```

Добавьте следующее определение класса университет в файл Program.cs.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>Определение производного типа DbContext

В дополнение к определению сущностей необходимо определить класс, производный от DbContext и предоставляющий свойства DbSet&lt;TEntity&gt;. Свойства DbSet&lt;TEntity&gt; позволяют контексту определить, какие типы необходимо включить в модель.

Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, в частности заполняет объекты данными из базы данных, отслеживает изменения и сохраняет данные в базе данных.

Типы DbContext и DbSet определены в сборке EntityFramework. Мы добавим ссылку на эту библиотеку DLL с помощью пакета NuGet EntityFramework.

1.  В обозреватель решений щелкните правой кнопкой мыши имя проекта.
2.  Выберите **Управление пакетами NuGet...**
3.  В диалоговом окне Управление пакетами NuGet выберите вкладку в **Интернете** и выберите пакет **EntityFramework** .
4.  Щелкните **Установить**.

Обратите внимание, что в дополнение к сборке EntityFramework также добавляется ссылка на сборку System. ComponentModel..

В верхней части файла Program.cs добавьте следующую инструкцию using:

``` csharp
using System.Data.Entity;
```

В Program.cs добавьте определение контекста. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Сохранение и получение данных

Откройте файл Program.cs, в котором определен метод Main. Добавьте следующий код в функцию main.

Код добавляет в контекст два новых объекта университета. Пространственные свойства инициализируются с помощью метода заданное DbGeography. FromText. Географическая точка, представленная как Веллкновнтекст, передается в метод. Затем код сохраняет данные. Затем выполняется построение и выполнение запроса LINQ, который возвращает объект университета, где его расположение ближе всего к указанному расположению.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Скомпилируйте и запустите приложение. Программа выдает следующие результаты.

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Просмотр созданной базы данных

При первом запуске приложения Entity Framework создает базу данных. Поскольку на компьютере установлен Visual Studio 2012, база данных будет создана на экземпляре LocalDB. По умолчанию Entity Framework присваивает базе данных имя, соответствующее полному имени производного контекста (в данном примере это **спатиалкодефирст. университиконтекст**). В последующих случаях будет использоваться существующая база данных.  

Обратите внимание, что при внесении изменений в модель после создания базы данных следует использовать Code First Migrations для обновления схемы базы данных. Пример использования миграций см. в разделе [Code First к новой базе данных](xref:ef6/modeling/code-first/workflows/new-database) .

Чтобы просмотреть базу данных и данные, выполните следующие действия.

1.  В главном меню Visual Studio 2012 выберите **Просмотреть**  - &gt; **Обозреватель объектов SQL Server**.
2.  Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши на **SQL Server** и выберите **Добавить SQL Server** использовать **проверку подлинности Windows** по умолчанию для подключения к экземпляру LocalDB.
3.  Разверните узел LocalDB.
4.  Unfold папку **databases** , чтобы увидеть новую базу данных, и перейдите к таблице **университетов** .
5.  Чтобы просмотреть данные, щелкните таблицу правой кнопкой мыши и выберите **Просмотр данных** .

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели использование пространственных типов с Entity Framework Code First. 
