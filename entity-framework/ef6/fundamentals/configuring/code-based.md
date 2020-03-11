---
title: Конфигурация на основе кода — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414803"
---
# <a name="code-based-configuration"></a><span data-ttu-id="969f7-102">Конфигурация на основе кода</span><span class="sxs-lookup"><span data-stu-id="969f7-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="969f7-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="969f7-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="969f7-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="969f7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="969f7-105">Конфигурацию Entity Frameworkного приложения можно указать в файле конфигурации (App. config/Web. config) или с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="969f7-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="969f7-106">Последнее называется конфигурацией на основе кода.</span><span class="sxs-lookup"><span data-stu-id="969f7-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="969f7-107">Конфигурация в файле конфигурации описывается в [отдельной статье](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="969f7-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="969f7-108">Файл конфигурации имеет приоритет над конфигурацией на основе кода.</span><span class="sxs-lookup"><span data-stu-id="969f7-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="969f7-109">Иными словами, если параметр конфигурации задан как в коде, так и в файле конфигурации, используется параметр в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="969f7-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="969f7-110">Использование DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="969f7-110">Using DbConfiguration</span></span>  

<span data-ttu-id="969f7-111">Конфигурация на основе кода в EF6 и более поздних версиях достигается путем создания подкласса System. Data. Entity. config. DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="969f7-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="969f7-112">При создании подкласса DbConfiguration следует следовать приведенным ниже рекомендациям.</span><span class="sxs-lookup"><span data-stu-id="969f7-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="969f7-113">Создайте только один класс DbConfiguration для приложения.</span><span class="sxs-lookup"><span data-stu-id="969f7-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="969f7-114">Этот класс задает параметры уровня приложения для домена.</span><span class="sxs-lookup"><span data-stu-id="969f7-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="969f7-115">Поместите класс DbConfiguration в ту же сборку, что и класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="969f7-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="969f7-116">(Если вы хотите изменить этот параметр, см. раздел " *Перемещение DbConfiguration* ".)</span><span class="sxs-lookup"><span data-stu-id="969f7-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="969f7-117">Предоставьте классу DbConfiguration открытый конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="969f7-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="969f7-118">Задайте параметры конфигурации, вызвав защищенные методы DbConfiguration в этом конструкторе.</span><span class="sxs-lookup"><span data-stu-id="969f7-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="969f7-119">Следуя этим рекомендациям, платформа EF будет автоматически обнаруживать и использовать конфигурацию с помощью средств, которым требуется доступ к вашей модели и при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="969f7-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="969f7-120">Пример</span><span class="sxs-lookup"><span data-stu-id="969f7-120">Example</span></span>  

<span data-ttu-id="969f7-121">Класс, производный от DbConfiguration, может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="969f7-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="969f7-122">Этот класс настраивает EF для использования стратегии выполнения SQL Azure — для автоматической повторной попытки неудачных операций с базой данных, а также для использования локальной базы данных в целях создания по соглашению из Code First.</span><span class="sxs-lookup"><span data-stu-id="969f7-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="969f7-123">Перемещение DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="969f7-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="969f7-124">Бывают случаи, когда класс DbConfiguration не может быть размещен в той же сборке, что и класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="969f7-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="969f7-125">Например, у вас может быть два класса DbContext в разных сборках.</span><span class="sxs-lookup"><span data-stu-id="969f7-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="969f7-126">Существует два способа обработки этого.</span><span class="sxs-lookup"><span data-stu-id="969f7-126">There are two options for handling this.</span></span>  

<span data-ttu-id="969f7-127">Первый вариант — использовать файл конфигурации, чтобы указать используемый экземпляр DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="969f7-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="969f7-128">Для этого задайте атрибут Кодеконфигуратионтипе раздела entityFramework.</span><span class="sxs-lookup"><span data-stu-id="969f7-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="969f7-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="969f7-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="969f7-130">Значение Кодеконфигуратионтипе должно быть именем сборки и пространством имен класса DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="969f7-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="969f7-131">Второй вариант — поместить Дбконфигуратионтипеаттрибуте в класс контекста.</span><span class="sxs-lookup"><span data-stu-id="969f7-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="969f7-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="969f7-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="969f7-133">Значение, передаваемое в атрибут, может быть либо типом DbConfiguration, как показано выше, либо строкой с полным именем типа сборки и пространства имен.</span><span class="sxs-lookup"><span data-stu-id="969f7-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="969f7-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="969f7-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="969f7-135">Явное задание DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="969f7-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="969f7-136">Существуют ситуации, в которых может потребоваться настройка до использования любого типа DbContext.</span><span class="sxs-lookup"><span data-stu-id="969f7-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="969f7-137">К таким примерам относятся:</span><span class="sxs-lookup"><span data-stu-id="969f7-137">Examples of this include:</span></span>  

