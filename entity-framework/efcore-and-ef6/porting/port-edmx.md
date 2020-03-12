---
title: Перенос из EF6 в EF Core — перенос модели на основе EDMX — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413531"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="6383c-102">Перенос модели на основе EDMX EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="6383c-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="6383c-103">EF Core не поддерживает формат файлов EDMX для моделей.</span><span class="sxs-lookup"><span data-stu-id="6383c-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="6383c-104">Оптимальным вариантом для переноса этих моделей является создание новой модели на основе кода из базы данных для приложения.</span><span class="sxs-lookup"><span data-stu-id="6383c-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="6383c-105">Установка пакетов NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="6383c-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="6383c-106">Установите пакет NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="6383c-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="6383c-107">Повторное создание модели</span><span class="sxs-lookup"><span data-stu-id="6383c-107">Regenerate the model</span></span>

<span data-ttu-id="6383c-108">Теперь можно использовать функцию реконструирования, чтобы создать модель на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="6383c-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="6383c-109">Выполните ниже команду в консоли диспетчера пакетов ("Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов").</span><span class="sxs-lookup"><span data-stu-id="6383c-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="6383c-110">В разделе [Консоль диспетчера пакетов (Visual Studio)](../../core/miscellaneous/cli/powershell.md) приведены параметры команды для формирования шаблонов для подмножества таблиц и т. п.</span><span class="sxs-lookup"><span data-stu-id="6383c-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="6383c-111">Например, ниже приведена команда для формирования шаблона модели из базы данных Blogging в экземпляре LocalDB SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6383c-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="6383c-112">Удаление модели EF6</span><span class="sxs-lookup"><span data-stu-id="6383c-112">Remove EF6 model</span></span>

<span data-ttu-id="6383c-113">Теперь можно удалить модель EF6 из приложения.</span><span class="sxs-lookup"><span data-stu-id="6383c-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="6383c-114">Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="6383c-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="6383c-115">Однако если вы не планируете использовать EF6 в каких-либо областях приложения, то удаление пакета поможет вывести ошибки компиляции для фрагментов кода, требующих внимания.</span><span class="sxs-lookup"><span data-stu-id="6383c-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="6383c-116">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="6383c-116">Update your code</span></span>

<span data-ttu-id="6383c-117">На этом этапе осуществляется устранение ошибок компиляции и проверка кода, чтобы узнать, затронут ли вас изменения поведения между EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="6383c-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="6383c-118">Тестирование переноса</span><span class="sxs-lookup"><span data-stu-id="6383c-118">Test the port</span></span>

<span data-ttu-id="6383c-119">Даже если приложение компилируется, это не означает, что оно успешно перенесено в EF Core.</span><span class="sxs-lookup"><span data-stu-id="6383c-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="6383c-120">Необходимо протестировать все области приложения, чтобы предотвратить отсутствие неблагоприятного воздействия каких-либо изменений поведения.</span><span class="sxs-lookup"><span data-stu-id="6383c-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
