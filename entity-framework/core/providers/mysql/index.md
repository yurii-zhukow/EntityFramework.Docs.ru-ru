---
title: "Поставщик базы данных MySQL — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="1655c-102">Поставщик базы данных MySQL EF Core</span><span class="sxs-lookup"><span data-stu-id="1655c-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="1655c-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с MySQL.</span><span class="sxs-lookup"><span data-stu-id="1655c-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="1655c-104">Его разработка ведется в рамках [проекта MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="1655c-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="1655c-105">Этот поставщик находится на этапе предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="1655c-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="1655c-106">В рамках проекта Entity Framework Core работы над ним не ведутся.</span><span class="sxs-lookup"><span data-stu-id="1655c-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="1655c-107">Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="1655c-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="1655c-108">Установка</span><span class="sxs-lookup"><span data-stu-id="1655c-108">Install</span></span>

<span data-ttu-id="1655c-109">Установите [пакет NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="1655c-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="1655c-110">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="1655c-110">Get Started</span></span>

<span data-ttu-id="1655c-111">См. статью [Начало работы с поставщиком MySQL EF Core и Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="1655c-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="1655c-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="1655c-112">Supported Database Engines</span></span>

* <span data-ttu-id="1655c-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="1655c-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="1655c-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="1655c-114">Supported Platforms</span></span>

* <span data-ttu-id="1655c-115">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="1655c-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="1655c-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="1655c-116">.NET Core</span></span>

<span data-ttu-id="1655c-117">Обязательно ознакомьтесь с документацией MySQL по совместимости версий [здесь](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) и [здесь](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="1655c-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
