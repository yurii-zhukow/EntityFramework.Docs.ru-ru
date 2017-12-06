---
title: "Маркеры параллелизма - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="0733c-102">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="0733c-102">Concurrency Tokens</span></span>

<span data-ttu-id="0733c-103">Если свойство настроено как маркер параллелизма затем EF будет проверять, что ни один другой пользователь изменил это значение в базе данных при сохранении изменений в этой записи.</span><span class="sxs-lookup"><span data-stu-id="0733c-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="0733c-104">EF используется модель оптимистичного параллелизма, что означает, что будет предполагается, что значение не изменилось и попробуйте сохранить данные, но создавать, если он обнаруживает, что значение было изменено.</span><span class="sxs-lookup"><span data-stu-id="0733c-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="0733c-105">Например необходимо настроить `LastName` на `Person` быть маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0733c-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="0733c-106">Это означает, что если один пользователь пытается сохранить некоторые изменения в `Person`, но другой пользователь изменил `LastName` , то будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="0733c-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="0733c-107">Это может быть нежелательно, чтобы ваше приложение может запрашивать у пользователя, чтобы убедиться, что эта запись по-прежнему представляет же реальный пользователь перед сохранением изменений.</span><span class="sxs-lookup"><span data-stu-id="0733c-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="0733c-108">Этой странице приведены инструкции по настройке маркеры параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0733c-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="0733c-109">В разделе [обработки параллелизма](../saving/concurrency.md) примеры использования оптимистичного параллелизма в приложении.</span><span class="sxs-lookup"><span data-stu-id="0733c-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="0733c-110">Как работают маркеры параллелизма в EF</span><span class="sxs-lookup"><span data-stu-id="0733c-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="0733c-111">Хранилища данных можно принудительно задать маркеры параллелизма, проверьте, что все записи в обновляемой или удаляемой по-прежнему имеет то же значение для маркер параллелизма, назначенный при контекст начальной загрузки данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="0733c-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="0733c-112">Например, реляционных баз данных для этого можно маркер параллелизма, в том числе `WHERE` предложение любого `UPDATE` или `DELETE` команды и выполняется проверка номера строк, которые были затронуты.</span><span class="sxs-lookup"><span data-stu-id="0733c-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="0733c-113">Если маркер параллелизма по-прежнему соответствует одной строке обновляются.</span><span class="sxs-lookup"><span data-stu-id="0733c-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="0733c-114">При изменении значения в базе данных, строки не обновляются.</span><span class="sxs-lookup"><span data-stu-id="0733c-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="0733c-115">Соглашения</span><span class="sxs-lookup"><span data-stu-id="0733c-115">Conventions</span></span>

<span data-ttu-id="0733c-116">По соглашению свойства никогда не настроен как маркеры параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0733c-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0733c-117">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="0733c-117">Data Annotations</span></span>

<span data-ttu-id="0733c-118">Заметки данных можно использовать для настройки свойства в качестве маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0733c-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="0733c-119">Fluent API</span><span class="sxs-lookup"><span data-stu-id="0733c-119">Fluent API</span></span>

<span data-ttu-id="0733c-120">Fluent API можно использовать для настройки свойства в качестве маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0733c-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="0733c-121">Отметка времени или строки версии</span><span class="sxs-lookup"><span data-stu-id="0733c-121">Timestamp/row version</span></span>

<span data-ttu-id="0733c-122">Отметки времени — это свойство, когда новое значение создается в базе данных каждый раз при вставке или обновлении строки.</span><span class="sxs-lookup"><span data-stu-id="0733c-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="0733c-123">Свойство также рассматриваются как маркер параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0733c-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="0733c-124">Это гарантирует, что если кто-то еще изменил строка, которую вы пытаетесь обновить, так как запросы для данных, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="0733c-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="0733c-125">Как это можно сделать, зависит от используемого поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="0733c-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="0733c-126">Отметка времени обычно используются для SQL Server на *byte []* свойство, которое будет настроить как *ROWVERSION* столбца в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0733c-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="0733c-127">Соглашения</span><span class="sxs-lookup"><span data-stu-id="0733c-127">Conventions</span></span>

<span data-ttu-id="0733c-128">По соглашению свойства никогда не настроен как отметки времени.</span><span class="sxs-lookup"><span data-stu-id="0733c-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="0733c-129">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="0733c-129">Data Annotations</span></span>

<span data-ttu-id="0733c-130">Заметок к данным можно использовать для настройки свойства в качестве метки времени.</span><span class="sxs-lookup"><span data-stu-id="0733c-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="0733c-131">Fluent API</span><span class="sxs-lookup"><span data-stu-id="0733c-131">Fluent API</span></span>

<span data-ttu-id="0733c-132">Fluent API можно использовать для настройки свойства в качестве метки времени.</span><span class="sxs-lookup"><span data-stu-id="0733c-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
