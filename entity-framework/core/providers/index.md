---
title: Поставщики базы данных — EF Core
author: ajcvickers
ms.date: 12/17/2019
uid: core/providers/index
ms.openlocfilehash: 7970636ab68b04d4d8583a4c126864ecc27ce6b2
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238168"
---
# <a name="database-providers"></a>Поставщики баз данных

Entity Framework Core поддерживает доступ к множеству разных баз данных с использованием библиотек подключаемых модулей, которые называются поставщиками баз данных.

## <a name="current-providers"></a>Текущие поставщики

> [!IMPORTANT]  
> Поставщики EF Core поступают из самых разных источников. Не все поставщики разрабатываются в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Выбирая поставщика, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям. Также обязательно ознакомьтесь подробными сведениями о совместимости версий, представленными в документации по каждому поставщику.

> [!IMPORTANT]  
> Поставщики EF Core обычно совместимы с дополнительными номерами версий, а не с основными. Например, поставщик, выпущенный для EF Core 2.1, должен работать с EF Core 2.2, но не будет работать с EF Core 3.0. 

| Пакет NuGet                                                                                                                                                                         | Поддерживаемые ядра СУБД | Программа обслуживания или поставщик                                                           | Примечания и требования     | Сборка для версии | Полезные ссылки                                                                                                                                                                                       |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:-------------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)                                                                     | SQL Server 2012 и последующие версии    | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) |                          | 3.1               | [Документы](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)                                                                           | SQLite 3.7 и выше         | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) |                          | 3.1               | [Документы](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)                                                                       | Выполняющаяся в памяти база данных EF Core | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) | [Ограничения](xref:core/miscellaneous/testing/in-memory)                 | 3.1               | [Документы](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)                                                                           | Azure Cosmos DB SQL API    | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) |                          | 3.1               | [Документы](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)                                                                         | PostgreSQL                 | [Команда разработчиков Npgsql](https://github.com/npgsql)                          |                          | 3.1               | [Документы](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                                                                                   | MySQL, MariaDB             | [Проект Pomelo Foundation](https://github.com/PomeloFoundation)              |                          | 3.1               | [Файл сведений](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                                                                                       | MySQL                      | [Проект MySQL](https://dev.mysql.com) (Oracle)                               |                          | 3.1               | [Документы](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                                                                                              | Oracle DB 11.2 и выше     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Бета-версия                     | 3.1               | [веб-сайт](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                                                                                  | MySQL 5 и выше            | [DevArt](https://www.devart.com/)                                             | Платный                     | 3.1               | [Документы](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                                                                                                | Oracle DB 9.2.0.4 и выше  | [DevArt](https://www.devart.com/)                                             | Платный                     | 3.1               | [Документы](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                                                                                        | PostgreSQL 8.0 и выше     | [DevArt](https://www.devart.com/)                                             | Платный                     | 3.1               | [Документы](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                                                                                                | SQLite 3 и выше           | [DevArt](https://www.devart.com/)                                             | Платный                     | 3.1               | [Документы](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/)                                                                  | Firebird 2.5 и 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                          | 3.1               | [Документы](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [IBM.EntityFrameworkCore](https://www-112.ibm.com/software/howtobuy/passportadvantage/paocustomer/sdma/SDMA?P0=DOWNLOAD_SEARCH_BY_PART_NO&FIELD_SEARCH_TYPE=3&searchVal=CC6XFML)      | Db2, Informix              | [IBM](https://ibm.com)                                                        | Платный, Windows            | 3.1               | [веб-сайт клиента](https://www.ibm.com/software/passportadvantage/pao_customer.html) |
| [IBM.EntityFrameworkCore-lnx](https://www-112.ibm.com/software/howtobuy/passportadvantage/paocustomer/sdma/SDMA?P0=DOWNLOAD_SEARCH_BY_PART_NO&FIELD_SEARCH_TYPE=3&searchVal=CC6XGML)  | Db2, Informix              | [IBM](https://ibm.com)                                                        | Платный, Linux              | 3.1               | [веб-сайт клиента](https://www.ibm.com/software/passportadvantage/pao_customer.html) |
| [IBM.EntityFrameworkCore-osx](https://www-112.ibm.com/software/howtobuy/passportadvantage/paocustomer/sdma/SDMA?P0=DOWNLOAD_SEARCH_BY_PART_NO&FIELD_SEARCH_TYPE=3&searchVal=CC6XHML)  | Db2, Informix              | [IBM](https://ibm.com)                                                        | Платный, macOS              | 3.1               | [веб-сайт клиента](https://www.ibm.com/software/passportadvantage/pao_customer.html) |
| [FileContextCore](https://www.nuget.org/packages/FileContextCore/)                                                                                                                    | Хранение данных в файлах       | [Моррис Янацек (Morris Janatzek)](https://github.com/morrisjdev)                              | Для разработки | 3.0               | [Файл сведений](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                                                                                    | Файлы Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework           | 2.2               | [Файл сведений](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)                                                                       | SQL Server Compact 3,5     | [Эрик Эйлсков Йенсен (Erik Ejlskov Jensen)](https://github.com/ErikEJ/)                             | .NET Framework           | 2.2               | [Вики-сайт](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)                                                                       | SQL Server Compact 4.0     | [Эрик Эйлсков Йенсен (Erik Ejlskov Jensen)](https://github.com/ErikEJ/)                             | .NET Framework           | 2.2               | [Вики-сайт](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [Teradata.EntityFrameworkCore](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                          | Teradata Database 16.10 и выше | [Teradata](https://downloads.teradata.com/download/connectivity/net-data-provider-for-teradata) |   | 2.2               |[веб-сайт](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                                                            |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                                                                                    | Firebird 2.5 и 3.x       | [Рафаэл Алмейда (Rafael Almeida)](https://github.com/ralmsdeveloper)                           |                          | 2.1               | [Вики-сайт](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                                                                                          | Ход выполнения OpenEdge          | [Алекс Вайс](https://github.com/alexwiese) (Alex Wiese)                                    |                          | 2.1               | [Файл сведений](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                                                                                   | Сервер MyCAT               | [Проект Pomelo Foundation](https://github.com/PomeloFoundation)              | Только в предварительном выпуске          | 1.1               | [Файл сведений](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |

## <a name="adding-a-database-provider-to-your-application"></a>Добавление поставщика базы данных в приложение

Большинство поставщиков баз данных для EF Core распространяется в виде пакетов NuGet. Их можно установить следующим образом:

## <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

После установки настройка поставщика осуществляется в `DbContext` с использованием либо метода `OnConfiguring`, либо метода `AddDbContext` (если применяется контейнер внедрения зависимостей).
Например, в следующей строке настраивается поставщик SQL Server с использованием переданной строки подключения:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Поставщики баз данных позволяют расширить возможности EF Core, реализуя уникальные функции для конкретных баз данных.
Некоторые концепции являются общими для большинства баз данных и включены в основной набор компонентов EF Core.
К ним относятся выражение запросов с помощью LINQ, транзакции и отслеживание изменений объектов при их загрузке из базы данных.
Некоторые концепции характерны для определенного поставщика.
Например, поставщик SQL Server позволяет [настроить таблицы, оптимизированные для памяти](xref:core/providers/sql-server/memory-optimized-tables) (функция, относящаяся к SQL Server).
Другие концепции характерны для класса поставщиков.
Например, поставщики EF Core для реляционных баз данных основаны на общей библиотеке `Microsoft.EntityFrameworkCore.Relational`, которая предоставляет API для настройки сопоставлений столбцов и таблиц, ограничения внешнего ключа и т. п. Поставщики обычно распространяются в виде пакетов NuGet.

> [!IMPORTANT]  
> Выпускаемые исправления EF Core часто содержат обновления пакета `Microsoft.EntityFrameworkCore.Relational`.
> При добавлении поставщика реляционной базы данных этот пакет становится транзитивной зависимостью вашего приложения.
> Тем не менее многие поставщики выпускаются независимо от EF Core и могут не обновляться при выпуске новых исправлений этого пакета.
> Чтобы гарантировать исправление всех обнаруженных ошибок, рекомендуется добавлять исправления `Microsoft.EntityFrameworkCore.Relational` в приложение в виде прямых зависимостей.
