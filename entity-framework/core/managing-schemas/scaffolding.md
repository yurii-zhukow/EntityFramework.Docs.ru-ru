---
title: Реконструирование — EF Core
description: Реконструирование модели из существующей базы данных с помощью Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: e88be834aa1f2f3be41657f07bc7ace3e34c58aa
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619218"
---
# <a name="reverse-engineering"></a><span data-ttu-id="cc5cf-103"> Реконструирование</span><span class="sxs-lookup"><span data-stu-id="cc5cf-103">Reverse Engineering</span></span>

<span data-ttu-id="cc5cf-104">Реконструирование — это процесс формирования шаблонов классов типов сущностей и класса DbContext на основе схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-104">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="cc5cf-105">Его можно выполнить с помощью `Scaffold-DbContext` команды EF Core инструментов консоли диспетчера пакетов или `dotnet ef dbcontext scaffold` команды интерфейса командной строки (CLI) .NET.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-105">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="cc5cf-106">Установка</span><span class="sxs-lookup"><span data-stu-id="cc5cf-106">Installing</span></span>

<span data-ttu-id="cc5cf-107">Перед реконструированием необходимо установить либо [средства PMC](xref:core/miscellaneous/cli/powershell) (только Visual Studio), либо [средства CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="cc5cf-107">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="cc5cf-108">Дополнительные сведения см. в ссылках.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-108">See links for details.</span></span>

<span data-ttu-id="cc5cf-109">Вам также потребуется установить соответствующий [поставщик базы данных](xref:core/providers/index) для схемы базы данных, которую необходимо реконструировать.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-109">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="cc5cf-110">Строка подключения</span><span class="sxs-lookup"><span data-stu-id="cc5cf-110">Connection string</span></span>

<span data-ttu-id="cc5cf-111">Первым аргументом команды является строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-111">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="cc5cf-112">Эти средства будут использовать эту строку соединения для чтения схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-112">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="cc5cf-113">Способ заключения и экранирования строки подключения зависит от того, какая оболочка используется для выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-113">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="cc5cf-114">Дополнительные сведения см. в документации по оболочке.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-114">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="cc5cf-115">Например, PowerShell требует экранирования `$` символа, но не `\` .</span><span class="sxs-lookup"><span data-stu-id="cc5cf-115">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="cc5cf-116">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc5cf-116">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="cc5cf-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc5cf-117">Visual Studio</span></span>](#tab/vs)

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

***

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="cc5cf-118">Конфигурация и секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="cc5cf-118">Configuration and User Secrets</span></span>

<span data-ttu-id="cc5cf-119">При наличии проекта ASP.NET Core можно использовать `Name=<connection-string>` синтаксис для считывания строки подключения из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-119">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="cc5cf-120">Это хорошо работает со [средством диспетчера секретов](/aspnet/core/security/app-secrets#secret-manager) , чтобы пароль базы данных не оставался в своей базе кода.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-120">This works well with the [Secret Manager tool](/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=ConnectionStrings.Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="cc5cf-121">Имя поставщика</span><span class="sxs-lookup"><span data-stu-id="cc5cf-121">Provider name</span></span>

<span data-ttu-id="cc5cf-122">Вторым аргументом является имя поставщика.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-122">The second argument is the provider name.</span></span> <span data-ttu-id="cc5cf-123">Имя поставщика обычно совпадает с именем пакета NuGet поставщика.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-123">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="cc5cf-124">Указание таблиц</span><span class="sxs-lookup"><span data-stu-id="cc5cf-124">Specifying tables</span></span>

<span data-ttu-id="cc5cf-125">По умолчанию все таблицы в схеме базы данных реконструированы в типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-125">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="cc5cf-126">Можно ограничить, какие таблицы реконструировать реконструирование, указав схемы и таблицы.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-126">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="cc5cf-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc5cf-127">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

<span data-ttu-id="cc5cf-128">`--schema`Параметр может использоваться для включения каждой таблицы в схему, в то время как `--table` может использоваться для включения конкретных таблиц.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-128">The `--schema` option can be used to include every table within a schema, while `--table` can be used to include specific tables.</span></span>

<span data-ttu-id="cc5cf-129">Чтобы включить несколько таблиц, укажите параметр несколько раз:</span><span class="sxs-lookup"><span data-stu-id="cc5cf-129">To include multiple tables, specify the option multiple times:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

### <a name="visual-studio"></a>[<span data-ttu-id="cc5cf-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc5cf-130">Visual Studio</span></span>](#tab/vs)

<span data-ttu-id="cc5cf-131">`-Schemas`Параметр может использоваться для включения каждой таблицы в схему, в то время как `-Tables` может использоваться для включения конкретных таблиц.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-131">The `-Schemas` option can be used to include every table within a schema, while `-Tables` can be used to include specific tables.</span></span>

<span data-ttu-id="cc5cf-132">Чтобы включить несколько таблиц, используйте массив:</span><span class="sxs-lookup"><span data-stu-id="cc5cf-132">To include multiple tables, use an array:</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

***

## <a name="preserving-names"></a><span data-ttu-id="cc5cf-133">Сохранение имен</span><span class="sxs-lookup"><span data-stu-id="cc5cf-133">Preserving names</span></span>

<span data-ttu-id="cc5cf-134">Имена таблиц и столбцов устраняются, чтобы лучше соответствовать соглашениям об именовании .NET для типов и свойств по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-134">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="cc5cf-135">При указании параметра `-UseDatabaseNames` в PMC или `--use-database-names` в .NET Core CLI будет отключено это поведение по мере возможности.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-135">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the .NET Core CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="cc5cf-136">Недопустимые идентификаторы .NET по-прежнему будут исправлены, а синтезированные имена, такие как свойства навигации, по-прежнему будут соответствовать соглашениям об именовании .NET.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-136">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="cc5cf-137">API Fluent или заметки к данным</span><span class="sxs-lookup"><span data-stu-id="cc5cf-137">Fluent API or Data Annotations</span></span>

<span data-ttu-id="cc5cf-138">Типы сущностей настраиваются с помощью API Fluent по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-138">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="cc5cf-139">Укажите `-DataAnnotations` вместо них (PMC) или `--data-annotations` (.NET Core CLI) использование заметок к данным, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-139">Specify `-DataAnnotations` (PMC) or `--data-annotations` (.NET Core CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="cc5cf-140">Например, с помощью API Fluent будет сформирован шаблон:</span><span class="sxs-lookup"><span data-stu-id="cc5cf-140">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="cc5cf-141">При использовании заметок к данным будет сформировано следующее:</span><span class="sxs-lookup"><span data-stu-id="cc5cf-141">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="cc5cf-142">Имя DbContext</span><span class="sxs-lookup"><span data-stu-id="cc5cf-142">DbContext name</span></span>

<span data-ttu-id="cc5cf-143">Шаблонное имя класса DbContext по умолчанию будет именем базы данных с суффиксом *context* .</span><span class="sxs-lookup"><span data-stu-id="cc5cf-143">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="cc5cf-144">Чтобы указать другую, используйте `-Context` в PMC и `--context` в .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-144">To specify a different one, use `-Context` in PMC and `--context` in the .NET Core CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="cc5cf-145">Каталоги и пространства имен</span><span class="sxs-lookup"><span data-stu-id="cc5cf-145">Directories and namespaces</span></span>

<span data-ttu-id="cc5cf-146">Классы сущностей и класс DbContext обрабатываются в корневом каталоге проекта и используют пространство имен по умолчанию проекта.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-146">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="cc5cf-147">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc5cf-147">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

<span data-ttu-id="cc5cf-148">Можно указать каталог, в котором разрабатываются классы с помощью `--output-dir` , и `--context-dir` можно использовать для формирования шаблона класса DbContext в отдельный каталог из классов типов сущностей:</span><span class="sxs-lookup"><span data-stu-id="cc5cf-148">You can specify the directory where classes are scaffolded using `--output-dir`, and `--context-dir` can be used to scaffold the DbContext class into a separate directory from the entity type classes:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

<span data-ttu-id="cc5cf-149">По умолчанию пространством имен будет корневое пространство имен и имена всех подкаталогов в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-149">By default, the namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span> <span data-ttu-id="cc5cf-150">Однако из Ефкоре 5,0 можно переопределить пространство имен для всех выходных классов с помощью `--namespace` .</span><span class="sxs-lookup"><span data-stu-id="cc5cf-150">However, from EFCore 5.0 onwards, you can override the namespace for all output classes by using `--namespace`.</span></span> <span data-ttu-id="cc5cf-151">Также можно переопределить пространство имен только для класса DbContext, используя `--context-namespace` :</span><span class="sxs-lookup"><span data-stu-id="cc5cf-151">You can also override the namespace for just the DbContext class using `--context-namespace`:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --namespace Your.Namespace --context-namespace Your.DbContext.Namespace
```

### <a name="visual-studio"></a>[<span data-ttu-id="cc5cf-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc5cf-152">Visual Studio</span></span>](#tab/vs)

<span data-ttu-id="cc5cf-153">Можно указать каталог, в котором разрабатываются классы с помощью `-OutputDir` , и `-ContextDir` можно использовать для формирования шаблона класса DbContext в отдельный каталог из классов типов сущностей:</span><span class="sxs-lookup"><span data-stu-id="cc5cf-153">You can specify the directory where classes are scaffolded using `-OutputDir`, and `-ContextDir` can be used to scaffold the DbContext class into a separate directory from the entity type classes:</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

<span data-ttu-id="cc5cf-154">По умолчанию пространством имен будет корневое пространство имен и имена всех подкаталогов в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-154">By default, the namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span> <span data-ttu-id="cc5cf-155">Однако из Ефкоре 5,0 можно переопределить пространство имен для всех выходных классов с помощью `-Namespace` .</span><span class="sxs-lookup"><span data-stu-id="cc5cf-155">However, from EFCore 5.0 onwards, you can override the namespace for all output classes by using `-Namespace`.</span></span> <span data-ttu-id="cc5cf-156">Также можно переопределить пространство имен только для класса DbContext с помощью `-ContextNamespace` .</span><span class="sxs-lookup"><span data-stu-id="cc5cf-156">You can also override the namespace for just the DbContext class using `-ContextNamespace`.</span></span>

``` powershell
Scaffold-DbContext ... -Namespace Your.Namespace -ContextNamespace Your.DbContext.Namespace
```

***

## <a name="how-it-works"></a><span data-ttu-id="cc5cf-157">Принцип работы</span><span class="sxs-lookup"><span data-stu-id="cc5cf-157">How it works</span></span>

<span data-ttu-id="cc5cf-158">Реконструирование начинается с считывания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-158">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="cc5cf-159">Он считывает сведения о таблицах, столбцах, ограничениях и индексах.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-159">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="cc5cf-160">Затем он использует сведения о схеме для создания модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-160">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="cc5cf-161">Таблицы используются для создания типов сущностей. столбцы используются для создания свойств; и внешние ключи используются для создания связей.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-161">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="cc5cf-162">Наконец, модель используется для создания кода.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-162">Finally, the model is used to generate code.</span></span> <span data-ttu-id="cc5cf-163">Для повторного создания той же модели из приложения создаются шаблоны соответствующих классов, API Fluent и заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-163">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="limitations"></a><span data-ttu-id="cc5cf-164">Ограничения</span><span class="sxs-lookup"><span data-stu-id="cc5cf-164">Limitations</span></span>

* <span data-ttu-id="cc5cf-165">Не все сведения о модели можно представить с помощью схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-165">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="cc5cf-166">Например, сведения о [**иерархиях наследования**](xref:core/modeling/inheritance), [**принадлежащих типах**](xref:core/modeling/owned-entities)и [**разбиении таблиц**](xref:core/modeling/table-splitting) отсутствуют в схеме базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-166">For example, information about [**inheritance hierarchies**](xref:core/modeling/inheritance), [**owned types**](xref:core/modeling/owned-entities), and [**table splitting**](xref:core/modeling/table-splitting) are not present in the database schema.</span></span> <span data-ttu-id="cc5cf-167">По этой причине эти конструкции никогда не будут обрабатываться в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-167">Because of this, these constructs will never be reverse engineered.</span></span>
* <span data-ttu-id="cc5cf-168">Кроме того, **некоторые типы столбцов** могут не поддерживаться поставщиком EF Core.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-168">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="cc5cf-169">Эти столбцы не будут включаться в модель.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-169">These columns won't be included in the model.</span></span>
* <span data-ttu-id="cc5cf-170">В модели EF Core можно определить [**маркеры параллелизма**](xref:core/modeling/concurrency), чтобы запретить двум пользователям одновременно обновлять одну и ту же сущность.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-170">You can define [**concurrency tokens**](xref:core/modeling/concurrency), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="cc5cf-171">Некоторые базы данных имеют специальный тип для представления этого типа столбца (например, rowversion в SQL Server). в этом случае мы можем реконструировать эту информацию. Однако другие маркеры параллелизма не будут реконструированы.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-171">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>
* <span data-ttu-id="cc5cf-172">[Функция ссылочного типа C# 8 Nullable](/dotnet/csharp/tutorials/nullable-reference-types) в настоящее время не поддерживается в обратном проектировании: EF Core всегда создает код C#, который предполагает, что функция отключена.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-172">[The C# 8 nullable reference type feature](/dotnet/csharp/tutorials/nullable-reference-types) is currently unsupported in reverse engineering: EF Core always generates C# code that assumes the feature is disabled.</span></span> <span data-ttu-id="cc5cf-173">Например, столбцы с текстом, допускающими значение null, будут формироваться в виде свойства с типом `string` , а не `string?` с помощью API-интерфейса Fluent или заметок к данным, используемых для настройки того, является ли свойство обязательным.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-173">For example, nullable text columns will be scaffolded as a property with type `string` , not `string?`, with either the Fluent API or Data Annotations used to configure whether a property is required or not.</span></span> <span data-ttu-id="cc5cf-174">Вы можете изменить сформированный код и заменить их на метки допустимости значений NULL в C#.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-174">You can edit the scaffolded code and replace these with C# nullability annotations.</span></span> <span data-ttu-id="cc5cf-175">Поддержка формирования шаблонов для ссылочных типов, допускающих значения NULL, ведется по выпуску [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span><span class="sxs-lookup"><span data-stu-id="cc5cf-175">Scaffolding support for nullable reference types is tracked by issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="cc5cf-176">Настройка модели</span><span class="sxs-lookup"><span data-stu-id="cc5cf-176">Customizing the model</span></span>

<span data-ttu-id="cc5cf-177">Кодом, созданным EF Core, является ваш код.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-177">The code generated by EF Core is your code.</span></span> <span data-ttu-id="cc5cf-178">Вы можете изменить его.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-178">Feel free to change it.</span></span> <span data-ttu-id="cc5cf-179">Он будет создан повторно только в том случае, если реконструировать ту же модель снова.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-179">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="cc5cf-180">Шаблонный код представляет *одну* модель, которую можно использовать для доступа к базе данных, но, конечно, не *единственная* модель, которую можно использовать.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-180">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="cc5cf-181">Настройте классы типов сущностей и класс DbContext в соответствии с вашими потребностями.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-181">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="cc5cf-182">Например, можно переименовать типы и свойства, ввести иерархии наследования или разделить таблицу на несколько сущностей.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-182">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="cc5cf-183">Можно также удалить Неуникальные индексы, неиспользуемые последовательности и свойства навигации, необязательные скалярные свойства и имена ограничений из модели.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-183">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="cc5cf-184">Можно также добавить дополнительные конструкторы, методы, свойства и т. д.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-184">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="cc5cf-185">Использование другого разделяемого класса в отдельном файле.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-185">using another partial class in a separate file.</span></span> <span data-ttu-id="cc5cf-186">Этот подход работает даже в том случае, если планируется реконструировать модель снова.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-186">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="cc5cf-187">Обновление модели</span><span class="sxs-lookup"><span data-stu-id="cc5cf-187">Updating the model</span></span>

<span data-ttu-id="cc5cf-188">После внесения изменений в базу данных может потребоваться обновить модель EF Core, чтобы отразить эти изменения.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-188">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="cc5cf-189">Если изменения базы данных просты, возможно, проще всего вручную внести изменения в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-189">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="cc5cf-190">Например, переименование таблицы или столбца, удаление столбца или обновление типа столбца являются тривиальными изменениями для внесения в код.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-190">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="cc5cf-191">Однако более существенные изменения не так просто выполнять вручную.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-191">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="cc5cf-192">Один из распространенных рабочих процессов — реконструирование модели из базы данных с помощью `-Force` (PMC) или `--force` (CLI) для перезаписи существующей модели обновленной.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-192">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="cc5cf-193">Еще одна часто запрашиваемая функция — возможность обновления модели из базы данных с сохранением настроек, таких как переименование, иерархии типов и т. д. Чтобы отслеживать ход выполнения этой функции, используйте [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) выдача.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-193">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="cc5cf-194">При повторном конструировании модели из базы данных все изменения, внесенные в файлы, будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="cc5cf-194">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
