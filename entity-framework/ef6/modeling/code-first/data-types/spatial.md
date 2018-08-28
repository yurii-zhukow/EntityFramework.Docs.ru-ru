---
title: Пространственные - EF6 Code First.
author: divega
ms.date: 2016-10-23
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: d15b407203a7dd7ddf92d0759af00f3ad5e41f57
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995216"
---
# <a name="spatial---code-first"></a>Пространственные - Code First
> [!NOTE]
> **EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

Видео и пошаговые руководства показано, как сопоставить пространственных типов с помощью Entity Framework Code First. Также показано, как использовать LINQ-запрос для поиска расстояние между двумя расположениями.

В этом пошаговом руководстве будет использовать Code First для создания новой базы данных, но можно также использовать [Code First для существующей базы данных](~/ef6/modeling/code-first/workflows/existing-database.md).

В Entity Framework 5 появилась поддержка пространственных типов. Обратите внимание на то, что чтобы использовать новые функции, например пространственного типа, перечислимые типы и функции, возвращающие табличные значения, необходимо ориентироваться .NET Framework 4.5. Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.

Для использования пространственных типов данных необходимо также использовать с поставщиком Entity Framework с поддержкой пространственных. См. в разделе [поддержка пространственных типов](~/ef6/fundamentals/providers/spatial-support.md) Дополнительные сведения.

Существует два типа основных пространственных данных: geography и geometry. Тип данных сохраняет эллиптические данные (например, GPS координаты широты и долготы). Тип данных geometry представляет Евклидовой (плоской) системе координат.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео показано, как сопоставить пространственных типов с помощью Entity Framework Code First. Также показано, как использовать LINQ-запрос для поиска расстояние между двумя расположениями.

**Представленный**: Юлия Корнич

**Видео**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Предварительные требования

Необходимо будет установлен выпуск Visual Studio 2012 Ultimate, Premium, Professional и Web Express для выполнения этого пошагового руководства.

## <a name="set-up-the-project"></a>Настройка проекта

1.  Откройте Visual Studio 2012
2.  На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**
3.  В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона
4.  Введите **SpatialCodeFirst** как имя проекта и нажмите кнопку **ОК**

## <a name="define-a-new-model-using-code-first"></a>Определения новой модели с помощью Code First

При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен). Приведенный ниже код определяет класс университета.

В университете ведется свойство Location типа DbGeography. Чтобы использовать тип DbGeography, необходимо добавить ссылку на сборку System.Data.Entity и также добавить System.Data.Spatial, с помощью инструкции.

Откройте файл Program.cs и вставьте следующие операторы using в верхней части файла:

``` csharp
using System.Data.Spatial;
```

Добавьте следующее определение класса университета в файл Program.cs.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
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

Обратите внимание, что помимо сборки EntityFramework, ссылку на сборку System.ComponentModel.DataAnnotations также добавляется.

В верхней части файла Program.cs, добавьте следующий оператор using:

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

## <a name="persist-and-retrieve-data"></a>Сохранения и извлечения данных

Откройте файл Program.cs, в котором определен метод Main. Добавьте следующий код в функцию Main.

Код добавляет два новых университета объектов к контексту. Пространственные свойства инициализируются с помощью метода DbGeography.FromText. Географическая точка представить в виде WellKnownText передается в метод. Затем код сохраняет данные. Затем запрос LINQ, который возвращает объект университета, где она будет находиться в указанное расположение, ближайшее составлением и выполнением.

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

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Представление создаваемой базы данных

При запуске приложение в первый раз, Entity Framework создает базу данных. Поскольку у нас есть установленной Visual Studio 2012, базе данных создается на экземпляре LocalDB. По умолчанию Entity Framework содержит имя базы данных после полное имя производного контекста (в этом примере, **SpatialCodeFirst.UniversityContext**). Последующие случаи, когда будет использоваться существующая база данных.  

Обратите внимание, что если внести изменения в модель после создания базы данных, необходимо использовать Code First Migrations для обновления схемы базы данных. См. в разделе [Code First для новой базы данных](~/ef6/modeling/code-first/workflows/new-database.md) пример с помощью миграций.

Чтобы просмотреть базы данных и данных, сделайте следующее:

1.  В главном меню Visual Studio 2012 выберите **представление**  - &gt; **обозреватель объектов SQL Server**.
2.  Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши **SQL Server** и выберите **добавить SQL Server** используйте значение по умолчанию **проверки подлинности Windows** для подключения к экземпляр LocalDB
3.  Разверните узел LocalDB
4.  Unfold **баз данных** папку для новой базы данных см. в разделе и перейдите к **университеты** таблицы
5.  Чтобы просмотреть данные, щелкните правой кнопкой мыши по таблице и выберите **Просмотр данных**

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели способы использования пространственных типов с Entity Framework Code First. 
