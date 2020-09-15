---
title: Возврат к ObjectContext в Entity Framework Designer-EF6
description: Возврат к ObjectContext в Entity Framework Designer в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/designer/codegen/legacy-objectcontext
ms.openlocfilehash: 9ceb8ef43e4df083fe3cc5e63862ba2eb338f659
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069968"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Возврат к ObjectContext в Entity Framework Designer
В предыдущей версии Entity Framework модель, созданная с помощью конструктора EF, создаст контекст, производный от ObjectContext и классов сущностей, производных от EntityObject.

Начиная с EF 4.1 мы рекомендуем поменять местами шаблон создания кода, который создает контекст, производный от классов сущностей DbContext и POCO.

В Visual Studio 2012 вы получаете код DbContext, созданный по умолчанию для всех новых моделей, созданных с помощью конструктора EF. Существующие модели будут по прежнему создавать код на основе ObjectContext, если не решено переключиться на генератор кода на основе DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Возврат к созданию кода ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Отключение создания кода DbContext

Создание производных классов DbContext и POCO обрабатывается двумя файлами. tt в проекте. Если в обозревателе решений вы развернете EDMX-файл, вы увидите эти файлы. Удалите оба этих файла из проекта.

![Файлы кода для генерации](~/ef6/media/codegenfiles.png)

При использовании VB.NET необходимо нажать кнопку **Показать все файлы** , чтобы просмотреть вложенные файлы.

![Показать все файлы](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Повторное включение создания кода ObjectContext

Откройте модель в конструкторе EF, щелкните правой кнопкой мыши пустую секцию в области конструктора и выберите пункт **Свойства**.

В окно свойств изменить **стратегию создания кода** с **None** на **Default**.

![Стратегия генерации кода](~/ef6/media/codegenstrategy.png)
