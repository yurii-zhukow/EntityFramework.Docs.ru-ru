---
title: Миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: c87864b3430d3cd42729c13ddde33c0cd9de9308
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672987"
---
# <a name="migrations"></a>Миграции

Модель данных в процессе разработки может измениться и перестанет соответствовать базе данных. Вы всегда можете удалить базу данных, и EF создаст для вас новую версию, в точности соответствующую модели, но такая процедура приводит к потере текущих данных. Функция миграции в EF Core позволяет последовательно применять изменения схемы к базе данных, чтобы синхронизировать ее с моделью данных в приложении без потери существующих данных.

Миграции включают средства командной строки и API-интерфейсы, которые помогают в решении следующих задач:

* [Создание миграции](#create-a-migration). Создайте код, который обновляет базу данных для синхронизации с набором изменений модели данных.
* [Обновление базы данных](#update-the-database). Примените ожидающие миграции, чтобы обновить схему базы данных.
* [Настройка кода миграции](#customize-migration-code). Иногда созданный код приходится изменять или дополнять.
* [Удаление миграции](#remove-a-migration). Удалите созданный код.
* [Отмена миграции](#revert-a-migration). Отмените примененные к базе данных изменения.
* [Создание скриптов SQL](#generate-sql-scripts). Иногда бывает нужно создать скрипт для обновления рабочей базы данных или для устранения неполадок в коде миграции.
* [Применение миграции во время выполнения](#apply-migrations-at-runtime). Если обновления во время разработки и выполнение скриптов в вашем сценарии работы подходят плохо, вызовите метод `Migrate()`.

> [!TIP]
> Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).

## <a name="install-the-tools"></a>Установка инструментов

Установите [средства командной строки](xref:core/miscellaneous/cli/index).

* Для Visual Studio мы рекомендуем [Инструменты консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell).
* Для других сред разработки выберите [Средства интерфейса командной строки .NET Core](xref:core/miscellaneous/cli/dotnet).

## <a name="create-a-migration"></a>Создание миграции

После того, как вы [определите начальную модель](xref:core/modeling/index), можно переходить к созданию базы данных. Чтобы добавить первоначальную миграцию, выполните следующую команду.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

В каталог **Migrations** проекта добавляются три файла.

* **XXXXXXXXXXXXXX_InitialCreate.cs** — главный файл миграций. Содержит операции, необходимые для применения миграции (в `Up()`) и ее отмены (в `Down()`).
* **XXXXXXXXXXXXXX_InitialCreate.Designer.cs** — файл метаданных миграций. Содержит сведения, используемые EF.
* **MyContextModelSnapshot.cs** — моментальный снимок текущей модели. Используется для определения появившихся изменений при добавлении следующей миграции.

Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.

### <a name="namespaces"></a>Пространства имен

Вы можете свободно перемещать файлы миграций и изменять их пространство имен вручную. Новые миграции создаются в виде элементов того же уровня, что и последняя миграция.

Кроме того, можно использовать параметр `-Namespace` (консоль диспетчера пакетов) или `--namespace` (.NET Core CLI), чтобы указать пространство имен во время создания.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --namespace Your.Namespace
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Namespace Your.Namespace
```

***

## <a name="update-the-database"></a>Обновление базы данных

После этого примените миграцию к базе данных, чтобы создать схему.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a>Настройка кода миграции

После внесения изменений в модель EF Core может нарушиться синхронизация схемы базы данных. Чтобы сделать ее актуальной, добавьте еще одну миграцию. Имя миграции можно использовать как сообщение фиксации в системе управления версиями. Например, вы можете выбрать имя *AddProductReviews*, если суть изменений заключается в создании нового класса сущностей для обзоров.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

После создания кода миграции проверьте, правильно ли он создан. Если потребуется, добавьте, удалите или измените любые операции для правильного применения изменений.

Например, миграция может содержать следующие операции.

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Хотя эти операции обеспечивают совместимость схемы базы данных, они не сохраняют существующие имена клиентов. Чтобы улучшить миграцию, перепишите ее следующим образом.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> Процесс создания новой миграции предупреждает, если операция может привести к потере данных (например, содержит удаление столбца). Увидев это предупреждение, проверьте точность кода миграции с особой тщательностью.

Примените миграцию к базе данных с помощью соответствующей команды.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a>Пустые миграции

Иногда бывает удобно добавить миграцию, не внося изменения в модель. В этом случае при добавлении новой миграции создаются файлы кода с пустыми классами. Вы можете настроить ее для выполнения операций, которые не связаны напрямую с моделью EF Core. Ниже приведен ряд вещей, которыми вам может потребоваться управлять.

* Полнотекстовый поиск
* Функции
* Хранимые процедуры
* Триггеры
* Представления

## <a name="remove-a-migration"></a>Удаление миграции

Иногда, добавив миграцию, вы понимаем, что нужно внести дополнительные изменения в модель EF Core перед ее применением. Для удаления последней миграции используйте приведенную ниже команду.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Remove-Migration
```

***

Удалив миграцию, вы можете внести в модель дополнительные изменения и снова добавить ее.

## <a name="revert-a-migration"></a>Отмена миграции

Если вы уже применили одну миграцию для базы данных или несколько и хотите отменить их, можно использовать ту же команду, что и для применения миграций, но указав имя миграции, к которой надо откатить изменения.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a>Создание скриптов SQL

При отладке миграций или их развертывании в рабочей базе данных бывает полезно создать скрипт SQL. Такой скрипт можно дополнительно проверить на точность и настроить в соответствии с потребностями рабочей базы данных. Кроме того, его можно использовать в сочетании с технологией развертывания. Базовая команда имеет следующий вид.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a>Основное использование
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a>С from (в подразумеваемую миграцию)
Будет создан скрипт SQL из этой миграции в последнюю миграцию.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>С from и to
Будет создан скрипт SQL из миграции `from`в указанную миграцию `to`.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`. *Обязательно учитывайте возможные сценарии потери данных.*

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

#### <a name="basic-usage"></a>Основное использование
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a>С from (в подразумеваемую миграцию)
Будет создан скрипт SQL из этой миграции в последнюю миграцию.
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>С from и to
Будет создан скрипт SQL из миграции `from`в указанную миграцию `to`.
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`. *Обязательно учитывайте возможные сценарии потери данных.*

***

Для этой команды доступно несколько параметров.

Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта. Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).

Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта. По умолчанию она является последней миграцией в проекте.

При необходимости можно создать **идемпотентный** скрипт. Он применяет миграции только в том случае, если они еще не были применены к базе данных. Это удобно, если точно неизвестно, какая последняя миграция была применена к базе данных, или вы развертываете несколько баз данных, каждая из которых может иметь отдельную миграцию.

## <a name="apply-migrations-at-runtime"></a>Применение миграции во время выполнения

Некоторым приложениям может потребоваться применить миграции во время выполнения — при запуске или первом выполнении. Для этого можно использовать метод `Migrate()`.

Этот метод основан на службе `IMigrator`, которую можно применять в более сложных сценариях. Для доступа к нему используйте `myDbContext.GetInfrastructure().GetService<IMigrator>()`.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * Такой подход не является универсальным. Хотя он отлично подходит для приложений с локальной базой данных, большинству приложений требуется более надежная стратегия развертывания, такая как создание скриптов SQL.
> * Не вызывайте `EnsureCreated()` перед `Migrate()`. `EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.

## <a name="next-steps"></a>Следующие шаги

Для получения дополнительной информации см. <xref:core/miscellaneous/cli/index>.
