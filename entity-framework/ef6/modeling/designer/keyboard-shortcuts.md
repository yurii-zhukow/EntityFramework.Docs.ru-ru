---
title: Entity Framework сочетания клавиш в конструкторе - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: 70c9705956b58f4d00908dd9cca6ad0e0a078fc6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997767"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework сочетания клавиш в конструкторе
Эта страница предоставляет список сочетания клавиатуры, которые доступны в различных экранов инструменты Entity Framework для Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Мастер моделей EDM ADO.NET

### <a name="step-one-choose-model-contents"></a>Шаг один: Выбор содержимого модели

![WizardOne](~/ef6/media/wizardone.png)

| Сочетание клавиш  | Действие                                                     | Примечания                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Перемещение к следующему экрану                                        | Не доступно для всех выбранных элементов для модели содержимого. |
| **ALT + f** | Завершение работы мастера                                              | Не доступно для всех выбранных элементов для модели содержимого. |
| **ALT + w** | Переключить фокус на «что должна содержать модель?» область. |                                                     |

### <a name="step-two-choose-your-connection"></a>Шаг 2: Выбор подключения

![WizardTwo](~/ef6/media/wizardtwo.png)

| Сочетание клавиш  | Действие                                                     | Примечания                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Перемещение к следующему экрану                                        |                                                         |
| **ALT + p** | Перемещение к предыдущему экрану                                    |                                                         |
| **ALT + w** | Переключить фокус на «что должна содержать модель?» область. |                                                         |
| **ALT + c** | Откройте окно «Свойства подключения»                    | Позволяет определять новые соединения с базой данных. |
| **ALT + e** | Исключить конфиденциальные данные из строки подключения          |                                                         |
| **ALT + i** | Включить конфиденциальные данные в строке подключения            |                                                         |
| **ALT + s** | Укажите для параметра «Сохранить параметры подключения в файле App.Config» |                                                         |

### <a name="step-three-choose-your-version"></a>Шаг 3: Выбор версии

![WizardThree](~/ef6/media/wizardthree.png)

| Сочетание клавиш  | Действие                                             | Примечания                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Перемещение к следующему экрану                                |                                                                                       |
| **ALT + p** | Перемещение к предыдущему экрану                            |                                                                                       |
| **ALT + w** | Переключить фокус на выбор версии Entity Framework | Позволяет указать другую платформы Entity Framework для использования в проекте. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Шаг 4: Выбор объектов базы данных и параметры

![WizardFour](~/ef6/media/wizardfour.png)

| Сочетание клавиш  | Действие                                                                                    | Примечания                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Завершение работы мастера                                                                             |                                                                     |
| **ALT + p** | Перемещение к предыдущему экрану                                                                   |                                                                     |
| **ALT + w** | Переключить фокус на панели выбора объектов базы данных                                            | Позволяет создавать для указания объектов базы данных для обратного разбору.    |
| **ALT + s** | Переключатель «множественном или единственном числе имена создаваемых объектов» параметр                       |                                                                     |
| **ALT + k** | Укажите для параметра «Включить столбцы внешнего ключа в модели»                              | Не доступно для всех выбранных элементов для модели содержимого.                 |
| **ALT + i** | Переключить режим «Импортировать выбранные хранимые процедуры и функции в модели» | Не доступно для всех выбранных элементов для модели содержимого.                 |
| **ALT + m** | Перенос фокуса ввода в текстовое поле «Пространство имен модели»                                        | Не доступно для всех выбранных элементов для модели содержимого.                 |
| **ПРОБЕЛ** | Переключить выделение в элементе                                                               | Если элемент имеет дочерние элементы, все дочерние элементы будут включаться также |
| **Слева**  | Свернуть дочернего дерева                                                                       |                                                                     |
| **Справа** | Разверните дерево дочерних                                                                         |                                                                     |
| **Вверх**    | Перейдите на предыдущий элемент в дереве                                                      |                                                                     |
| **Вниз**  | Перейдите к следующему элементу в дереве                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Область конструктора EF

![DesignerSurface](~/ef6/media/designersurface.png)

