---
title: Поставщик выполняющейся в памяти базы данных — EF Core
description: Сведения о поставщике базы данных InMemory Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/providers/in-memory/index
ms.openlocfilehash: 1af75088ae892e3b428caf6bdb31dd2b750a05fe
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430174"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="23bca-103">Поставщик выполняющейся в памяти базы данных EF Core</span><span class="sxs-lookup"><span data-stu-id="23bca-103">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="23bca-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="23bca-104">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="23bca-105">База данных в памяти может быть удобна для тестирования, хотя поставщик SQLite в памяти может лучше подходить для тестирования реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="23bca-105">The in-memory database can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="23bca-106">База данных в памяти предназначена только для тестирования.</span><span class="sxs-lookup"><span data-stu-id="23bca-106">The in-memory database is designed for testing only.</span></span> <span data-ttu-id="23bca-107">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/dotnet/efcore).</span><span class="sxs-lookup"><span data-stu-id="23bca-107">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/dotnet/efcore).</span></span>

## <a name="install"></a><span data-ttu-id="23bca-108">Установка</span><span class="sxs-lookup"><span data-stu-id="23bca-108">Install</span></span>

<span data-ttu-id="23bca-109">Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="23bca-109">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="23bca-110">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="23bca-110">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="23bca-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="23bca-111">Visual Studio</span></span>](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="23bca-112">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="23bca-112">Get Started</span></span>

<span data-ttu-id="23bca-113">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="23bca-113">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="23bca-114">Тестирование с использованием выполнения в памяти</span><span class="sxs-lookup"><span data-stu-id="23bca-114">Testing with InMemory</span></span>](xref:core/testing/in-memory)
* [<span data-ttu-id="23bca-115">Тесты примера приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="23bca-115">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="23bca-116">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="23bca-116">Supported Database Engines</span></span>

<span data-ttu-id="23bca-117">База данных во внутрипроцессной памяти, предназначенная исключительно для тестирования.</span><span class="sxs-lookup"><span data-stu-id="23bca-117">In-process memory database, designed for testing purposes only.</span></span>
