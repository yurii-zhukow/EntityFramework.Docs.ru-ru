---
title: Начало работы в ASP.NET Core — существующая база данных — Core EF
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 23cd53b0e162afc5db0243b7032bb9c5f18bfb35
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688697"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Начало работы с EF Core в ASP.NET Core с существующей базой данных

В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core. Для создания модели Entity Framework используется реконструирование существующей базы данных.

[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

* [Visual Studio 2017 версии 15.7](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:
  * **ASP.NET и веб-разработка** в разделе **Интернет и облако**)
  * **Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)
* [Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Создание базы данных Blogging

В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb. Если вы уже создали базу данных **Blogging**, работая с другим руководством, пропустите эти шаги.

* Открытие Visual Studio
* Выберите **Инструменты -> Подключиться к базе данных...**.
* Выберите **Microsoft SQL Server** и щелкните **Продолжить**.
* Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.
* Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.
* Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.
* Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.
* Скопируйте представленный ниже скрипт и вставьте его в редактор запросов
* Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Создание нового проекта

* Откройте Visual Studio 2017.
* **"Файл" > "Создать" > "Проект"…**
* В меню слева выберите **"Установленные" > Visual C# > Интернет**
* Выберите шаблон проекта **Веб-приложение ASP.NET Core**
* Введите имя проекта **EFGetStarted.AspNetCore.ExistingDb** и щелкните **ОК**.
* Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core**.
* В соответствующих раскрывающихся списках должна быть выбрана целевая платформа **.NET Core** и версия **ASP.NET Core 2.1**
* Выберите шаблон **Веб-приложение (модель-представление-контроллер)**
* Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.
* Нажмите кнопку **ОК**.

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных). 

В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется. Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Реконструирование модели

Теперь пора создать модель EF на основе существующей базы данных:

* Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.
* Выполните следующую команду, чтобы создать модель на основе существующей базы данных:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Если появляется сообщение об ошибке `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.

> [!TIP]  
> Вы можете выбрать, для каких таблиц нужно создать сущности, указав в команде выше аргумент `-Tables`. Например, `-Tables Blog,Post`.

Процесс реконструирования создает классы сущностей (`Blog.cs` & `Post.cs`) и производный контекст (`BloggingContext.cs`) на основе схемы существующей базы данных.

 Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения. Ниже приведены классы сущностей `Blog` и `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Чтобы включить отложенную загрузку, можно присвоить свойствам навигации (Blog.Post и Post.Blog) значение `virtual`.

 Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

### <a name="register-and-configure-your-context-in-startupcs"></a>Регистрация и настройка контекста в файле Startup.cs

Чтобы сделать контекст `BloggingContext` доступным MVC-контроллерам, зарегистрируйте его как службу.

* Откройте файл **Startup.cs**.
* Добавьте в начало этого файла следующие инструкции `using`.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Теперь вы можете использовать метод `AddDbContext(...)`, чтобы зарегистрировать его как службу.
* Найдите в коде метод `ConfigureServices(...)`.
* Добавьте в него следующий выделенный код, который регистрирует контекст в качестве службы

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> В реальном приложении строка подключения обычно помещается в файл конфигурации или переменную среды. Для простоты в этом руководстве она будет задана в коде. Дополнительные сведения см. в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).

## <a name="create-a-controller-and-views"></a>Создание контроллера и представлений

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.
* Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.
* Щелкните **Добавить**.

## <a name="run-the-application"></a>Запуск приложения

Теперь вы можете запустить приложение и увидеть, как оно работает:

* Выберите **Отладка -> Запустить без отладки**.
* Будет выполнена сборка приложения, и оно откроется в браузере
* Перейдите к папке `/Blogs`.
* Щелкните **Создать**.
* Введите **URL-адрес** для нового блога и щелкните **Создать**.

  ![Страница "Создать"](_static/create.png)

  ![Страница индексов](_static/index-existing-db.png)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о формировании классов контекста и сущности см. в следующих статьях:
* [Реконструирование](xref:core/managing-schemas/scaffolding)
* [Entity Framework Core tools reference - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold) (Справочник по основным инструментам Entity Framework Core — .NET CLI)
* [Entity Framework Core tools reference - Package Manager Console](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext) (Справочник по основным инструментам Entity Framework Core — консоль диспетчера пакетов)