---
title: Параметры файла конфигурации — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414857"
---
# <a name="configuration-file-settings"></a>Параметры файла конфигурации
Entity Framework позволяет указать несколько параметров из файла конфигурации. В целом EF соответствует принципу "соглашение над конфигурацией". все параметры, обсуждаемые в этой записи, имеют поведение по умолчанию. вам нужно только изменить параметр, когда значение по умолчанию больше не соответствует вашим требованиям.  

## <a name="a-code-based-alternative"></a>Альтернатива на основе кода  

Все эти параметры также можно применить с помощью кода. Начиная с EF6 мы предоставили [конфигурацию на основе кода](code-based.md), которая предоставляет централизованный способ применения конфигурации из кода. До EF6 конфигурация по-прежнему может быть применена из кода, но для настройки различных областей необходимо использовать различные интерфейсы API. Параметр файла конфигурации позволяет легко изменять эти параметры во время развертывания без обновления кода.

## <a name="the-entity-framework-configuration-section"></a>Раздел конфигурации Entity Framework  

Начиная с EF 4.1, можно задать инициализатор базы данных для контекста с помощью раздела **appSettings** файла конфигурации. В EF 4,3 мы предоставили раздел Custom **entityFramework** для управления новыми параметрами. Entity Framework будет по-прежнему распознать инициализаторы баз данных, установленные с использованием старого формата, но мы рекомендуем по возможности перейти к новому формату.

Раздел **entityFramework** был автоматически добавлен в файл конфигурации проекта при установке пакета NuGet entityFramework.  

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

## <a name="connection-strings"></a>Строки соединения  

На [этой странице](~/ef6/fundamentals/configuring/connection-strings.md) содержатся дополнительные сведения о том, как Entity Framework определяет используемую базу данных, включая строки подключения в файле конфигурации.  

Строки подключения попадают в стандартный элемент **ConnectionString** и не нуждаются в разделе **entityFramework** .  

Модели на основе Code First используют обычные строки подключения ADO.NET. Пример:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Модели на основе конструктора EF используют специальные строки подключения EF. Пример:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Тип конфигурации на основе кода (EF6)  

Начиная с EF6, можно указать DbConfiguration для EF для использования в [конфигурации на основе кода](code-based.md) в приложении. В большинстве случаев не нужно указывать этот параметр, так как EF автоматически обнаружит ваш DbConfiguration. Дополнительные сведения о том, когда может потребоваться указать DbConfiguration в файле конфигурации, см. в разделе **Перемещение DbConfiguration** в [конфигурации на основе кода](code-based.md).  

Чтобы задать тип DbConfiguration, укажите полное имя типа сборки в элементе **кодеконфигуратионтипе** .  

> [!NOTE]
> Полное имя сборки — это полное имя пространства имен, за которым следует запятая, а затем сборка, в которой находится тип. При необходимости можно также указать версию сборки, язык и региональные параметры и токен открытого ключа.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Поставщики баз данных EF (EF6)  

До EF6 часть поставщика базы данных, относящаяся к Entity Framework, должна быть включена в состав основного поставщика ADO.NET. Начиная с EF6, относящиеся к EF части теперь управляются и регистрируются отдельно.  

Обычно вам не нужно регистрировать поставщиков самостоятельно. Обычно это происходит поставщиком при установке.  

Поставщики регистрируются путем включения элемента **поставщика** в дочерний раздел **providers** раздела **entityFramework** . Существует два обязательных атрибута для записи поставщика:  

- **invariantName** идентифицирует основного поставщика ADO.NET, к которому обращается этот поставщик EF  
- **Type** — это полное имя типа сборки для реализации поставщика EF  

> [!NOTE]
> Полное имя сборки — это полное имя пространства имен, за которым следует запятая, а затем сборка, в которой находится тип. При необходимости можно также указать версию сборки, язык и региональные параметры и токен открытого ключа.  

В качестве примера здесь приведена запись, созданная для регистрации поставщика SQL Server по умолчанию при установке Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Перехватчики (EF 6.1 и выше)  

Начиная с EF 6.1, можно регистрировать перехватчики в файле конфигурации. Перехватчики позволяют выполнять дополнительную логику, когда EF выполняет определенные операции, такие как выполнение запросов к базе данных, открытие подключений и т. д.  

