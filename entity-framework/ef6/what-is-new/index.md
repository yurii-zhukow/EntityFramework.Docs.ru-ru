---
title: Новые возможности EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198037"
---
# <a name="whats-new-in-ef6"></a>Новые возможности EF6

Корпорация Майкрософт настоятельно рекомендует использовать последнюю выпущенную версию Entity Framework, чтобы вы могли использовать новые функции с высочайшим уровнем надежности.
Тем не менее мы понимаем, что вам может потребоваться предыдущая версия или вы захотите поэкспериментировать с новыми усовершенствованиями в последнем предварительном выпуске.
Чтобы установить определенные версии EF, см. сведения в разделе о [получении Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

Среда выполнения EF 6.3.0 выпущена в NuGet в октябре 2019 г. Основной целью этого выпуска было упрощение переноса существующих приложений, использующих EF 6, в .NET Core 3.0. Сообществом также было внесено несколько исправлений и усовершенствований. См. сведения о проблемах, устраненных в каждой [контрольной точке](https://github.com/aspnet/EntityFramework6/milestones?state=closed) версии 6.3.0. Ниже перечислены некоторые основные моменты:

- Включена поддержка .NET Core 3.0.
  - Теперь пакет EntityFramework можно использовать с .NET Standard 2.1 наряду с .NET Framework 4.x.
  - Команды миграции были переписаны для внепроцессного выполнения и поддержки проектов на основе пакета SDK.
- Включена поддержка SQL Server HierarchyId.
- Улучшена совместимость с Roslyn и NuGet PackageReference.
- Добавлена служебная программа `ef6.exe` для включения, добавления, создания скриптов и применения миграций из сборок. Это замена для `migrate.exe`.

### <a name="ef-designer-support"></a>Поддержка конструктора EF

Сейчас использование конструктора EF непосредственно в проектах .NET Core или .NET Standard не поддерживается. 

Это ограничение можно обойти, добавив EDMX-файл и созданные классы для сущностей, а также DbContext в качестве связанных файлов в проект .NET Core 3.0 или .NET Standard 2.1 в одном решении.

В файле проекта связанные файлы будут выглядеть так:

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

Обратите внимание, что EDMX-файл связан с действием сборки EntityDeploy. Это специальная задача MSBuild (теперь включена в пакет EF 6.3), которая добавляет модель EF в целевую сборку в качестве внедренных ресурсов (или копирует ее как файлы в выходную папку в зависимости от параметра обработки артефакта метаданных в EDMX). См. сведения об этом параметре в [примере EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).

## <a name="past-releases"></a>Прошлые выпуски

На странице [прошлых выпусков](past-releases.md) содержится архив всех предыдущих версий EF и основных функций, которые были представлены в каждом выпуске.
