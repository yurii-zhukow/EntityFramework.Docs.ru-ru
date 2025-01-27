---
title: Несколько диаграмм на модель — EF6
description: Несколько схем на модель в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/multiple-diagrams
ms.openlocfilehash: 625cb932d23805f6bd22b658e4cddab1a487751c
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066217"
---
# <a name="multiple-diagrams-per-model"></a>Несколько диаграмм на модель
> [!NOTE]
> **Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

В этом видео и на странице показано, как разделить модель на несколько диаграмм с помощью Entity Framework Designer (конструктор EF). Эту функцию можно использовать, когда модель станет слишком большой для просмотра или редактирования.

В более ранних версиях конструктора EF можно было иметь только одну схему для файла EDMX. Начиная с Visual Studio 2012, можно использовать конструктор EF для разделения файла EDMX на несколько диаграмм.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео показано, как разделить модель на несколько диаграмм с помощью Entity Framework Designer (конструктор EF). Эту функцию можно использовать, когда модель станет слишком большой для просмотра или редактирования.

**Представлено**: Julia Корнич

**Видео**: [WMV](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv)  |  [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Общие сведения о конструкторе EF

При создании модели с помощью мастера EDM конструктора EF создается EDMX-файл, который добавляется в решение. Этот файл определяет форму сущностей и способ их соответствия с базой данных.

Конструктор EF состоит из следующих компонентов:

-   Визуальная область конструктора для редактирования модели. Сущности и взаимосвязи можно создавать, изменять или удалять.
-   Окно **обозревателя моделей**   , предоставляющее древовидные представления модели.Сущности и их связи находятся в папке * \[ modelname \] * . Таблицы и ограничения базы данных расположены в каталоге * \[ modelname \] *. Папка хранилища.
-   Окно **сведений о сопоставлении** для просмотра и изменения сопоставлений. Типы сущностей и взаимосвязи можно сопоставить с таблицами базы данных, столбцами и хранимыми процедурами. 

При завершении работы мастера EDM автоматически открывается окно визуальной области конструирования. Если обозреватель моделей не отображается, щелкните правой кнопкой мыши основную область конструктора и выберите пункт **Обозреватель моделей**.

На следующем снимке экрана показан файл EDMX, Открытый в конструкторе EF. На снимке экрана показана область визуального конструирования (слева) и окно **обозревателя моделей**   (справа).

![Конструктор EF 2](~/ef6/media/efdesigner2.png)

Чтобы отменить операцию, выполненную в конструкторе EF, нажмите клавиши CTRL + Z.

## <a name="working-with-diagrams"></a>Работа с диаграммами

По умолчанию конструктор EF создает одну схему с именем Diagram1. Если у вас есть схема с большим количеством сущностей и ассоциаций, вам будет проще разделить их логически. Начиная с Visual Studio 2012, концептуальную модель можно просматривать на нескольких схемах.   

При добавлении новых схем они отображаются в папке диаграммы в окне Обозреватель моделей. Для переименования диаграммы: Выберите диаграмму в окне "Обозреватель моделей", щелкните один раз над именем и введите новое имя. Можно также щелкнуть правой кнопкой мыши имя схемы и выбрать команду **Переименовать**.

Имя схемы отображается рядом с именем EDMX-файла в редакторе Visual Studio. Например, Model1. EDMX \[ Diagram1 \] .

![Имя схемы](~/ef6/media/diagramname.png)

Содержимое диаграмм (форма и цвет сущностей и ассоциаций) хранится в файле EDMX. Diagram. Чтобы просмотреть этот файл, выберите обозреватель решений и unfold файл EDMX. 

![Файлы схемы](~/ef6/media/diagramfiles.png)

Не следует изменять файл EDMX. diagram вручную, содержимое этого файла может быть перезаписано конструктором EF.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Разделение сущностей и ассоциаций на новую диаграмму

Можно выбрать сущности на существующей диаграмме (для выбора нескольких сущностей удерживайте клавишу Shift). Щелкните правой кнопкой мыши и выберите **переместить в новую диаграмму**. Создается новая диаграмма, и выбранные сущности и их связи перемещаются на схему.

Кроме того, можно щелкнуть правой кнопкой мыши папку диаграммы в обозревателе моделей и выбрать команду **Добавить новую диаграмму.** Затем можно перетащить сущности из папки "типы сущностей" в обозревателе моделей в область конструктора.

Можно также вырезать или копировать сущности (с помощью клавиш CTRL + X или CTRL + C) из одной диаграммы и вставить (с помощью клавиши CTRL + V) на другой. Если схема, в которую вставляется сущность, уже содержит сущность с тем же именем, в модель будет создана и добавлена новая сущность.Например: Diagram2 содержит сущность Department. Затем вы вставляете другой отдел в Diagram2. Сущность Department1 создается и добавляется в концептуальную модель.   

Чтобы включить связанные сущности в схему, Рик щелкните сущность и выберите **включить связанные**. Это приведет к копированию связанных сущностей и ассоциаций в указанной диаграмме.

## <a name="changing-the-color-of-entities"></a>Изменение цвета сущностей

Помимо разделения модели на несколько диаграмм можно также изменить цвета сущностей.

Чтобы изменить цвет, выберите сущность (или несколько сущностей) в области конструктора. Затем щелкните правой кнопкой мыши и выберите пункт **Свойства**. В окно свойств выберите свойство **Цвет заливки** . Укажите цвет, используя допустимое имя цвета (например, красный цвет) или допустимый RGB (например, 255, 128, 128). 

![Изменить цвет](~/ef6/media/color.png)

## <a name="summary"></a>Итоги

В этом разделе мы рассмотрели разделение модели на несколько диаграмм, а также способ указания другого цвета для сущности с помощью Entity Framework Designer. 