Перехватчики регистрируются путем включения элемента **перехватчика** в дочерний раздел **перехватчиков** раздела **entityFramework** . Например, следующая конфигурация регистрирует встроенный перехватчик **датабаселогжер** , который будет записывать все операции базы данных в консоль.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Ведение журнала операций базы данных в файл (EF 6.1/назад)  

Регистрация перехватчиков с помощью файла конфигурации особенно полезна, если требуется добавить ведение журнала в существующее приложение, чтобы упростить отладку проблемы. **Датабаселогжер** поддерживает ведение журнала в файл, предоставляя имя файла в качестве параметра конструктора.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

По умолчанию файл журнала будет перезаписан новым файлом при каждом запуске приложения. Чтобы присоединиться к файлу журнала, если он уже существует, используйте нечто вроде:  

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

Дополнительные сведения о **датабаселогжер** и регистрации перехватчиков см. в записи блога [EF 6,1: Включение ведения журнала без](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)повторной компиляции.  

## <a name="code-first-default-connection-factory"></a>Фабрика подключений по умолчанию Code First  

Раздел конфигурации позволяет указать фабрику соединений по умолчанию, которую Code First следует использовать для определения местоположения базы данных, используемой для контекста. Фабрика соединений по умолчанию используется только в том случае, если строка подключения не была добавлена в файл конфигурации для контекста.  

При установке пакета для EF NuGet была зарегистрирована фабрика соединений по умолчанию, указывающая на SQL Express или LocalDB в зависимости от того, какая из них установлена.  

Чтобы задать фабрику подключений, укажите полное имя типа сборки в элементе **дефаултконнектионфактори** .  

> [!NOTE]
> Полное имя сборки — это полное имя пространства имен, за которым следует запятая, а затем сборка, в которой находится тип. При необходимости можно также указать версию сборки, язык и региональные параметры и токен открытого ключа.  

Ниже приведен пример настройки фабрики подключений по умолчанию.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

В приведенном выше примере для пользовательской фабрики требуется конструктор без параметров. При необходимости можно указать параметры конструктора с помощью элемента **Parameters** .  

Например, Склцеконнектионфактори, который входит в Entity Framework, требует указать неизменяемое имя поставщика для конструктора. Неизменяемое имя поставщика определяет версию SQL Compact, которую вы хотите использовать. Следующая конфигурация приведет к тому, что контексты будут использовать SQL Compact версии 4,0 по умолчанию.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Если не задать фабрику соединений по умолчанию, Code First использует Склконнектионфактори, указав `.\SQLEXPRESS`. Склконнектионфактори также имеет конструктор, позволяющий переопределять части строки подключения. Если вы хотите использовать SQL Server экземпляр, отличный от `.\SQLEXPRESS`, можно использовать этот конструктор для задания сервера.  

Следующая конфигурация приведет к тому, что Code First будет использовать **мидатабасесервер** для контекстов, для которых не задана явная строка подключения.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

По умолчанию предполагается, что аргументы конструктора имеют тип String. Для изменения этого атрибута можно использовать атрибут Type.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Инициализаторы баз данных  

Инициализаторы баз данных настраиваются отдельно для каждого контекста. Их можно задать в файле конфигурации с помощью элемента **context** . Этот элемент использует полное имя сборки для задания настраиваемого контекста.  

По умолчанию Code First контексты настроены для использования инициализатора CreateDatabaseIfNotExists. В элементе **context** имеется атрибут **дисабледатабасеинитиализатион** , который можно использовать для отключения инициализации базы данных.  

Например, следующая конфигурация отключает инициализацию базы данных для контекста блога. BlogContext, определенного в файле MyAssembly. dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Для задания пользовательского инициализатора можно использовать элемент **датабасеинитиализер** .  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Параметры конструктора используют тот же синтаксис, что и фабрики соединений по умолчанию.  

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

Можно настроить один из универсальных инициализаторов базы данных, которые включены в Entity Framework. Атрибут **Type** использует формат .NET Framework для универсальных типов.  

Например, при использовании Code First Migrations можно настроить автоматическую миграцию базы данных с помощью инициализатора `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>`.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
