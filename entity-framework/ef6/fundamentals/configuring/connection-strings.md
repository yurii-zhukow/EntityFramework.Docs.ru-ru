---
title: Строки подключения и модели — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415955"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="2e375-102">Строки подключения и модели</span><span class="sxs-lookup"><span data-stu-id="2e375-102">Connection strings and models</span></span>
<span data-ttu-id="2e375-103">В этом разделе описано, как Entity Framework обнаруживает, какое подключение к базе данных следует использовать, и как его можно изменить.</span><span class="sxs-lookup"><span data-stu-id="2e375-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="2e375-104">В этом разделе рассматриваются модели, созданные с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="2e375-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="2e375-105">Обычно Entity Framework приложение использует класс, производный от DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="2e375-106">Этот производный класс будет вызывать один из конструкторов базового класса DbContext для управления:</span><span class="sxs-lookup"><span data-stu-id="2e375-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="2e375-107">Как контекст будет подключаться к базе данных, то есть как строка подключения будет найдена или использована</span><span class="sxs-lookup"><span data-stu-id="2e375-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="2e375-108">Будет ли контекст использовать вычисление модели с помощью Code First или загрузить модель, созданную с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="2e375-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="2e375-109">Дополнительные дополнительные параметры</span><span class="sxs-lookup"><span data-stu-id="2e375-109">Additional advanced options</span></span>  

<span data-ttu-id="2e375-110">В следующих фрагментах показаны некоторые способы использования конструкторов DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="2e375-111">Использование Code First с подключением по соглашению</span><span class="sxs-lookup"><span data-stu-id="2e375-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="2e375-112">Если вы еще не выполнили какие-либо другие настройки в приложении, то вызов конструктора без параметров в DbContext приведет к запуску DbContext в режиме Code First с подключением к базе данных, созданным по соглашению.</span><span class="sxs-lookup"><span data-stu-id="2e375-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="2e375-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-113">For example:</span></span>  

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

<span data-ttu-id="2e375-114">В этом примере DbContext использует полное имя пространства имен производного класса контекста — Demo. EF. BloggingContext — в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express или LocalDB.</span><span class="sxs-lookup"><span data-stu-id="2e375-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="2e375-115">Если установлены оба варианта, будет использоваться SQL Express.</span><span class="sxs-lookup"><span data-stu-id="2e375-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="2e375-116">Visual Studio 2010 включает SQL Express по умолчанию, а Visual Studio 2012 и более поздней версии включает LocalDB.</span><span class="sxs-lookup"><span data-stu-id="2e375-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="2e375-117">Во время установки пакет NuGet EntityFramework проверяет, какой сервер базы данных доступен.</span><span class="sxs-lookup"><span data-stu-id="2e375-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="2e375-118">Пакет NuGet обновит файл конфигурации, задав сервер базы данных по умолчанию, который Code First использует при создании соединения по соглашению.</span><span class="sxs-lookup"><span data-stu-id="2e375-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="2e375-119">Если SQL Express работает, он будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="2e375-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="2e375-120">Если SQL Express недоступен, то вместо этого будет зарегистрирована LocalDB в качестве значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2e375-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="2e375-121">В файл конфигурации не вносятся изменения, если он уже содержит параметр для фабрики подключений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2e375-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="2e375-122">Использование Code First с подключением по соглашению и указанным именем базы данных</span><span class="sxs-lookup"><span data-stu-id="2e375-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="2e375-123">Если вы еще не выполнили какие-либо другие настройки в приложении, то вызов конструктора строк для DbContext с именем базы данных, которое вы хотите использовать, приведет к запуску DbContext в режиме Code First с подключением к базе данных, созданным по соглашению с базой данных Это имя.</span><span class="sxs-lookup"><span data-stu-id="2e375-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="2e375-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="2e375-125">В этом примере DbContext использует "Блоггингдатабасе" в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express (устанавливается вместе с Visual Studio 2010) или LocalDB (устанавливается вместе с Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="2e375-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="2e375-126">Если установлены оба варианта, будет использоваться SQL Express.</span><span class="sxs-lookup"><span data-stu-id="2e375-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="2e375-127">Использование Code First со строкой подключения в файле App. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="2e375-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="2e375-128">Вы можете указать строку подключения в файле App. config или Web. config.</span><span class="sxs-lookup"><span data-stu-id="2e375-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="2e375-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="2e375-130">Это простой способ указать DbContext использовать сервер базы данных, отличный от SQL Express или LocalDB. в приведенном выше примере указывается база данных SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="2e375-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="2e375-131">Если имя строки подключения совпадает с именем контекста (с квалификатором пространства имен или без него), то при использовании конструктора без параметров он будет найден с помощью DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="2e375-132">Если имя строки подключения отличается от имени контекста, можно указать DbContext использовать это соединение в режиме Code First, передав имя строки подключения конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="2e375-133">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="2e375-134">Кроме того, можно использовать форму "имя =\<имя строки подключения\>" для строки, передаваемой конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="2e375-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="2e375-136">Эта форма явно делает то, что строка подключения будет найдена в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="2e375-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="2e375-137">Если строка подключения с данным именем не найдена, будет выдано исключение.</span><span class="sxs-lookup"><span data-stu-id="2e375-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="2e375-138">База данных/Model First со строкой подключения в файле App. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="2e375-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="2e375-139">Модели, созданные с помощью конструктора EF, отличаются от Code First тем, что модель уже существует и не создается из кода при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="2e375-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="2e375-140">Модель обычно существует в файле EDMX в проекте.</span><span class="sxs-lookup"><span data-stu-id="2e375-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="2e375-141">Конструктор добавит строку подключения EF в файл App. config или Web. config.</span><span class="sxs-lookup"><span data-stu-id="2e375-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="2e375-142">Эта строка подключения является особой в том, что она содержит сведения о том, как найти информацию в файле EDMX.</span><span class="sxs-lookup"><span data-stu-id="2e375-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="2e375-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-143">For example:</span></span>  

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

