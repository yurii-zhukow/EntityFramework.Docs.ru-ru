---
title: Обновление с EF Core 1.0 RC2 для RTM-версии — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949061"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Обновление с EF Core 1.0 RC2 до RTM

Эта статья содержит руководство по переносу приложения, созданного с помощью пакетов 1.0.0, версия-кандидат 2 RTM.

## <a name="package-versions"></a>Версии пакетов

Имена пакетов верхнего уровня, которые требуется установить в приложение обычно не изменился между RC2 и RTM.

**Вам потребуется обновить установленные пакеты до RTM-версии.**

* Пакеты среды выполнения (например, `Microsoft.EntityFrameworkCore.SqlServer`) изменилось с `1.0.0-rc2-final` для `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Пакета изменен с `1.0.0-preview1-final` для `1.0.0-preview2-final`. Обратите внимание, что оборудование по-прежнему предварительной версии.

## <a name="existing-migrations-may-need-maxlength-added"></a>Существующие миграции может потребоваться добавить maxLength

В версии-кандидата 2, определению столбца в миграции выглядело как `table.Column<string>(nullable: true)` и длину столбца было искать в некоторые метаданные, мы сохраняем в коде для миграции. В версии RTM, длина теперь входит в шаблонном коде `table.Column<string>(maxLength: 450, nullable: true)`.

Все существующие миграции, которые были скаффолдинга перед использованием RTM не будет иметь `maxLength` указан аргумент. Это означает, что будет использоваться максимальную длину, поддерживаемую в базе данных (`nvarchar(max)` на сервере SQL Server). Это может быть нормально для некоторых столбцов, но столбцы, являющиеся частью ключа, внешний ключ или индекс должен обновляться по максимальной длиной. По соглашению 450 — Максимальная длина для ключи, внешние ключи и индексированных столбцов. Если вы явным образом настроили длиной в модели, затем следует использовать этой длины, вместо этого.

**ASP.NET Identity**

Это изменение влияет на проекты, использующие ASP.NET Identity и были созданы из подготовительной-RTM шаблона проекта. Шаблон проекта включает миграции, используемый для создания базы данных. Этот вид миграции необходимо отредактировать для указания максимальной длиной `256` для следующих столбцов.

*  **AspNetRoles**

    * name

    * NormalizedName

*  **AspNetUsers**

   * Адрес эл. почты

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Не удалось внести это изменение приведет к следующее исключение при первоначальной миграции применяется к базе данных.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Удаление «import» в project.json

Если вы для .NET Core с помощью версии-кандидата 2, необходимо добавить `imports` в файл project.json в качестве временного решения для некоторых зависимостей EF Core не поддерживает .NET Standard. Они могут быть удален.

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
> Начиная с версии 1.0 RTM, [пакет SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` или разработки приложений .NET Core с помощью Visual Studio 2015. Корпорация Майкрософт рекомендует [выполнить миграцию project.json в формат csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Если вы используете Visual Studio, мы рекомендуем обновить до [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Добавить переадресации привязок

При попытке выполнения команд EF на результатов проектов универсальной платформы Windows (UWP) следующая ошибка:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Необходимо вручную добавить перенаправления привязки в проект универсальной платформы Windows. Создайте файл с именем `App.config` в проекте корневую папку и добавьте перенаправления правильные версии сборок.

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
