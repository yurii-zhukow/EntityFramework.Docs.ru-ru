---
title: Выбор версии среды выполнения Entity Framework для моделей конструктора EF — EF6
description: Выбор версии среды выполнения Entity Framework для моделей конструктора EF в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 77830aaa3c7365e05e867d4ced88bdb8a11e7d6b
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620390"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="2f908-103">Выбор версии среды выполнения Entity Framework для моделей конструктора EF</span><span class="sxs-lookup"><span data-stu-id="2f908-103">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="2f908-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2f908-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="2f908-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="2f908-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2f908-106">Начиная с EF6 в конструктор EF был добавлен следующий экран, позволяющий выбрать версию среды выполнения, которую вы хотите ориентировать при создании модели.</span><span class="sxs-lookup"><span data-stu-id="2f908-106">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="2f908-107">Экран появится, когда последняя версия Entity Framework еще не установлена в проекте.</span><span class="sxs-lookup"><span data-stu-id="2f908-107">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="2f908-108">Если последняя версия уже установлена, она будет использоваться по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2f908-108">If the latest version is already installed it will just be used by default.</span></span>

![Выбор версии среды выполнения](~/ef6/media/screen.png)

## <a name="targeting-ef6x"></a><span data-ttu-id="2f908-110">Нацеливание на EF6. x</span><span class="sxs-lookup"><span data-stu-id="2f908-110">Targeting EF6.x</span></span>

<span data-ttu-id="2f908-111">Вы можете выбрать EF6 на экране "Выбор версии", чтобы добавить среду выполнения EF6 в проект.</span><span class="sxs-lookup"><span data-stu-id="2f908-111">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="2f908-112">После добавления EF6 вы больше не увидите этот экран в текущем проекте.</span><span class="sxs-lookup"><span data-stu-id="2f908-112">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="2f908-113">EF6 будет отключен, если уже установлена более старая версия EF (так как вы не можете ориентироваться на несколько версий среды выполнения из одного проекта).</span><span class="sxs-lookup"><span data-stu-id="2f908-113">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="2f908-114">Если параметр EF6 не включен, выполните следующие действия, чтобы обновить проект до EF6:</span><span class="sxs-lookup"><span data-stu-id="2f908-114">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="2f908-115">Щелкните проект правой кнопкой мыши в обозреватель решений и выберите пункт **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="2f908-115">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="2f908-116">Выбор **обновлений**</span><span class="sxs-lookup"><span data-stu-id="2f908-116">Select **Updates**</span></span>
3.  <span data-ttu-id="2f908-117">Выберите **EntityFramework** (убедитесь, что он будет обновлен до нужной версии).</span><span class="sxs-lookup"><span data-stu-id="2f908-117">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="2f908-118">Нажмите кнопку **Обновить** .</span><span class="sxs-lookup"><span data-stu-id="2f908-118">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="2f908-119">Нацеливание на EF5. x</span><span class="sxs-lookup"><span data-stu-id="2f908-119">Targeting EF5.x</span></span>

<span data-ttu-id="2f908-120">Вы можете выбрать EF5 на экране "Выбор версии", чтобы добавить среду выполнения EF5 в проект.</span><span class="sxs-lookup"><span data-stu-id="2f908-120">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="2f908-121">После добавления EF5 вы по-прежнему увидите экран с отключенным параметром EF6.</span><span class="sxs-lookup"><span data-stu-id="2f908-121">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="2f908-122">Если у вас уже установлена версия EF4. x среды выполнения, вы увидите, что эта версия EF отображается на экране, а не в EF5.</span><span class="sxs-lookup"><span data-stu-id="2f908-122">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="2f908-123">В этой ситуации можно выполнить обновление до EF5, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="2f908-123">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="2f908-124">Выберите **инструменты — &gt; Диспетчер пакетов библиотек — &gt; консоль диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="2f908-124">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="2f908-125">Выполните команду **Install-package entityframework-Version 5.0.0** .</span><span class="sxs-lookup"><span data-stu-id="2f908-125">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="2f908-126">Нацеливание на EF4. x</span><span class="sxs-lookup"><span data-stu-id="2f908-126">Targeting EF4.x</span></span>

<span data-ttu-id="2f908-127">Среду выполнения EF4. x можно установить в проект, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="2f908-127">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="2f908-128">Выберите **инструменты — &gt; Диспетчер пакетов библиотек — &gt; консоль диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="2f908-128">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="2f908-129">Выполните команду **Install-package entityframework-Version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="2f908-129">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
