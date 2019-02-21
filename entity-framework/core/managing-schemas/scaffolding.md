---
title: Реконструирование — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459189"
---
# <a name="reverse-engineering"></a><span data-ttu-id="083c2-102">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="083c2-102">Reverse Engineering</span></span>

<span data-ttu-id="083c2-103">Реконструирование — это процесс формирования шаблонов классы типов сущностей и класс DbContext, основанный на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="083c2-104">Он может быть выполнена с использованием `Scaffold-DbContext` команды консоли диспетчера пакетов (PMC) Core EF средств или `dotnet ef dbcontext scaffold` команды средства интерфейса командной строки (CLI) .NET.</span><span class="sxs-lookup"><span data-stu-id="083c2-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="083c2-105">Установка</span><span class="sxs-lookup"><span data-stu-id="083c2-105">Installing</span></span>

<span data-ttu-id="083c2-106">Для реконструирования, потребуется установить [инструменты PMC](xref:core/miscellaneous/cli/powershell) (только для Visual Studio) или [средств интерфейса командной строки](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="083c2-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="083c2-107">См. сведения о ресурсах.</span><span class="sxs-lookup"><span data-stu-id="083c2-107">See links for details.</span></span>

<span data-ttu-id="083c2-108">Кроме того, потребуется установить соответствующий [поставщик базы данных](xref:core/providers/index) для реконструирования схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="083c2-109">Строка подключения</span><span class="sxs-lookup"><span data-stu-id="083c2-109">Connection string</span></span>

<span data-ttu-id="083c2-110">Первый аргумент для команды является строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="083c2-111">Средства будет использовать эту строку подключения для чтения схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="083c2-112">Как заключения в кавычки и escape-строку подключения зависит от того какой оболочки используется для выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="083c2-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="083c2-113">К документации в оболочке конкретные сведения.</span><span class="sxs-lookup"><span data-stu-id="083c2-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="083c2-114">Например, PowerShell, которые необходимо экранировать `$` символов, но не `\`.</span><span class="sxs-lookup"><span data-stu-id="083c2-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="083c2-115">Конфигурация и секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="083c2-115">Configuration and User Secrets</span></span>

<span data-ttu-id="083c2-116">Если у вас есть проект ASP.NET Core, можно использовать `Name=<connection-string>` синтаксис для чтения строку подключения из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="083c2-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="083c2-117">Это хорошо работает с [средство Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) чтобы хранить пароль базы данных отдельно от базы кода.</span><span class="sxs-lookup"><span data-stu-id="083c2-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="083c2-118">Имя поставщика</span><span class="sxs-lookup"><span data-stu-id="083c2-118">Provider name</span></span>

<span data-ttu-id="083c2-119">Вторым аргументом является имя поставщика.</span><span class="sxs-lookup"><span data-stu-id="083c2-119">The second argument is the provider name.</span></span> <span data-ttu-id="083c2-120">Имя поставщика обычно совпадает имя пакета NuGet поставщика.</span><span class="sxs-lookup"><span data-stu-id="083c2-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="083c2-121">Определение таблиц</span><span class="sxs-lookup"><span data-stu-id="083c2-121">Specifying tables</span></span>

<span data-ttu-id="083c2-122">Все таблицы в схеме базы данных являются обратного разработанная в виде типов сущностей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="083c2-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="083c2-123">Можно ограничить, какие таблицы обратного спроектировали путем указания схемы и таблицы.</span><span class="sxs-lookup"><span data-stu-id="083c2-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="083c2-124">`-Schemas` Параметр в PMC и `--schema` параметр в интерфейсе командной строки можно использовать для включения каждой таблицы в схеме.</span><span class="sxs-lookup"><span data-stu-id="083c2-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="083c2-125">`-Tables` (PMC) и `--table` (CLI) можно использовать для включения определенных таблиц.</span><span class="sxs-lookup"><span data-stu-id="083c2-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="083c2-126">Чтобы включить несколько таблиц в PMC, используйте массив.</span><span class="sxs-lookup"><span data-stu-id="083c2-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="083c2-127">Чтобы включить несколько таблиц в интерфейсе командной строки, укажите параметр несколько раз.</span><span class="sxs-lookup"><span data-stu-id="083c2-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="083c2-128">Сохранение имен</span><span class="sxs-lookup"><span data-stu-id="083c2-128">Preserving names</span></span>

<span data-ttu-id="083c2-129">Имена таблиц и столбцов фиксируются лучше соответствовать соглашениям об именовании .NET для типов и свойств по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="083c2-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="083c2-130">Указание `-UseDatabaseNames` перейдите в консоли диспетчера пакетов или `--use-database-names` параметр в интерфейсе командной строки будет отключить это поведение, сохраняя исходные имена базы данных как можно больше.</span><span class="sxs-lookup"><span data-stu-id="083c2-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="083c2-131">Недопустимые идентификаторы .NET будет по-прежнему исправлена и Синтезированная имена, такие как свойства навигации по-прежнему будет соответствовать соглашениям об именовании .NET для.</span><span class="sxs-lookup"><span data-stu-id="083c2-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="083c2-132">Fluent API или заметки к данным</span><span class="sxs-lookup"><span data-stu-id="083c2-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="083c2-133">Типы сущностей настраиваются с помощью Fluent API по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="083c2-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="083c2-134">Укажите `-DataAnnotations` (PMC) или `--data-annotations` (CLI), чтобы вместо этого используйте аннотации данных, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="083c2-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="083c2-135">Например с помощью Fluent API будет формировать на это:</span><span class="sxs-lookup"><span data-stu-id="083c2-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="083c2-136">При использовании заметок к данным будет формировать это:</span><span class="sxs-lookup"><span data-stu-id="083c2-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="083c2-137">Имя DbContext</span><span class="sxs-lookup"><span data-stu-id="083c2-137">DbContext name</span></span>

<span data-ttu-id="083c2-138">Шаблонный DbContext класс будет называться имя базы данных, заканчивающиеся *контекст* по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="083c2-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="083c2-139">Чтобы указать другую, используйте `-Context` в PMC и `--context` в интерфейсе командной строки.</span><span class="sxs-lookup"><span data-stu-id="083c2-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="083c2-140">Каталоги и пространства имен</span><span class="sxs-lookup"><span data-stu-id="083c2-140">Directories and namespaces</span></span>

<span data-ttu-id="083c2-141">Классы сущностей и класс DbContext сформированные в корневой каталог проекта и использовать пространство имен по умолчанию проекта.</span><span class="sxs-lookup"><span data-stu-id="083c2-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="083c2-142">Можно указать каталог, где это классы сформированные с помощью `-OutputDir` (PMC) или `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="083c2-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="083c2-143">Пространство имен будет корневое пространство имен, а также имена всех подкаталогах, в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="083c2-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="083c2-144">Можно также использовать `-ContextDir` (PMC) и `--context-dir` (CLI) для формирования шаблонов класса DbContext в отдельный каталог из классы типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="083c2-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="083c2-145">Принцип работы</span><span class="sxs-lookup"><span data-stu-id="083c2-145">How it works</span></span>

<span data-ttu-id="083c2-146">Реконструирование начинает работу со считывания схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="083c2-147">Он считывает сведения о таблицах, столбцы, ограничения и индексы.</span><span class="sxs-lookup"><span data-stu-id="083c2-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="083c2-148">Затем он использует сведения о схеме для создания модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="083c2-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="083c2-149">Таблицы используются для создания типов сущностей; столбцы используются для создания свойства; и внешние ключи используются для создания связей.</span><span class="sxs-lookup"><span data-stu-id="083c2-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="083c2-150">Наконец модель используется для создания кода.</span><span class="sxs-lookup"><span data-stu-id="083c2-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="083c2-151">Соответствующие сущности типа классам, Fluent API и данные заметки сформированные для повторного создания одной и той же модели из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="083c2-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="083c2-152">Что не работает</span><span class="sxs-lookup"><span data-stu-id="083c2-152">What doesn't work</span></span>

<span data-ttu-id="083c2-153">Не все о модели могут быть представлены с помощью схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="083c2-154">Например, сведения о **иерархий наследования**, **принадлежащие типы**, и **таблицы разделение** не существуют в схеме базы данных.</span><span class="sxs-lookup"><span data-stu-id="083c2-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="083c2-155">По этой причине эти конструкции будет никогда не быть реконструированию.</span><span class="sxs-lookup"><span data-stu-id="083c2-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="083c2-156">Кроме того **некоторые типы столбцов** могут не поддерживаться поставщика EF Core.</span><span class="sxs-lookup"><span data-stu-id="083c2-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="083c2-157">Эти столбцы не будут включены в модель.</span><span class="sxs-lookup"><span data-stu-id="083c2-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="083c2-158">EF Core требуется иметь ключ каждого типа сущностей.</span><span class="sxs-lookup"><span data-stu-id="083c2-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="083c2-159">Таблицы, не должны указать первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="083c2-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="083c2-160">**Таблицы без первичного ключа** являются в настоящее время не реконструирования.</span><span class="sxs-lookup"><span data-stu-id="083c2-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="083c2-161">Вы можете определить **маркеры параллелизма** в модель EF Core для запрещения два обновления той же сущности, в то же время.</span><span class="sxs-lookup"><span data-stu-id="083c2-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="083c2-162">Некоторые базы данных имеют специальный тип для представления этого типа столбцов (например, rowversion в SQL Server) в этом случае можно проектировать эти данные; Тем не менее другие маркеры параллелизма будет не быть реконструированию.</span><span class="sxs-lookup"><span data-stu-id="083c2-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="083c2-163">Настройка модели</span><span class="sxs-lookup"><span data-stu-id="083c2-163">Customizing the model</span></span>

<span data-ttu-id="083c2-164">Код, сгенерированный EF Core является кодом.</span><span class="sxs-lookup"><span data-stu-id="083c2-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="083c2-165">Вы можете изменить его.</span><span class="sxs-lookup"><span data-stu-id="083c2-165">Feel free to change it.</span></span> <span data-ttu-id="083c2-166">Он будет повторно, если вы снова обратного проектирования той же модели.</span><span class="sxs-lookup"><span data-stu-id="083c2-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="083c2-167">Представляет сформированный код *один* модели, который может использоваться для доступа к базе данных, но определенно не является *только* модели, который может использоваться.</span><span class="sxs-lookup"><span data-stu-id="083c2-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="083c2-168">Настройте классы типов сущностей и класс DbContext в соответствии с потребностями.</span><span class="sxs-lookup"><span data-stu-id="083c2-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="083c2-169">Например вы можете переименовать типы и свойства, вводят иерархии наследования или разбиение таблицы на несколько сущностей в.</span><span class="sxs-lookup"><span data-stu-id="083c2-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="083c2-170">Вы также можно удалить из модели неуникальных индексов, неиспользуемые последовательностей и свойства навигации, необязательно скалярных свойств и имена ограничений.</span><span class="sxs-lookup"><span data-stu-id="083c2-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="083c2-171">Можно также добавить дополнительные конструкторы, методы, свойства, и т.д.</span><span class="sxs-lookup"><span data-stu-id="083c2-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="083c2-172">с помощью другой разделяемый класс в отдельном файле.</span><span class="sxs-lookup"><span data-stu-id="083c2-172">using another partial class in a separate file.</span></span> <span data-ttu-id="083c2-173">Этот подход работает даже в том случае, если вы намереваетесь Реконструировать модель еще раз.</span><span class="sxs-lookup"><span data-stu-id="083c2-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="083c2-174">Обновление модели</span><span class="sxs-lookup"><span data-stu-id="083c2-174">Updating the model</span></span>

<span data-ttu-id="083c2-175">После внесения изменений в базу данных, может потребоваться обновить модель EF Core для отражения этих изменений.</span><span class="sxs-lookup"><span data-stu-id="083c2-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="083c2-176">В случае простого изменения базы данных может быть гораздо проще просто вручную внесите изменения в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="083c2-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="083c2-177">Например переименование таблицы или столбца, удаление столбца или обновление столбца типа являются обычные изменения вносить в код.</span><span class="sxs-lookup"><span data-stu-id="083c2-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="083c2-178">Более значительные изменения, но не как легко создать вручную.</span><span class="sxs-lookup"><span data-stu-id="083c2-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="083c2-179">Один общий рабочий процесс — обратный реконструирование модели из базы данных, снова используя `-Force` (PMC) или `--force` (CLI), чтобы перезаписать существующую модель, используя обновленный.</span><span class="sxs-lookup"><span data-stu-id="083c2-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="083c2-180">Другой часто запрашиваемых функцией является возможность обновить модель из базы данных, сохраняя настройки, такие как переименование, иерархии типов и т. д. Использовать проблема [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) для отслеживания хода выполнения этой функции.</span><span class="sxs-lookup"><span data-stu-id="083c2-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="083c2-181">Если вы обратного проектирования модели из базы данных будут потеряны все изменения, внесенные в файлы.</span><span class="sxs-lookup"><span data-stu-id="083c2-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
