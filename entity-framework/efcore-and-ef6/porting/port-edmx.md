---
title: Перенос приложений из EF6 в основные EF – перенос модель на основе EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812694"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="2f3ff-102">Перенос EF6 модели на основе EDMX EF ядра</span><span class="sxs-lookup"><span data-stu-id="2f3ff-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="2f3ff-103">EF Core не поддерживает формат файла EDMX для модели.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="2f3ff-104">Перенос этих моделей, лучше создать новую модель на основе кода из базы данных для приложения.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="2f3ff-105">Установить пакеты EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="2f3ff-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="2f3ff-106">Установить `Microsoft.EntityFrameworkCore.Tools` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="2f3ff-107">Повторное создание модели</span><span class="sxs-lookup"><span data-stu-id="2f3ff-107">Regenerate the model</span></span>

<span data-ttu-id="2f3ff-108">Чтобы создать модель, основанную на существующей базы данных теперь можно использовать функцию реконструирования.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="2f3ff-109">Выполните следующую команду в консоли диспетчера пакетов (средства –> диспетчера пакетов NuGet –> консоль диспетчера пакетов).</span><span class="sxs-lookup"><span data-stu-id="2f3ff-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="2f3ff-110">В разделе [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) для параметров команды для формировать подмножество таблиц и т. д.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="2f3ff-111">Например вот команду, чтобы сформировать модель на основе базы данных ведения блогов на экземпляре SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="2f3ff-112">Удаление модели EF6</span><span class="sxs-lookup"><span data-stu-id="2f3ff-112">Remove EF6 model</span></span>

<span data-ttu-id="2f3ff-113">Теперь EF6 модели будут удалены из приложения.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="2f3ff-114">Это нормально оставить пакет EF6 NuGet (EntityFramework) установлен, как ядро EF и EF6 может быть используется side-by-side в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="2f3ff-115">Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, затем при удалении пакета поможет выдают ошибки компиляции на элементы кода, требующие внимания.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="2f3ff-116">Обновите ваш код</span><span class="sxs-lookup"><span data-stu-id="2f3ff-116">Update your code</span></span>

<span data-ttu-id="2f3ff-117">На этом этапе это вопрос адресации ошибки компиляции и проверка кода на предмет изменения в работе между EF6 и EF Core повлияет.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="2f3ff-118">Проверить порт</span><span class="sxs-lookup"><span data-stu-id="2f3ff-118">Test the port</span></span>

<span data-ttu-id="2f3ff-119">Только потому, что компилирует приложения, не означает, что он успешно перенесена на EF Core.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="2f3ff-120">Необходимо для проверки всех областях приложения, чтобы убедиться, что никакие изменения в поведении отрицательно повлиять приложения.</span><span class="sxs-lookup"><span data-stu-id="2f3ff-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="2f3ff-121">В разделе [Приступая к работе с EF Core в ASP.NET Core с существующей базы данных](xref:core/get-started/aspnetcore/existing-db) для ссылки на дополнительные по работе с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="2f3ff-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
