---
title: "EF Core или EF6: помощь в выборе"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="b1382-102">EF Core или EF6: помощь в выборе</span><span class="sxs-lookup"><span data-stu-id="b1382-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="b1382-103">Следующие сведения помогут вам сделать выбор между Entity Framework Core и Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b1382-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="b1382-104">Рекомендации для новых приложений</span><span class="sxs-lookup"><span data-stu-id="b1382-104">Guidance for new applications</span></span>

<span data-ttu-id="b1382-105">Так как EF Core — это новый продукт, и в нем нет некоторых важных объектно-реляционных функций, EF6 пока остается оптимальным выбором для большинства приложений.</span><span class="sxs-lookup"><span data-stu-id="b1382-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="b1382-106">**Мы рекомендуем использовать EF Core для следующих типов приложений:**</span><span class="sxs-lookup"><span data-stu-id="b1382-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="b1382-107">Новые приложения, которым не требуются функции, еще не реализованные в EF Core.</span><span class="sxs-lookup"><span data-stu-id="b1382-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="b1382-108">Изучите [сравнительный анализ функций](features.md), чтобы узнать, подойдет ли EF Core для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="b1382-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="b1382-109">Приложения, специально предназначенные для .NET Core, например приложения универсальной платформы Windows (UWP) и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1382-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="b1382-110">Эти приложения не могут использовать EF6, поскольку требуют наличия .NET Framework (например, .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="b1382-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="b1382-111">Рекомендации для существующих приложений EF6</span><span class="sxs-lookup"><span data-stu-id="b1382-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="b1382-112">Из-за фундаментальных отличий от EF Core мы не рекомендуем переносить приложения EF6 на EF Core, если для этого нет веской причины.</span><span class="sxs-lookup"><span data-stu-id="b1382-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="b1382-113">Если вы хотите выполнить миграцию на EF Core, чтобы использовать новые возможности, заранее ознакомьтесь со всеми существующими ограничениями.</span><span class="sxs-lookup"><span data-stu-id="b1382-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="b1382-114">Изучите [сравнительный анализ функций](features.md), чтобы узнать, подойдет ли EF Core для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="b1382-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="b1382-115">**Переход с EF6 на EF Core следует рассматривать как портирование, а не обновление.**</span><span class="sxs-lookup"><span data-stu-id="b1382-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="b1382-116">Дополнительные сведения см. в разделе [Porting from EF6 to EF Core](porting/index.md) (Перенос приложений из EF6 в EF).</span><span class="sxs-lookup"><span data-stu-id="b1382-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
