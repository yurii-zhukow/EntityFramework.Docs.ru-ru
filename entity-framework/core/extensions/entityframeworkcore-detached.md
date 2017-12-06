---
title: "Detached.EntityFramework - средств и расширений - Core EF"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: DE1C3FEA-F618-4C11-932F-7C392A1B2C94
ms.technology: entity-framework-core
uid: core/extensions/entityframeworkcore-detached
ms.openlocfilehash: 93937eee07a9e1a0b94bd92140faec7d1753332f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="efdetachedentityframework-extension"></a><span data-ttu-id="f254d-102">Расширение EFDetached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="f254d-102">EFDetached.EntityFramework Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="f254d-103">Это расширение не сохраняется как часть проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f254d-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="f254d-104">При рассмотрении возможности расширения сторонних разработчиков, убедитесь, что оценка качества, лицензирования, поддержка, т. д., чтобы убедиться, что они отвечают требованиям.</span><span class="sxs-lookup"><span data-stu-id="f254d-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="f254d-105">Загружает и сохраняет графы отсоединенные сущность целиком (сущность, с их дочерних сущностей и списков).</span><span class="sxs-lookup"><span data-stu-id="f254d-105">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="f254d-106">Вдохновлен [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="f254d-106">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="f254d-107">Идея заключается в также добавлять некоторые подключаемые модули для simplificate некоторых повторяющихся задач, таких как аудит и разбиение на страницы.</span><span class="sxs-lookup"><span data-stu-id="f254d-107">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

<span data-ttu-id="f254d-108">Следующий материал поможет вам приступить к работе с Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="f254d-108">The following resource will help you get started with Detached.EntityFramework</span></span>
* [<span data-ttu-id="f254d-109">Репозитории Detached.EntityFramework GitHub</span><span class="sxs-lookup"><span data-stu-id="f254d-109">Detached.EntityFramework GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)
