---
title: Разрешение зависимостей — EF6
description: Разрешение зависимостей в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/configuring/dependency-resolution
ms.openlocfilehash: 84f2391cdad974bb14aecd7e8650d46e78e47b75
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063313"
---
# <a name="dependency-resolution"></a>Разрешение зависимостей
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

Начиная с EF6, Entity Framework содержит универсальный механизм для получения необходимых реализаций служб. То есть, когда EF использует экземпляр некоторых интерфейсов или базовых классов, он запросит конкретную реализацию интерфейса или базового класса, который будет использоваться. Это достигается с помощью интерфейса Идбдепенденциресолвер:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Метод WebMethod обычно вызывается EF и обрабатывается реализацией Идбдепенденциресолвер, предоставляемой EF или приложением. При вызове аргумент типа является интерфейсом или типом базового класса запрашиваемой службы, а ключевой объект имеет значение null или объект, предоставляющий контекстные сведения о запрашиваемой службе.  

Если не указано иное, то возвращаемый объект должен быть потокобезопасным, так как он может использоваться как одноэлементный. Во многих случаях возвращаемый объект является фабрикой. в этом случае сама фабрика должна быть потокобезопасной, но объект, возвращаемый фабрикой, не обязательно должен быть потокобезопасным, так как новый экземпляр запрашивается из фабрики для каждого использования.

В этой статье не содержатся полные сведения о том, как реализовать Идбдепенденциресолвер, но он действует как ссылка на типы служб (то есть интерфейс и типы базовых классов), для которых EF вызывает GetObject и семантику ключевого объекта для каждого из этих вызовов.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System. Data. Entity. Идатабасеинитиализер<Тконтекст\>  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: инициализатор базы данных для данного типа контекста  

**Ключ**: не используется; будет иметь значение null  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func<System. Data. Entity. migrations. SQL. Мигратионсклженератор\>  

**Представленная версия**: EF 6.0.0

**Возвращенный объект**: фабрика для создания генератора SQL, который можно использовать для миграций и других действий, которые приводят к созданию базы данных, например для создания базы данных с инициализаторами баз данных.  

**Key**: строка, содержащая инвариантное имя поставщика ADO.NET, указывающее тип базы данных, для которой будет сформирован SQL. Например, для ключа "System. Data. SqlClient" возвращается генератор SQL Server SQL.  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="systemdataentitycorecommondbproviderservices"></a>System. Data. Entity. Core. Common. DbProviderServices  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: поставщик EF, используемый для неизменяемого имени поставщика  

**Key**: строка, содержащая инвариантное имя поставщика ADO.NET, указывающее тип базы данных, для которой требуется поставщик. Например, для ключа "System. Data. SqlClient" возвращается поставщик SQL Server.  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System. Data. Entity. Infrastructure. Идбконнектионфактори  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: фабрика соединения, которая будет использоваться, когда EF создает подключение к базе данных по соглашению. Это значит, что если соединение или строка подключения не переданы EF, а строка подключения не найдена в app.config или web.config, то эта служба используется для создания соединения по соглашению. Изменение фабрики соединений может позволить EF использовать базу данных другого типа (например, SQL Server Compact Edition) по умолчанию.  

**Ключ**: не используется; будет иметь значение null  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System. Data. Entity. Infrastructure. Иманифесттокенсервице  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: служба, которая может создать токен манифеста поставщика из соединения. Эта служба обычно используется двумя способами. Во-первых, ее можно использовать, чтобы избежать Code First соединения с базой данных при построении модели. Во-вторых, его можно использовать для принудительного Code First создания модели для конкретной версии базы данных, например, для принудительного применения модели для SQL Server 2005, даже если иногда используется SQL Server 2008.  

**Время существования объекта**: Singleton — один и тот же объект может использоваться в разных потоках несколько раз и одновременно.  

**Ключ**: не используется; будет иметь значение null  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System. Data. Entity. Infrastructure. Идбпровидерфакторисервице  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: служба, которая может получить фабрику поставщика из заданного соединения. В .NET 4,5 поставщик общедоступен из подключения. В .NET 4 реализация по умолчанию для этой службы использует некоторые эвристические методы для поиска соответствующего поставщика. В случае сбоя можно зарегистрировать новую реализацию этой службы, чтобы обеспечить соответствующее разрешение.  

**Ключ**: не используется; будет иметь значение null  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func<DbContext, System. Data. Entity. Infrastructure. Идбмоделкачекэй\>  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: фабрика, которая создаст ключ кэша модели для данного контекста. По умолчанию EF кэширует по одной модели для каждого типа DbContext на каждого поставщика. Другую реализацию этой службы можно использовать для добавления в ключ кэша других сведений, например имени схемы.  

**Ключ**: не используется; будет иметь значение null  

## <a name="systemdataentityspatialdbspatialservices"></a>System. Data. Entity. пространственный. Дбспатиалсервицес  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**. ПРОСТРАНСТВЕННЫЙ поставщик EF, который добавляет поддержку базового поставщика EF для пространственных и геометрических типов.  

