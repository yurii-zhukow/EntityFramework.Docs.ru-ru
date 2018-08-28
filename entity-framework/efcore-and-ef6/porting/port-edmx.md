---
title: Перенос приложений из EF6 в EF Core — перенос модели на основе EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997415"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="c2fa4-102">Перенос модель на основе EDMX EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="c2fa4-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="c2fa4-103">EF Core не поддерживает формат файла EDMX для моделей.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="c2fa4-104">Перенос этих моделей, лучше создать новую модель на основе кода из базы данных для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="c2fa4-105">Установить пакеты EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="c2fa4-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="c2fa4-106">Установить `Microsoft.EntityFrameworkCore.Tools` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="c2fa4-107">Повторное создание модели</span><span class="sxs-lookup"><span data-stu-id="c2fa4-107">Regenerate the model</span></span>

<span data-ttu-id="c2fa4-108">Теперь можно использовать функциональные возможности реконструирования, чтобы создать модель, основанную на существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="c2fa4-109">Выполните следующую команду в консоли диспетчера пакетов (средства –> Диспетчер пакетов NuGet –> консоль диспетчера пакетов).</span><span class="sxs-lookup"><span data-stu-id="c2fa4-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="c2fa4-110">См. в разделе [консоль диспетчера пакетов (Visual Studio)](../../core/miscellaneous/cli/powershell.md) для параметры команд для формирования шаблонов подмножество таблиц и т.д.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="c2fa4-111">Например вот команду, чтобы сформировать модель из базы данных блогов на локальном экземпляре SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="c2fa4-112">Удаление модели EF6</span><span class="sxs-lookup"><span data-stu-id="c2fa4-112">Remove EF6 model</span></span>

<span data-ttu-id="c2fa4-113">Теперь вам нужно удалить модель EF6 из приложения.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="c2fa4-114">Это можно оставить EF6 установленный пакет NuGet (EntityFramework), как EF Core и EF6 можно использовать side-by-side в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="c2fa4-115">Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, то при удалении пакета поможет выдают ошибки компиляции на фрагменты кода, требуют внимания.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="c2fa4-116">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="c2fa4-116">Update your code</span></span>

<span data-ttu-id="c2fa4-117">На этом этапе это вопрос адресации ошибки компиляции и проверка кода ли изменения в работе между EF6 и EF Core повлияет на вас.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="c2fa4-118">Протестировать порт</span><span class="sxs-lookup"><span data-stu-id="c2fa4-118">Test the port</span></span>

<span data-ttu-id="c2fa4-119">Компилирует приложение, не означает, что он успешно перенесена в EF Core.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="c2fa4-120">Необходимо будет проверить все области приложения, чтобы убедиться, что ни одно из изменений поведение негативно повлиять на приложения.</span><span class="sxs-lookup"><span data-stu-id="c2fa4-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="c2fa4-121">См. в разделе [Приступая к работе с EF Core в ASP.NET Core с существующей базой данных](xref:core/get-started/aspnetcore/existing-db) для ссылки на дополнительные о том, как работать с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="c2fa4-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
