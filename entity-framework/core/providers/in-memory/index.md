---
title: Поставщик выполняющейся в памяти базы данных — EF Core
description: Сведения о поставщике базы данных InMemory Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/providers/in-memory/index
ms.openlocfilehash: 1857ebbfa0eded1572220825a5b0d75961bcf3dd
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064015"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="bbb01-103">Поставщик выполняющейся в памяти базы данных EF Core</span><span class="sxs-lookup"><span data-stu-id="bbb01-103">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="bbb01-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="bbb01-104">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="bbb01-105">База данных в памяти может быть удобна для тестирования, хотя поставщик SQLite в памяти может лучше подходить для тестирования реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="bbb01-105">The in-memory database can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="bbb01-106">База данных в памяти предназначена только для тестирования.</span><span class="sxs-lookup"><span data-stu-id="bbb01-106">The in-memory database is designed for testing only.</span></span> <span data-ttu-id="bbb01-107">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="bbb01-107">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="bbb01-108">Установка</span><span class="sxs-lookup"><span data-stu-id="bbb01-108">Install</span></span>

<span data-ttu-id="bbb01-109">Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="bbb01-109">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="bbb01-110">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bbb01-110">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="bbb01-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbb01-111">Visual Studio</span></span>](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="bbb01-112">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="bbb01-112">Get Started</span></span>

<span data-ttu-id="bbb01-113">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="bbb01-113">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="bbb01-114">Тестирование с использованием выполнения в памяти</span><span class="sxs-lookup"><span data-stu-id="bbb01-114">Testing with InMemory</span></span>](xref:core/miscellaneous/testing/in-memory)
* [<span data-ttu-id="bbb01-115">Тесты примера приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="bbb01-115">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="bbb01-116">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="bbb01-116">Supported Database Engines</span></span>

<span data-ttu-id="bbb01-117">База данных во внутрипроцессной памяти, предназначенная исключительно для тестирования.</span><span class="sxs-lookup"><span data-stu-id="bbb01-117">In-process memory database, designed for testing purposes only.</span></span>
