---
title: Поддерживаемые реализации .NET — EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413069"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="5f097-102">Реализации .NET, поддерживаемые EF Core</span><span class="sxs-lookup"><span data-stu-id="5f097-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="5f097-103">Мы стремимся обеспечить поддержку EF Core во всех современных реализациях .NET и продолжаем работу над этим.</span><span class="sxs-lookup"><span data-stu-id="5f097-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="5f097-104">Поддержка EF Core в .NET Core подтверждена результатами автоматизированного тестирования и нормальной работой многих приложений, тогда как с использованием этого решения с Mono, Xamarin и UWP возникают некоторые проблемы.</span><span class="sxs-lookup"><span data-stu-id="5f097-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="5f097-105">Обзор</span><span class="sxs-lookup"><span data-stu-id="5f097-105">Overview</span></span>

<span data-ttu-id="5f097-106">В следующей таблице приводятся рекомендации для каждой реализации .NET:</span><span class="sxs-lookup"><span data-stu-id="5f097-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="5f097-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="5f097-107">EF Core</span></span>                       | <span data-ttu-id="5f097-108">2.1 и 3.1</span><span class="sxs-lookup"><span data-stu-id="5f097-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="5f097-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="5f097-109">.NET Standard</span></span>                 | <span data-ttu-id="5f097-110">2.0</span><span class="sxs-lookup"><span data-stu-id="5f097-110">2.0</span></span>         |
| <span data-ttu-id="5f097-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f097-111">.NET Core</span></span>                     | <span data-ttu-id="5f097-112">2.0</span><span class="sxs-lookup"><span data-stu-id="5f097-112">2.0</span></span>         |
| <span data-ttu-id="5f097-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="5f097-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="5f097-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="5f097-114">4.7.2</span></span>       |
| <span data-ttu-id="5f097-115">Mono</span><span class="sxs-lookup"><span data-stu-id="5f097-115">Mono</span></span>                          | <span data-ttu-id="5f097-116">5,4</span><span class="sxs-lookup"><span data-stu-id="5f097-116">5.4</span></span>         |
| <span data-ttu-id="5f097-117">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="5f097-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="5f097-118">10.14</span><span class="sxs-lookup"><span data-stu-id="5f097-118">10.14</span></span>       |
| <span data-ttu-id="5f097-119">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="5f097-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="5f097-120">8.0</span><span class="sxs-lookup"><span data-stu-id="5f097-120">8.0</span></span>         |
| <span data-ttu-id="5f097-121">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="5f097-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="5f097-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="5f097-122">10.0.16299</span></span>  |
| <span data-ttu-id="5f097-123">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="5f097-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="5f097-124">2018.1</span><span class="sxs-lookup"><span data-stu-id="5f097-124">2018.1</span></span>      |

<span data-ttu-id="5f097-125"><sup>(1)</sup> См. раздел [.NET Framework](#net-framework) далее.</span><span class="sxs-lookup"><span data-stu-id="5f097-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="5f097-126"><sup>(2)</sup> При использовании с Xamarin возникают некоторые проблемы и ограничения, которые могут препятствовать правильной работе некоторых приложений, разработанных с использованием EF Core.</span><span class="sxs-lookup"><span data-stu-id="5f097-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="5f097-127">Обходные решения см. в списке [активных проблем](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin).</span><span class="sxs-lookup"><span data-stu-id="5f097-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="5f097-128"><sup>(3)</sup> Рекомендуется использовать EF Core 2.0.1 и последующих версий.</span><span class="sxs-lookup"><span data-stu-id="5f097-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="5f097-129">Установите [пакет .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="5f097-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="5f097-130">См. раздел [Универсальная платформа Windows](#universal-windows-platform) далее.</span><span class="sxs-lookup"><span data-stu-id="5f097-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="5f097-131"><sup>(4)</sup> В Unity существуют проблемы и известные ограничения.</span><span class="sxs-lookup"><span data-stu-id="5f097-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="5f097-132">См. список [актуальных проблем](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="5f097-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="5f097-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="5f097-133">.NET Framework</span></span>

<span data-ttu-id="5f097-134">Приложения, предназначенные для .NET Framework, могут потребовать внесения изменений для работы с библиотеками .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="5f097-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="5f097-135">Измените файл проекта и включите в группу начальной свойств следующую запись.</span><span class="sxs-lookup"><span data-stu-id="5f097-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="5f097-136">Для тестовых проектов также включите следующую запись.</span><span class="sxs-lookup"><span data-stu-id="5f097-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="5f097-137">Если вы хотите использовать старую версию Visual Studio, [обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads) для работы с библиотеками .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="5f097-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="5f097-138">Мы также рекомендуем вместо NuGet packages.config по возможности использовать PackageReference.</span><span class="sxs-lookup"><span data-stu-id="5f097-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="5f097-139">Добавьте в файл проекта следующее свойство:</span><span class="sxs-lookup"><span data-stu-id="5f097-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="5f097-140">Универсальная платформа Windows</span><span class="sxs-lookup"><span data-stu-id="5f097-140">Universal Windows Platform</span></span>

<span data-ttu-id="5f097-141">В ранних версиях EF Core и .NET UWP присутствовали некоторые проблемы с совместимостью, особенно в отношении приложений, скомпилированных с использованием цепочки инструментов .NET Native.</span><span class="sxs-lookup"><span data-stu-id="5f097-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="5f097-142">В новой версии универсальной платформы Windows .NET добавлена поддержка платформы .NET Standard 2.0 и содержится .NET Native 2.0, где исправлено большинство ранее выявленных проблем с совместимостью.</span><span class="sxs-lookup"><span data-stu-id="5f097-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="5f097-143">Было проведено более тщательное тестирование EF Core 2.0.1 с универсальной платформой Windows, однако эти тесты не были автоматизированными.</span><span class="sxs-lookup"><span data-stu-id="5f097-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="5f097-144">При использовании EF Core в UWP:</span><span class="sxs-lookup"><span data-stu-id="5f097-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="5f097-145">Чтобы оптимизировать производительность запросов, не используйте анонимные типы в запросах LINQ.</span><span class="sxs-lookup"><span data-stu-id="5f097-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="5f097-146">Чтобы развернуть приложение UWP в магазине приложений, оно должно быть скомпилировано в .NET Native.</span><span class="sxs-lookup"><span data-stu-id="5f097-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="5f097-147">Запросы с анонимными типами плохо работают в среде .NET Native.</span><span class="sxs-lookup"><span data-stu-id="5f097-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="5f097-148">Чтобы оптимизировать производительность `SaveChanges()`, используйте [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) и реализуйте [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging ](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) и [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) в типах сущностей.</span><span class="sxs-lookup"><span data-stu-id="5f097-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="5f097-149">Сообщить о проблеме</span><span class="sxs-lookup"><span data-stu-id="5f097-149">Report issues</span></span>

<span data-ttu-id="5f097-150">Если какое-либо сочетание работает неправильно, мы рекомендуем добавить новые вопросы в [средство отслеживания вопросов EF Core](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="5f097-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="5f097-151">Для проблем, относящихся к Xamarin, используйте средство отслеживания проблем [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) или [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="5f097-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
