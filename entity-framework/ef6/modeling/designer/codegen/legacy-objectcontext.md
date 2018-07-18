---
title: Возврат к ObjectContext в Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122342"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Возврат к ObjectContext в Entity Framework Designer
С помощью предыдущей версии Entity Framework модели, созданной с помощью конструктора EF вызовет контекст, производный от ObjectContext и классов сущностей, производные от класса EntityObject.

Начиная с EF4.1 рекомендуется обращений шаблон генерации кода, который создает контекст, наследование от классов сущностей DbContext и POCO.

В Visual Studio 2012 вы получаете DbContext код, созданный по умолчанию для всех новых моделей, созданных с помощью конструктора EF. Существующие модели будут создавать ObjectContext на основе кода, если только вы сами замены в генератор кода на основе DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Возвращение обратно к генерации кода ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Отключите формирование кода DbContext

Создание производных классов DbContext и POCO обрабатывается двумя tt-файлов в проекте, если развернуть узел EDMX-файл в обозревателе решений появится эти файлы. Удалите оба файла из проекта.

![CodeGenFiles](~/ef6/media/codegenfiles.png)

При использовании VB.NET необходимо выбрать **Показать все файлы** кнопку, чтобы увидеть вложенные файлы.

![ShowAllFiles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Повторно включите создание кода ObjectContext

Открыть модель в конструкторе EF, щелкните правой кнопкой мыши пустую часть области конструктора и выберите **свойства**.

В окне изменения свойства **стратегия создания кода** из **None** для **по умолчанию**.

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)
