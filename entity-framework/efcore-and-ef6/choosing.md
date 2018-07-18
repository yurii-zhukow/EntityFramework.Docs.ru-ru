---
title: 'EF Core или EF6: помощь в выборе'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949144"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="05ecc-102">EF Core или EF6: помощь в выборе</span><span class="sxs-lookup"><span data-stu-id="05ecc-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="05ecc-103">Следующие сведения помогут вам сделать выбор между Entity Framework Core и Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="05ecc-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="05ecc-104">Рекомендации для новых приложений</span><span class="sxs-lookup"><span data-stu-id="05ecc-104">Guidance for new applications</span></span>

<span data-ttu-id="05ecc-105">Рассмотрите возможность использования EF Core для новых приложений в случае, если вы хотите использовать все возможности EF Core и для вашего приложения не потребуется никакие из возможностей, которые еще не реализованы в EF Core.</span><span class="sxs-lookup"><span data-stu-id="05ecc-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="05ecc-106">Для EF6 требуется наличие .NET Framework 4.0 (или более поздней версии), и он поддерживается только в Windows (т. е. сейчас EF6 не выполняется на .NET Core и не поддерживается в других операционных системах), но он по-прежнему хорошо подходит для создания новых приложений в случаях, когда эти ограничения являются допустимыми и в приложении не требуется использовать новые возможности EF Core, которые недоступны в EF6.</span><span class="sxs-lookup"><span data-stu-id="05ecc-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="05ecc-107">Изучите [сравнительный анализ функций](features.md), чтобы узнать, подойдет ли EF Core для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="05ecc-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="05ecc-108">Рекомендации для существующих приложений EF6</span><span class="sxs-lookup"><span data-stu-id="05ecc-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="05ecc-109">Из-за фундаментальных отличий от EF Core мы не рекомендуем переносить приложения EF6 на EF Core, если для этого нет веской причины.</span><span class="sxs-lookup"><span data-stu-id="05ecc-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="05ecc-110">Если вы хотите выполнить миграцию на EF Core, чтобы использовать новые возможности, заранее ознакомьтесь со всеми существующими ограничениями.</span><span class="sxs-lookup"><span data-stu-id="05ecc-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="05ecc-111">Изучите [сравнительный анализ функций](features.md), чтобы узнать, подойдет ли EF Core для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="05ecc-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="05ecc-112">**Переход с EF6 на EF Core следует рассматривать как портирование, а не обновление.**</span><span class="sxs-lookup"><span data-stu-id="05ecc-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="05ecc-113">Дополнительные сведения см. в разделе [Porting from EF6 to EF Core](porting/index.md) (Перенос приложений из EF6 в EF).</span><span class="sxs-lookup"><span data-stu-id="05ecc-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
