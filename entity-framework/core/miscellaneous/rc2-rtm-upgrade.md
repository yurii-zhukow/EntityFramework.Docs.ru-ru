---
title: Обновление с EF Core RC2 1.0 до RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2018
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Обновление до RTM EF Core 1.0 RC2

Эта статья содержит руководство по переносу в приложении, построенном с пакетами RC2 для 1.0.0 RTM.

## <a name="package-versions"></a>Версии пакетов

Имена пакетов верхнего уровня, которые вы обычно устанавливаются в приложение, не изменился между RC2 и RTM.

**Необходимо обновить до RTM-версии установленных пакетов:**

* Среда выполнения пакетов (например `Microsoft.EntityFrameworkCore.SqlServer`) изменен с `1.0.0-rc2-final` для `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Пакета изменен с `1.0.0-preview1-final` для `1.0.0-preview2-final`. Обратите внимание, что инструментарий по-прежнему предварительного выпуска.

## <a name="existing-migrations-may-need-maxlength-added"></a>Существующие миграции может понадобиться добавить maxLength

В версии-кандидата 2, определение столбца при миграции выглядела `table.Column<string>(nullable: true)` и длина столбца было искать в некоторые метаданные хранятся в коде для миграции. В окончательной первоначальной версии, длина теперь включены в создании кода `table.Column<string>(maxLength: 450, nullable: true)`.

Любой существующий миграций, которые были формирования шаблонов перед использованием RTM не будет иметь `maxLength` указан аргумент. Это означает, что будет использоваться максимальную длину, поддерживаемую базой данных (`nvarchar(max)` на сервере SQL Server). Это может быть нормально для некоторых столбцов, но столбцы, являющиеся частью ключа, внешнего ключа или индекса должны быть обновлены для включения максимальную длину. По соглашению 450 имеет максимальную длину для ключей, внешние ключи и индексированных столбцов. Если длина настроены явным образом в модели, затем вам следует использовать заданной длины.

**ASP.NET Identity**

Это изменение влияет на проекты, использующие ASP.NET Identity и были созданы из предварительной-RTM шаблона проекта. Шаблон проекта включает миграции, используемый для создания базы данных. Этот вид миграции необходимо отредактировать для указания максимальной длиной `256` для следующих столбцов.

*  **AspNetRoles**

    * name

    * NormalizedName

*  **AspNetUsers**

   * Адрес эл. почты

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Чтобы внести это изменение приведет к следующее исключение при применении первоначальной миграции базы данных.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Удалите «imports» в project.json

Если нацеливания .NET Core с RC2, необходимо добавить `imports` в файл project.json в качестве временного решения для некоторых EF основных зависимостей, не поддерживая .NET Standard. Теперь можно извлечь их.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> Начиная с версии 1.0 RTM, [пакета SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` или при разработке приложений .NET Core с помощью Visual Studio 2015. Корпорация Майкрософт рекомендует [выполнить миграцию project.json в формат csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Если вы используете Visual Studio, рекомендуется обновить для [2017 г. Visual Studio](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Добавление переадресации привязок

При попытке выполнения команд EF на универсальной платформы Windows (UWP) проектов приводит к возникновению следующей ошибки:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Необходимо вручную добавить перенаправления привязки в проекте UWP. Создайте файл с именем `App.config` в проекте корневой папки и добавления перенаправления для правильные версии сборок.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
