---
title: Строки подключения и модели — EF6
description: Строки подключения и модели в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
uid: ef6/fundamentals/configuring/connection-strings
ms.openlocfilehash: 2203d7f2168dc9d4ae5a6b1914742c7c2b6fbf77
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618438"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="ec7c7-103">Строки подключения и модели</span><span class="sxs-lookup"><span data-stu-id="ec7c7-103">Connection strings and models</span></span>
<span data-ttu-id="ec7c7-104">В этом разделе описано, как Entity Framework обнаруживает, какое подключение к базе данных следует использовать, и как его можно изменить.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-104">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="ec7c7-105">В этом разделе рассматриваются модели, созданные с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-105">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="ec7c7-106">Обычно Entity Framework приложение использует класс, производный от DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-106">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="ec7c7-107">Этот производный класс будет вызывать один из конструкторов базового класса DbContext для управления:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-107">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="ec7c7-108">Как контекст будет подключаться к базе данных, то есть как строка подключения будет найдена или использована</span><span class="sxs-lookup"><span data-stu-id="ec7c7-108">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="ec7c7-109">Будет ли контекст использовать вычисление модели с помощью Code First или загрузить модель, созданную с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="ec7c7-109">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="ec7c7-110">Дополнительные дополнительные параметры</span><span class="sxs-lookup"><span data-stu-id="ec7c7-110">Additional advanced options</span></span>  

<span data-ttu-id="ec7c7-111">В следующих фрагментах показаны некоторые способы использования конструкторов DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-111">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="ec7c7-112">Использование Code First с подключением по соглашению</span><span class="sxs-lookup"><span data-stu-id="ec7c7-112">Use Code First with connection by convention</span></span>  

