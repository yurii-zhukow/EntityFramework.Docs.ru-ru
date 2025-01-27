---
title: Разделение таблиц конструктора — EF6
description: Разделение таблиц конструктора в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/table-splitting
ms.openlocfilehash: 8f6db22913fb3ac8a4254c29b5f986eb6e14f18e
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066048"
---
# <a name="designer-table-splitting"></a>Разделение таблиц конструктора
В этом пошаговом руководстве показано, как сопоставлять несколько типов сущностей с одной таблицей путем изменения модели с помощью Entity Framework Designer (конструктор EF).

Одной из причин, по которой можно использовать разделение таблиц, является задержка загрузки некоторых свойств при использовании отложенной загрузки для загрузки объектов.Можно разделять свойства, которые могут содержать очень большой объем данных, в отдельную сущность и загружать их только при необходимости.

На следующем рисунке показаны основные окна, используемые при работе с конструктором EF.

![Конструктор EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Предварительные требования

Для выполнения данного пошагового руководства требуется:

- Последняя версия Visual Studio.
- [Образец базы данных School](xref:ef6/resources/school-database).

## <a name="set-up-the-project"></a>Настройка проекта

В этом пошаговом руководстве используется Visual Studio 2012.

-   Откройте Visual Studio 2012.
-   В меню **Файл** укажите **Создать**, затем нажмите **Проект**.
-   В левой области щелкните Visual C \# , а затем выберите шаблон Консольное приложение.
-   Введите **таблесплиттингсампле** в качестве имени проекта и нажмите кнопку **ОК**.

## <a name="create-a-model-based-on-the-school-database"></a>Создание модели на основе базы данных School

-   Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент**.
-   Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.
-   Введите **таблесплиттингмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.
-   В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее.**
-   Нажмите кнопку Создать соединение. В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB) \\ mssqllocaldb**), выберите метод проверки подлинности, введите **School**   в качестве имени базы данных и нажмите кнопку **ОК**.
    В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.
-   В диалоговом окне Выбор объектов базы данных unfold узел **таблицы**   и проверьте таблицу **Person** . Это приведет к добавлению указанной таблицы в модель **School** .
-   Нажмите кнопку **Готово**.

Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели. Все объекты, выбранные в диалоговом окне **Выбор объектов базы данных**   , добавляются в модель.

## <a name="map-two-entities-to-a-single-table"></a>Сопоставьте две сущности с одной таблицей

В этом разделе сущность **Person** будет разделяться на две сущности, а затем сопоставляться с одной таблицей.

> [!NOTE]
> Сущность **Person** не содержит свойств, которые могут содержать большой объем данных. Он просто используется в качестве примера.

-   Щелкните правой кнопкой мыши пустую область области конструктора, наведите указатель на пункт **Добавить новый**и выберите пункт **сущность**.
    Откроется диалоговое окно **создать сущность**   .
-   Введите **хиреинфо**   в качестве **имени сущности** и **PersonID** в качестве имени **ключевого свойства** .
-   Нажмите кнопку **ОК**.
-   Новый тип сущности будет создан и отобразится в области конструктора.
-   Выберите свойство **HireDate**    **Person**   типа сущности Person и нажмите клавиши **CTRL + X** .
-   Выберите **HireInfo**   сущность хиреинфо и нажмите клавиши **CTRL + V** .
-   Создание связи между **лицом** и **хиреинфо**. Для этого щелкните правой кнопкой мыши пустую область в области конструктора, наведите указатель на пункт **Добавить новый**и выберите пункт **Ассоциация**.
-    **Add Association**   Откроется диалоговое окно Добавление связи. Имя **персонхиреинфо** предоставляется по умолчанию.
-   Укажите кратность **1 (один)** на обоих концах связи.
-   Нажмите кнопку **ОК**.

Для следующего шага требуется окно **сведения о сопоставлении**   . Если это окно не отображается, щелкните правой кнопкой мыши область конструктора и выберите пункт **сведения о сопоставлении**.

-   Выберите **HireInfo**   тип сущности хиреинфо и нажмите кнопку ** &lt; Добавить таблицу или представление &gt; **   в окне **сведения о сопоставлении**   .
-   Выберите **пользователь** из раскрывающегося списка ** &lt; Добавить таблицу &gt; или представление**   . Список содержит таблицы или представления, к которым можно сопоставить выбранную сущность.
    Соответствующие свойства должны быть сопоставлены по умолчанию.

    ![Свойства сопоставления](~/ef6/media/mapping.png)

-   Выберите ассоциацию **персонхиреинфо** в области конструктора.
-   Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите пункт **Свойства**.
-   В окне **Свойства** выберите свойство **ссылочные ограничения** и нажмите кнопку с многоточием.
-   Выберите **Person** из раскрывающегося списка **основное** .
-   Нажмите кнопку **ОК**.

 

## <a name="use-the-model"></a>Использование модели

-   Вставьте следующий код в метод Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Скомпилируйте и запустите приложение.

Следующие инструкции T-SQL были выполнены в базе данных **School** в результате выполнения этого приложения. 

-   Следующая **инструкция INSERT** была выполнена в результате выполнения context. SaveChanges () и объединяет данные из сущностей **Person** и **хиреинфо**

    ![Вставка объединения данных Person и Хиреинфо](~/ef6/media/insert.png)

-   Следующий **SELECT** был выполнен в результате выполнения context. People. FirstOrDefault () и выбирает только столбцы, сопоставленные с **лицом**

    ![Выберите 1.](~/ef6/media/select1.png)

-   Следующий **SELECT** был выполнен в результате обращения к свойству навигации Ексистингперсон. Instructor и выбирает только столбцы, сопоставленные с **хиреинфо**

    ![Выберите 2](~/ef6/media/select2.png)
