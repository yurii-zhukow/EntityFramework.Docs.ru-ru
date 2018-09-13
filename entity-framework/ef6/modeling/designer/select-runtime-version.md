---
title: Выбрав версия среды выполнения Entity Framework для конструктора моделей EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488495"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="dbfc5-102">Выбрав версия среды выполнения Entity Framework для конструктора моделей EF</span><span class="sxs-lookup"><span data-stu-id="dbfc5-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="dbfc5-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="dbfc5-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="dbfc5-105">Начиная с EF6 был добавлен следующий экран конструкторе EF, позволяющее выбрать версию среды выполнения, которые вы хотите применить при создании модели.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="dbfc5-106">Экран появляется, если последнюю версию Entity Framework не установлена в проекте.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="dbfc5-107">Если уже установлена последняя версия просто будет использоваться по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-107">If the latest version is already installed it will just be used by default.</span></span>

![Экран](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="dbfc5-109">Нацеливание EF6.x</span><span class="sxs-lookup"><span data-stu-id="dbfc5-109">Targeting EF6.x</span></span>

<span data-ttu-id="dbfc5-110">Вы можете EF6 на экране «Выбор версии» для добавления среды выполнения EF6 в проект.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="dbfc5-111">После добавления EF6, будет остановлено, отображается этот экран в текущем проекте.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="dbfc5-112">EF6 будет отключен, если у вас уже есть более старая версия EF, установлена (поскольку невозможно указать несколько версий среды выполнения, в том же проекте).</span><span class="sxs-lookup"><span data-stu-id="dbfc5-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="dbfc5-113">Если параметр EF6 здесь не включен, выполните следующие действия для обновления проекта к EF6.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="dbfc5-114">Щелкните правой кнопкой мыши на проекте в обозревателе решений и выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="dbfc5-115">Выберите **обновлений**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-115">Select **Updates**</span></span>
3.  <span data-ttu-id="dbfc5-116">Выберите **EntityFramework** (Убедитесь, что нужно обновить его до версии, требуется)</span><span class="sxs-lookup"><span data-stu-id="dbfc5-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="dbfc5-117">Нажмите кнопку **обновления**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="dbfc5-118">Нацеливание EF5.x</span><span class="sxs-lookup"><span data-stu-id="dbfc5-118">Targeting EF5.x</span></span>

<span data-ttu-id="dbfc5-119">Вы можете EF5 на экране «Выбор версии» в среду выполнения EF5 добавить в проект.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="dbfc5-120">После добавления EF5, по-прежнему появится экран с параметром EF6 отключена.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="dbfc5-121">Если у вас есть более EF4.x версии уже установлена среда выполнения вы увидите этой версии платформы EF, перечисленные в экран, а не EF5.</span><span class="sxs-lookup"><span data-stu-id="dbfc5-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="dbfc5-122">В этой ситуации, можно обновить до EF5, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="dbfc5-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="dbfc5-123">Выберите **средства —&gt; диспетчер пакетов библиотеки -&gt; консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="dbfc5-124">Запустите **EntityFramework Install-Package-версии 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="dbfc5-125">Нацеливание EF4.x</span><span class="sxs-lookup"><span data-stu-id="dbfc5-125">Targeting EF4.x</span></span>

<span data-ttu-id="dbfc5-126">Среда выполнения EF4.x можно установить в проект, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="dbfc5-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="dbfc5-127">Выберите **средства —&gt; диспетчер пакетов библиотеки -&gt; консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="dbfc5-128">Запустите **EntityFramework Install-Package-версии 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="dbfc5-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
