---
title: Заполнение данных — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414575"
---
# <a name="data-seeding"></a><span data-ttu-id="163b9-102">Присвоение начальных значений данных</span><span class="sxs-lookup"><span data-stu-id="163b9-102">Data Seeding</span></span>

<span data-ttu-id="163b9-103">Заполнение данных — это процесс заполнения базы данных начальным набором данных.</span><span class="sxs-lookup"><span data-stu-id="163b9-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="163b9-104">Существует несколько способов, которые можно выполнить в EF Core.</span><span class="sxs-lookup"><span data-stu-id="163b9-104">There are several ways this can be accomplished in EF Core:</span></span>

* <span data-ttu-id="163b9-105">Начальные данные модели</span><span class="sxs-lookup"><span data-stu-id="163b9-105">Model seed data</span></span>
* <span data-ttu-id="163b9-106">Настройка миграции вручную</span><span class="sxs-lookup"><span data-stu-id="163b9-106">Manual migration customization</span></span>
* <span data-ttu-id="163b9-107">Пользовательская логика инициализации</span><span class="sxs-lookup"><span data-stu-id="163b9-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="163b9-108">Начальные данные модели</span><span class="sxs-lookup"><span data-stu-id="163b9-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="163b9-109">Это новая возможность в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="163b9-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="163b9-110">В отличие от EF6, в EF Core данные заполнения могут быть связаны с типом сущности как часть конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="163b9-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="163b9-111">Затем EF Core [миграции](xref:core/managing-schemas/migrations/index) могут автоматически вычислять, какие операции вставки, обновления или удаления должны быть применены при обновлении базы данных до новой версии модели.</span><span class="sxs-lookup"><span data-stu-id="163b9-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="163b9-112">Миграция учитывает изменения модели только при определении того, какая операция должна быть выполнена, чтобы получить начальные данные в требуемое состояние.</span><span class="sxs-lookup"><span data-stu-id="163b9-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="163b9-113">Поэтому любые изменения данных, выполненные за пределами миграции, могут быть потеряны или вызывать ошибку.</span><span class="sxs-lookup"><span data-stu-id="163b9-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="163b9-114">В качестве примера вы настроите начальные данные для `Blog` в `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="163b9-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="163b9-115">Чтобы добавить сущности с отношением, необходимо указать значения внешнего ключа:</span><span class="sxs-lookup"><span data-stu-id="163b9-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="163b9-116">Если тип сущности имеет какие бы то ни было свойства в теневом состоянии, для предоставления значений можно использовать анонимный класс.</span><span class="sxs-lookup"><span data-stu-id="163b9-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="163b9-117">Собственные типы сущностей могут быть заполнены подобным образом:</span><span class="sxs-lookup"><span data-stu-id="163b9-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="163b9-118">См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) для получения дополнительных контекстов.</span><span class="sxs-lookup"><span data-stu-id="163b9-118">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="163b9-119">После добавления данных в модель для применения изменений следует использовать [миграции](xref:core/managing-schemas/migrations/index) .</span><span class="sxs-lookup"><span data-stu-id="163b9-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="163b9-120">Если необходимо применить миграции в рамках автоматизированного развертывания, можно [создать скрипт SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , который можно просмотреть перед выполнением.</span><span class="sxs-lookup"><span data-stu-id="163b9-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="163b9-121">Кроме того, можно использовать `context.Database.EnsureCreated()` для создания новой базы данных, содержащей начальные данные, например для тестовой базы данных или при использовании поставщика в памяти или любой другой базы данных, не являющейся реляционной.</span><span class="sxs-lookup"><span data-stu-id="163b9-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="163b9-122">Обратите внимание, что если база данных уже существует, `EnsureCreated()` не будет обновлять схему или начальные данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="163b9-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="163b9-123">Для реляционных баз данных не следует вызывать `EnsureCreated()`, если планируется использовать миграции.</span><span class="sxs-lookup"><span data-stu-id="163b9-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="163b9-124">Ограничения данных начального значения модели</span><span class="sxs-lookup"><span data-stu-id="163b9-124">Limitations of model seed data</span></span>

<span data-ttu-id="163b9-125">Этот тип данных начального значения управляется миграцией, а скрипт для обновления данных, которые уже находятся в базе данных, должен быть создан без подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="163b9-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="163b9-126">Это накладывает некоторые ограничения:</span><span class="sxs-lookup"><span data-stu-id="163b9-126">This imposes some restrictions:</span></span>

* <span data-ttu-id="163b9-127">Значение первичного ключа необходимо указать, даже если оно обычно создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="163b9-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="163b9-128">Он будет использоваться для обнаружения изменений данных между миграции.</span><span class="sxs-lookup"><span data-stu-id="163b9-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="163b9-129">Ранее заполненные данные будут удалены, если первичный ключ изменяется каким-либо образом.</span><span class="sxs-lookup"><span data-stu-id="163b9-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="163b9-130">Поэтому эта функция наиболее полезна для статических данных, которые не должны изменяться за пределами миграции и не зависят от каких-либо других элементов в базе данных, например ZIP-кодов.</span><span class="sxs-lookup"><span data-stu-id="163b9-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="163b9-131">Если сценарий включает в себя любое из следующих действий, рекомендуется использовать настраиваемую логику инициализации, описанную в последнем разделе.</span><span class="sxs-lookup"><span data-stu-id="163b9-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>

* <span data-ttu-id="163b9-132">Временные данные для тестирования</span><span class="sxs-lookup"><span data-stu-id="163b9-132">Temporary data for testing</span></span>
* <span data-ttu-id="163b9-133">Данные, зависящие от состояния базы данных</span><span class="sxs-lookup"><span data-stu-id="163b9-133">Data that depends on database state</span></span>
* <span data-ttu-id="163b9-134">Данные, которым требуются значения ключей, создаваемые базой данных, включая сущности, которые используют альтернативные ключи в качестве удостоверения.</span><span class="sxs-lookup"><span data-stu-id="163b9-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="163b9-135">Данные, для которых требуется пользовательское преобразование (которое не обрабатывается при [преобразовании значений](xref:core/modeling/value-conversions)), например, хэширование паролей</span><span class="sxs-lookup"><span data-stu-id="163b9-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="163b9-136">Данные, требующие вызовов к внешнему API, такие как ASP.NET Coreные роли удостоверения и создание пользователей</span><span class="sxs-lookup"><span data-stu-id="163b9-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="163b9-137">Настройка миграции вручную</span><span class="sxs-lookup"><span data-stu-id="163b9-137">Manual migration customization</span></span>

<span data-ttu-id="163b9-138">При добавлении миграции изменения данных, указанных в `HasData`, преобразуются в вызовы `InsertData()`, `UpdateData()`и `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="163b9-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="163b9-139">Одним из способов обойти некоторые ограничения `HasData` является ручное добавление этих вызовов или [пользовательских операций](xref:core/managing-schemas/migrations/operations) в процесс миграции.</span><span class="sxs-lookup"><span data-stu-id="163b9-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="163b9-140">Пользовательская логика инициализации</span><span class="sxs-lookup"><span data-stu-id="163b9-140">Custom initialization logic</span></span>

<span data-ttu-id="163b9-141">Простой и эффективный способ выполнения заполнения данных заключается в использовании [`DbContext.SaveChanges()`](xref:core/saving/index) до начала выполнения главной логики приложения.</span><span class="sxs-lookup"><span data-stu-id="163b9-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="163b9-142">Код заполнения не должен быть частью обычного выполнения приложения, так как это может привести к проблемам параллелизма при запуске нескольких экземпляров и потребовать от приложения разрешения на изменение схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="163b9-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="163b9-143">В зависимости от ограничений развертывания код инициализации можно выполнять различными способами:</span><span class="sxs-lookup"><span data-stu-id="163b9-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>

* <span data-ttu-id="163b9-144">Локальное выполнение приложения инициализации</span><span class="sxs-lookup"><span data-stu-id="163b9-144">Running the initialization app locally</span></span>
* <span data-ttu-id="163b9-145">Развертывание приложения инициализации с помощью основного приложения, вызов процедуры инициализации и отключение или удаление приложения инициализации.</span><span class="sxs-lookup"><span data-stu-id="163b9-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="163b9-146">Обычно это можно автоматизировать с помощью [профилей публикации](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="163b9-146">This can usually be automated by using [publish profiles](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
