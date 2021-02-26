---
title: Пакеты NuGet EF Core
description: Общие сведения о различных пакетах NuGet Entity Framework Core
author: ajcvickers
ms.date: 01/21/2021
uid: core/what-is-new/nuget-packages
ms.openlocfilehash: 4b6e210f2324ea97e006d681d399bfdd6918d1b4
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100544587"
---
# <a name="ef-core-nuget-packages"></a>Пакеты NuGet EF Core

Entity Framework Core (EF Core) поставляется в виде пакетов [NuGet](https://www.nuget.org/). Пакеты, необходимые для приложения, зависят от следующего:

- тип используемой системы базы данных (SQL Server, SQLite и т. д.);
- требуемые функции EF Core.

Обычный процесс установки пакетов:

- Определите поставщик базы данных и установите соответствующий пакет ([см. ниже](#database-providers)).
- Также установите [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/) и [Microsoft.EntityFrameworkCore.Relational](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Relational/), если используется реляционный поставщик. Это помогает обеспечить использование последовательных версий, а также означает, что NuGet будет сообщать о предоставлении новых версий пакетов.
- При необходимости определите, какой тип инструментария необходим, и установите соответствующие пакеты (см. ниже).

Дополнительные сведения о начале работы с EF Core см. в [руководстве по началу работы с Entity Framework Core](xref:core/get-started/overview/first-app).

## <a name="package-versions"></a>Версии пакетов

Убедитесь, что установлена одна и та же версия всех пакетов EF Core, поставляемых корпорацией Майкрософт. Например, если установлена версия 5.0.3 пакета Microsoft.EntityFrameworkCore.SqlServer, то все остальные пакеты Microsoft.EntityFrameworkCore.* также должны быть версии 5.0.3.

Также убедитесь, что все внешние пакеты совместимы с используемой версией EF Core. В частности, убедитесь, что поставщик внешней базы данных поддерживает версию EF Core, которую вы используете. Для новых основных версий EF Core обычно требуется обновленный поставщик базы данных.

> [!WARNING]
> NuGet не применяет последовательных версий пакетов. Всегда тщательно проверяйте, на какие версии вы ссылаетесь в файле `csproj` или эквивалентном ему.

## <a name="database-providers"></a>Поставщики баз данных

EF Core поддерживает различные системы баз данных с помощью "поставщиков баз данных". Каждая система имеет собственный поставщик базы данных, который поставляется в качестве пакета NuGet. Приложения должны установить один или несколько из этих пакетов поставщика.

Общие поставщики баз данных перечислены в таблице ниже. Более полный список доступных поставщиков см. в разделе со сведениями о [поставщиках баз данных](xref:core/providers/index).

| Система базы данных                   | Пакет
|:----------------------------------|----------------------
| SQL Server и SQL Azure          | [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)
| SQLite                            | [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)
| Azure Cosmos DB                   | [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)
| PostgreSQL                        | [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL/)*
| MySQL                             | [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql/)*
| Выполняющаяся в памяти база данных EF Core**      | [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)

* Это популярные, высококачественные поставщики с открытым кодом, разработанные и поставляемые сообществом. Другие перечисленные поставщики поставляются корпорацией Майкрософт.

** Тщательно определите, следует ли использовать поставщик, который выполняется в памяти. Он не предназначен для использования в рабочей среде, а также не является [лучшим решением для тестирования](xref:core/testing/index).

## <a name="tools"></a>Инструменты

Использование средств для [миграции в EF Core](xref:core/managing-schemas/migrations/index) и [реконструирования (формирования шаблонов) из существующей базы данных](xref:core/managing-schemas/scaffolding) требует установки соответствующего пакета инструментов:

- [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) для инструментов PowerShell, который работает в [консоли диспетчера пакетов](/nuget/consume-packages/install-use-packages-powershell) Visual Studio
- [dotnet-ef](https://www.nuget.org/packages/dotnet-ef/) и [Microsoft.EntityFrameworkCore.Design](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Design/) для кроссплатформенных инструментов командной строки

Дополнительные сведения об использовании инструментов EF Core, в том числе о том, как правильно установить инструмент `dotnet-ef` в проекте или глобально, см. в [справочнике по инструментам Entity Framework Core](xref:core/cli/index).

> [!TIP]
> По умолчанию пакет Microsoft.EntityFrameworkCore.Design устанавливается таким образом, что он не будет развернут вместе с приложением. Это также означает, что его типы не могут транзитно использоваться в других проектах. Используйте обычный `PackageReference` в файле `.csproj` или эквивалентный, если требуется доступ к типам в пакете. Дополнительные сведения см. в разделе [Службы времени разработки](xref:core/cli/services).

## <a name="extension-packages"></a>Пакеты расширений

Существует множество [расширений для EF Core](xref:core/extensions/index), опубликованных корпорацией Майкрософт и сторонними производителями в виде пакетов NuGet. К часто используемым пакетам относятся следующие:

| функциональное назначение;                                | Пакет | Дополнительные зависимости
|:---------------------------------------------|---------|------------------------
| Прокси-серверы для отложенной загрузки и отслеживания изменений | [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) | [Castle.Core](https://www.nuget.org/packages/Castle.Core/)
| Пространственная поддержка для SQL Server               | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite/) and [NetTopologySuite.IO.SqlServerBytes](https://www.nuget.org/packages/NetTopologySuite.IO.SqlServerBytes/)
| Пространственная поддержка для SQLite                   | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite/) and [NetTopologySuite.IO.SpatiaLite](https://www.nuget.org/packages/NetTopologySuite.IO.SpatiaLite/)
| Пространственная поддержка для PostgreSQL               | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite) | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite/) и [NetTopologySuite.IO.PostGIS](https://www.nuget.org/packages/NetTopologySuite.IO.PostGIS/) (посредством [Npgsql.NetTopologySuite](https://www.nuget.org/packages/Npgsql.NetTopologySuite/))
| Пространственная поддержка MySQL                    | [Pomelo.EntityFrameworkCore.MySql.NetTopologySuite](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql.NetTopologySuite) | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite/)

## <a name="other-packages"></a>другие пакеты.

Другие пакеты EF Core извлекаются в качестве зависимостей пакета поставщика базы данных. Однако может потребоваться добавить явные ссылки на эти пакеты, чтобы NuGet отправлял уведомления при выпуске новых версий.

| функциональное назначение;                                              | Пакет
|:-----------------------------------------------------------|--------
| Основные функции EF Core                                | [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/)
| Общие функции реляционной базы данных                   | [Microsoft.EntityFrameworkCore.Relational](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Relational/)
| Упрощенный пакет для атрибутов EF Core и т. д.           | [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/)
| Анализаторы кода Roslyn для использования EF Core                    | [Microsoft.EntityFrameworkCore.Analyzers](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Analyzers/)
| Поставщик EF Core SQLite без собственной зависимости SQLite | [Microsoft.EntityFrameworkCore.Sqlite.Core](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.Core/)

## <a name="packages-for-database-provider-testing"></a>Пакеты для тестирования поставщика базы данных

Следующие пакеты используются для проверки поставщиков баз данных, созданных во внешних репозиториях GitHub. Примеры см. в разделах [efcore.pg](https://github.com/npgsql/efcore.pg) и [Pomelo.EntityFrameworkCore.MySql](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql). Приложения не должны устанавливать эти пакеты.

| функциональное назначение;                                              | Пакет
|:-----------------------------------------------------------|--------
| Тесты для любого поставщика базы данных                            | [Microsoft.EntityFrameworkCore.Specification.Tests](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Specification.Tests/)
| Тесты для поставщиков реляционных баз данных                    | [Microsoft.EntityFrameworkCore.Relational.Specification.Tests](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Relational.Specification.Tests/)

## <a name="obsolete-packages"></a>Устаревшие пакеты

**Не** устанавливайте следующие устаревшие пакеты и удалите их, если сейчас они установлены в проектах:

- Microsoft.EntityFrameworkCore.Relational.Design
- Microsoft.EntityFrameworkCore.Tools.DotNet
- Microsoft.EntityFrameworkCore.SqlServer.Design
- Microsoft.EntityFrameworkCore.Sqlite.Design
- Microsoft.EntityFrameworkCore.Relational.Design.Specification.Tests
