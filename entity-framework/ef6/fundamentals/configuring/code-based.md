---
title: Конфигурация на основе кода - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619289"
---
# <a name="code-based-configuration"></a><span data-ttu-id="2557e-102">Конфигурация на основе кода</span><span class="sxs-lookup"><span data-stu-id="2557e-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="2557e-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2557e-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="2557e-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="2557e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="2557e-105">Конфигурация приложения Entity Framework можно указать в файле конфигурации (app.config/web.config) или с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="2557e-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="2557e-106">Последний известен как конфигурация на основе кода.</span><span class="sxs-lookup"><span data-stu-id="2557e-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="2557e-107">Конфигурации в файле конфигурации описан в [отдельной статье](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="2557e-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="2557e-108">Файл конфигурации имеет приоритет над конфигурация на основе кода.</span><span class="sxs-lookup"><span data-stu-id="2557e-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="2557e-109">Другими словами Если параметр конфигурации в коде и в файле конфигурации, используется параметр в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="2557e-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="2557e-110">С помощью DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="2557e-110">Using DbConfiguration</span></span>  

<span data-ttu-id="2557e-111">Конфигурация на основе кода в EF6 и выше достигается путем создания подкласса System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="2557e-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="2557e-112">Следующие рекомендации необходимо соблюдать при подклассификация DbConfiguration:</span><span class="sxs-lookup"><span data-stu-id="2557e-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="2557e-113">Создайте только один класс DbConfiguration для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="2557e-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="2557e-114">Этот класс задает параметры на уровне домена приложения.</span><span class="sxs-lookup"><span data-stu-id="2557e-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="2557e-115">Поместите класс DbConfiguration в ту же сборку класса DbContext.</span><span class="sxs-lookup"><span data-stu-id="2557e-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="2557e-116">(См. в разделе *перемещение DbConfiguration* Если вы хотите изменить этот раздел.)</span><span class="sxs-lookup"><span data-stu-id="2557e-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="2557e-117">Присвойте классу DbConfiguration открытый конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="2557e-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="2557e-118">Задать параметры конфигурации, вызвав защищенных методов DbConfiguration в этот конструктор.</span><span class="sxs-lookup"><span data-stu-id="2557e-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="2557e-119">Следуя этим рекомендациям позволяет EF для обнаружения и автоматически использовать конфигурацию, оба средства, требуется доступ к модели и при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="2557e-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="2557e-120">Пример</span><span class="sxs-lookup"><span data-stu-id="2557e-120">Example</span></span>  

<span data-ttu-id="2557e-121">Класс, производный от DbConfiguration может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2557e-121">A class derived from DbConfiguration might look like this:</span></span>  

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

<span data-ttu-id="2557e-122">Этот класс задает EF для использования стратегии выполнения SQL Azure — автоматический повтор операций поврежденной базы данных - и локальная база данных для базы данных, которые создаются в соответствии с соглашением с Code First.</span><span class="sxs-lookup"><span data-stu-id="2557e-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="2557e-123">Перемещение DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="2557e-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="2557e-124">Бывают случаи, когда не может поместить класс DbConfiguration в ту же сборку класса DbContext.</span><span class="sxs-lookup"><span data-stu-id="2557e-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="2557e-125">Например в разных сборках имеется два класса DbContext.</span><span class="sxs-lookup"><span data-stu-id="2557e-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="2557e-126">Существует два варианта обработки такой.</span><span class="sxs-lookup"><span data-stu-id="2557e-126">There are two options for handling this.</span></span>  

<span data-ttu-id="2557e-127">Первый вариант — использовать файл конфигурации можно указать экземпляр DbConfiguration для использования.</span><span class="sxs-lookup"><span data-stu-id="2557e-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="2557e-128">Чтобы сделать это, установите атрибут codeConfigurationType разделе entityFramework.</span><span class="sxs-lookup"><span data-stu-id="2557e-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="2557e-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="2557e-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="2557e-130">Значение codeConfigurationType должен быть сборки и полное имя пространства имен класса DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="2557e-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="2557e-131">Второй вариант — разместить DbConfigurationTypeAttribute на класс контекста.</span><span class="sxs-lookup"><span data-stu-id="2557e-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="2557e-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="2557e-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="2557e-133">Значение, передаваемое в атрибут может быть ваш тип DbConfiguration — как показано выше - или сборки и пространства имен полное имя типа string.</span><span class="sxs-lookup"><span data-stu-id="2557e-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="2557e-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="2557e-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="2557e-135">Явно настроить параметр DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="2557e-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="2557e-136">Существуют ситуации, где конфигурации могут быть необходимы перед использует любого типа DbContext.</span><span class="sxs-lookup"><span data-stu-id="2557e-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="2557e-137">Примеры включают:</span><span class="sxs-lookup"><span data-stu-id="2557e-137">Examples of this include:</span></span>  

