---
title: Службы времени разработки — EF Core
description: Сведения о Entity Framework Core служб времени разработки
author: bricelam
ms.date: 10/22/2020
uid: core/cli/services
ms.openlocfilehash: 07fc8a39fb106f357e135a17f6867ab338621910
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431355"
---
# <a name="design-time-services"></a>Службы времени разработки

Некоторые службы, используемые этими инструментами, используются только во время разработки. Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении. Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте реализацию `IDesignTimeServices` в запускаемый проект.

[!code-csharp[Main](../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs#DesignTimeServices)]

## <a name="referencing-microsoftentityframeworkcoredesign"></a>Ссылка на Microsoft. EntityFrameworkCore. Design

Microsoft. EntityFrameworkCore. Design — это пакет DevelopmentDependency. Это означает, что зависимость не будет передаваться в другие проекты и не может быть по умолчанию ссылаться на ее типы.

Чтобы сослаться на типы и переопределить службы времени разработки, обновите метаданные элемента PackageReference в файле проекта.

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.1.9">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Если пакет упоминается транзитным образом через Microsoft. EntityFrameworkCore. Tools, необходимо добавить явный PackageReference в пакет и изменить его метаданные.

## <a name="list-of-services"></a>Список служб

Ниже приведен список служб времени разработки.

Служба                                                                              | Описание
------------------------------------------------------------------------------------ | -----------
<xref:Microsoft.EntityFrameworkCore.Design.IAnnotationCodeGenerator>                 | Создает код для соответствующих заметок модели.
<xref:Microsoft.EntityFrameworkCore.Design.ICSharpHelper>                            | Помогает создавать код C#.
<xref:Microsoft.EntityFrameworkCore.Design.IPluralizer>                              | Перевод и сингуларизес слова.
<xref:Microsoft.EntityFrameworkCore.Migrations.Design.IMigrationsCodeGenerator>      | Создает код для миграции.
<xref:Microsoft.EntityFrameworkCore.Migrations.Design.IMigrationsScaffolder>         | Основной класс для управления файлами миграции.
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IDatabaseModelFactory>               | Создает модель базы данных на основе базы данных.
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IModelCodeGenerator>                 | Создает код для модели.
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IProviderConfigurationCodeGenerator> | Создает код для onconfiguring.
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IReverseEngineerScaffolder>          | Основной класс для формирования шаблонов реконструированных моделей.

## <a name="using-services"></a>Использование служб

Эти службы также могут быть полезны при создании собственных средств. Например, если необходимо автоматизировать часть рабочего процесса времени разработки.

Поставщик услуг, содержащий эти службы, можно создать с помощью методов расширения Аддентитифрамеворкдесигнтимесервицес и Адддбконтекстдесигнтимесервицес.

[!code-csharp[](../../../samples/core/Miscellaneous/CommandLine/CustomTools.cs#CustomTools)]
