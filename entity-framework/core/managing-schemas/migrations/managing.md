---
title: Управление миграцией — EF Core
author: bricelam
ms.author: bricelam
ms.date: 05/06/2020
uid: core/managing-schemas/migrations/managing
ms.openlocfilehash: e52d3680360a1e83e05f04650c735c5a67680094
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238738"
---
# <a name="managing-migrations"></a>Управление миграцией

При изменении модели миграция добавляется и удаляется в рамках обычной разработки, а файлы миграции возвращаются в систему управления версиями проекта. Для управления миграцией необходимо сначала установить [EF Core программ командной строки](xref:core/miscellaneous/cli/index).

> [!TIP]
> Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).

## <a name="add-a-migration"></a>Добавление миграции

После изменения модели можно добавить миграцию для этого изменения:

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddBlogCreatedTimestamp
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration AddBlogCreatedTimestamp
```

***

Имя миграции можно использовать как сообщение фиксации в системе управления версиями. Например, можно выбрать имя, например *аддблогкреатедтиместамп* , если изменение является новым `CreatedTimestamp` свойством `Blog` сущности.

В каталог **Migrations** проекта добавляются три файла.

* **XXXXXXXXXXXXXX_AddCreatedTimestamp. CS**— основной файл миграции. Содержит операции, необходимые для применения миграции (в `Up`) и ее отмены (в `Down`).
* **XXXXXXXXXXXXXX_AddCreatedTimestamp. Designer. CS**— файл метаданных миграции. Содержит сведения, используемые EF.
* **MyContextModelSnapshot.cs** — моментальный снимок текущей модели. Используется для определения появившихся изменений при добавлении следующей миграции.

Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.

### <a name="namespaces"></a>Пространства имен

Вы можете свободно перемещать файлы миграций и изменять их пространство имен вручную. Новые миграции создаются в виде элементов того же уровня, что и последняя миграция. Кроме того, можно указать пространство имен во время формирования следующим образом:

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --namespace Your.Namespace
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Namespace Your.Namespace
```

***

## <a name="customize-migration-code"></a>Настройка кода миграции

Хотя EF Core обычно создает точную миграцию, следует всегда проверять код и убедиться, что он соответствует требуемому изменению. в некоторых случаях это даже необходимо сделать.

### <a name="column-renames"></a>Переименование столбцов

Одним из важных примеров, где требуется настройка миграции, является переименование свойства. Например, если переименовать свойство из `Name` в `FullName` , EF Core создаст следующую миграцию:

```c#
migrationBuilder.DropColumn(
    name: "Name",
    table: "Customers");

migrationBuilder.AddColumn<string>(
    name: "FullName",
    table: "Customers",
    nullable: true);
```

EF Core, как правило, не может быть уверенным в том, что нужно удалить столбец и создать новый (два отдельных изменения) и когда столбец должен быть переименован. Если приведенная выше миграция применяется "как есть", все имена клиентов будут потеряны. Чтобы переименовать столбец, замените созданную выше миграцию следующей:

```c#
migrationBuilder.RenameColumn(
    name: "Name",
    table: "Customers",
    newName: "FullName");
```

> [!TIP]
> Процесс создания новой миграции предупреждает, если операция может привести к потере данных (например, содержит удаление столбца). Увидев это предупреждение, проверьте точность кода миграции с особой тщательностью.

### <a name="adding-raw-sql"></a>Добавление необработанного SQL

В то время как переименование столбца может быть выполнено через встроенный API, во многих случаях это невозможно. Например, может потребоваться заменить существующие `FirstName` `LastColumn` Свойства и одним новым `FullName` свойством. Процесс миграции, создаваемый EF Core, будет следующим:

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

Как и раньше, это вызовет нежелательные потери данных. Чтобы перенести данные из старых столбцов, мы переупорядочиваем миграцию и представляем необработанную операцию SQL следующим образом:

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

### <a name="arbitrary-changes-via-raw-sql"></a>Произвольные изменения через необработанный SQL

Необработанный SQL можно также использовать для управления объектами базы данных, которые EF Core не знают. Для этого добавьте миграцию, не внося изменения в модель. будет создана пустая миграция, которая затем может быть заполнена необработанными операциями SQL.

Например, следующая миграция создает SQL Server хранимую процедуру:

```c#
migrationBuilder.Sql(
@"
    EXEC ('CREATE PROCEDURE getFullName
        @LastName nvarchar(50),
        @FirstName nvarchar(50)
    AS
        RETURN @LastName + @FirstName;')");
```

Это можно использовать для управления любым аспектом базы данных, в том числе:

* Хранимые процедуры
* Полнотекстовый поиск
* Функции
* Триггеры
* Представления

В большинстве случаев EF Core будет автоматически переносить каждую миграцию в собственную транзакцию при применении миграции. К сожалению, некоторые операции миграции не могут быть выполнены внутри транзакции в некоторых базах данных. в таких случаях вы можете отказаться от транзакции, передав ее `suppressTransaction: true` `migrationBuilder.Sql` .

Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).

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

> [!WARNING]
> Не следует удалять все миграции, которые уже применены к рабочим базам данных. Если этого не сделать, вы не сможете отменить изменения и может привести к нарушению предположений, сделанных последующими миграциями.

## <a name="listing-migrations"></a>Перечисление миграций

Список всех существующих миграций можно просмотреть следующим образом:

```dotnetcli
dotnet ef migrations list
```

## <a name="resetting-all-migrations"></a>Сброс всех миграций

В некоторых крайних случаях может потребоваться удалить все миграции и начать заново. Это можно легко сделать, удалив папку **миграций** и удалив базу данных. на этом этапе можно создать новую начальную миграцию, которая будет содержать всю текущую схему.

Кроме того, можно сбросить все миграции и создать одну из них без потери данных. Это иногда называется "использование параметра squash" и требует выполнения некоторых действий вручную:

* Удаление папки **миграций**
* Создать новую миграцию и создать для нее скрипт SQL
* В базе данных удалите все строки из таблицы журнала миграции.
* Вставка одной строки в журнал миграции для записи того, что первая миграция уже применена, так как таблицы уже есть. СЕКЛ вставки — это последняя операция в созданном выше скрипте SQL.
