---
title: Тестирование с помощью базы данных EF в памяти — EF Core
description: Использование базы данных EF в памяти для тестирования приложения Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 353f67fb0e78fefa74fc77d302e505bacb692ed4
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619396"
---
# <a name="testing-with-the-ef-in-memory-database"></a><span data-ttu-id="a4313-103">Тестирование с помощью базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="a4313-103">Testing with the EF In-Memory Database</span></span>

> [!WARNING]
> <span data-ttu-id="a4313-104">База данных EF в памяти часто ведет себя иначе, чем в реляционных базах данных.</span><span class="sxs-lookup"><span data-stu-id="a4313-104">The EF in-memory database often behaves differently than relational databases.</span></span>
> <span data-ttu-id="a4313-105">Используйте только базу данных EF в памяти после полного понимания проблем и связанных с ними компромиссов, как описано в разделе [тестирование кода, использующего EF Core](xref:core/miscellaneous/testing/index).</span><span class="sxs-lookup"><span data-stu-id="a4313-105">Only use the EF in-memory database after fully understanding the issues and trade-offs involved, as discussed in [Testing code that uses EF Core](xref:core/miscellaneous/testing/index).</span></span>  

> [!TIP]
> <span data-ttu-id="a4313-106">SQLite — это реляционный поставщик, который также может использовать базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="a4313-106">SQLite is a relational provider and can also use in-memory databases.</span></span>
> <span data-ttu-id="a4313-107">Рассмотрите возможность использования этого метода для тестирования в целях более точного соответствия общим поведениям реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="a4313-107">Consider using this for testing to more closely match common relational database behaviors.</span></span>
> <span data-ttu-id="a4313-108">Это описано в разделе [Использование SQLite для тестирования приложения EF Core](xref:core/miscellaneous/testing/sqlite).</span><span class="sxs-lookup"><span data-stu-id="a4313-108">This is covered in [Using SQLite to test an EF Core application](xref:core/miscellaneous/testing/sqlite).</span></span>   

<span data-ttu-id="a4313-109">Информация на этой странице теперь находится в других местах:</span><span class="sxs-lookup"><span data-stu-id="a4313-109">The information on this page now lives in other locations:</span></span>
* <span data-ttu-id="a4313-110">Общие сведения о тестировании с помощью базы данных EF в памяти см. в разделе [тестирование кода, использующего EF Core](xref:core/miscellaneous/testing/index) .</span><span class="sxs-lookup"><span data-stu-id="a4313-110">See [Testing code that uses EF Core](xref:core/miscellaneous/testing/index) for general information on testing with the EF in-memory database.</span></span>
* <span data-ttu-id="a4313-111">См. [пример, демонстрирующий тестирование приложений, использующих EF Core,](xref:core/miscellaneous/testing/testing-sample) для примера с использованием базы данных EF в памяти.</span><span class="sxs-lookup"><span data-stu-id="a4313-111">See [Sample showing how to test applications that use EF Core](xref:core/miscellaneous/testing/testing-sample) for a sample using the EF in-memory database.</span></span>
* <span data-ttu-id="a4313-112">Общие сведения о базе данных EF в памяти см. в описании [поставщика базы данных EF в памяти](xref:core/providers/in-memory/index) .</span><span class="sxs-lookup"><span data-stu-id="a4313-112">See [The EF in-memory database provider](xref:core/providers/in-memory/index) for general information about the EF in-memory database.</span></span>
