---
title: Тестирование с помощью базы данных EF в памяти — EF Core
author: ajcvickers
description: Использование базы данных EF в памяти для тестирования приложения EF Core
ms.date: 10/27/2016
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f31b3bdedb8c339dbb6baa9d7f2031d023d5757f
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82538354"
---
# <a name="testing-with-the-ef-in-memory-database"></a>Тестирование с помощью базы данных EF в памяти

> [!WARNING]
> База данных EF в памяти часто ведет себя иначе, чем в реляционных базах данных.
> Используйте только базу данных EF в памяти после полного понимания проблем и связанных с ними компромиссов, как описано в разделе [тестирование кода, использующего EF Core](xref:core/miscellaneous/testing/index).  

> [!TIP]
> SQLite — это реляционный поставщик, который также может использовать базы данных в памяти.
> Рассмотрите возможность использования этого метода для тестирования в целях более точного соответствия общим поведениям реляционной базы данных.
> Это описано в разделе [Использование SQLite для тестирования приложения EF Core](xref:core/miscellaneous/testing/sqlite).   

Информация на этой странице теперь находится в других местах:
* Общие сведения о тестировании с помощью базы данных EF в памяти см. в разделе [тестирование кода, использующего EF Core](xref:core/miscellaneous/testing/index) .
* См. [пример, демонстрирующий тестирование приложений, использующих EF Core,](xref:core/miscellaneous/testing/testing-sample) для примера с использованием базы данных EF в памяти.
* Общие сведения о базе данных EF в памяти см. в описании [поставщика базы данных EF в памяти](xref:core/providers/in-memory/index) .
