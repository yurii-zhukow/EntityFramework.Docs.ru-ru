---
title: Тестирование с помощью SQLite-EF Core
description: Использование SQLite для тестирования приложения Entity Framework Core
author: ajcvickers
ms.date: 04/24/2020
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: b580ed58cb9466c8eac32f71951734f4bd565733
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617687"
---
# <a name="using-sqlite-to-test-an-ef-core-application"></a><span data-ttu-id="99858-103">Использование SQLite для тестирования приложения EF Core</span><span class="sxs-lookup"><span data-stu-id="99858-103">Using SQLite to test an EF Core application</span></span>

> [!WARNING]
> <span data-ttu-id="99858-104">Использование SQLite может быть эффективным способом тестирования EF Core приложения.</span><span class="sxs-lookup"><span data-stu-id="99858-104">Using SQLite can be an effective way to test an EF Core application.</span></span>
> <span data-ttu-id="99858-105">Однако могут возникнуть проблемы, когда SQLite ведет себя иначе из других систем баз данных.</span><span class="sxs-lookup"><span data-stu-id="99858-105">However, problems can arise where SQLite behaves differently from other database systems.</span></span> <span data-ttu-id="99858-106">Описание проблем и компромиссов см. в разделе [тестирование кода, в котором используется EF Core](xref:core/miscellaneous/testing/index) .</span><span class="sxs-lookup"><span data-stu-id="99858-106">See [Testing code that uses EF Core](xref:core/miscellaneous/testing/index) for a discussion of the issues and trade-offs.</span></span>  

<span data-ttu-id="99858-107">В этом документе используются основные понятия, представленные в [примере, демонстрирующие тестирование приложений, использующих EF Core](xref:core/miscellaneous/testing/testing-sample).</span><span class="sxs-lookup"><span data-stu-id="99858-107">This document builds uses on the concepts introduced in [Sample showing how to test applications that use EF Core](xref:core/miscellaneous/testing/testing-sample).</span></span>
<span data-ttu-id="99858-108">Приведенные здесь примеры кода взяты из этого примера.</span><span class="sxs-lookup"><span data-stu-id="99858-108">The code examples shown here come from this sample.</span></span>

## <a name="using-sqlite-in-memory-databases"></a><span data-ttu-id="99858-109">Использование SQLite в памяти баз данных</span><span class="sxs-lookup"><span data-stu-id="99858-109">Using SQLite in-memory databases</span></span>

<span data-ttu-id="99858-110">Как правило, SQLite создает базы данных в виде простых файлов и обращается к файлу внутри процесса с приложением.</span><span class="sxs-lookup"><span data-stu-id="99858-110">Normally, SQLite creates databases as simple files and accesses the file in-process with your application.</span></span>
<span data-ttu-id="99858-111">Это очень быстро, особенно при использовании быстрого [SSD](https://en.wikipedia.org/wiki/Solid-state_drive).</span><span class="sxs-lookup"><span data-stu-id="99858-111">This is very fast, especially when using a fast [SSD](https://en.wikipedia.org/wiki/Solid-state_drive).</span></span> 

<span data-ttu-id="99858-112">SQLite также может использовать базы данных, созданные исключительно в памяти.</span><span class="sxs-lookup"><span data-stu-id="99858-112">SQLite can also use databases created purely in-memory.</span></span>
<span data-ttu-id="99858-113">Это легко можно использовать с EF Core, если вы понимаете время существования базы данных в памяти:</span><span class="sxs-lookup"><span data-stu-id="99858-113">This is easy to use with EF Core as long as you understand the in-memory database lifetime:</span></span>
* <span data-ttu-id="99858-114">База данных создается при открытии подключения к ней</span><span class="sxs-lookup"><span data-stu-id="99858-114">The database is created when the connection to it is opened</span></span>
* <span data-ttu-id="99858-115">База данных удаляется при закрытии соединения с ней</span><span class="sxs-lookup"><span data-stu-id="99858-115">The database is deleted when the connection to it is closed</span></span>

<span data-ttu-id="99858-116">EF Core будет использовать уже открытое соединение при его наличии и никогда не попытается закрыть его.</span><span class="sxs-lookup"><span data-stu-id="99858-116">EF Core will use an already open connection when given one, and will never attempt to close it.</span></span>
<span data-ttu-id="99858-117">Поэтому для использования EF Core с базой данных SQLite в памяти необходимо открыть подключение перед передачей его в EF.</span><span class="sxs-lookup"><span data-stu-id="99858-117">So the key to using EF Core with an in-memory SQLite database is to open the connection before passing it to EF.</span></span>  

<span data-ttu-id="99858-118">В [примере](xref:core/miscellaneous/testing/testing-sample) это достигается с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="99858-118">The [sample](xref:core/miscellaneous/testing/testing-sample) achieves this with the following code:</span></span>

[!code-csharp[SqliteInMemory](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/SqliteInMemoryItemsControllerTest.cs?name=SqliteInMemory)]

<span data-ttu-id="99858-119">Примечание:</span><span class="sxs-lookup"><span data-stu-id="99858-119">Notice:</span></span>
* <span data-ttu-id="99858-120">`CreateInMemoryDatabase`Метод создает базу данных SQLite в памяти и открывает к ней подключение.</span><span class="sxs-lookup"><span data-stu-id="99858-120">The `CreateInMemoryDatabase` method creates a SQLite in-memory database and opens the connection to it.</span></span>
* <span data-ttu-id="99858-121">Созданный объект `DbConnection` извлекается из `ContextOptions` и сохраняется.</span><span class="sxs-lookup"><span data-stu-id="99858-121">The created `DbConnection` is extracted from the `ContextOptions` and saved.</span></span>
* <span data-ttu-id="99858-122">Соединение удаляется, когда тест удаляется, чтобы ресурсы не были утечки.</span><span class="sxs-lookup"><span data-stu-id="99858-122">The connection is disposed when the test is disposed so that resources are not leaked.</span></span> 

> [!NOTE]
> <span data-ttu-id="99858-123">[Проблема #16103](https://github.com/dotnet/efcore/issues/16103) заключается в том, чтобы упростить управление подключениями.</span><span class="sxs-lookup"><span data-stu-id="99858-123">[Issue #16103](https://github.com/dotnet/efcore/issues/16103) is tracking ways to make this connection management easier.</span></span> 
