---
title: Возврат к ObjectContext в Entity Framework Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488951"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Возврат к ObjectContext в Entity Framework Designer
С помощью предыдущей версии Entity Framework модели, созданной с помощью конструктора EF вызовет контекст, производный от ObjectContext и классов сущностей, производные от класса EntityObject.

Начиная с EF4.1 рекомендуется обращений шаблон генерации кода, который создает контекст, наследование от классов сущностей DbContext и POCO.

В Visual Studio 2012 вы получаете DbContext код, созданный по умолчанию для всех новых моделей, созданных с помощью конструктора EF. Существующие модели будут создавать ObjectContext на основе кода, если только вы сами замены в генератор кода на основе DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Возвращение обратно к генерации кода ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Отключите формирование кода DbContext

Создание производных классов DbContext и POCO обрабатывается двумя tt-файлов в проекте, если развернуть узел EDMX-файл в обозревателе решений появится эти файлы. Удалите оба файла из проекта.

![Файлы общего кода](~/ef6/media/codegenfiles.png)

При использовании VB.NET необходимо выбрать **Показать все файлы** кнопку, чтобы увидеть вложенные файлы.

![Показать все файлы](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Повторно включите создание кода ObjectContext

Открыть модель в конструкторе EF, щелкните правой кнопкой мыши пустую часть области конструктора и выберите **свойства**.

В окне изменения свойства **стратегия создания кода** из **None** для **по умолчанию**.

![Стратегия общего кода](~/ef6/media/codegenstrategy.png)