<span data-ttu-id="2e375-144">Конструктор EF также создаст код, который сообщает DbContext использовать это соединение, передав имя строки подключения конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="2e375-145">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e375-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="2e375-146">DbContext знает о загрузке существующей модели (вместо использования Code First для ее вычисления из кода), так как строка подключения является строкой соединения EF, содержащей сведения о модели для использования.</span><span class="sxs-lookup"><span data-stu-id="2e375-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="2e375-147">Другие параметры конструктора DbContext</span><span class="sxs-lookup"><span data-stu-id="2e375-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="2e375-148">Класс DbContext содержит другие конструкторы и шаблоны использования, которые позволяют использовать некоторые более сложные сценарии.</span><span class="sxs-lookup"><span data-stu-id="2e375-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="2e375-149">Вот некоторые из них:</span><span class="sxs-lookup"><span data-stu-id="2e375-149">Some of these are:</span></span>  

- <span data-ttu-id="2e375-150">Класс Дбмоделбуилдер можно использовать для создания модели Code First без создания экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="2e375-151">Результатом этого является объект Дбмодел.</span><span class="sxs-lookup"><span data-stu-id="2e375-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="2e375-152">Затем этот объект Дбмодел можно передать в один из конструкторов DbContext, когда вы будете готовы создать экземпляр DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="2e375-153">Можно передать полную строку подключения в DbContext, а не только имя базы данных или строки подключения.</span><span class="sxs-lookup"><span data-stu-id="2e375-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="2e375-154">По умолчанию эта строка подключения используется с поставщиком System. Data. SqlClient. Это можно изменить, задав другую реализацию Иконнектионфактори на контекст. База данных. Дефаултконнектионфактори.</span><span class="sxs-lookup"><span data-stu-id="2e375-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="2e375-155">Можно использовать существующий объект DbConnection, передав его в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e375-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="2e375-156">Если объект Connection является экземпляром EntityConnection, то модель, указанная в соединении, будет использоваться вместо вычисления модели с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="2e375-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="2e375-157">Если объект является экземпляром какого-либо другого типа, например SqlConnection, то контекст будет использовать его для Code Firstного режима.</span><span class="sxs-lookup"><span data-stu-id="2e375-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="2e375-158">Вы можете передать существующий контекст ObjectContext в конструктор DbContext, чтобы создать DbContext, который создает оболочку для существующего контекста.</span><span class="sxs-lookup"><span data-stu-id="2e375-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="2e375-159">Это можно использовать для существующих приложений, использующих ObjectContext, которые хотят использовать преимущества DbContext в некоторых частях приложения.</span><span class="sxs-lookup"><span data-stu-id="2e375-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
