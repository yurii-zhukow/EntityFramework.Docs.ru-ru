---
title: Поставщики базы данных — EF Core
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 6e39ded6e45f616e2080a23efff939e74de133cf
ms.sourcegitcommit: 4aaf6049521019c13594076fcd776feb8cd879c6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2018
---
# <a name="database-providers"></a>Поставщики баз данных

Entity Framework Core поддерживает доступ к множеству разных баз данных с использованием библиотек подключаемых модулей, которые называются поставщиками баз данных.

## <a name="current-providers"></a>Текущие поставщики
> [!IMPORTANT]  
> Поставщики EF Core поступают из самых разных источников. Не все поставщики разрабатываются в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Выбирая поставщика, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям. Также обязательно ознакомьтесь подробными сведениями о совместимости версий, представленными в документации по каждому поставщику.

| Пакет NuGet                                                                                                        | Поддерживаемые ядра СУБД | Программа обслуживания или поставщик                                                           | Примечания и требования             | Полезные ссылки                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 и выше    | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) |                                  | [Документы](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 и выше         | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) |                                  | [Документы](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Выполняющаяся в памяти база данных EF Core | [Проект EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Майкрософт) | Только для тестирования                 | [Документы](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)      | PostgreSQL                 | [Команда разработчиков Npgsql](https://github.com/npgsql)                          |                                  | [Документы](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Проект Pomelo Foundation](https://github.com/PomeloFoundation)              |                                  | [Файл сведений](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Сервер MyCAT               | [Проект Pomelo Foundation](https://github.com/PomeloFoundation)              | Предварительная версия до EF Core 1.1   | [Файл сведений](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Эрик Эйлсков Йенсен (Erik Ejlskov Jensen)](https://github.com/ErikEJ/)                             | .NET Framework                   | [Вики-сайт](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3,5     | [Эрик Эйлсков Йенсен (Erik Ejlskov Jensen)](https://github.com/ErikEJ/)                             | .NET Framework                   | [Вики-сайт](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Проект MySQL](http://dev.mysql.com) (Oracle)                                | Предварительная версия                      | [Документы](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                                                |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 и 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | EF Core 2.0 и более поздние версии, предварительный выпуск | [Блог](https://www.tabsoverspaces.com/233653-preview-of-entity-framework-core-2-0-support-for-firebird-and-firebirdclient-6-0/)                                                                    |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 и 3.x       | [Рафаэл Алмейда (Rafael Almeida)](https://github.com/ralmsdeveloper)                           | EF Core 2.0 и выше              | [Вики-сайт](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Версия Windows                  | [Блог](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Версия Linux                    | [Блог](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Версия macOS                    | [Блог](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 и выше     | [DevArt](https://www.devart.com/)                                             | Оплаченный                             | [Документы](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 и выше     | [DevArt](https://www.devart.com/)                                             | Оплаченный                             | [Документы](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 и выше           | [DevArt](https://www.devart.com/)                                             | Оплаченный                             | [Документы](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 и выше            | [DevArt](https://www.devart.com/)                                             | Оплаченный                             | [Документы](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Файлы Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | EF Core 2.0, .NET Framework      | [Файл сведений](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |

## <a name="future-providers"></a>Будущие поставщики

### <a name="cosmos-db"></a>Cosmos DB

Мы ведем разработку поставщика EF Core для API DocumentDB в Cosmos DB. Это будет первый разработанный нами полноценный поставщик базы данных, ориентированный на документы. Опираясь на полученный в ходе разработки опыт, мы будем вносить в проект дальнейшие изменения в версиях после 2.1. На данный момент планируется публикация ранней предварительной версии этого поставщика (2.1).

### <a name="oracle"></a>Oracle
Команда разработчиков Oracle .NET объявила о планах по выпуску основного поставщика для EF Core 2.0 приблизительно в третьем квартале 2018 года. Дополнительные сведения см. в соответствующем [заявлении о направлении разработки для .NET Core и Entity Framework Core](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf).
Направляйте любые вопросы об этом поставщике, включая сроки выпуска, на [веб-сайт сообщества Oracle](https://community.oracle.com/).

Тем временем команда разработчиков EF представила [образец поставщика EF Core для баз данных Oracle](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md). Этот проект не был предназначен для разработки поставщика EF Core, принадлежащего Майкрософт. Его целью было помочь нам выявить недостатки в реляционных и базовых функциях EF Core, которые необходимо устранить для улучшения поддержки Oracle, а также ускорить разработку других поставщиков Oracle для EF Core силами Oracle или других компаний.

Мы рассмотрим предложения по улучшению реализации этого образца. Мы также с радостью примем и поддержим усилия сообщества по созданию поставщика Oracle с открытым исходным кодом для EF Core на основе предлагаемого образца.

## <a name="adding-a-database-provider-to-your-application"></a>Добавление поставщика базы данных в приложение

Большинство поставщиков баз данных для EF Core распространяется в виде пакетов NuGet. Это значит, что для их установки можно использовать средство командной строки `dotnet`:

``` console
dotnet add package provider_package_name
```

В Visual Studio для этого также можно использовать консоль диспетчера пакетов NuGet:

``` powershell
install-package provider_package_name
```

После установки настройка поставщика осуществляется в `DbContext` с использованием либо метода `OnConfiguring`, либо метода `AddDbContext` (если применяется контейнер внедрения зависимостей). Например, в следующей строке настраивается поставщик SQL Server с использованием переданной строки подключения:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Поставщики баз данных позволяют расширить возможности EF Core, реализуя уникальные функции для конкретных баз данных. Некоторые концепции являются общими для большинства баз данных и включены в основной набор компонентов EF Core. К ним относятся выражение запросов с помощью LINQ, транзакции и отслеживание изменений объектов при их загрузке из базы данных. Некоторые концепции характерны для определенного поставщика. Например, поставщик SQL Server позволяет [настроить таблицы, оптимизированные для памяти](xref:core/providers/sql-server/memory-optimized-tables) (функция, относящаяся к SQL Server). Другие концепции характерны для класса поставщиков. Например, поставщики EF Core для реляционных баз данных основаны на общей библиотеке `Microsoft.EntityFrameworkCore.Relational`, которая предоставляет API для настройки сопоставлений столбцов и таблиц, ограничения внешнего ключа и т. п. Поставщики обычно распространяются в виде пакетов NuGet.

> [!IMPORTANT]  
> Выпускаемые исправления EF Core часто содержат обновления пакета `Microsoft.EntityFrameworkCore.Relational`. При добавлении поставщика реляционной базы данных этот пакет становится транзитивной зависимостью вашего приложения. Тем не менее многие поставщики выпускаются независимо от EF Core и могут не обновляться при выпуске новых исправлений этого пакета. Чтобы гарантировать исправление всех обнаруженных ошибок, рекомендуется добавлять исправления `Microsoft.EntityFrameworkCore.Relational` в приложение в виде прямых зависимостей.
