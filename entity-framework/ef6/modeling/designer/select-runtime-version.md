---
title: Выбрав версия среды выполнения Entity Framework для конструктора моделей EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998246"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="6910a-102">Выбрав версия среды выполнения Entity Framework для конструктора моделей EF</span><span class="sxs-lookup"><span data-stu-id="6910a-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="6910a-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6910a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="6910a-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="6910a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="6910a-105">Начиная с EF6 был добавлен следующий экран конструкторе EF, позволяющее выбрать версию среды выполнения, которые вы хотите применить при создании модели.</span><span class="sxs-lookup"><span data-stu-id="6910a-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="6910a-106">Экран появляется, если последнюю версию Entity Framework не установлена в проекте.</span><span class="sxs-lookup"><span data-stu-id="6910a-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="6910a-107">Если уже установлена последняя версия просто будет использоваться по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6910a-107">If the latest version is already installed it will just be used by default.</span></span>

![Экран](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="6910a-109">Нацеливание EF6.x</span><span class="sxs-lookup"><span data-stu-id="6910a-109">Targeting EF6.x</span></span>

<span data-ttu-id="6910a-110">Вы можете EF6 на экране «Выбор версии» для добавления среды выполнения EF6 в проект.</span><span class="sxs-lookup"><span data-stu-id="6910a-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="6910a-111">После добавления EF6, будет остановлено, отображается этот экран в текущем проекте.</span><span class="sxs-lookup"><span data-stu-id="6910a-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="6910a-112">EF6 будет отключен, если у вас уже есть более старая версия EF, установлена (поскольку невозможно указать несколько версий среды выполнения, в том же проекте).</span><span class="sxs-lookup"><span data-stu-id="6910a-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="6910a-113">Если параметр EF6 здесь не включен, выполните следующие действия для обновления проекта к EF6.</span><span class="sxs-lookup"><span data-stu-id="6910a-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="6910a-114">Щелкните правой кнопкой мыши на проекте в обозревателе решений и выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="6910a-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="6910a-115">Выберите **обновлений**</span><span class="sxs-lookup"><span data-stu-id="6910a-115">Select **Updates**</span></span>
3.  <span data-ttu-id="6910a-116">Выберите **EntityFramework** (Убедитесь, что нужно обновить его до версии, требуется)</span><span class="sxs-lookup"><span data-stu-id="6910a-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="6910a-117">Нажмите кнопку **обновления**</span><span class="sxs-lookup"><span data-stu-id="6910a-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="6910a-118">Нацеливание EF5.x</span><span class="sxs-lookup"><span data-stu-id="6910a-118">Targeting EF5.x</span></span>

<span data-ttu-id="6910a-119">Вы можете EF5 на экране «Выбор версии» в среду выполнения EF5 добавить в проект.</span><span class="sxs-lookup"><span data-stu-id="6910a-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="6910a-120">После добавления EF5, по-прежнему появится экран с параметром EF6 отключена.</span><span class="sxs-lookup"><span data-stu-id="6910a-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="6910a-121">Если у вас есть более EF4.x версии уже установлена среда выполнения вы увидите этой версии платформы EF, перечисленные в экран, а не EF5.</span><span class="sxs-lookup"><span data-stu-id="6910a-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="6910a-122">В этой ситуации, можно обновить до EF5, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="6910a-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="6910a-123">Выберите **средства —&gt; диспетчер пакетов библиотеки -&gt; консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="6910a-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="6910a-124">Запустите **EntityFramework Install-Package-версии 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="6910a-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="6910a-125">Нацеливание EF4.x</span><span class="sxs-lookup"><span data-stu-id="6910a-125">Targeting EF4.x</span></span>

<span data-ttu-id="6910a-126">Среда выполнения EF4.x можно установить в проект, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="6910a-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="6910a-127">Выберите **средства —&gt; диспетчер пакетов библиотеки -&gt; консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="6910a-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="6910a-128">Запустите **EntityFramework Install-Package-версии 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="6910a-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