- <span data-ttu-id="2557e-138">С помощью DbModelBuilder для построения модели без контекста</span><span class="sxs-lookup"><span data-stu-id="2557e-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="2557e-139">С помощью другой служебная программа framework код, использующий DbContext, где используется этот контекст, прежде чем использовать контексте приложения</span><span class="sxs-lookup"><span data-stu-id="2557e-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="2557e-140">В таких ситуациях не удается автоматически обнаружить конфигурации EF и вместо этого необходимо выполнить одно из следующих:</span><span class="sxs-lookup"><span data-stu-id="2557e-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="2557e-141">Задайте для типа DbConfiguration в файле конфигурации, как описано в разделе *перемещение DbConfiguration* выше</span><span class="sxs-lookup"><span data-stu-id="2557e-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="2557e-142">Вызовите статический метод DbConfiguration.SetConfiguration во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="2557e-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="2557e-143">Переопределение DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="2557e-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="2557e-144">Существуют ситуации, когда необходимо переопределяют конфигурацию, заданную в DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="2557e-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="2557e-145">Обычно этого не сделать, разработчиками приложений, а скорее сторонних поставщиков и подключаемые модули, которые нельзя использовать в производном классе DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="2557e-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="2557e-146">Для этого EntityFramework позволяет обработчик событий для регистрации, можно изменить текущую конфигурацию, непосредственно перед ее блокировкой.</span><span class="sxs-lookup"><span data-stu-id="2557e-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="2557e-147">Он также предоставляет метод sugar специально для замены любая служба, возвращенный указатель служб EF.</span><span class="sxs-lookup"><span data-stu-id="2557e-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="2557e-148">Это, как он предназначен для использования:</span><span class="sxs-lookup"><span data-stu-id="2557e-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="2557e-149">При запуске приложения (до использования EF) подключаемого модуля или поставщик должен зарегистрировать метод обработчика событий для данного события.</span><span class="sxs-lookup"><span data-stu-id="2557e-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="2557e-150">(Обратите внимание, что это должно произойти, прежде чем приложение использует EF).</span><span class="sxs-lookup"><span data-stu-id="2557e-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="2557e-151">Обработчик событий вызывает ReplaceService для каждой службы, необходимо заменить.</span><span class="sxs-lookup"><span data-stu-id="2557e-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="2557e-152">Например для замены IDbConnectionFactory и DbProviderService зарегистрировать обработчик примерно так:</span><span class="sxs-lookup"><span data-stu-id="2557e-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="2557e-153">В коде выше MyProviderServices и MyConnectionFactory представляют реализации службы.</span><span class="sxs-lookup"><span data-stu-id="2557e-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="2557e-154">Можно также добавить обработчики дополнительную зависимость, чтобы получить тот же эффект.</span><span class="sxs-lookup"><span data-stu-id="2557e-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="2557e-155">Обратите внимание, что можно также поместить DbProviderFactory таким образом, но это таким образом влияют только на EF и не использования DbProviderFactory вне EF.</span><span class="sxs-lookup"><span data-stu-id="2557e-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="2557e-156">По этой причине, вероятно, стоит продолжить перенос DbProviderFactory, у вас есть до.</span><span class="sxs-lookup"><span data-stu-id="2557e-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="2557e-157">Следует также помнить служб, выполняемых извне приложения. Например, при выполнении миграции из консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="2557e-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="2557e-158">При выполнении миграции из консоли, предпринимается попытка найти ваш DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="2557e-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="2557e-159">Тем не менее, зависит от того ли он получит в оболочку службу о том, где он зарегистрирован обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="2557e-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="2557e-160">Если он зарегистрирован как часть конструкции вашего DbConfiguration код должен выполняться, и служба должны получить оболочку.</span><span class="sxs-lookup"><span data-stu-id="2557e-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="2557e-161">Обычно это работать не будет, и это означает, что инструменты не получить службу оболочку.</span><span class="sxs-lookup"><span data-stu-id="2557e-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