| Сочетание клавиш                                                                                | Действие                      | Примечания                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Или введите пространства**                                                                         | Переключить выделение            | Переключает выбор в объект с фокусом.                                                                                                                                                                                         |
| **ESC**                                                                                 | Отменить выделение            | Отмена текущего выделения.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Выделить все                  | Выбирает все фигуры на поверхности разработки.                                                                                                                                                                                       |
| **Стрелка вверх**                                                                            | Переместить вверх                     | Перемещение выбранных сущностей один шаг сетки вверх. <br/> Если в списке, перемещается к предыдущему вложенному.                                                                                                                            |
| **Стрелка вниз**                                                                          | Переместить вниз                   | Перемещение выбранных сущностей вниз один шаг сетки. <br/> Если в списке, перемещает Далее вложенному.                                                                                                                              |
| **СТРЕЛКА ВЛЕВО**                                                                          | Переместить влево                   | Перемещение выбранных сущностей один шаг сетки влево. <br/> Если в списке, перемещается к предыдущему вложенному.                                                                                                                          |
| **СТРЕЛКА ВПРАВО**                                                                         | Переместить вправо                  | Перемещение выделенных шаг сетки вправо на одну сущность. <br/> Если в списке, перемещает Далее вложенному.                                                                                                                             |
| **Shift + Стрелка влево**                                                                  | Размер фигуры влево             | Уменьшение ширины элемента выбранной сущности на один шаг сетки.                                                                                                                                                                     |
| **Shift + Стрелка вправо**                                                                 | Размер фигуры вправо            | Увеличение ширины элемента выбранной сущности на один шаг сетки.                                                                                                                                                                   |
| **Корневая папка**                                                                                | Первый узел                  | Перемещение фокуса и выделение на первый объект в области конструктора на том же уровне.                                                                                                                                         |
| **END**                                                                                 | Последний однорангового узла                   | Перемещение фокуса и выделения до последнего объекта в области конструктора на том же уровне.                                                                                                                                          |
| **CTRL + Home**                                                                         | Первый узел (фокус)          | Совпадает со значением первого узла, но перемещение фокуса без перемещения фокуса и выделения.                                                                                                                                                          |
| **Ctrl + End**                                                                          | Последний однорангового узла (фокус)           | Так же, как Дата последнего узла, но можно перемещать фокус без перемещения фокуса и выделения.                                                                                                                                                           |
| **TAB**                                                                                 | Далее однорангового узла                   | Перемещение фокуса и выделение на следующий объект в области конструктора на том же уровне.                                                                                                                                          |
| **SHIFT+TAB**                                                                           | Предыдущий однорангового узла               | Перемещение фокуса и выделение предыдущего объекта в области конструктора на том же уровне.                                                                                                                                      |
| **Alt + Ctrl + Tab**                                                                        | Далее однорангового узла (фокус)           | Так же, как следующего узла, но можно перемещать фокус без перемещения фокуса и выделения.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | Предыдущий узел (фокус)       | Совпадает со значением предыдущего однорангового узла, но перемещение фокуса без перемещения фокуса и выделения.                                                                                                                                                       |
| **&lt;**                                                                                | Перемещения вверх                      | Переход к следующему объекту на проектирование поверхности один уровень выше в иерархии. Если нет ни одной фигуры, расположенные уровнем выше в иерархии (то есть объект помещается непосредственно в рабочей области конструирования), выбирается схема. |
| **&gt;**                                                                                | Опускаются                     | Переходит к следующей содержащийся объект на поверхности разработки один уровень, ниже в иерархии. Если не содержащегося объекта, это холостой.                                                                              |
| **CTRL + &lt;**                                                                         | Ascend (фокус)              | Так же, как ascend команды, однако перемещение фокуса без выделения.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Опускаются (фокус)             | Так же, как опускаются команды, однако перемещение фокуса без выделения.                                                                                                                                                                         |
| **Shift + End**                                                                         | Выполните подключенный         | Из сущности перемещает сущность, которая подключена к этой сущности.                                                                                                                                                               |
| **DEL**                                                                                 | Удаление                      | Удалите из схемы объекта или соединитель.                                                                                                                                                                                     |
| **Модули**                                                                                 | Insert                      | Добавляет новое свойство сущности, при выборе заголовка секции «Скалярные свойства» или само свойство.                                                                                                           |
| **Стр.**                                                                               | Схема прокрутки вверх           | Прокручивает область конструктора, с шагом, равным 75% от высоты в области конструктора, видимой в данный момент.                                                                                                                    |
| **Стр.**                                                                             | Схема прокрутки вниз         | Прокручивает область конструктора вниз.                                                                                                                                                                                                    |
| **SHIFT + стр.**                                                                     | Схема прокрутки вправо        | Прокручивает область конструктора, чтобы справа.                                                                                                                                                                                            |
| **SHIFT + стр.**                                                                       | Прокрутите схему влево         | Прокручивает область конструктора, чтобы слева.                                                                                                                                                                                             |
| **F2**                                                                                  | Режим редактирования             | Стандартные сочетания клавиш для вход в режим правки для текстового элемента управления.                                                                                                                                                               |
| **SHIFT + F10**                                                                         | Отображение контекстного меню       | Стандартные сочетания клавиш для отображения контекстного меню выбранного элемента.                                                                                                                                                          |
| **Control + Shift + мыши щелчок левой кнопкой**  <br/> **Control + Shift + колесико мыши вперед**  | Семантический масштаб в            | Увеличение масштаба области под курсором мыши представления "Схема".                                                                                                                                                                 |
| **Control + Shift + мыши щелкните правой кнопкой мыши** <br/> **Control + Shift + колесико мыши назад** | Семантический масштаб Out           | Уменьшает масштаб в области представления "Схема" под курсором мыши. Повторно диаграмме центров, при уменьшении слишком далеко для текущего центра схеме.                                                                          |
| **Удерживая нажатой клавишу CTRL Shift + «+»** <br/> **Control + колесико мыши вперед**                        | Увеличить                     | Увеличение в центре представления "Схема".                                                                                                                                                                                         |
| **Удерживая нажатой клавишу CTRL Shift + "-"** <br/> **Control + колесико мыши назад**                       | Уменьшить                    | Уменьшает масштаб из выбранной области представления "Схема". Повторно диаграмме центров, при уменьшении слишком далеко для текущего центра схеме.                                                                                            |
| **Управлять + Shift + нарисовать прямоугольник с левой кнопки мыши**                  | Области увеличения                   | Увеличение по центру области, которые вы выбрали. При удержании нажатой клавиши Shift + элемента управления, вы увидите сообщение о том, что курсор изменяется в лупу, можно задать область для увеличения масштаба.                        |
| **Клавиша контекстного меню + меня "**                                                              | Откройте окно сведений о сопоставлении | Откроется окно сведения о сопоставлении для изменения сопоставления для выбранной сущности                                                                                                                                                               |

