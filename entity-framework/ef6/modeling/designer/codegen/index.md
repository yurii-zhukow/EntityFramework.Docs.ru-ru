---
title: Шаблоны создания кода конструктора в EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
caps.latest.revision: 3
ms.openlocfilehash: e06dc1c35f8d74772e5c7d69b29553288fd652d0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911728"
---
# <a name="designer-code-generation-templates"></a>Шаблоны создания кода конструктора
При создании модели с помощью Entity Framework Designer классы и производный контекст создаются автоматически. Помимо создания кода по умолчанию, вам доступны несколько шаблонов, которые можно использовать для настройки генерируемого кода. Эти шаблоны представляют собой текстовые шаблоны T4, позволяющие при необходимости настраивать шаблоны.

Создаваемый по умолчанию код зависит от версии Visual Studio, в которой создается модель.

-   Модели, созданные в Visual Studio 2012 и 2013, будут создавать классы простых сущностей POCO и контекст, который является производным от упрощенного контекста DbContext.
-   Модели, созданные в Visual Studio 2010, будут создавать классы сущностей, которые являются производными от класса EntityObject, и контекст, который является производным от ObjectContext.
  > [!NOTE]    
  > После добавления модели рекомендуется переключиться на шаблон генератора DbContext.

На этой странице рассматриваются доступные шаблоны, а затем приводятся инструкции по добавлению шаблона в модель.

## <a name="available-templates"></a>Доступные шаблоны

Группа разработки Entity Framework предоставляет перечисленные ниже шаблоны.

### <a name="dbcontext-generator"></a>Генератор DbContext

Этот шаблон предназначен для создания классов простых сущностей POCO и контекста, который является производным от DbContext, с помощью EF6.
Это рекомендуемый шаблон, если только у вас нет причин для использования одного из других шаблонов, перечисленных ниже.
Этот шаблон создания кода предоставляется по умолчанию при использовании последних версий Visual Studio (Visual Studio 2013 и более поздние версии): при создании модели этот шаблон применяется по умолчанию, а файлы T4 (TT-файлы) вложены в EDMX-файл.

#### <a name="older-versions-of-visual-studio"></a>Предыдущие версии Visual Studio
- **Visual Studio 2012.** Для получения шаблонов **DbContextGenerator для EF 6.x** потребуется установить последнюю версию **инструментов Entity Framework для Visual Studio**. Дополнительные сведения см. на странице о [получении Entity Framework](~/ef6/fundamentals/install.md).
- **Visual Studio 2010.** Шаблоны **DbContextGenerator для EF 6.x** недоступны для Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Генератор DbContext для EF 5.x

При работе в более старой версии пакета NuGet для EntityFramework (с номером основной версии 5) необходимо использовать шаблон **генератора DbContext для EF 5.x**.

Если вы используете Visual Studio 2013 или 2012, этот шаблон уже установлен.

Если вы используете Visual Studio 2010, при добавлении шаблона необходимо выбрать вкладку **Online** (В Интернете), чтобы скачать его из коллекции Visual Studio. Или шаблон можно установить заранее непосредственно из коллекции Visual Studio. Так как шаблоны входят в состав более поздних версий Visual Studio, версии из коллекции устанавливаются только в Visual Studio 2010.

