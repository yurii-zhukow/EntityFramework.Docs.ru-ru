---
title: Поставщик выполняющейся в памяти базы данных — EF Core
description: Сведения о поставщике базы данных InMemory Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
uid: core/providers/in-memory/index
ms.openlocfilehash: 9d0a5d14cdb047b80788fbe4d9d34deccdbd4ce1
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071359"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="72055-103">Поставщик выполняющейся в памяти базы данных EF Core</span><span class="sxs-lookup"><span data-stu-id="72055-103">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="72055-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="72055-104">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="72055-105">Это может быть полезно для тестирования несмотря на то, что поставщик SQLite в режиме выполнения в памяти может в большей степени подходить для тестирования реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="72055-105">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="72055-106">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="72055-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="72055-107">Установка</span><span class="sxs-lookup"><span data-stu-id="72055-107">Install</span></span>

<span data-ttu-id="72055-108">Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="72055-108">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="72055-109">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="72055-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="72055-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72055-110">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="72055-111">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="72055-111">Get Started</span></span>

<span data-ttu-id="72055-112">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="72055-112">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="72055-113">Тестирование с использованием выполнения в памяти</span><span class="sxs-lookup"><span data-stu-id="72055-113">Testing with InMemory</span></span>](xref:core/miscellaneous/testing/in-memory)
* [<span data-ttu-id="72055-114">Тесты примера приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="72055-114">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="72055-115">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="72055-115">Supported Database Engines</span></span>

<span data-ttu-id="72055-116">База данных во внутрипроцессной памяти (предназначена исключительно для тестирования)</span><span class="sxs-lookup"><span data-stu-id="72055-116">In-process memory database (designed for testing purposes only)</span></span>
