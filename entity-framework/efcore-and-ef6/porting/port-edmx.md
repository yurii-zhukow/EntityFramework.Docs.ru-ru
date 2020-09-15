---
title: Перенос из EF6 в EF Core — перенос модели на основе EDMX — EF
description: Конкретные сведения о переносе приложения модели на основе Entity Framework 6 EDMX в Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 7bd832f459ae3893e6a90e8483c95a41ca13f9ab
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90070007"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="490bb-103">Перенос модели на основе EDMX EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="490bb-103">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="490bb-104">EF Core не поддерживает формат файлов EDMX для моделей.</span><span class="sxs-lookup"><span data-stu-id="490bb-104">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="490bb-105">Оптимальным вариантом для переноса этих моделей является создание новой модели на основе кода из базы данных для приложения.</span><span class="sxs-lookup"><span data-stu-id="490bb-105">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="490bb-106">Установка пакетов NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="490bb-106">Install EF Core NuGet packages</span></span>

<span data-ttu-id="490bb-107">Установите пакет NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="490bb-107">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="490bb-108">Повторное создание модели</span><span class="sxs-lookup"><span data-stu-id="490bb-108">Regenerate the model</span></span>

<span data-ttu-id="490bb-109">Теперь можно использовать функцию реконструирования, чтобы создать модель на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="490bb-109">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="490bb-110">Выполните ниже команду в консоли диспетчера пакетов ("Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов").</span><span class="sxs-lookup"><span data-stu-id="490bb-110">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="490bb-111">В разделе [Консоль диспетчера пакетов (Visual Studio)](xref:core/miscellaneous/cli/powershell) приведены параметры команды для формирования шаблонов для подмножества таблиц и т. п.</span><span class="sxs-lookup"><span data-stu-id="490bb-111">See [Package Manager Console (Visual Studio)](xref:core/miscellaneous/cli/powershell) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="490bb-112">Например, ниже приведена команда для формирования шаблона модели из базы данных Blogging в экземпляре LocalDB SQL Server.</span><span class="sxs-lookup"><span data-stu-id="490bb-112">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="490bb-113">Удаление модели EF6</span><span class="sxs-lookup"><span data-stu-id="490bb-113">Remove EF6 model</span></span>

<span data-ttu-id="490bb-114">Теперь можно удалить модель EF6 из приложения.</span><span class="sxs-lookup"><span data-stu-id="490bb-114">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="490bb-115">Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="490bb-115">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="490bb-116">Однако если вы не планируете использовать EF6 в каких-либо областях приложения, то удаление пакета поможет вывести ошибки компиляции для фрагментов кода, требующих внимания.</span><span class="sxs-lookup"><span data-stu-id="490bb-116">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="490bb-117">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="490bb-117">Update your code</span></span>

<span data-ttu-id="490bb-118">На этом этапе осуществляется устранение ошибок компиляции и проверка кода, чтобы узнать, затронут ли вас изменения поведения между EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="490bb-118">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="490bb-119">Тестирование переноса</span><span class="sxs-lookup"><span data-stu-id="490bb-119">Test the port</span></span>

<span data-ttu-id="490bb-120">Даже если приложение компилируется, это не означает, что оно успешно перенесено в EF Core.</span><span class="sxs-lookup"><span data-stu-id="490bb-120">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="490bb-121">Необходимо протестировать все области приложения, чтобы предотвратить отсутствие неблагоприятного воздействия каких-либо изменений поведения.</span><span class="sxs-lookup"><span data-stu-id="490bb-121">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
