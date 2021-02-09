---
title: Начало работы с EF Core и Xamarin — EF Core
description: Руководство по началу работы с Xamarin для создания мобильных приложений с помощью Entity Framework Core
author: codemillmatt
ms.date: 07/07/2020
ms.author: masoucou
uid: core/get-started/xamarin
ms.openlocfilehash: e4a26d7a0c6909002be0974ee9fe1c5db7c1046a
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983603"
---
# <a name="getting-started-with-ef-core-and-xamarin"></a>Начало работы с EF Core и Xamarin

В этом руководстве вы создадите приложение [Xamarin.Forms](/xamarin/get-started/what-is-xamarin-forms), которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.

Инструкции из этого руководства можно выполнять с помощью Visual Studio в Windows или Visual Studio для Mac.

> [!TIP]
> Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Xamarin) из репозитория GitHub.

## <a name="prerequisites"></a>Предварительные требования

Установите одно из следующих приложений:

* [Visual Studio 2019 версии 16.3 или более поздней](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:
  * **Разработка мобильных приложений на .NET**
* [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/)

В этой [документации содержатся подробные пошаговые инструкции по установке](/xamarin/get-started/installation) для каждой платформы.

## <a name="download-and-run-the-sample-project"></a>Скачивание и запуск примера проекта

Чтобы запустить и изучить этот пример приложения, скачайте код на сайте GitHub.

После скачивания откройте файл решения `EFGettingStarted.sln` в Visual Studio или Visual Studio для Mac и запустите приложение на выбранной платформе.

При первом запуске приложения локальная база данных SQLite будет заполнена двумя записями, представляющими блоги.

![Снимок экрана: страница со списком всех блогов](_static/xamarin-tutorial-1.png)

На панели инструментов нажмите кнопку **Add** (Добавить).

Появится новая страница, на которой можно ввести сведения о новом блоге.

![Снимок экрана: страница изменения сведений о новом блоге](_static/xamarin-tutorial-2.png)

Введите все сведения и на панели инструментов нажмите кнопку **Save** (Сохранить). Новый блог сохранится в базе данных SQLite приложения и появится в списке.

Вы можете щелкнуть одну из записей блога в списке и просмотреть все записи в этом блоге.

![Снимок экрана: страница со списком записей в блоге](_static/xamarin-tutorial-3.png)

На панели инструментов нажмите кнопку **Add** (Добавить).

Появится страница, на которой можно ввести сведения о новой записи блога.

![Снимок экрана: страница добавления новой записи](_static/xamarin-tutorial-4.png)

Введите все сведения и на панели инструментов нажмите кнопку **Save** (Сохранить).

Новая запись будет связана с записью блога, которую вы выбрали на предыдущем шаге, сохранится в базе данных SQLite приложения и появится в списке.

Вернитесь на страницу со списком блогов. На панели инструментов нажмите кнопку **Delete All** (Удалить все). Все блоги и соответствующие записи будут удалены из базы данных SQLite приложения.

![Снимок экрана приложения после удаления всех блогов](_static/xamarin-tutorial-5.png)

## <a name="explore-the-code"></a>Обзор кода

В следующих разделах последовательно разбирается код из образца проекта, который считывает, создает, обновляет и удаляет данные в базе данных SQLite с помощью EF Core с Xamarin.Forms.

Предполагается, что вы знаете как [отображать данные](/xamarin/xamarin-forms/app-fundamentals/data-binding/) и [переходить между страницами](/xamarin/xamarin-forms/app-fundamentals/navigation/) с помощью Xamarin.Forms.

> [!IMPORTANT]
> Entity Framework Core применяет отражение для вызова функций, которые компоновщик Xamarin.iOS может исключить, если используются конфигурации режима **Выпуск**. Предотвратить это можно одним из двух способов.
>
> * Первый способ: добавьте `--linkskip System.Core` в поле **Дополнительные аргументы mtouch** в параметрах **сборки iOS**.
> * Второй способ: в параметрах **сборки iOS** задайте для параметра **Поведение компоновщика** Xamarin.iOS значение `Don't Link`.
> Дополнительные сведения о компоновщике Xamarin.iOS, включая настройку его поведения для Xamarin.iOS, приведены в [этой статье](/xamarin/ios/deploy-test/linker). Такой подход не идеален, так как он может стать причиной отклонения в магазине.

## <a name="entity-framework-core-nuget-packages"></a>Пакеты Entity Framework Core NuGet

Чтобы создавать приложения Xamarin.Forms с помощью EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать, во все проекты в решении Xamarin.Forms. В этом руководстве используется поставщик SQLite.

В каждом из проектов решения Xamarin.Forms требуется указанный ниже пакет NuGet.

* `Microsoft.EntityFrameworkCore.Sqlite`

## <a name="model-classes"></a>Классы модели

Каждая таблица в базе данных SQLite, доступ к которой осуществляется через EF Core, моделируется в классе. В этом примере используются два класса: `Blog` и `Post`. Они находятся в папке `Models`.

Классы модели состоят только из свойств, которые моделируют столбцы в базе данных.

* **Blog.cs**

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Models/Blog.cs)]