## <a name="mapping-details-window"></a>Окно «Сведения о сопоставлении»

![MappingDetailsShortcuts](~/ef6/media/mappingdetailsshortcuts.png)

| Сочетание клавиш                  | Действие         | Примечания                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **TAB**                   | Переключение контекста | Переключение между основную область окна и панели инструментов слева                                                                     |
| **Клавиши со стрелками**            | Навигация     | Перемещение вверх и вниз строки, или вправо и влево по столбцам в основную область окна. Перемещение между кнопками на панели инструментов в левой части. |
| **ВВОД** <br/> **ПРОБЕЛ** | Выбрать         | Выбор кнопки на панели инструментов в левой части.                                                                                          |
| **Alt + Стрелка вниз**      | Открытие списка      | Раскрывающийся список, если ячейка выбрана, имеющий раскрывающегося списка.                                                                     |
| **ВВОД**                 | Выберите    | Выбирает элемент в раскрывающемся списке.                                                                                               |
| **ESC**                   | Закройте список     | Закрытие раскрывающегося списка.                                                                                                              |

## <a name="visual-studio-navigation"></a>Visual Studio навигации

Платформа Entity Framework также предоставляет ряд определенных действий, которые могут иметь сопоставленные пользовательских сочетаний клавиш (по умолчанию сопоставлены никакие ярлыки). Чтобы создать эти настраиваемые сочетания клавиш, щелкните меню "Сервис", а затем параметры.  В среде выберите клавиатура.  Назначьте прокрутите вниз список в середине пока можно выбрать нужную команду, введите в текстовом поле «Введите сочетание клавиш» и нажмите кнопку. Ниже приведены возможные сочетания клавиш.

| Сочетание клавиш                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
