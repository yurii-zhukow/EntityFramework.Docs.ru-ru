# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="12b72-101">Участие в разработке документации по Entity Framework</span><span class="sxs-lookup"><span data-stu-id="12b72-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="12b72-102">В этом документе рассматривается процесс участия в написании статей и примеров кода, размещенных на [сайте документации по Entity Framework](https://docs.microsoft.com/ef).</span><span class="sxs-lookup"><span data-stu-id="12b72-102">The document covers the process for contributing to the articles and code samples that are hosted on the [Entity Framework documentation site](https://docs.microsoft.com/ef).</span></span> <span data-ttu-id="12b72-103">Это могут быть простые действия по исправлению опечаток или сложная процедура написания статьей.</span><span class="sxs-lookup"><span data-stu-id="12b72-103">Contributions may be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="12b72-104">Как создать простое исправление или предложение</span><span class="sxs-lookup"><span data-stu-id="12b72-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="12b72-105">Статьи хранятся в репозитории в виде файлов Markdown.</span><span class="sxs-lookup"><span data-stu-id="12b72-105">Articles are stored in the repository as Markdown files.</span></span> <span data-ttu-id="12b72-106">Простые изменения в содержимое файла Markdown можно вносить в браузере — нажмите ссылку **Изменить** в правом верхнем углу окна браузера.</span><span class="sxs-lookup"><span data-stu-id="12b72-106">Simple changes to the content of a Markdown file can be made in the browser by tapping the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="12b72-107">(В узком окне браузера может потребоваться развернуть панель **Параметры**, чтобы увидеть ссылку **Изменить**.) Следуйте инструкциям по созданию запроса на вытягивание (PR).</span><span class="sxs-lookup"><span data-stu-id="12b72-107">(In narrow browser windows you might need to expand the **options** bar to see the **Edit** link.) Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="12b72-108">Команда разработчиков EF рассмотрит запрос на вытягивание и примет его или предложит изменения.</span><span class="sxs-lookup"><span data-stu-id="12b72-108">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="12b72-109">Как сделать более сложные правки</span><span class="sxs-lookup"><span data-stu-id="12b72-109">How to make a more complex submission</span></span>

