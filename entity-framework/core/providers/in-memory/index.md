---
title: Поставщик выполняющейся в памяти базы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502127"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="3df06-102">Поставщик выполняющейся в памяти базы данных EF Core</span><span class="sxs-lookup"><span data-stu-id="3df06-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="3df06-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="3df06-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="3df06-104">Это может быть полезно для тестирования несмотря на то, что поставщик SQLite в режиме выполнения в памяти может в большей степени подходить для тестирования реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="3df06-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="3df06-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="3df06-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="3df06-106">Установка</span><span class="sxs-lookup"><span data-stu-id="3df06-106">Install</span></span>

<span data-ttu-id="3df06-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="3df06-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="3df06-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="3df06-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="3df06-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3df06-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="3df06-110">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="3df06-110">Get Started</span></span>

<span data-ttu-id="3df06-111">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="3df06-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="3df06-112">Тестирование с использованием выполнения в памяти</span><span class="sxs-lookup"><span data-stu-id="3df06-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="3df06-113">Тесты примера приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="3df06-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="3df06-114">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="3df06-114">Supported Database Engines</span></span>

<span data-ttu-id="3df06-115">База данных во внутрипроцессной памяти (предназначена исключительно для тестирования)</span><span class="sxs-lookup"><span data-stu-id="3df06-115">In-process memory database (designed for testing purposes only)</span></span>