**Ключ**: дбсптиалсервицес запрашивается двумя способами. Во-первых, пространственные службы, зависящие от поставщика, запрашиваются с помощью объекта Дбпровидеринфо (который содержит инвариантное имя и токен манифеста) в качестве ключа. Во вторых, Дбспатиалсервицес может запрашиваться без ключа. Используется для разрешения "глобального пространственного поставщика", который используется при создании изолированных типов заданное DbGeography или заданное DbGeometry.  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func<System. Data. Entity. Infrastructure. IDbExecutionStrategy\>  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: фабрика для создания службы, которая позволяет поставщику реализовать повторные попытки или другое поведение при выполнении запросов и команд в базе данных. Если реализация не предоставлена, EF будет просто выполнять команды и распространять все возникшие исключения. Для SQL Server эта служба используется для предоставления политики повтора, которая особенно полезна при запуске на облачных серверах баз данных, таких как SQL Azure.  

**Key**: объект ексекутионстратегикэй, содержащий инвариантное имя поставщика, и, при необходимости, имя сервера, для которого будет использоваться стратегия выполнения.  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func<DbConnection, String, System. Data. Entity. migrations. History. Хисториконтекст\>  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: фабрика, позволяющая поставщику настроить сопоставление хисториконтекст с `__MigrationHistory` таблицей, используемой в миграции EF. Хисториконтекст — это Code First DbContext. его можно настроить с помощью обычного API-интерфейса Fluent, чтобы изменить такие параметры, как имя таблицы и спецификации сопоставления столбцов.  

**Ключ**: не используется; будет иметь значение null  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="systemdatacommondbproviderfactory"></a>System. Data. Common. DbProviderFactory  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: поставщик ADO.NET, используемый для данного инвариантного имени поставщика.  

**Key**: строка, содержащая неизменяемое имя поставщика ADO.NET  

>[!NOTE]
> Эта служба обычно не изменяется напрямую, так как реализация по умолчанию использует обычную регистрацию поставщика ADO.NET. Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System. Data. Entity. Infrastructure. Ипровидеринвариантнаме  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: служба, используемая для определения инвариантного имени поставщика для данного типа DbProviderFactory. Реализация по умолчанию этой службы использует регистрацию поставщика ADO.NET. Это означает, что если поставщик ADO.NET не зарегистрирован обычным образом, поскольку DbProviderFactory разрешается EF, то он также потребуется для разрешения этой службы.  

**Key**: экземпляр DbProviderFactory, для которого требуется инвариантное имя.  

>[!NOTE]
> Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System. Data. Entity. Core. mapping. Виевженератион. Ивиевассембликаче  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: кэш сборок, содержащих предварительно созданные представления. Замена обычно используется, чтобы сообщить EF, какие сборки содержат предварительно созданные представления без выполнения обнаружения.  

**Ключ**: не используется; будет иметь значение null  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System. Data. Entity. Infrastructure. plural. Иплурализатионсервице

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: служба, используемая EF к именам создания множественного и единственного числа. По умолчанию используется служба множественного преобразования английского языка.  

**Ключ**: не используется; будет иметь значение null  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System. Data. Entity. Infrastructure. перехват. Идбинтерцептор  

**Представленная версия**: EF 6.0.0

**Возвращенные объекты**: любые перехватчики, которые должны быть зарегистрированы при запуске приложения. Обратите внимание, что эти объекты запрашиваются с помощью вызова служб и все перехватчики, возвращаемые любым сопоставительом зависимостей, будут зарегистрированы.

**Ключ**: не используется; будет иметь значение null.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func<System. Data. Entity. DbContext, действие<строка \> , System. Data. Entity. Infrastructure. перехват. датабаселогформаттер\>  

**Представленная версия**: EF 6.0.0  

**Возвращенный объект**: фабрика, которая будет использоваться для создания модуля форматирования журнала базы данных, который будет использоваться при создании контекста. Свойство базы данных. log задано для данного контекста.  

**Ключ**: не используется; будет иметь значение null.  

## <a name="funcsystemdataentitydbcontext"></a>Func<System. Data. Entity. DbContext\>  

**Представленная версия**: EF 6.1.0  

**Возвращенный объект**: фабрика, которая будет использоваться для создания экземпляров контекста для миграции, если контекст не имеет доступного конструктора без параметров.  

**Key**: тип объекта для типа производного DbContext, для которого требуется фабрика.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func<System. Data. Entity. Core. Metadata. EDM. Иметадатааннотатионсериализер\>  

**Представленная версия**: EF 6.1.0  

**Возвращенный объект**: фабрика, которая будет использоваться для создания сериализаторов для сериализации строго типизированных пользовательских заметок, чтобы их можно было сериализовать и ДЕСТЕРИЛИЗЕД в XML для использования в Code First migrations.  

**Key**: имя заметки, которая сериализуется или десериализуется.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func<System. Data. Entity. Infrastructure. Трансактионхандлер\>  

**Представленная версия**: EF 6.1.0  

**Возвращенный объект**: фабрика, которая будет использоваться для создания обработчиков транзакций, чтобы особая обработка могла применяться в таких ситуациях, как обработка ошибок фиксации.  

**Key**: объект ексекутионстратегикэй, содержащий инвариантное имя поставщика, и, при необходимости, имя сервера, для которого будет использоваться обработчик транзакций.  
