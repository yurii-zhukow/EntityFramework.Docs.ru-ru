---
title: Новые возможности EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136141"
---
# <a name="whats-new-in-ef6"></a>Новые возможности EF6

Корпорация Майкрософт настоятельно рекомендует использовать последнюю выпущенную версию Entity Framework, чтобы вы могли использовать новые функции с высочайшим уровнем надежности.
Тем не менее мы понимаем, что вам может потребоваться предыдущая версия или вы захотите поэкспериментировать с новыми усовершенствованиями в последнем предварительном выпуске.
Чтобы установить определенные версии EF, см. сведения в разделе о [получении Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-640"></a>EF 6.4.0

Среда выполнения EF 6.4.0 была выпущена в NuGet в декабре 2019 г. Основной целью EF 6.4 является представление улучшенных функций и сценариев, которые были выпущены в EF 6.3. См. [список важных исправлений](https://github.com/dotnet/ef6/milestone/14?closed=1) на сайте GitHub.

## <a name="ef-630"></a>EF 6.3.0

Среда выполнения EF 6.3.0 выпущена в NuGet в октябре 2019 г. Основной целью этого выпуска было упрощение переноса существующих приложений, использующих EF 6, в .NET Core 3.0. Сообществом также было внесено несколько исправлений и усовершенствований. См. сведения о проблемах, устраненных в каждой [контрольной точке](https://github.com/aspnet/EntityFramework6/milestones?state=closed) версии 6.3.0. Ниже перечислены некоторые основные моменты:

- Включена поддержка .NET Core 3.0.
  - Теперь пакет Entity Framework можно использовать для .NET Standard 2.1 наряду с .NET Framework 4.x.
  - Это означает, что платформа EF 6.3 является кроссплатформенной и поддерживается не только в Windows, но и в других операционных системах, таких как Linux и macOS.
  - Команды миграции были переписаны: теперь они поддерживают внепроцессное выполнение и проекты на базе пакетов SDK.
- Поддержка HierarchyId SQL Server.
- Улучшена совместимость с Roslyn и NuGet PackageReference.
- Добавлена служебная программа `ef6.exe` для включения, добавления, создания скриптов и применения миграций из сборок. Это замена для `migrate.exe`.

### <a name="ef-designer-support"></a>Поддержка конструктора EF

Сейчас использование конструктора EF непосредственно в проектах .NET Core, .NET Standard или проекте .NET Framework в стиле SDK не поддерживается. 

Это ограничение можно обойти, добавив EDMX-файл и созданные классы для сущностей, а также DbContext в качестве связанных файлов в проект .NET Core 3.0 или .NET Standard 2.1 в одном решении.

В файле проекта связанные файлы будут выглядеть так:

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

Обратите внимание, что EDMX-файл связан с действием сборки EntityDeploy. Это специальная задача MSBuild (теперь включена в пакет EF 6.3), которая добавляет модель EF в целевую сборку в качестве внедренных ресурсов (или копирует ее как файлы в выходную папку в зависимости от параметра обработки артефакта метаданных в EDMX). См. сведения об этом параметре в [примере EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).

Внимание! Убедитесь, что проект .NET Framework в старом стиле (т. е. не в стиле SDK), определяющий реальный EDMX-файл, _предшествует_ проекту, определяющему ссылку в SLN-файле. В противном случае при открытии EDMX-файла в конструкторе появится сообщение об ошибке: "The Entity Framework is not available in the target framework currently specified for the project. You can change the target framework of the project or edit the model in the XmlEditor" (Платформа Entity Framework недоступна на целевой платформе, указанной в настоящий момент для проекта. Вы можете изменить требуемую версию .NET Framework для проекта или изменить модель в XmlEditor.)

## <a name="past-releases"></a>Прошлые выпуски

На странице [прошлых выпусков](past-releases.md) содержится архив всех предыдущих версий EF и основных функций, которые были представлены в каждом выпуске.