* Свойство `Posts` определяет связь "родитель-потомок" между `Blog` и `Post`.

* **Post.cs**

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Models/Post.cs)]

* Свойства `BlogId` и `Blog` определяют обратные связи с родительским объектом `Blog` для экземпляра `Post`.

## <a name="data-context"></a>Контекст данных

Класс `BloggingContext` находится в папке `Services` и наследуется от класса EF Core `DbContext`. `DbContext` используется для группирования запросов к базе данных и изменений.

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Services/BloggingContext.cs)]

* Оба свойства в этом классе типа `DbSet` служат для работы с базовыми таблицами, представляющими блоги и записи.
* `SQLitePCL.Batteries_V2.Init()` требуется в конструкторе для инициации SQLite в iOS.
* Функция `OnConfiguring` задает расположение базы данных SQLite на физическом устройстве.

## <a name="create-read-update--delete"></a>Создание, чтение, обновление и удаление

Ниже приведены некоторые места в приложении, где EF Core используется для доступа к SQLite.

### <a name="read"></a>Чтение

* Возврат всех записей.
  * Функция `OnAppearing` в `BlogsPage.xaml.cs` возвращает все записи `Blog` и сохраняет их в переменной `List`.

```csharp
using (var blogContext = new BloggingContext())
{
    var theBlogs = blogContext.Blogs.ToList();
}
```

* Возврат определенных записей.
  * Функция `OnAppearing` в `PostsPage.xaml.cs` возвращает записи `Post`, содержащие определенное значение `BlogId`.

```csharp
using (var blogContext = new BloggingContext())
{
    var postList = blogContext.Posts
        .Where(p => p.BlogId == BlogId)
        .ToList();
}
```

### <a name="create"></a>Создать

* Вставка новой записи.
  * Функция `Save_Clicked` в `AddBlogPage.xaml.cs` вставляет новый объект `Blog` в базу данных SQLite.

```csharp
var blog = new Blog { Url = blogUrl.Text };

using (var blogContext = new BloggingContext())
{
    blogContext.Add(blog);

    await blogContext.SaveChangesAsync();
}
```

### <a name="update"></a>Обновление

* Изменение существующей записи.
  * Функция `Save_Clicked` в `AddPostPage.xaml.cs` изменяет существующий объект `Blog`, используя новый объект `Post`.

```csharp
var newPost = new Post
{
    BlogId = BlogId,
    Content = postCell.Text,
    Title = titleCell.Text
};

using (var blogContext = new BloggingContext())
{
    var blog = await blogContext
        .Blogs
        .FirstAsync(b => b.BlogId == BlogId);

    blog.Posts.Add(newPost);

    await blogContext.SaveChangesAsync();
}
```

### <a name="delete"></a>Удаление

* Удаление всех записей, включая дочерние.
  * Функция `DeleteAll_Clicked` в `BlogsPage.xaml.cs` удаляет все записи `Blog` в базе данных SQLite, распространяя удаление на все дочерние записи `Post` объекта `Blog`.

```csharp
using (var blogContext = new BloggingContext())
{
    blogContext.RemoveRange(blogContext.Blogs);

    await blogContext.SaveChangesAsync();
}
```

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве вы узнали, как использовать приложение Xamarin.Forms для доступа к базе данных SQLite с помощью Entity Framework Core.

Другие разделы по Entity Framework Core, которые могут представлять интерес для разработчиков Xamarin:

* [Настройка `DbContext`](xref:core/dbcontext-configuration/index)
* См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).
* [Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).
