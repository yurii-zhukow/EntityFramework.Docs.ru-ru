---
title: Поставщик выполняющейся в памяти базы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149227"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="49dd3-102">Поставщик выполняющейся в памяти базы данных EF Core</span><span class="sxs-lookup"><span data-stu-id="49dd3-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="49dd3-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="49dd3-104">Это может быть полезно для тестирования несмотря на то, что поставщик SQLite в режиме выполнения в памяти может в большей степени подходить для тестирования реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="49dd3-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="49dd3-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="49dd3-106">Установка</span><span class="sxs-lookup"><span data-stu-id="49dd3-106">Install</span></span>

<span data-ttu-id="49dd3-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="49dd3-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="49dd3-108">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="49dd3-108">Get Started</span></span>

<span data-ttu-id="49dd3-109">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="49dd3-110">Тестирование с использованием выполнения в памяти</span><span class="sxs-lookup"><span data-stu-id="49dd3-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="49dd3-111">Тесты примера приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="49dd3-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="49dd3-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="49dd3-112">Supported Database Engines</span></span>

* <span data-ttu-id="49dd3-113">Встроенная выполняющаяся в памяти база (предназначена исключительно для тестирования)</span><span class="sxs-lookup"><span data-stu-id="49dd3-113">Built-in in-memory database (designed for testing purposes only)</span></span>
