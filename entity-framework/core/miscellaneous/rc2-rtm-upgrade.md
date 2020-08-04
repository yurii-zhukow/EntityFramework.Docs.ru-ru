---
title: Обновление с EF Core 1,0 RC2 до RTM — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: f496806ea6330c60cf43068882b7de839e18e383
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526775"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Обновление с EF Core 1,0 от RC2 до RTM

В этой статье приводятся рекомендации по перемещению приложения, созданного с помощью пакетов RC2, в 1.0.0 RTM.

## <a name="package-versions"></a>Версии пакета

Имена пакетов верхнего уровня, которые обычно устанавливаются в приложение, не изменились между RC2 и RTM.

**Необходимо обновить установленные пакеты до версий RTM:**

* Пакеты среды выполнения (например, `Microsoft.EntityFrameworkCore.SqlServer` ) изменились с `1.0.0-rc2-final` на `1.0.0` .

* `Microsoft.EntityFrameworkCore.Tools`Пакет изменен с `1.0.0-preview1-final` на `1.0.0-preview2-final` . Обратите внимание, что инструментарий по-прежнему является предварительным выпуском.

## <a name="existing-migrations-may-need-maxlength-added"></a>Для существующих миграций может потребоваться добавить maxLength

В RC2 определение столбца в процессе миграции выглядит так, как `table.Column<string>(nullable: true)` и длина столбца была просмотрена в некоторых метаданных, хранимых в коде для миграции. В RTM длина теперь включена в сформированный код `table.Column<string>(maxLength: 450, nullable: true)` .

Для всех существующих миграций, сформированных до использования RTM, не будет `maxLength` указан аргумент. Это означает, что будет использоваться максимальная длина, поддерживаемая базой данных ( `nvarchar(max)` на SQL Server). Это может быть удобно для некоторых столбцов, но столбцы, которые являются частью ключа, внешнего ключа или индекса, должны быть обновлены для включения максимальной длины. По соглашению 450 — это максимальная длина, используемая для ключей, внешних ключей и индексированных столбцов. Если вы явно настроили длину в модели, вместо нее следует использовать эту длину.

### <a name="aspnet-identity"></a>ASP.NET Identity

Это изменение влияет на проекты, использующие ASP.NET Identity и созданные из шаблона проекта предварительной версии RTM. Шаблон проекта включает миграцию, используемую для создания базы данных. Чтобы указать максимальную длину для следующих столбцов, необходимо изменить эту миграцию `256` .

* **аспнетролес**
  * name
  * нормализеднаме
* **AspNetUsers**
  * Адрес электронной почты
  * нормализедемаил
  * нормализедусернаме
  * UserName

Если не выполнить это изменение, при первоначальной миграции к базе данных будет получено следующее исключение.

``` Console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: удаление "Imports" в project.js

Если вы намерены ориентироваться на .NET Core с RC2, вам нужно было добавить `imports` project.jsв качестве временного решения для некоторых зависимостей EF Core, не поддерживающих .NET Standard. Теперь их можно удалить.

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
> Начиная с версии 1,0 RTM, [пакет SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` и не разрабатывает приложения .NET Core с помощью Visual Studio 2015. Корпорация Майкрософт рекомендует [выполнить миграцию project.json в формат csproj](/dotnet/articles/core/migration/). При использовании Visual Studio рекомендуется выполнить обновление до [Visual studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Добавление перенаправлений привязок

Попытка выполнить команды EF в проектах универсальная платформа Windows (UWP) приводит к следующей ошибке:

```output
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

Необходимо вручную добавить перенаправления привязок в проект UWP. Создайте файл с именем `App.config` в корневой папке проекта и добавьте перенаправления к правильным версиям сборки.

```xml
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
