---
title: Параметры файла конфигурации - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 88c2439f3a89c9fb9ee22f828789df4decf39cc5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996507"
---
# <a name="configuration-file-settings"></a>Параметры файла конфигурации
Entity Framework дает ряд параметров, чтобы указать в файле конфигурации. В целом EF следует принципу «соглашение относительно настройки»: все параметры, описываемые в этой записи имеют поведение по умолчанию, необходимо только волноваться об изменении параметра, когда значение по умолчанию больше не удовлетворяет вашим требованиям.  

## <a name="a-code-based-alternative"></a>Это альтернативное решение на уровне кода  

Все эти параметры можно также применять, используя код. Начиная с версии EF6, мы представили [конфигурация на основе кода](code-based.md), который предоставляет основные способы применения конфигурации из кода. До EF6 конфигурация может применяться по-прежнему из кода, но вам нужно использовать различные API-интерфейсы для настройки различных областей. Параметр файла конфигурации позволяет легко изменять во время развертывания без обновления кода эти параметры.

## <a name="the-entity-framework-configuration-section"></a>Раздел конфигурации Entity Framework  

Начиная с EF4.1 удалось задать инициализации базы данных для контекста с помощью **appSettings** раздел файла конфигурации. В EF 4.3 мы представили пользовательский **entityFramework** разделе для обработки новых параметров. Платформа Entity Framework по-прежнему будет распознавать инициализаторы базы данных, заданные с помощью старого формата, но мы рекомендуем перейти в новый формат, где это возможно.

**EntityFramework** раздел был автоматически добавлен в файл конфигурации проекта при установке пакета EntityFramework NuGet.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Строки подключения  

[Эта страница](~/ef6/fundamentals/configuring/connection-strings.md) Дополнительные сведения о как Entity Framework определяет базу данных для использования, включая строки подключения в файле конфигурации.  

Строки подключения перейдите в стандарте **connectionStrings** элемента и не требуют **entityFramework** раздел.  

Модели кода, в первую очередь основано используйте обычные строки подключения ADO.NET. Пример:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Конструктор EF на основе строки подключения специальные EF моделей использования. Пример:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Тип конфигурации на основе кода (для EF6 и выше)  

Начиная с EF6, можно указать DbConfiguration для Entity FRAMEWORK для [конфигурация на основе кода](code-based.md) в приложении. В большинстве случаев не нужно указать этот параметр, как EF автоматически обнаружит ваш DbConfiguration. Подробные сведения о при необходимо указать в файле config DbConfiguration **перемещение DbConfiguration** раздел [конфигурация на основе кода](code-based.md).  

Чтобы задать тип DbConfiguration, укажите имя типа с указанием сборки в **codeConfigurationType** элемент.  

> [!NOTE]
> Полное имя сборки — это полное имя пространства имен, используя запятую, затем сборку, тип находится в. При необходимости вы можете также указать версию сборки, язык и региональные параметры и маркер открытого ключа.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Поставщики EF баз данных (для EF6 и выше)  

До EF6 приходилось включаемое как часть основной поставщик ADO.NET Entity Framework части поставщика базы данных. Начиная с EF6, конкретных частей EF теперь управляемых и зарегистрировать отдельно.  

Обычно не нужно регистрировать поставщики самостоятельно. Это обычно выполняется поставщиком при его установке.  

Поставщики регистрируются, включив **поставщика** элемента под **поставщики** дочернего **entityFramework** раздел. Существует два обязательных атрибута для запись поставщика:  

- **invariantName** определяет основной поставщик ADO.NET, целевые объекты поставщика EF  
- **Тип** — это имя типа с указанием сборки для реализации поставщика EF  

> [!NOTE]
> Полное имя сборки — это полное имя пространства имен, используя запятую, затем сборку, тип находится в. При необходимости вы можете также указать версию сборки, язык и региональные параметры и маркер открытого ключа.  

В качестве примера ниже приведен записи, созданной для регистрации поставщика SQL Server по умолчанию при установке платформы Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Перехватчики (EF6.1 и более поздние версии)  

