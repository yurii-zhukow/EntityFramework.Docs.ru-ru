---
title: Поставщики Entity Framework в EF6
description: Использование поставщиков Entity Framework в Entity Framework 6
author: ajcvickers
ms.date: 06/27/2018
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: d53cc98471597e98d5f54024b87e1f238b31473b
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065411"
---
# <a name="entity-framework-6-providers"></a>Поставщики Entity Framework 6
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

Сейчас Entity Framework разрабатывается в рамках лицензии на ПО с открытым исходным кодом, поэтому EF6 и более поздние версии не будут поставляться в составе платформы .NET Framework. Такая ситуация дает множество преимуществ, но также требует, чтобы поставщики EF были перестроены в соответствии со сборками EF6. Это означает, что поставщики EF для EF5 и предыдущих версий будут работать с EF6 только после их перестроения.

## <a name="which-providers-are-available-for-ef6"></a>Какие поставщики доступны для EF6?

В число известных перестроенных для EF6 поставщиков входят следующие.

*   **Поставщик Microsoft SQL Server**
    *   Создан на [базе открытого исходного кода Entity Framework](https://github.com/aspnet/EntityFramework6).
    *   Поставляется в составе [пакета NuGet для EntityFramework](https://nuget.org/packages/EntityFramework).
*   **Поставщик Microsoft SQL Server Compact Edition**
    *   Создан на [базе открытого исходного кода Entity Framework](https://github.com/aspnet/EntityFramework6).
    *   Поставляется в [пакете NuGet EntityFramework.SqlServerCompact](https://nuget.org/packages/EntityFramework.SqlServerCompact).
*   [**Поставщики данных Devart dotConnect**](https://www.devart.com/dotconnect/)
    *   Существуют сторонние поставщики от компании [Devart](https://www.devart.com/) для различных баз данных, включая Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 и SQL Server.
*   [**Поставщики CData Software**](https://www.cdata.com/ado/)
    *   Существуют сторонние поставщики от [CData Software](https://www.cdata.com/ado/) для различных хранилищ данных, включая Salesforce, хранилище таблиц Azure, MySql и многие другие.
*   **Поставщик Firebird**
    *   Доступен как [пакет NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/).
*   **Поставщик Visual Fox Pro**
    *   Доступен как [пакет NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/).
*   **MySQL**
    *   [MySQL Connector/NET для Entity Framework](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql доступен как [пакет NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/).
*   **Oracle**
    *   ODP.NET доступен как [пакет NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/).
*   **SQLite**
    *   System.Data.SQLite доступен в виде [пакета NuGet](https://www.nuget.org/packages/System.Data.SQLite/)

Обратите внимание, что при включении в этот список не указывается уровень функциональности или поддержки для данного поставщика. Это значит только то, что стала доступной сборка для EF6.

## <a name="registering-ef-providers"></a>Регистрация поставщиков EF

Начиная с Entity Framework 6, поставщики EF можно регистрировать, используя либо конфигурацию на основе кода, либо файл конфигурации приложения.

### <a name="config-file-registration"></a>Регистрация в файле конфигурации

Регистрация поставщика EF в файлах app.config или web.config имеет следующий формат.


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Обратите внимание: часто, если поставщик EF устанавливается из пакета NuGet, пакет NuGet автоматически добавит эту регистрацию в файл конфигурации. При установке пакета NuGet в проект, который не является запускаемым проектом для вашего приложения, может потребоваться скопировать регистрацию в файл конфигурации для запускаемого проекта.

invariantName в этой регистрации является тем же инвариантным именем, которое используется для идентификации поставщика ADO.NET. Его можно найти в виде атрибута invariant в регистрации DbProviderFactory и в виде атрибута providerName в регистрации строки подключения. Используемое инвариантное имя также должно быть включено в документацию для поставщика. Примеры инвариантных имен: System.Data.SqlClient для SQL Server и System.Data.SqlServerCe.4.0 для SQL Server Compact.

type в данной регистрации — это имя с указанием сборки для типа поставщика, который является производным от System.Data.Entity.Core.Common.DbProviderServices. Например, для SQL Compact будет использоваться строка System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact. Используемый здесь тип должен быть включен в документацию по поставщику.

### <a name="code-based-registration"></a>Регистрация на основе кода

Начиная с Entity Framework 6, конфигурацию в масштабе приложения для EF можно указывать в коде. Все сведения см. в разделе _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_ (Конфигурация Entity Framework на основе кода). Стандартным способом регистрации поставщика EF с помощью конфигурации на основе кода является создание класса, производного от System.Data.Entity.DbConfiguration, и его размещение в одной сборке с классом DbContext. Затем класс DbConfiguration должен зарегистрировать поставщик в своем конструкторе. Например, чтобы зарегистрировать поставщик SQL Compact, класс DbConfiguration выглядит следующим образом.

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

В этом коде SqlCeProviderServices.ProviderInvariantName является удобным способом выражения строки инвариантного имени поставщика SQL Server Compact (System.Data.SqlServerCe.4.0), а SqlCeProviderServices.Instance возвращает единственный экземпляр поставщика SQL Compact EF.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Что делать, если нужный поставщик недоступен?

Если поставщик доступен в предыдущих версиях EF, мы рекомендуем обратиться к владельцу поставщика и попросить его создать версию EF6. Необходимо включить ссылку на [документацию к модели поставщика EF6](xref:ef6/fundamentals/providers/provider-model).

## <a name="can-i-write-a-provider-myself"></a>Можно ли создать поставщик самостоятельно?

Конечно, вы можете создать поставщик EF самостоятельно несмотря на то, что эта процедура не должна считаться тривиальной задачей. Лучше всего начать с приведенной выше ссылки о модели поставщика EF6. Кроме того, возможно, вы сочтете полезным использовать качестве отправной точки или в справочных целях код для поставщика SQL Server и SQL CE, включенный в [базу открытого исходного кода EF](https://github.com/aspnet/EntityFramework6).

Обратите внимание, что, начиная с EF6, поставщик EF не так тесно связан с поставщиком ADO.NET. Это упрощает создание поставщика EF без написания или упаковки классов ADO.NET.
