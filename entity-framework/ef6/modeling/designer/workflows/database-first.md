---
title: Database First EF6
description: Database First в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/workflows/database-first
ms.openlocfilehash: 4046144ff3f1ecb4a3c9e7d4cb5d6c462c3782c7
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066113"
---
# <a name="database-first"></a>Database First
В этом видео и пошаговом руководстве представлены общие сведения о Database First разработке с помощью Entity Framework. Database First позволяет реконструировать модель из существующей базы данных. Модель хранится в EDMX-файле (расширение EDMX) и может быть просмотрена и изменена в Entity Framework Designer. Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видеоролике приводятся общие сведения о Database First разработке с помощью Entity Framework. Database First позволяет реконструировать модель из существующей базы данных. Модель хранится в EDMX-файле (расширение EDMX) и может быть просмотрена и изменена в Entity Framework Designer. Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.

**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)

**Видео**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv)  |  [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.

Если вы используете Visual Studio 2010, вам также потребуется установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

 

## <a name="1-create-an-existing-database"></a>1. Создание существующей базы данных

Обычно при использовании существующей базы данных она будет создана, но в этом пошаговом руководстве необходимо создать базу данных для доступа к ней.

Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.

-   При использовании Visual Studio 2010 будет создаваться база данных SQL Express.
-   Если вы используете Visual Studio 2012, то будете создавать базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

 

Давайте создадим базу данных.

-   Открытие Visual Studio
-   **Просмотр — &gt; Обозреватель сервера**
-   Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**
-   Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   Подключитесь к LocalDB или SQL Express, в зависимости от того, какой из установленных служб, и введите **датабасефирст. блога** в качестве имени базы данных.

    ![DF подключения к SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![DF подключения LocalDB](~/ef6/media/localdbconnectiondf.png)

-   Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .

    ![Диалоговое окно создания базы данных](~/ef6/media/createdatabasedialog.png)

-   Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .
-   Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a>2. Создание приложения

Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Database First для доступа к данным:

-   Открытие Visual Studio
-   **Файл- &gt; создать &gt; проект...**
-   Выбор **окон** в меню слева и **консольное приложение**
-   Введите **датабасефирстсампле** в качестве имени
-   Нажмите кнопку **ОК**.

 

## <a name="3-reverse-engineer-model"></a>3. реконструирование модели

Мы будем использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.

-   **Проект- &gt; Добавить новый элемент...**
-   Выберите **данные** в меню слева, а затем **ADO.NET EDM**
-   Введите **блоггингмодел** в качестве имени и нажмите кнопку **ОК** .
-   Запустится **мастер EDM**
-   Выберите **создать из базы данных** и нажмите кнопку **Далее** .

    ![Шаг 1 мастера](~/ef6/media/wizardstep1.png)

-   Выберите подключение к базе данных, созданной в первом разделе, введите **BloggingContext** в качестве имени строки подключения и нажмите кнопку **Далее** .

    ![Шаг 2 мастера](~/ef6/media/wizardstep2.png)

-   Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".

    ![Шаг 3 мастера](~/ef6/media/wizardstep3.png)

 

После завершения процесса реконструирования новая модель добавляется в проект и открывается для просмотра в Entity Framework Designer. В проект также добавлен файл App.config с подробными сведениями о соединении для базы данных.

![Начальная модель](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные действия в Visual Studio 2010

При работе в Visual Studio 2010 необходимо выполнить некоторые дополнительные действия по обновлению до последней версии Entity Framework. Обновление важно, так как оно предоставляет доступ к улучшенной поверхности API, которая гораздо проще в использовании, а также последние исправления ошибок.

Сначала необходимо получить последнюю версию Entity Framework из NuGet.

-   **Проект — &gt; Управление пакетами NuGet...** 
     *Если у вас нет параметра **Управление пакетами NuGet** , следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*
-   Выбор вкладки "в **сети** "
-   Выберите пакет **EntityFramework**
-   Щелкните **Установить**.

Далее необходимо поменять модель, чтобы создать код, который использует API DbContext, который появился в более поздних версиях Entity Framework.

-   Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите пункт **Добавить элемент создания кода...**
-   В меню слева выберите **шаблоны в Интернете** и выполните поиск по запросу **DbContext**
-   Выберите генератор EF **5. x DbContext для C \# **, введите **блоггингмодел** в качестве имени и нажмите кнопку **Добавить** .

    ![Шаблон DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. чтение & запись данных

Теперь, когда у нас есть модель, пришло время использовать ее для доступа к некоторым данным. Классы, которые мы будем использовать для доступа к данным, создаются автоматически на основе файла EDMX.

*Этот снимок экрана находится в Visual Studio 2012, если вы используете Visual Studio 2010 файлы BloggingModel.tt и BloggingModel.Context.tt будут находиться непосредственно в проекте, а не вложены в EDMX-файл.*

![Создаваемые классы DF](~/ef6/media/generatedclassesdf.png)

 

Реализуйте метод Main в Program.cs, как показано ниже. Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового блога. Затем он использует запрос LINQ для получения всех блогов из базы данных, упорядоченного в алфавитном порядке по названию.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Теперь можно запустить приложение и протестировать его.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. Работа с изменениями базы данных

Пришло время внести некоторые изменения в схему базы данных, при внесении этих изменений нам также потребуется обновить нашу модель, чтобы отразить эти изменения.

Первым шагом является внесение некоторых изменений в схему базы данных. В схему будет добавлена таблица пользователей.

-   Щелкните правой кнопкой мыши базу данных **датабасефирст. блоги** в обозреватель сервера и выберите **создать запрос** .
-   Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Теперь, когда схема обновлена, пришло время обновить модель с помощью этих изменений.

-   Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите "обновить модель из базы данных...", после чего запустится мастер обновления.
-   На вкладке Добавить мастера обновления установите флажок рядом с таблицами. Это означает, что нам нужно добавить новые таблицы из схемы.
    *На вкладке обновление отображаются все имеющиеся в модели таблицы, которые будут проверяться на наличие изменений во время обновления. На вкладках удаление отображаются все таблицы, которые были удалены из схемы и также будут удалены из модели в рамках обновления. Информация на этих двух вкладках автоматически обнаруживается и предоставляется только в информационных целях, поэтому изменить параметры нельзя.*

    ![Мастер обновления](~/ef6/media/refreshwizard.png)

-   Нажмите кнопку "Готово" в мастере обновления.

 

Теперь модель обновлена, чтобы включить новую сущность пользователя, сопоставленную с таблицей Users, добавленной в базу данных.

![Модель обновлена](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Итоги

В этом пошаговом руководстве мы рассматривали Database Firstную разработку, которая позволила нам создать модель в конструкторе EF на основе существующей базы данных. Затем мы использовали эту модель для чтения и записи данных из базы данных. Наконец, мы обновили модель, чтобы отразить изменения, внесенные в схему базы данных.
