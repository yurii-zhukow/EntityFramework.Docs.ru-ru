---
title: Ограничения внешнего ключа-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655999"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="c76fb-102">Ограничения внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="c76fb-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="c76fb-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="c76fb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c76fb-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="c76fb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c76fb-105">Ограничение внешнего ключа вводится для каждой связи в модели.</span><span class="sxs-lookup"><span data-stu-id="c76fb-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="c76fb-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="c76fb-106">Conventions</span></span>

<span data-ttu-id="c76fb-107">По соглашению ограничения внешнего ключа именуются `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c76fb-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="c76fb-108">Для составных внешних ключей `<foreign key property name>` преобразуется в список имен свойств внешнего ключа с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="c76fb-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c76fb-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="c76fb-109">Data Annotations</span></span>

<span data-ttu-id="c76fb-110">Имена ограничений внешнего ключа нельзя настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="c76fb-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c76fb-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="c76fb-111">Fluent API</span></span>

<span data-ttu-id="c76fb-112">С помощью API-интерфейса Fluent можно настроить имя ограничения внешнего ключа для связи.</span><span class="sxs-lookup"><span data-stu-id="c76fb-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