<span data-ttu-id="ec7c7-113">Если вы еще не выполнили какие-либо другие настройки в приложении, то вызов конструктора без параметров в DbContext приведет к запуску DbContext в режиме Code First с подключением к базе данных, созданным по соглашению.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-113">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="ec7c7-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-114">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="ec7c7-115">В этом примере DbContext использует полное имя пространства имен производного класса контекста — Demo. EF. BloggingContext — в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express или LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-115">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="ec7c7-116">Если установлены оба варианта, будет использоваться SQL Express.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-116">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="ec7c7-117">Visual Studio 2010 включает SQL Express по умолчанию, а Visual Studio 2012 и более поздней версии включает LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-117">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="ec7c7-118">Во время установки пакет NuGet EntityFramework проверяет, какой сервер базы данных доступен.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-118">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="ec7c7-119">Пакет NuGet обновит файл конфигурации, задав сервер базы данных по умолчанию, который Code First использует при создании соединения по соглашению.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-119">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="ec7c7-120">Если SQL Express работает, он будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-120">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="ec7c7-121">Если SQL Express недоступен, то вместо этого будет зарегистрирована LocalDB в качестве значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-121">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="ec7c7-122">В файл конфигурации не вносятся изменения, если он уже содержит параметр для фабрики подключений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-122">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="ec7c7-123">Использование Code First с подключением по соглашению и указанным именем базы данных</span><span class="sxs-lookup"><span data-stu-id="ec7c7-123">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="ec7c7-124">Если вы не выполнили какие-либо другие настройки в приложении, то вызов конструктора строк для DbContext с именем базы данных, которое вы хотите использовать, приведет к запуску DbContext в режиме Code First с подключением к базе данных, созданным по соглашению с базой данных этого имени.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-124">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="ec7c7-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-125">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="ec7c7-126">В этом примере DbContext использует "Блоггингдатабасе" в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express (устанавливается вместе с Visual Studio 2010) или LocalDB (устанавливается вместе с Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="ec7c7-126">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="ec7c7-127">Если установлены оба варианта, будет использоваться SQL Express.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-127">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="ec7c7-128">Использование Code First со строкой подключения в файле app.config или web.config</span><span class="sxs-lookup"><span data-stu-id="ec7c7-128">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="ec7c7-129">Вы можете указать строку подключения в app.config или web.config файле.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-129">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="ec7c7-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-130">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="ec7c7-131">Это простой способ указать DbContext использовать сервер базы данных, отличный от SQL Express или LocalDB. в приведенном выше примере указывается база данных SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-131">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="ec7c7-132">Если имя строки подключения совпадает с именем контекста (с квалификатором пространства имен или без него), то при использовании конструктора без параметров он будет найден с помощью DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-132">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="ec7c7-133">Если имя строки подключения отличается от имени контекста, можно указать DbContext использовать это соединение в режиме Code First, передав имя строки подключения конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-133">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="ec7c7-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-134">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="ec7c7-135">Кроме того, можно использовать форму "Name = \<connection string name\> " для строки, передаваемой конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-135">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="ec7c7-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-136">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="ec7c7-137">Эта форма явно делает то, что строка подключения будет найдена в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-137">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="ec7c7-138">Если строка подключения с данным именем не найдена, будет выдано исключение.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-138">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="ec7c7-139">База данных/Model First со строкой подключения в файле app.config или web.config</span><span class="sxs-lookup"><span data-stu-id="ec7c7-139">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="ec7c7-140">Модели, созданные с помощью конструктора EF, отличаются от Code First тем, что модель уже существует и не создается из кода при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-140">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="ec7c7-141">Модель обычно существует в файле EDMX в проекте.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-141">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="ec7c7-142">Конструктор добавит строку подключения EF в app.config или web.config файл.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-142">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="ec7c7-143">Эта строка подключения является особой в том, что она содержит сведения о том, как найти информацию в файле EDMX.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-143">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="ec7c7-144">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-144">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="ec7c7-145">Конструктор EF также создаст код, который сообщает DbContext использовать это соединение, передав имя строки подключения конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-145">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="ec7c7-146">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-146">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="ec7c7-147">DbContext знает о загрузке существующей модели (вместо использования Code First для ее вычисления из кода), так как строка подключения является строкой соединения EF, содержащей сведения о модели для использования.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-147">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="ec7c7-148">Другие параметры конструктора DbContext</span><span class="sxs-lookup"><span data-stu-id="ec7c7-148">Other DbContext constructor options</span></span>  

<span data-ttu-id="ec7c7-149">Класс DbContext содержит другие конструкторы и шаблоны использования, которые позволяют использовать некоторые более сложные сценарии.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-149">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="ec7c7-150">Вот некоторые из них:</span><span class="sxs-lookup"><span data-stu-id="ec7c7-150">Some of these are:</span></span>  

- <span data-ttu-id="ec7c7-151">Класс Дбмоделбуилдер можно использовать для создания модели Code First без создания экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-151">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="ec7c7-152">Результатом этого является объект Дбмодел.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-152">The result of this is a DbModel object.</span></span> <span data-ttu-id="ec7c7-153">Затем этот объект Дбмодел можно передать в один из конструкторов DbContext, когда вы будете готовы создать экземпляр DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-153">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="ec7c7-154">Можно передать полную строку подключения в DbContext, а не только имя базы данных или строки подключения.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-154">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="ec7c7-155">По умолчанию эта строка подключения используется с поставщиком System. Data. SqlClient. Это можно изменить, задав другую реализацию Иконнектионфактори на контекст. База данных. Дефаултконнектионфактори.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-155">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="ec7c7-156">Можно использовать существующий объект DbConnection, передав его в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-156">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="ec7c7-157">Если объект Connection является экземпляром EntityConnection, то модель, указанная в соединении, будет использоваться вместо вычисления модели с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-157">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="ec7c7-158">Если объект является экземпляром какого-либо другого типа, например SqlConnection, то контекст будет использовать его для Code Firstного режима.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-158">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="ec7c7-159">Вы можете передать существующий контекст ObjectContext в конструктор DbContext, чтобы создать DbContext, который создает оболочку для существующего контекста.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-159">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="ec7c7-160">Это можно использовать для существующих приложений, использующих ObjectContext, которые хотят использовать преимущества DbContext в некоторых частях приложения.</span><span class="sxs-lookup"><span data-stu-id="ec7c7-160">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
