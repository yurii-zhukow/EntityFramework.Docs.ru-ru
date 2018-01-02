---
title: "Поставщик выполняющейся в памяти базы данных — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="0d2f2-102">Поставщик выполняющейся в памяти базы данных EF Core</span><span class="sxs-lookup"><span data-stu-id="0d2f2-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="0d2f2-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="0d2f2-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="0d2f2-104">Это удобно при тестировании кода, использующего Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0d2f2-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="0d2f2-105">Его разработка ведется в рамках [проекта GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="0d2f2-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="0d2f2-106">Установка</span><span class="sxs-lookup"><span data-stu-id="0d2f2-106">Install</span></span>

<span data-ttu-id="0d2f2-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="0d2f2-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="0d2f2-108">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="0d2f2-108">Get Started</span></span>

<span data-ttu-id="0d2f2-109">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="0d2f2-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="0d2f2-110">Тестирование с использованием выполнения в памяти</span><span class="sxs-lookup"><span data-stu-id="0d2f2-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="0d2f2-111">Тесты примера приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="0d2f2-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="0d2f2-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="0d2f2-112">Supported Database Engines</span></span>

* <span data-ttu-id="0d2f2-113">Встроенная выполняющаяся в памяти база (предназначена исключительно для тестирования)</span><span class="sxs-lookup"><span data-stu-id="0d2f2-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0d2f2-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="0d2f2-114">Supported Platforms</span></span>

* <span data-ttu-id="0d2f2-115">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="0d2f2-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="0d2f2-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d2f2-116">.NET Core</span></span>

* <span data-ttu-id="0d2f2-117">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="0d2f2-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="0d2f2-118">Универсальная платформа Windows </span><span class="sxs-lookup"><span data-stu-id="0d2f2-118">Universal Windows Platform</span></span>
