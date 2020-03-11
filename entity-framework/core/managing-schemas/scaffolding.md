---
title: Реконструирование — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414239"
---
# <a name="reverse-engineering"></a><span data-ttu-id="10de5-102">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="10de5-102">Reverse Engineering</span></span>

<span data-ttu-id="10de5-103">Реконструирование — это процесс формирования шаблонов классов типов сущностей и класса DbContext на основе схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="10de5-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="10de5-104">Его можно выполнить с помощью команды `Scaffold-DbContext` средств консоли диспетчера пакетов EF Core (PMC) или команды `dotnet ef dbcontext scaffold` средств интерфейса командной строки (CLI) .NET.</span><span class="sxs-lookup"><span data-stu-id="10de5-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="10de5-105">Установка</span><span class="sxs-lookup"><span data-stu-id="10de5-105">Installing</span></span>

<span data-ttu-id="10de5-106">Перед реконструированием необходимо установить либо [средства PMC](xref:core/miscellaneous/cli/powershell) (только Visual Studio), либо [средства CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="10de5-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="10de5-107">Дополнительные сведения см. в ссылках.</span><span class="sxs-lookup"><span data-stu-id="10de5-107">See links for details.</span></span>

<span data-ttu-id="10de5-108">Вам также потребуется установить соответствующий [поставщик базы данных](xref:core/providers/index) для схемы базы данных, которую необходимо реконструировать.</span><span class="sxs-lookup"><span data-stu-id="10de5-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="10de5-109">Строка подключения</span><span class="sxs-lookup"><span data-stu-id="10de5-109">Connection string</span></span>

<span data-ttu-id="10de5-110">Первым аргументом команды является строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="10de5-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="10de5-111">Эти средства будут использовать эту строку соединения для чтения схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="10de5-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="10de5-112">Способ заключения и экранирования строки подключения зависит от того, какая оболочка используется для выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="10de5-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="10de5-113">Дополнительные сведения см. в документации по оболочке.</span><span class="sxs-lookup"><span data-stu-id="10de5-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="10de5-114">Например, PowerShell требует экранирования `$` символа, но не `\`.</span><span class="sxs-lookup"><span data-stu-id="10de5-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="10de5-115">Конфигурация и секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="10de5-115">Configuration and User Secrets</span></span>

<span data-ttu-id="10de5-116">При наличии проекта ASP.NET Core можно использовать синтаксис `Name=<connection-string>` для чтения строки подключения из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="10de5-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="10de5-117">Это хорошо работает со [средством диспетчера секретов](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) , чтобы пароль базы данных не оставался в своей базе кода.</span><span class="sxs-lookup"><span data-stu-id="10de5-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="10de5-118">Название поставщика</span><span class="sxs-lookup"><span data-stu-id="10de5-118">Provider name</span></span>

<span data-ttu-id="10de5-119">Вторым аргументом является имя поставщика.</span><span class="sxs-lookup"><span data-stu-id="10de5-119">The second argument is the provider name.</span></span> <span data-ttu-id="10de5-120">Имя поставщика обычно совпадает с именем пакета NuGet поставщика.</span><span class="sxs-lookup"><span data-stu-id="10de5-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="10de5-121">Указание таблиц</span><span class="sxs-lookup"><span data-stu-id="10de5-121">Specifying tables</span></span>

<span data-ttu-id="10de5-122">По умолчанию все таблицы в схеме базы данных реконструированы в типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="10de5-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="10de5-123">Можно ограничить, какие таблицы реконструировать реконструирование, указав схемы и таблицы.</span><span class="sxs-lookup"><span data-stu-id="10de5-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="10de5-124">Параметр `-Schemas` в PMC и параметр `--schema` в интерфейсе командной строки можно использовать для включения каждой таблицы в схему.</span><span class="sxs-lookup"><span data-stu-id="10de5-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="10de5-125">для включения конкретных таблиц можно использовать `-Tables` (PMC) и `--table` (CLI).</span><span class="sxs-lookup"><span data-stu-id="10de5-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="10de5-126">Чтобы включить несколько таблиц в PMC, используйте массив.</span><span class="sxs-lookup"><span data-stu-id="10de5-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="10de5-127">Чтобы включить в интерфейс командной строки несколько таблиц, укажите параметр несколько раз.</span><span class="sxs-lookup"><span data-stu-id="10de5-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="10de5-128">Сохранение имен</span><span class="sxs-lookup"><span data-stu-id="10de5-128">Preserving names</span></span>

<span data-ttu-id="10de5-129">Имена таблиц и столбцов устраняются, чтобы лучше соответствовать соглашениям об именовании .NET для типов и свойств по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="10de5-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="10de5-130">Если указать `-UseDatabaseNames` коммутатора в PMC или параметре `--use-database-names` в интерфейсе командной строки, это повлечет за собой сохранение имен исходных баз данных с максимально возможной точностью.</span><span class="sxs-lookup"><span data-stu-id="10de5-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="10de5-131">Недопустимые идентификаторы .NET по-прежнему будут исправлены, а синтезированные имена, такие как свойства навигации, по-прежнему будут соответствовать соглашениям об именовании .NET.</span><span class="sxs-lookup"><span data-stu-id="10de5-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="10de5-132">API Fluent или заметки к данным</span><span class="sxs-lookup"><span data-stu-id="10de5-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="10de5-133">Типы сущностей настраиваются с помощью API Fluent по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="10de5-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="10de5-134">Укажите `-DataAnnotations` (PMC) или `--data-annotations` (CLI), чтобы вместо этого использовать заметки к данным, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="10de5-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="10de5-135">Например, с помощью API Fluent будет сформирован шаблон:</span><span class="sxs-lookup"><span data-stu-id="10de5-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="10de5-136">При использовании заметок к данным будет сформировано следующее:</span><span class="sxs-lookup"><span data-stu-id="10de5-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="10de5-137">Имя DbContext</span><span class="sxs-lookup"><span data-stu-id="10de5-137">DbContext name</span></span>

<span data-ttu-id="10de5-138">Шаблонное имя класса DbContext по умолчанию будет именем базы данных с суффиксом *context* .</span><span class="sxs-lookup"><span data-stu-id="10de5-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="10de5-139">Чтобы указать другую, используйте `-Context` в PMC и `--context` в интерфейсе командной строки.</span><span class="sxs-lookup"><span data-stu-id="10de5-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="10de5-140">Каталоги и пространства имен</span><span class="sxs-lookup"><span data-stu-id="10de5-140">Directories and namespaces</span></span>

<span data-ttu-id="10de5-141">Классы сущностей и класс DbContext обрабатываются в корневом каталоге проекта и используют пространство имен по умолчанию проекта.</span><span class="sxs-lookup"><span data-stu-id="10de5-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="10de5-142">Можно указать каталог, в котором разрабатываются классы с помощью `-OutputDir` (PMC) или `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="10de5-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="10de5-143">Пространством имен будет корневое пространство имен и имена всех подкаталогов в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="10de5-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="10de5-144">Можно также использовать `-ContextDir` (PMC) и `--context-dir` (CLI) для формирования шаблона класса DbContext в отдельном каталоге из классов типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="10de5-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="10de5-145">Принцип работы</span><span class="sxs-lookup"><span data-stu-id="10de5-145">How it works</span></span>

<span data-ttu-id="10de5-146">Реконструирование начинается с считывания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="10de5-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="10de5-147">Он считывает сведения о таблицах, столбцах, ограничениях и индексах.</span><span class="sxs-lookup"><span data-stu-id="10de5-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="10de5-148">Затем он использует сведения о схеме для создания модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="10de5-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="10de5-149">Таблицы используются для создания типов сущностей. столбцы используются для создания свойств; и внешние ключи используются для создания связей.</span><span class="sxs-lookup"><span data-stu-id="10de5-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="10de5-150">Наконец, модель используется для создания кода.</span><span class="sxs-lookup"><span data-stu-id="10de5-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="10de5-151">Для повторного создания той же модели из приложения создаются шаблоны соответствующих классов, API Fluent и заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="10de5-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="limitations"></a><span data-ttu-id="10de5-152">Ограничения</span><span class="sxs-lookup"><span data-stu-id="10de5-152">Limitations</span></span>

* <span data-ttu-id="10de5-153">Не все сведения о модели можно представить с помощью схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="10de5-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="10de5-154">Например, сведения о [**иерархиях наследования**](../modeling/inheritance.md), [**принадлежащих типах**](../modeling/owned-entities.md)и [**разбиении таблиц**](../modeling/table-splitting.md) отсутствуют в схеме базы данных.</span><span class="sxs-lookup"><span data-stu-id="10de5-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="10de5-155">По этой причине эти конструкции никогда не будут обрабатываться в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="10de5-155">Because of this, these constructs will never be reverse engineered.</span></span>
* <span data-ttu-id="10de5-156">Кроме того, **некоторые типы столбцов** могут не поддерживаться поставщиком EF Core.</span><span class="sxs-lookup"><span data-stu-id="10de5-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="10de5-157">Эти столбцы не будут включаться в модель.</span><span class="sxs-lookup"><span data-stu-id="10de5-157">These columns won't be included in the model.</span></span>
* <span data-ttu-id="10de5-158">В модели EF Core можно определить [**маркеры параллелизма**](../modeling/concurrency.md), чтобы запретить двум пользователям одновременно обновлять одну и ту же сущность.</span><span class="sxs-lookup"><span data-stu-id="10de5-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="10de5-159">Некоторые базы данных имеют специальный тип для представления этого типа столбца (например, rowversion в SQL Server). в этом случае мы можем реконструировать эту информацию. Однако другие маркеры параллелизма не будут реконструированы.</span><span class="sxs-lookup"><span data-stu-id="10de5-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>
* <span data-ttu-id="10de5-160">[Функция C# ссылочного типа 8 Nullable](/dotnet/csharp/tutorials/nullable-reference-types) в настоящее время не поддерживается в обратном проектировании: EF Core C# всегда создает код, который предполагает, что функция отключена.</span><span class="sxs-lookup"><span data-stu-id="10de5-160">[The C# 8 nullable reference type feature](/dotnet/csharp/tutorials/nullable-reference-types) is currently unsupported in reverse engineering: EF Core always generates C# code that assumes the feature is disabled.</span></span> <span data-ttu-id="10de5-161">Например, текстовые столбцы, допускающие значение null, будут формироваться в виде свойства с типом `string`, а не `string?`с помощью интерфейса API Fluent или заметок к данным, используемых для настройки того, является ли свойство обязательным.</span><span class="sxs-lookup"><span data-stu-id="10de5-161">For example, nullable text columns will be scaffolded as a property with type `string` , not `string?`, with either the Fluent API or Data Annotations used to configure whether a property is required or not.</span></span> <span data-ttu-id="10de5-162">Можно изменить сформированный код и заменить его заметками C# , имеющими значения NULL.</span><span class="sxs-lookup"><span data-stu-id="10de5-162">You can edit the scaffolded code and replace these with C# nullability annotations.</span></span> <span data-ttu-id="10de5-163">Поддержка формирования шаблонов для ссылочных типов, допускающих значения NULL, ведется по выпуску [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span><span class="sxs-lookup"><span data-stu-id="10de5-163">Scaffolding support for nullable reference types is tracked by issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="10de5-164">Настройка модели</span><span class="sxs-lookup"><span data-stu-id="10de5-164">Customizing the model</span></span>

<span data-ttu-id="10de5-165">Кодом, созданным EF Core, является ваш код.</span><span class="sxs-lookup"><span data-stu-id="10de5-165">The code generated by EF Core is your code.</span></span> <span data-ttu-id="10de5-166">Вы можете изменить его.</span><span class="sxs-lookup"><span data-stu-id="10de5-166">Feel free to change it.</span></span> <span data-ttu-id="10de5-167">Он будет создан повторно только в том случае, если реконструировать ту же модель снова.</span><span class="sxs-lookup"><span data-stu-id="10de5-167">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="10de5-168">Шаблонный код представляет *одну* модель, которую можно использовать для доступа к базе данных, но, конечно, не *единственная* модель, которую можно использовать.</span><span class="sxs-lookup"><span data-stu-id="10de5-168">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="10de5-169">Настройте классы типов сущностей и класс DbContext в соответствии с вашими потребностями.</span><span class="sxs-lookup"><span data-stu-id="10de5-169">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="10de5-170">Например, можно переименовать типы и свойства, ввести иерархии наследования или разделить таблицу на несколько сущностей.</span><span class="sxs-lookup"><span data-stu-id="10de5-170">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="10de5-171">Можно также удалить Неуникальные индексы, неиспользуемые последовательности и свойства навигации, необязательные скалярные свойства и имена ограничений из модели.</span><span class="sxs-lookup"><span data-stu-id="10de5-171">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="10de5-172">Можно также добавить дополнительные конструкторы, методы, свойства и т. д.</span><span class="sxs-lookup"><span data-stu-id="10de5-172">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="10de5-173">Использование другого разделяемого класса в отдельном файле.</span><span class="sxs-lookup"><span data-stu-id="10de5-173">using another partial class in a separate file.</span></span> <span data-ttu-id="10de5-174">Этот подход работает даже в том случае, если планируется реконструировать модель снова.</span><span class="sxs-lookup"><span data-stu-id="10de5-174">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="10de5-175">Обновление модели</span><span class="sxs-lookup"><span data-stu-id="10de5-175">Updating the model</span></span>

<span data-ttu-id="10de5-176">После внесения изменений в базу данных может потребоваться обновить модель EF Core, чтобы отразить эти изменения.</span><span class="sxs-lookup"><span data-stu-id="10de5-176">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="10de5-177">Если изменения базы данных просты, возможно, проще всего вручную внести изменения в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="10de5-177">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="10de5-178">Например, переименование таблицы или столбца, удаление столбца или обновление типа столбца являются тривиальными изменениями для внесения в код.</span><span class="sxs-lookup"><span data-stu-id="10de5-178">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="10de5-179">Однако более существенные изменения не так просто выполнять вручную.</span><span class="sxs-lookup"><span data-stu-id="10de5-179">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="10de5-180">Один из распространенных рабочих процессов — реконструирование модели из базы данных с помощью `-Force` (PMC) или `--force` (CLI) для перезаписи существующей модели обновленной.</span><span class="sxs-lookup"><span data-stu-id="10de5-180">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="10de5-181">Еще одна часто запрашиваемая функция — возможность обновления модели из базы данных с сохранением настроек, таких как переименование, иерархии типов и т. д. Чтобы отслеживать ход выполнения этой функции, используйте [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) выдача.</span><span class="sxs-lookup"><span data-stu-id="10de5-181">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="10de5-182">При повторном конструировании модели из базы данных все изменения, внесенные в файлы, будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="10de5-182">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
