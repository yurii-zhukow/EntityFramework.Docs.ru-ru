---
ms.openlocfilehash: 79a2a10cae9f8a5541bca132e407d4abbe95e093
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914106"
---
# <a name="contributing-to-the-entity-framework-documentation"></a>Участие в разработке документации по Entity Framework

Далее описывается процесс участия в создании статей и примеров кода для документации по Entity Framework. Это могут быть простые действия по исправлению опечаток или сложная процедура написания статьей.

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>Как создать простое исправление или предложение

Статьи хранятся в репозитории в виде файлов Markdown. Чтобы внести простое изменение в содержимое файла Markdown, щелкните ссылку **Изменить** в правом верхнем углу окна браузера. Чтобы увидеть ссылку **Изменить**, может потребоваться развернуть панель **Параметры**. Следуйте инструкциям по созданию запроса на вытягивание (PR). Команда разработчиков EF рассмотрит запрос на вытягивание и примет его или предложит изменения.

## <a name="how-to-make-a-more-complex-submission"></a>Как сделать более сложные правки

Потребуется базовое представление о [Git и GitHub.com](https://guides.github.com/activities/hello-world/).

* Создайте [запрос](https://github.com/aspnet/EntityFramework.Docs/issues/new), описывающий, что нужно сделать, например изменить существующую статью или создать новую. Дождитесь одобрения от команды EF, прежде чем тратить время.
* Создайте вилку репозитория [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) и создайте ветвь для изменений.
* Отправьте запрос на вытягивание с вашими изменениями в главную ветвь.
* Ответьте на обратную связь по запросу на вытягивание.

## <a name="markdown-syntax"></a>Синтаксис Markdown

Статьи написаны в формате [DocFx-flavored Markdown (DFM)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), который представляет собой расширенную версию [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/). Примеры синтаксиса DFM и метаданных для функций пользовательского интерфейса, обычно используемых в документации по EF, см. в разделе [Шаблон метаданных и Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) руководства по стилю репозитория .NET Core.

## <a name="folder-structure-conventions"></a>Соглашения о структуре папок

Изображения и другое статическое содержимое хранится в папке `_static` в каждой области или папке сайта.

Примеры кода хранятся в корневой папке `samples`. Они организованы в структуру папок, которая имитирует структуру документации (находящуюся в корневой папке `entity-framework`).

## <a name="code-snippets"></a>Фрагменты кода

Статьи часто содержат фрагменты кода для иллюстрации. DFM позволяет копировать код в файл Markdown или ссылаться на отдельный файл кода. Когда это возможно, рекомендуется использовать отдельные файлы кода, чтобы свести к минимуму вероятность ошибок в коде. Файлы кода следует хранить в репозитории в соответствии со структурой папок, описанной ранее для образцов проектов.

Ниже приведены некоторые примеры [синтаксиса фрагмента кода DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).

Для отображения всего файла кода в виде фрагмента:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

Для подготовки к просмотру части файла как фрагмента с помощью номеров строк:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

Для фрагментов C# можно ссылаться на [область C#](https://msdn.microsoft.com/library/9a1ybwek.aspx). Используйте области, а не номера строк. Номера строк в файле кода, как правило, меняются и теряют синхронизацию с номерами строк в Markdown. Области C# могут быть вложенными. Если вы ссылаетесь на внешнюю область, внутренняя область `#region` и директивы `#endregion` не отображаются во фрагменте.

Для подготовки области C# с именем "snippet_Example":

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

Чтобы выделить выбранные строки в готовом для просмотра фрагменте кода (обычно выделяются желтым цветом):

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a>Тестирование изменений с помощью DocFX

Протестируйте изменения с помощью [средства командной строки DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), которое создает локально расположенную версию сайта. DocFX не отображает стиль и расширения сайта, созданные для сайта docs.microsoft.com.

Для работы с DocFX требуется .NET Framework в Windows или Mono (для Linux или macOS).

### <a name="windows-instructions"></a>Инструкции для Windows

* Скачайте и распакуйте *docfx.zip* из [выпусков DocFX](https://github.com/dotnet/docfx/releases).
* Добавьте DocFX в PATH.
* В окне командной строки перейдите в клонированный репозиторий (который содержит файл *docfx.json*) и выполните следующую команду:

   ``` console
   docfx -t default --serve
   ```

* В браузере перейдите на адрес `http://localhost:8080`.

### <a name="mono-instructions"></a>Инструкции для Mono

* Установите Mono через Homebrew: `brew install mono`.
* Загрузите [последнюю версию DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).
* Извлеките в `\bin\docfx`.
* Создайте псевдоним для **docfx**:

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* Выполните команду **docfx** в клонированном репозитории, чтобы создать сайт, и **docfx-serve**, чтобы просмотреть сайт по адресу `http://localhost:8080`.

## <a name="voice-and-tone"></a>Стиль

Наши документы должны быть понятными для самой широкой аудитории. С этой целью мы разработали рекомендации по стилю и просим наших авторов следовать им. Дополнительные сведения см. в разделе [Рекомендации по стилю](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) в репозитории .NET.
