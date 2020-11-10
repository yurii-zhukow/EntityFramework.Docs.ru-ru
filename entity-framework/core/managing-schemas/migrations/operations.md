---
title: Операции с настраиваемыми миграциями — EF Core
description: Управление пользовательскими и необработанными миграциями SQL для управления схемой базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 10/27/2020
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 2abde4d5eac977a746863dcfd77bc85a34e2166c
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429836"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="ff7fa-103">Операции пользовательской миграции</span><span class="sxs-lookup"><span data-stu-id="ff7fa-103">Custom Migrations Operations</span></span>

<span data-ttu-id="ff7fa-104">API Мигратионбуилдер позволяет выполнять различные виды операций во время миграции, но это далеко не все.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-104">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="ff7fa-105">Однако API также является расширяемым, позволяя определять собственные операции.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-105">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="ff7fa-106">Существует два способа расширения API: с помощью `Sql()` метода или путем определения пользовательских `MigrationOperation` объектов.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-106">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="ff7fa-107">Чтобы проиллюстрировать это, давайте взглянем на реализацию операции, которая создает пользователя базы данных, используя каждый из подходов.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-107">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="ff7fa-108">В наших переносах мы хотим включить написание следующего кода:</span><span class="sxs-lookup"><span data-stu-id="ff7fa-108">In our migrations, we want to enable writing the following code:</span></span>

```csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="ff7fa-109">Использование Мигратионбуилдер. SQL ()</span><span class="sxs-lookup"><span data-stu-id="ff7fa-109">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="ff7fa-110">Самый простой способ реализовать пользовательскую операцию — определить метод расширения, который вызывает `MigrationBuilder.Sql()` .</span><span class="sxs-lookup"><span data-stu-id="ff7fa-110">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="ff7fa-111">Ниже приведен пример, в котором создается соответствующий Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-111">Here is an example that generates the appropriate Transact-SQL.</span></span>

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperationSql.cs#snippet_CustomOperationSql)]

> [!TIP]
> <span data-ttu-id="ff7fa-112">Используйте `EXEC` функцию, если инструкция должна быть первой или единственной в пакете SQL.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-112">Use the `EXEC` function when a statement must be the first or only one in a SQL batch.</span></span> <span data-ttu-id="ff7fa-113">Также может потребоваться обойти ошибки синтаксического анализатора в сценариях миграции идемпотентными, которые могут возникать, если в таблице не существует столбцов, на которые имеются ссылки.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-113">It might also be needed to work around parser errors in idempotent migration scripts that can occur when referenced columns don't currently exist on a table.</span></span>

<span data-ttu-id="ff7fa-114">Если для миграции требуется поддержка нескольких поставщиков баз данных, можно использовать `MigrationBuilder.ActiveProvider` свойство.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-114">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="ff7fa-115">Ниже приведен пример, поддерживающий как Microsoft SQL Server, так и PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-115">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperationMultiSql.cs#snippet_CustomOperationMultiSql)]

<span data-ttu-id="ff7fa-116">Этот подход работает только в том случае, если вы знакомы с каждым поставщиком, к которому будет применена пользовательская операция.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-116">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="ff7fa-117">Использование Мигратионоператион</span><span class="sxs-lookup"><span data-stu-id="ff7fa-117">Using a MigrationOperation</span></span>

<span data-ttu-id="ff7fa-118">Чтобы отделить пользовательскую операцию от SQL, можно определить собственную `MigrationOperation` для ее представления.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-118">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="ff7fa-119">Затем операция передается поставщику, чтобы он мог определить соответствующий SQL.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-119">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_CreateUserOperation)]

<span data-ttu-id="ff7fa-120">При таком подходе методу расширения просто нужно добавить одну из этих операций в `MigrationBuilder.Operations` .</span><span class="sxs-lookup"><span data-stu-id="ff7fa-120">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_MigrationBuilderExtension)]

<span data-ttu-id="ff7fa-121">Этот подход требует, чтобы каждый поставщик знал, как создать SQL для этой операции в своей `IMigrationsSqlGenerator` службе.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-121">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="ff7fa-122">Ниже приведен пример переопределения генератора SQL Server для управления новой операцией.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-122">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_MigrationsSqlGenerator)]

<span data-ttu-id="ff7fa-123">Замените службу генератора SQL по умолчанию новой.</span><span class="sxs-lookup"><span data-stu-id="ff7fa-123">Replace the default migrations sql generator service with the updated one.</span></span>

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_OnConfiguring)]
