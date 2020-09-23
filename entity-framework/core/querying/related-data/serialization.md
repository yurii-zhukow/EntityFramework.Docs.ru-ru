---
title: Связанные данные и сериализация — EF Core
description: Сведения о том, как циклы в связанных данных Entity Framework Core могут влиять на платформы сериализации
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/serialization
ms.openlocfilehash: 6b28a1fc79a8c23e5249d36386c8ed06805dbe30
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078890"
---
# <a name="related-data-and-serialization"></a>Связанные данные и сериализация

Так как EF Core автоматически исправляет свойства навигации, вы можете завершить циклы в графе объектов. Например, загрузка блога и связанных записей приведет к созданию объекта блога, который ссылается на коллекцию записей. Каждая из этих записей будет ссылкой на блог.

Некоторые платформы сериализации не допускают такие циклы. Например Json.NET вызовет следующее исключение при обнаружении цикла.

> Newtonsoft.Json.JsonSerializationException: обнаружен самоссылающийся цикл для свойства "Blog" с типом "MyApplication.Models.Blog".

Если вы используете ASP.NET Core, вы можете настроить Json.NET для игнорирования циклов, которые он находит в графе объектов. Настройка осуществляется с помощью метода `ConfigureServices(...)` в `Startup.cs`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

Другой вариант — дополнение одного из свойств навигации атрибутом `[JsonIgnore]`, который указывает Json.NET игнорировать это свойство навигации во время сериализации.