- <span data-ttu-id="969f7-138">Использование Дбмоделбуилдер для построения модели без контекста</span><span class="sxs-lookup"><span data-stu-id="969f7-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="969f7-139">Использование другого кода платформы или программы, использующего DbContext, где используется контекст, прежде чем будет использоваться контекст приложения</span><span class="sxs-lookup"><span data-stu-id="969f7-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="969f7-140">В таких ситуациях EF не может автоматически обнаружить конфигурацию, и вместо этого необходимо выполнить одно из следующих действий.</span><span class="sxs-lookup"><span data-stu-id="969f7-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="969f7-141">Задайте тип DbConfiguration в файле конфигурации, как описано в разделе *Перемещение DbConfiguration* выше.</span><span class="sxs-lookup"><span data-stu-id="969f7-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="969f7-142">Вызов статического метода DbConfiguration. Сетконфигуратион во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="969f7-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="969f7-143">Переопределение DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="969f7-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="969f7-144">В некоторых ситуациях необходимо переопределить набор конфигураций в DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="969f7-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="969f7-145">Обычно это не делается разработчиками приложений, а сторонними поставщиками и подключаемыми модулями, которые не могут использовать производный класс DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="969f7-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="969f7-146">Для этого EntityFramework позволяет зарегистрировать обработчик событий, который может изменить существующую конфигурацию непосредственно перед тем, как он будет заблокирован.</span><span class="sxs-lookup"><span data-stu-id="969f7-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="969f7-147">Он также предоставляет метод SugarCRM, специально предназначенный для замены любой службы, возвращенной локатором службы EF.</span><span class="sxs-lookup"><span data-stu-id="969f7-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="969f7-148">Вот как он предназначен для использования:</span><span class="sxs-lookup"><span data-stu-id="969f7-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="969f7-149">При запуске приложения (перед использованием EF) подключаемый модуль или поставщик должен зарегистрировать метод обработчика событий для этого события.</span><span class="sxs-lookup"><span data-stu-id="969f7-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="969f7-150">(Обратите внимание, что это должно произойти до того, как приложение использует EF.)</span><span class="sxs-lookup"><span data-stu-id="969f7-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="969f7-151">Обработчик событий вызывает Реплацесервице для каждой службы, которую необходимо заменить.</span><span class="sxs-lookup"><span data-stu-id="969f7-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="969f7-152">Например, чтобы заменить Идбконнектионфактори и Дбпровидерсервице, необходимо зарегистрировать обработчик примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="969f7-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="969f7-153">В приведенном выше коде Мипровидерсервицес и Миконнектионфактори представляют свои реализации службы.</span><span class="sxs-lookup"><span data-stu-id="969f7-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="969f7-154">Можно также добавить дополнительные обработчики зависимостей, чтобы получить тот же результат.</span><span class="sxs-lookup"><span data-stu-id="969f7-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="969f7-155">Обратите внимание, что можно также переносить DbProviderFactory таким образом, но это повлияет только на EF и не использует DbProviderFactory, находящиеся за пределами EF.</span><span class="sxs-lookup"><span data-stu-id="969f7-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="969f7-156">По этой причине вы, вероятно, захотите продолжить переносить DbProviderFactory, как было раньше.</span><span class="sxs-lookup"><span data-stu-id="969f7-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="969f7-157">Следует также помнить о службах, которые вы запускаете в приложении извне, например при выполнении миграций из консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="969f7-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="969f7-158">При выполнении миграции из консоли будет предпринята попытка найти DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="969f7-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="969f7-159">Тем не менее, независимо от того, получится ли служба в оболочке, зависит от того, где зарегистрирован обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="969f7-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="969f7-160">Если оно зарегистрировано как часть создания DbConfiguration, код должен выполняться и служба должна быть упакована.</span><span class="sxs-lookup"><span data-stu-id="969f7-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="969f7-161">Обычно это не так, и это означает, что средства не будут получать упакованную службу.</span><span class="sxs-lookup"><span data-stu-id="969f7-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