- [Генератор DbContext для EF 5.x для C#](http://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [Генератор DbContext для EF 5.x для веб-сайтов на C#](http://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [Генератор DbContext для EF 5.x для VB.NET](http://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [Генератор DbContext для EF 5.x для веб-сайтов на VB.NET](http://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Генератор DbContext для EF 4.x

Применяя более старую версию пакета NuGet для EntityFramework (с номером основной версии 4), необходимо использовать шаблон **Генератор DbContext для EF 4.x**. Его можно найти на вкладке **Online** (В сети) при добавлении шаблона, либо шаблон можно установить заранее непосредственно из коллекции Visual Studio.

- [Генератор DbContext для EF 4.x для C#](http://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [Генератор DbContext для EF 4.x для веб-сайтов на C#](http://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [Генератор DbContext для EF 4.x для VB.NET](http://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [Генератор DbContext для EF 4.x для веб-сайтов на VB.NET](http://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>Генератор EntityObject

Этот шаблон используется для создания классов сущностей, которые являются производными от класса EntityObject, и контекста, который является производным от ObjectContext.

> [!NOTE]
> Возможность использования шаблона "Генератор DbContext"

Сейчас генератор DbContext представляет собой рекомендуемый шаблон для новых приложений. Генератор DbContext использует преимущества более простого API DbContext. Генератор EntityObject по-прежнему доступен для поддержки существующих приложений.

**Visual Studio 2010, 2012 &amp; 2013**

При добавлении шаблона необходимо выбрать вкладку **Online** (В сети), чтобы скачать его из коллекции Visual Studio. Или шаблон можно установить заранее непосредственно из коллекции Visual Studio.

- [Генератор EntityObject для EF 6.x для C#](http://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Генератор EntityObject для EF 6.x для веб-сайтов на C#](http://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [Генератор EntityObject для EF 6.x для VB.NET](http://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [Генератор EntityObject для EF 6.x для веб-сайтов на VB.NET](http://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Генератор EntityObject для EF 5.x**


Если вы используете Visual Studio 2012 или 2013, при добавлении шаблона необходимо выбрать вкладку **Online** (В сети), чтобы скачать его из коллекции Visual Studio. Или шаблон можно установить заранее непосредственно из коллекции Visual Studio. Так как шаблоны входят в состав Visual Studio 2010, версии из коллекции устанавливаются только в Visual Studio 2012 &amp; 2013.

- [Генератор EntityObject для EF 5.x для C#](http://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Генератор EntityObject для EF 5.x для веб-сайтов на C#](http://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [Генератор EntityObject для EF 5.x для VB.NET](http://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [Генератор EntityObject для EF 5.x для веб-сайтов на VB.NET](http://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Если требуется просто создать код ObjectContext без изменения шаблона, вернитесь к [созданию кода EntityObject](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Если вы используете Visual Studio 2010, этот шаблон уже установлен. При создании модели в Visual Studio 2010 этот шаблон используется по умолчанию, но TT-файлы не включены в проект. Чтобы настроить шаблон, его следует добавить в проект.

### <a name="self-tracking-entities-ste-generator"></a>Генератор сущностей с самостоятельным отслеживанием (STE)

Этот шаблон предназначен для создания классов сущностей с самостоятельным отслеживанием и контекста, который является производным от ObjectContext. В приложении EF контекст объекта отвечает за отслеживание изменений в сущностях. Однако в многоуровневых сценариях на уровне, изменяющем сущности, контекст может быть недоступен. Сущности с самостоятельным отслеживанием позволяют отслеживать изменения на любом уровне. Дополнительные сведения см. в разделе [Сущности с самостоятельным отслеживанием](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Шаблон STE является нерекомендуемым

Больше не рекомендуется использовать шаблон STE в новых приложениях, но он по-прежнему будет доступен для поддержки существующих приложений. Другие варианты, применимые в многоуровневых сценариях, см. в статье об [отключенных сущностях](~/ef6/fundamentals/disconnected-entities/index.md).

> [!NOTE]
> У шаблона STE нет версии для EF 6.x.


> [!NOTE]
> У шаблона STE нет версии для Visual Studio 2013.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Если вы используете Visual Studio 2012, при добавлении шаблона необходимо выбрать вкладку **Online** (В Интернете), чтобы скачать его из коллекции Visual Studio. Или шаблон можно установить заранее непосредственно из коллекции Visual Studio. Так как шаблоны входят в состав Visual Studio 2010, версии из коллекции устанавливаются только в Visual Studio 2012.

- [Генератор STE для EF 5.x для C#](http://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [Генератор STE для EF 5.x для веб-сайтов на C#](http://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [Генератор STE для EF 5.x для VB.NET](http://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [Генератор STE для EF 5.x для веб-сайтов на VB.NET](http://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010**

Если вы используете Visual Studio 2010, этот шаблон уже установлен.

### <a name="poco-entity-generator"></a>Генератор сущностей POCO

Этот шаблон предназначен для создания классов сущностей POCO и контекста, который является производным от ObjectContext.

> [!NOTE]
> Возможность использования шаблона "Генератор DbContext"

Сейчас "Генератор DbContext" представляет собой рекомендуемый шаблон для создания классов POCO в новых приложениях. "Генератор DbContext" использует новый API DbContext и может создавать упрощенные классы POCO. "Генератор сущностей POCO" по-прежнему доступен для поддержки существующих приложений.

> [!NOTE]
> У шаблона STE нет версии для EF 5.x или EF 6.x.

> [!NOTE]
> У шаблона POCO нет версии для Visual Studio 2013.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

При добавлении шаблона необходимо выбрать вкладку **Online** (В сети), чтобы скачать его из коллекции Visual Studio. Или шаблон можно установить заранее непосредственно из коллекции Visual Studio.

- [Генератор POCO для EF 4.x для C#](http://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Генератор POCO для EF 4.x для веб-сайтов на C#](http://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [Генератор POCO для EF 4.x для VB.NET](http://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [Генератор POCO для EF 4.x для веб-сайтов на VB.NET](http://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Что такое шаблоны "Веб-сайты"

Шаблоны "Веб-сайты" (т. е. **Генератор DbContext для EF 5.x для веб-сайтов на C\#**) предназначены для использования в проектах по веб-сайтам, создаваемым при выборе **Файл —&gt; Создать —&gt; Веб-сайт**. Они отличаются от веб-приложений, создаваемых при выборе **Файл —&gt; Создать —&gt;Проект**  и использующих стандартные шаблоны. Для этих целей предоставляются отдельные шаблоны,которые требуются системе шаблонов элементов в Visual Studio.

## <a name="using-a-template"></a>Использование шаблона

Чтобы приступить к использованию шаблона создания кода, щелкните правой кнопкой мыши пустое место в области конструктора в конструкторе EF Designer и выберите **Добавить элемент создания кода…**.

![Add_Code_Gen_Item](~/ef6/media/add-code-gen-item.png)

Если вы уже установили нужный шаблон (или он был включен в состав Visual Studio), он будет доступен в разделе **Код** или **Данные** в меню слева.

![Установленные](~/ef6/media/installed.png)

Если шаблон еще не установлен, в левом меню выберите пункт **Online** (В Интернете) и найдите нужный шаблон.

![Поиск](~/ef6/media/search.png) 

Если вы используете Visual Studio 2012, новые TT-файлы будут вложены в EDMX-файл.*

> [!NOTE]
> Для моделей, созданных в Visual Studio 2012, потребуется удалить шаблоны, используемые для создания кода по умолчанию. В противном случае будут созданы повторяющиеся классы и контекст. Файлы по умолчанию: **&lt;имя_модели&gt;.tt** и **&lt;имя_модели&gt;.context.tt**. 

![VS2012_Templates](~/ef6/media/vs2012-templates.png)

Если вы используете Visual Studio 2010, TT-файлы добавляются непосредственно в проект.  

![VS2010_Templates](~/ef6/media/vs2010-templates.png)
