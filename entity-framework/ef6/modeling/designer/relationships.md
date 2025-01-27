---
title: Связи — конструктор EF — EF6
description: Связи — конструктор EF в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/relationships
ms.openlocfilehash: 0211eb1916a7372874f0b7812c512e6eeba0974f
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066139"
---
# <a name="relationships---ef-designer"></a>Связи — конструктор EF
> [!NOTE]
> На этой странице содержатся сведения о настройке связей в модели с помощью конструктора EF. Общие сведения о связях в EF и доступе к данным и управлении ими с помощью связей см. в разделе [связи & свойства навигации](xref:ef6/fundamentals/relationships).

Ассоциации определяют связи между типами сущностей в модели. В этом разделе показано, как сопоставлять ассоциации с Entity Framework Designer (конструктор EF). На следующем рисунке показаны основные окна, используемые при работе с конструктором EF.

![Конструктор EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> При построении концептуальной модели в окне Список ошибок могут появиться предупреждения о несопоставленных сущностях и ассоциациях. Эти предупреждения можно игнорировать, так как после выбора создания базы данных из модели ошибки будут исчезнуть.

## <a name="associations-overview"></a>Общие сведения о ассоциациях

При проектировании модели с помощью конструктора EF файл. EDMX представляет вашу модель. В EDMX-файле элемент **Association** определяет связь между двумя типами сущностей. Ассоциация должна указывать типы сущностей, которые участвуют в связи, и возможное количество типов сущностей на каждом конце связи, которое называется кратностью. Кратность элемента ассоциации может иметь значение один (1), ноль или один (0.. 1) или многие ( \* ). Эти сведения указываются в **двух дочерних** элементах.

Во время выполнения экземпляры типа сущности на одном конце ассоциации могут быть доступны через свойства навигации или внешние ключи (если вы решили предоставить внешние ключи в сущностях). При предоставлении внешних ключей связь между сущностями управляется с помощью элемента **ReferentialConstraint** (дочерний элемент элемента **Association** ). Рекомендуется всегда предоставлять внешние ключи для связей в сущностях.

> [!NOTE]
> В сущности «многие ко многим» ( \* : \* ) нельзя добавлять внешние ключи к сущностям. В \* связи: \* сведения о взаимосвязи управляются с помощью независимого объекта.

Сведения об элементах CSDL (**ReferentialConstraint**, **Association**и т. д.) см. в [спецификации языка CSDL](xref:ef6/modeling/designer/advanced/edmx/csdl-spec).

## <a name="create-and-delete-associations"></a>Создание и удаление ассоциаций

При создании связи с помощью конструктора EF содержимое модели обновляется в файле EDMX. После создания ассоциации необходимо создать сопоставления для ассоциации (рассматривается далее в этом разделе).

> [!NOTE]
> В этом разделе предполагается, что вы уже добавили сущности, между которыми вы хотите создать связь, с моделью.

### <a name="to-create-an-association"></a>Создание ассоциации

1.  Щелкните правой кнопкой мыши пустую область области конструктора, наведите указатель на пункт **Добавить новый**и выберите **связь...**.
2.  Заполните параметры сопоставления в диалоговом окне **Добавление ассоциации** .

    ![Добавить ассоциацию](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Вы можете не добавлять свойства навигации или свойства внешнего ключа в сущности на концах ассоциации, очистив **свойство навигации **и **добавляя свойства внешнего ключа в флажки &lt; &gt; сущность типа сущности Name **. При добавлении одного свойства навигации ассоциацию можно будет перемещать только в одном направлении. При добавлении свойств навигации необходимо добавить свойства внешнего ключа, чтобы обеспечить доступ к сущностям в элементах ассоциации.
    
3.  Нажмите кнопку **ОК**.

### <a name="to-delete-an-association"></a>Удаление ассоциации

Чтобы удалить связь, выполните одно из следующих действий.

-   Щелкните ассоциацию правой кнопкой мыши в области конструктора EF и выберите **Удалить**.

- OR -

-   Выберите одну или несколько ассоциаций и нажмите клавишу DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Включить свойства внешнего ключа в сущности (ссылочные ограничения)

Рекомендуется всегда предоставлять внешние ключи для связей в сущностях. Entity Framework использует ссылочное ограничение, чтобы выяснить, что свойство выступает в качестве внешнего ключа для связи.

Если установлен флажок ***Добавить свойства внешнего ключа в &lt; &gt; сущность имя типа сущности*** при создании связи, это ссылочное ограничение было добавлено.

При использовании конструктора EF для добавления или изменения ссылочного ограничения конструктор EF добавляет или изменяет элемент **ReferentialConstraint**   в CSDL-файле EDMX.

-   Дважды щелкните ассоциацию, которую необходимо изменить.
    Откроется диалоговое окно **ссылочное ограничение**   .
-   В раскрывающемся списке **основной субъект**   выберите сущность субъекта в справочном ограничении.
    Ключевые свойства сущности добавляются в список **ключей субъекта**   в диалоговом окне.
-   Из **Dependent**   раскрывающегося списка зависимый выберите зависимую сущность в справочном ограничении.
-   Для каждого ключа субъекта, имеющего зависимый ключ, выберите соответствующий зависимый ключ из раскрывающихся списков в столбце **зависимых ключей**   .

    ![Ограничение ref](~/ef6/media/refconstraint.png)

-   Нажмите кнопку **ОК**.

## <a name="create-and-edit-association-mappings"></a>создавать и изменять сопоставления ассоциаций

Можно указать, как Ассоциация сопоставляется с базой данных в окне **сведения о сопоставлении**   конструктора EF.

> [!NOTE]
> Можно сопоставлять только сведения для ассоциаций, для которых не задано ссылочное ограничение. Если указано ссылочное ограничение, свойство внешнего ключа включается в сущность и можно использовать сведения о сопоставлении для сущности, чтобы контролировать, к какому столбцу сопоставлен внешний ключ.

### <a name="create-an-association-mapping"></a>Создание сопоставления ассоциации

-   Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите **Сопоставление таблиц**.
    В окне **сведения о сопоставлении**отобразится сопоставление ассоциации   .
-   Нажмите кнопку **Добавить таблицу или представление**.
    Появится раскрывающийся список, содержащий все таблицы в модели хранения.
-   Выберите таблицу, с которой будет сопоставлена ассоциация.
    В окне **сведения о сопоставлении**   отображаются оба конца ассоциации и ключевые свойства для типа сущности в каждой **стороне**.
-   Для каждого ключевого свойства щелкните поле **столбец**   и выберите столбец, с которым будет сопоставляться свойство.

    ![Сведения о сопоставлении 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Изменение сопоставления ассоциации

-   Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите **Сопоставление таблиц**.
    В окне **сведения о сопоставлении**отобразится сопоставление ассоциации   .
-   Щелкните **сопоставить с &lt; именем &gt; таблицы**.
    Появится раскрывающийся список, содержащий все таблицы в модели хранения.
-   Выберите таблицу, с которой будет сопоставлена ассоциация.
    В окне **сведения о сопоставлении**   отображаются оба конца ассоциации и ключевые свойства для типа сущности в каждой стороне.
-   Для каждого ключевого свойства щелкните поле **столбец**   и выберите столбец, с которым будет сопоставляться свойство.

## <a name="edit-and-delete-navigation-properties"></a>Изменение и удаление свойств навигации

Свойства навигации — это свойства ярлыка, используемые для поиска сущностей на концах ассоциации в модели. Свойства навигации могут быть созданы при создании ассоциации между двумя типами сущностей.

#### <a name="to-edit-navigation-properties"></a>Изменение свойств навигации

-   Выберите свойство навигации в области конструктора EF.
    Сведения о свойстве навигации отображаются в окне свойств Visual Studio **Properties**   .
-   Измените параметры свойств в окне **Свойства**   .

#### <a name="to-delete-navigation-properties"></a>Удаление свойств навигации

-   Если внешние ключи не представлены в типах сущностей в концептуальной модели, то удаление свойства навигации может обеспечить перемещение соответствующей ассоциации только в одном направлении или сделать ее неперемещаемой.
-   Щелкните правой кнопкой мыши свойство навигации в области конструктора EF и выберите **Удалить**.
