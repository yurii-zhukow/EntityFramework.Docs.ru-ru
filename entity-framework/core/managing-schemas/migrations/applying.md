---
title: Применение миграций — EF Core
description: Стратегии применения миграций схемы к базам данных в рабочей среде и разработке с помощью Entity Framework Core
author: bricelam
ms.date: 05/06/2020
uid: core/managing-schemas/migrations/applying
ms.openlocfilehash: cb4e6f719ba5ab4ef70e2e1d06760db1de5658b4
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543605"
---
# <a name="applying-migrations"></a>Применение миграций

После добавления миграций их необходимо развернуть и применить к базам данных. Для этого существуют различные стратегии, которые более подходят для рабочих сред, а другие — для жизненного цикла разработки.

> [!NOTE]
> Независимо от стратегии развертывания всегда следует проверять созданные миграции и тестировать их перед применением к рабочей базе данных. Миграция может привести к удалению столбца, когда намерение приходило переименовать его или может завершиться ошибкой по различным причинам при применении к базе данных.

## <a name="sql-scripts"></a>Скрипты SQL

Рекомендуемый способ развертывания миграций в рабочей базе данных — создание скриптов SQL. Ниже перечислены преимущества этой стратегии.

* Скрипты SQL могут быть проверены на точность; Это важно, поскольку применение изменений схемы к рабочим базам данных является потенциально опасной операцией, которая может привести к потере данных.
* В некоторых случаях сценарии могут быть настроены в соответствии с конкретными потребностями рабочей базы данных.
* Скрипты SQL можно использовать в сочетании с технологией развертывания и даже создавать в рамках процесса непрерывной интеграции.
* Скрипты SQL могут быть предоставлены администратору баз данных и могут управляться и архивироваться отдельно.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a>Основное использование

Следующая строка создает скрипт SQL из пустой базы данных в последнюю миграцию:

```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a>С from (в подразумеваемую миграцию)

В следующем примере создается скрипт SQL из данной миграции в последнюю миграцию.

```dotnetcli
dotnet ef migrations script AddNewTables
```

#### <a name="with-from-and-to"></a>С from и to

В следующем примере создается скрипт SQL из указанной `from` миграции в указанную `to` миграцию.

```dotnetcli
dotnet ef migrations script AddNewTables AddAuditTable
```

Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.

> [!WARNING]
> Обязательно учитывайте возможные сценарии потери данных.

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

#### <a name="basic-usage"></a>Основное использование

Следующая строка создает скрипт SQL из пустой базы данных в последнюю миграцию:

```powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a>С from (в подразумеваемую миграцию)

В следующем примере создается скрипт SQL из данной миграции в последнюю миграцию.

```powershell
Script-Migration AddNewTables
```

#### <a name="with-from-and-to"></a>С from и to

В следующем примере создается скрипт SQL из указанной `from` миграции в указанную `to` миграцию.

```powershell
Script-Migration AddNewTables AddAuditTable
```

Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`. *Обязательно учитывайте возможные сценарии потери данных.*

***

Создание скрипта принимает следующие два аргумента, указывающие, какой диапазон миграций должен быть создан:

* Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта. Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).
* Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта. По умолчанию она является последней миграцией в проекте.

## <a name="idempotent-sql-scripts"></a>Идемпотентными скрипты SQL

Приведенные выше скрипты SQL можно применять только для изменения схемы с одной миграции на другую. Вы обязаны правильно применять сценарий и только к базе данных в правильном состоянии миграции. EF Core также поддерживает создание сценариев **идемпотентными** , которые внутренне проверяют, какие миграции уже применены (с помощью таблицы журнала миграции), и применяют только недостающие. Это полезно, если вы не точно понимаете, какая последняя миграция была применена к базе данных, или если вы развертываете несколько баз данных, которые могут быть в разных переносах.

Ниже приводится пример идемпотентными миграций.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations script --idempotent
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Script-Migration -Idempotent
```

***

## <a name="command-line-tools"></a>Программы командной строки

Программы командной строки EF можно использовать для применения миграций к базе данных. При работе с локальной средой разработки и тестирования миграций этот подход не идеально подходит для управления производственными базами данных.

* Команды SQL применяются непосредственно средством, не давая разработчику возможность их исследовать или изменять. Это может быть опасно в рабочей среде.
* Пакет SDK для .NET и средство EF должны быть установлены на рабочих серверах.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

В следующем примере база данных обновляется до последней миграции:

```dotnetcli
dotnet ef database update
```

Следующий пример обновляет базу данных на заданную миграцию:

```dotnetcli
dotnet ef database update AddNewTables
```

Обратите внимание, что это можно использовать для отката к предыдущей миграции.

> [!WARNING]
> Обязательно учитывайте возможные сценарии потери данных.

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

В следующем примере база данных обновляется до последней миграции:

```powershell
Update-Database
```

Следующий пример обновляет базу данных на заданную миграцию:

```powershell
Update-Database AddNewTables
```

Обратите внимание, что это можно использовать для отката к предыдущей миграции.

> [!WARNING]
> Обязательно учитывайте возможные сценарии потери данных.

***

Дополнительные сведения о применении миграций с помощью программ командной строки см. в [справочнике по инструментам EF Core](xref:core/cli/index).

## <a name="apply-migrations-at-runtime"></a>Применение миграции во время выполнения

Приложение может применять миграцию программным способом, как правило, во время запуска. При работе с локальной разработкой и тестированием миграций этот подход не подходит для управления производственными базами данных по следующим причинам.

* Если выполняется несколько экземпляров приложения, то оба приложения могут попытаться одновременно применить миграцию и завершиться сбоем (или, что еще хуже, привести к повреждению данных).
* Аналогично, если приложение обращается к базе данных во время его миграции другим приложением, это может привести к серьезным проблемам.
* Приложение должно иметь повышенный уровень доступа для изменения схемы базы данных. Обычно рекомендуется ограничить разрешения базы данных приложения в рабочей среде.
* В случае возникновения проблемы важно иметь возможность отката примененной миграции. Другие стратегии позволяют легко и без труда использовать это окно.
* Команды SQL применяются непосредственно программой, не давая разработчику возможность их исследовать или изменять. Это может быть опасно в рабочей среде.

Чтобы программно применить миграции, вызовите `context.Database.Migrate()` . Например, типичное приложение ASP.NET может выполнять следующие действия:

```csharp
public static void Main(string[] args)
{
    var host = CreateHostBuilder(args).Build();

    using (var scope = host.Services.CreateScope())
    {
        var db = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
        db.Database.Migrate();
    }

    host.Run();
}
```

Обратите внимание, что `Migrate()` строится на основе `IMigrator` службы, которую можно использовать для более сложных сценариев. Для доступа к нему используйте `myDbContext.GetInfrastructure().GetService<IMigrator>()`.

> [!WARNING]
>
> * Перед использованием этого подхода в рабочей среде тщательно обсудите этот подход. Опыт показал, что простота этой стратегии развертывания определяется проблемами, которые она создает. Вместо этого рекомендуется создавать скрипты SQL на основе миграций.
> * Не вызывайте `EnsureCreated()` перед `Migrate()`. `EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.