Начиная с EF6.1 перехватчики можно зарегистрировать в файле конфигурации. Перехватчики позволяют запускать дополнительной логики, когда EF выполняет некоторые операции, такие как выполнение запросов к базе данных, открытия подключений и т. д.  

Перехватчики зарегистрированы, включая **перехватчик** элемента под **перехватчики** дочернего **entityFramework** раздел. Например, следующая конфигурация регистрирует встроенной **DatabaseLogger** перехватчика, которые регистрируются все операции с базой данных в консоль.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Ведение журнала операций базы данных в файл (EF6.1 и более поздние версии)  

Регистрация перехватчики через файл конфигурации особенно полезна при вы хотите добавить в существующее приложение для отладки проблемы ведения журнала. **DatabaseLogger** поддерживает ведение журнала в файл, указав имя файла в качестве параметра конструктора.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

По умолчанию в результате файл журнала перезапись файла каждый раз, когда приложение запускается. Для вместо добавления в журнал файла, если он уже существует рекомендуется использовать примерно следующим образом:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Дополнительные сведения о **DatabaseLogger** и регистрации перехватчики, см. в записи блога [EF 6.1: Включение ведения журнала без повторной компиляции](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Фабрика соединений по умолчанию первый код  

Раздел конфигурации позволяет указать фабрика соединений по умолчанию, который Code First следует использовать для поиска в базе данных для контекста. Фабрика соединений по умолчанию используется только в том случае, если строка подключения не был добавлен в файл конфигурации для контекста.  

При установке пакета EF NuGet фабрика соединений по умолчанию был зарегистрирован, указывающий на SQL Express или LocalDB, в зависимости от того, какой из них установки.  

Чтобы задать фабрику соединений, укажите имя типа с указанием сборки в **deafultConnectionFactory** элемент.  

> [!NOTE]
> Полное имя сборки — это полное имя пространства имен, используя запятую, затем сборку, тип находится в. При необходимости вы можете также указать версию сборки, язык и региональные параметры и маркер открытого ключа.  

Ниже приведен пример настройки собственной фабрики подключения по умолчанию:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Приведенный выше пример требует собственного производства, чтобы иметь конструктор без параметров. При необходимости можно указать с помощью параметров конструктора **параметры** элемент.  

Например SqlCeConnectionFactory, который включен в Entity Framework, необходимо предоставить неизменяемое имя поставщика в конструктор. Неизменяемое имя поставщика определяет версию SQL Compact, вы хотите использовать. Следующая конфигурация вызовет контекстов для использования SQL Compact версии 4.0 по умолчанию.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Если фабрика соединений по умолчанию не задан, Code First использует SqlConnectionFactory, указывающий на `.\SQLEXPRESS`. SqlConnectionFactory также имеет конструктор, который позволяет переопределить части строки подключения. Если вы хотите использовать экземпляр SQL Server, отличное от `.\SQLEXPRESS` этот конструктор можно использовать для настройки сервера.  

Следующая конфигурация приведет к Code First для использования **Сервер_базы_данных** для контекстов, не имеющие явную строку подключения значение.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

По умолчанию предполагается, что аргументы конструктора являются типом string. Чтобы изменить этот параметр можно использовать атрибут type.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Инициализаторы базы данных  

Инициализаторы базы данных настраиваются на основе каждого контекста. Их можно задать в файле конфигурации с помощью **контекст** элемент. Этот элемент используется полное имя сборки для определения контекста выполняется настройка.  

По умолчанию Code First контекстов настроены на использование инициализатора CreateDatabaseIfNotExists. Существует **disableDatabaseInitialization** атрибут **контекст** элемент, который может использоваться для отключения инициализация базы данных.  

Например следующая конфигурация отключает инициализацию базы данных для Blogging.BlogContext контексте, определенном в файле MyAssembly.dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Можно использовать **databaseInitializer** элемент для задания настраиваемого инициализатора.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Параметры конструктора используется тот же синтаксис, как фабрики подключения по умолчанию.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Можно настроить один инициализаторов универсальную базу данных, включенных в Entity Framework. **Тип** формата .NET Framework использует атрибут для универсальных типов.  

Например, при использовании Code First Migrations, можно настроить базу данных для миграции автоматически с помощью `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` инициализатора.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