<span data-ttu-id="12b72-110">Потребуется базовое представление о [Git и GitHub.com](https://guides.github.com/activities/hello-world/).</span><span class="sxs-lookup"><span data-stu-id="12b72-110">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="12b72-111">Создайте [запрос](https://github.com/aspnet/EntityFramework.Docs/issues/new), описывающий, что нужно сделать, например изменить существующую статью или создать новую.</span><span class="sxs-lookup"><span data-stu-id="12b72-111">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="12b72-112">Дождитесь одобрения от команды EF, прежде чем тратить время.</span><span class="sxs-lookup"><span data-stu-id="12b72-112">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="12b72-113">Создайте вилку репозитория [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) и создайте ветвь для изменений.</span><span class="sxs-lookup"><span data-stu-id="12b72-113">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="12b72-114">Отправьте запрос на вытягивание с вашими изменениями в главную ветвь.</span><span class="sxs-lookup"><span data-stu-id="12b72-114">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="12b72-115">Ответьте на обратную связь по запросу на вытягивание.</span><span class="sxs-lookup"><span data-stu-id="12b72-115">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="12b72-116">Синтаксис Markdown</span><span class="sxs-lookup"><span data-stu-id="12b72-116">Markdown syntax</span></span>

<span data-ttu-id="12b72-117">Статьи написаны в формате [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), который представляет собой расширенную версию [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span><span class="sxs-lookup"><span data-stu-id="12b72-117">Articles are written in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), which is a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="12b72-118">Примеры синтаксиса DFM для функций пользовательского интерфейса, обычно используемых в документации по EF, см. в разделе [Шаблон метаданных и Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) руководства по стилю репозитория .NET Core.</span><span class="sxs-lookup"><span data-stu-id="12b72-118">For examples of DFM syntax for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span> 

## <a name="folder-structure-conventions"></a><span data-ttu-id="12b72-119">Соглашения о структуре папок</span><span class="sxs-lookup"><span data-stu-id="12b72-119">Folder structure conventions</span></span>

<span data-ttu-id="12b72-120">Изображения и другое статическое содержимое хранятся в папке `_static` в каждой области или папке сайта.</span><span class="sxs-lookup"><span data-stu-id="12b72-120">Images, and other static content, are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="12b72-121">Примеры кода хранятся в корневой папке `samples`.</span><span class="sxs-lookup"><span data-stu-id="12b72-121">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="12b72-122">Они организованы в структуру папок, которая имитирует структуру документации (находящуюся в корневой папке `entity-framework`).</span><span class="sxs-lookup"><span data-stu-id="12b72-122">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="12b72-123">Фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="12b72-123">Code snippets</span></span>

<span data-ttu-id="12b72-124">Статьи часто содержат фрагменты кода для иллюстрации.</span><span class="sxs-lookup"><span data-stu-id="12b72-124">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="12b72-125">DFM позволяет копировать код в файл Markdown или ссылаться на отдельный файл кода.</span><span class="sxs-lookup"><span data-stu-id="12b72-125">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="12b72-126">Рекомендуется использовать отдельные файлы кода, когда это возможно, чтобы свести к минимуму вероятность ошибок в коде.</span><span class="sxs-lookup"><span data-stu-id="12b72-126">We prefer to use separate code files whenever possible, to minimize the chance of errors in the code.</span></span> <span data-ttu-id="12b72-127">Файлы кода следует хранить в репозитории в соответствии со структурой папок, описанной ранее для образцов проектов.</span><span class="sxs-lookup"><span data-stu-id="12b72-127">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="12b72-128">Ниже приведены некоторые примеры [синтаксиса фрагмента кода DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span><span class="sxs-lookup"><span data-stu-id="12b72-128">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="12b72-129">Для отображения всего файла кода в виде фрагмента:</span><span class="sxs-lookup"><span data-stu-id="12b72-129">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="12b72-130">Для подготовки к просмотру части файла как фрагмента с помощью номеров строк:</span><span class="sxs-lookup"><span data-stu-id="12b72-130">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="12b72-131">Для фрагментов C# можно ссылаться на [область C#](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span><span class="sxs-lookup"><span data-stu-id="12b72-131">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="12b72-132">По возможности используйте области, а не номера строк, так как номера строк в файле кода, как правило, меняются и теряют синхронизацию с номерами строк в Markdown.</span><span class="sxs-lookup"><span data-stu-id="12b72-132">Whenever possible, use regions rather than line numbers, because line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="12b72-133">Области C# могут быть вложенными, и если вы ссылаетесь на внешнюю область, внутренняя область `#region` и директивы `#endregion` не отображаются во фрагменте.</span><span class="sxs-lookup"><span data-stu-id="12b72-133">C# regions can be nested, and if you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="12b72-134">Для подготовки области C# с именем "snippet_Example":</span><span class="sxs-lookup"><span data-stu-id="12b72-134">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="12b72-135">Чтобы выделить выбранные строки в готовом для просмотра фрагменте кода (обычно выделяются желтым цветом):</span><span class="sxs-lookup"><span data-stu-id="12b72-135">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="12b72-136">Тестирование изменений с помощью DocFX</span><span class="sxs-lookup"><span data-stu-id="12b72-136">Test your changes with DocFX</span></span>

<span data-ttu-id="12b72-137">Протестируйте изменения с помощью [средства командной строки DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), которое создает локально расположенную версию сайта.</span><span class="sxs-lookup"><span data-stu-id="12b72-137">Test your changes with the [DocFX command line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="12b72-138">DocFX не отображает стиль и расширения сайта, созданные для сайта docs.microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="12b72-138">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="12b72-139">Для работы с DocFX требуется .NET Framework в Windows или Mono (для Linux или macOS).</span><span class="sxs-lookup"><span data-stu-id="12b72-139">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="12b72-140">Инструкции для Windows</span><span class="sxs-lookup"><span data-stu-id="12b72-140">Windows instructions</span></span>

* <span data-ttu-id="12b72-141">Скачайте и распакуйте *docfx.zip* из [выпусков DocFX](https://github.com/dotnet/docfx/releases).</span><span class="sxs-lookup"><span data-stu-id="12b72-141">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="12b72-142">Добавьте DocFX в PATH.</span><span class="sxs-lookup"><span data-stu-id="12b72-142">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="12b72-143">В окне командной строки перейдите в клонированный репозиторий (который содержит файл *docfx.json*) и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="12b72-143">In a command line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="12b72-144">В браузере перейдите на адрес `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="12b72-144">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="12b72-145">Инструкции для Mono</span><span class="sxs-lookup"><span data-stu-id="12b72-145">Mono instructions</span></span>

* <span data-ttu-id="12b72-146">Установите Mono через Homebrew: `brew install mono`.</span><span class="sxs-lookup"><span data-stu-id="12b72-146">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="12b72-147">Загрузите [последнюю версию DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span><span class="sxs-lookup"><span data-stu-id="12b72-147">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="12b72-148">Извлеките в `\bin\docfx`.</span><span class="sxs-lookup"><span data-stu-id="12b72-148">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="12b72-149">Создайте псевдоним для **docfx**:</span><span class="sxs-lookup"><span data-stu-id="12b72-149">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="12b72-150">Выполните команду **docfx** в клонированном репозитории, чтобы создать сайт, и **docfx-serve**, чтобы просмотреть сайт по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="12b72-150">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="12b72-151">Стиль</span><span class="sxs-lookup"><span data-stu-id="12b72-151">Voice and tone</span></span>

<span data-ttu-id="12b72-152">Наши документы должны быть понятными для самой широкой аудитории.</span><span class="sxs-lookup"><span data-stu-id="12b72-152">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="12b72-153">С этой целью мы разработали рекомендации по стилю и просим наших авторов следовать им.</span><span class="sxs-lookup"><span data-stu-id="12b72-153">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="12b72-154">Дополнительные сведения см. в разделе [Рекомендации по стилю](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) в репозитории .NET.</span><span class="sxs-lookup"><span data-stu-id="12b72-154">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
