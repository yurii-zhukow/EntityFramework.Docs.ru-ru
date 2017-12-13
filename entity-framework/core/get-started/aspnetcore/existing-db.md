---
title: "Начало работы в ASP.NET Core — существующая база данных — Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Начало работы с EF Core в ASP.NET Core с существующей базой данных

> [!IMPORTANT]  
> [Пакет SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` и Visual Studio 2015. Всем, кто работает с .NET Core, мы рекомендуем выполнить [миграцию из project.json на csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) и [Visual Studio 2017](https://www.visualstudio.com/downloads/).

В этом пошаговом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework. Чтобы создать модель Entity Framework для существующей базы данных, мы применим метод реконструирования.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) из репозитория GitHub.

## <a name="prerequisites"></a>Предварительные требования

Для прохождения этого пошагового руководства нужны следующие элементы:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) с такими рабочими нагрузками:
  * **ASP.NET и веб-разработка** в разделе **Интернет и облако**;
  * **кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**).
* [пакет SDK для .NET Core 2.0](https://www.microsoft.com/net/download/core);
* [база данных для ведения блогов](#blogging-database).

### <a name="blogging-database"></a>База данных для ведения блогов

В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.

> [!TIP]  
> Если вы уже создали эту базу данных **для ведения блогов**, работая с другим руководством, можете пропустить эти шаги.

* Открытие Visual Studio
* Выберите **Инструменты -> Подключиться к базе данных...**.
* Выберите **Microsoft SQL Server** и щелкните **Продолжить**.
* Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.
* Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.
* Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.
* Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.
* Скопируйте представленный ниже скрипт и вставьте его в редактор запросов.
* Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Создание нового проекта

* Откройте Visual Studio 2017.
* Последовательно выберите **Файл > Создать > Проект**.
* В меню слева выберите **Установленные -> Шаблоны -> Visual C# -> Веб**.
* Выберите шаблон проекта **Веб-приложение ASP.NET Core (.NET Core)**.
* Введите имя проекта **EFGetStarted.AspNetCore.ExistingDb** и щелкните **ОК**.
* Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core**.
* В разделе **Шаблоны ASP.NET Core 2.0** выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").
* Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.
* Нажмите кнопку **ОК**.

## <a name="install-entity-framework"></a>Установка Entity Framework

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом пошаговом руководстве используется SQL Server. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Нам понадобятся некоторые средства платформы Entity Framework, чтобы создать модель на основе базы данных. Поэтому мы установим пакет средств:

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`

Позднее мы применим средства формирования шаблонов для ASP.NET Core, чтобы создать контроллеры и представления. Поэтому мы установим этот пакет разработки:

* Запуск `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Реконструирование модели

Теперь пора создать модель EF на основе существующей базы данных:

* Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.
* Выполните следующую команду, чтобы создать модель на основе существующей базы данных:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Если появляется сообщение об ошибке `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.

> [!TIP]  
> Вы можете выбрать, для каких таблиц нужно создать сущности, указав в команде выше аргумент `-Tables`. Пример: `-Tables Blog,Post`.

Процесс реконструирования создает классы сущностей (`Blog.cs` & `Post.cs`) и производный контекст (`BloggingContext.cs`) на основе схемы существующей базы данных.

 Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Регистрация контекста с помощью внедрения зависимостей

Внедрение зависимостей является ключевой концепцией для ASP.NET Core. С помощью внедрения зависимостей службы (например, `BloggingContext`) регистрируются во время запуска приложения. Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора. Дополнительные сведения о внедрении зависимостей вы найдете в статье [Introduction to Dependency Injection in ASP.NET Core](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) (Знакомство с концепцией внедрения зависимостей в ASP.NET Core) на сайте документации по этой платформе.

### <a name="remove-inline-context-configuration"></a>Удаление встроенного кода для настройки контекста

В ASP.NET Core настройка обычно выполняется в **файле Startup.cs**. Мы тоже применим этот вариант, а для этого перенесем в **Startup.cs** конфигурацию поставщика базы данных.

* Откройте `Models\BloggingContext.cs`.
* Удалите метод `OnConfiguring(...)`.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Добавьте следующий конструктор, который позволяет передавать в контекст конфигурацию с помощью внедрения зависимостей.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Регистрация и настройка контекста в файле Startup.cs

Чтобы контроллеры MVC могли использовать службу `BloggingContext`, ее нужно зарегистрировать:

* Откройте файл **Startup.cs**.
* Добавьте в начало этого файла следующие инструкции `using`.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Теперь мы можем использовать метод `AddDbContext(...)`, чтобы зарегистрировать нашу службу:
* Найдите в коде метод `ConfigureServices(...)`.
* Добавьте в него следующий фрагмент, который регистрирует контекст в качестве службы.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> В реальном приложении строка подключения обычно помещается в файл конфигурации. Здесь мы для простоты определяем ее прямо в коде. Дополнительные сведения см. в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).

## <a name="create-a-controller"></a>Создание контроллера

Теперь мы добавим в проект возможность формирования шаблонов:

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Полные зависимости** и щелкните **Добавить**.
* Откроется файл `ScaffoldingReadMe.txt` с инструкциями, которые вы можете проигнорировать.

Теперь формирование шаблонов включено, и мы можем сформировать контроллер для сущности `Blog`:

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.
* Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.
* Щелкните **Добавить**.

## <a name="run-the-application"></a>Запуск приложения

Теперь вы можете запустить приложение и увидеть, как оно работает:

* Выберите **Отладка -> Запустить без отладки**.
* Это действие выполняет сборку приложения и открывает его в веб-браузере.
* Перейдите к папке `/Blogs`.
* Щелкните **Создать**.
* Введите **URL-адрес** для нового блога и щелкните **Создать**.

![image](_static/create.png)

![image](_static/index-existing-db.png)
