---
title: Строки подключения и модели — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: dce414ea84f13235691abf0dcadef5c743d90f9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994963"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="4a90c-102">Строки подключения и моделей</span><span class="sxs-lookup"><span data-stu-id="4a90c-102">Connection strings and models</span></span>
<span data-ttu-id="4a90c-103">В этом разделе рассматриваются как Entity Framework обнаруживает какие подключения к базе данных, и как его можно изменить.</span><span class="sxs-lookup"><span data-stu-id="4a90c-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="4a90c-104">Обе модели, созданные с помощью Code First и конструкторе EF рассматриваются в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="4a90c-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="4a90c-105">Обычно в приложении Entity Framework использует класс, производный от DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="4a90c-106">Этот производный класс будет вызовите один из конструкторов в базовом классе DbContext для элемента управления.</span><span class="sxs-lookup"><span data-stu-id="4a90c-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="4a90c-107">Как контекст будет подключаться к базе данных, то есть как строка подключения — обнаружении используется</span><span class="sxs-lookup"><span data-stu-id="4a90c-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="4a90c-108">Будет использовать контекст вычисления модели с помощью Code First или загрузить модели, созданной с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="4a90c-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="4a90c-109">Дополнительные параметры</span><span class="sxs-lookup"><span data-stu-id="4a90c-109">Additional advanced options</span></span>  

<span data-ttu-id="4a90c-110">Следующие фрагменты Показать некоторые из способов конструкторы DbContext может использоваться.</span><span class="sxs-lookup"><span data-stu-id="4a90c-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="4a90c-111">Использовать Code First с подключением по соглашению</span><span class="sxs-lookup"><span data-stu-id="4a90c-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="4a90c-112">Если вы не сделали любую другую конфигурацию в приложении, затем вызов конструктора без параметров для DbContext приведет к DbContext для запуска в режиме Code First с помощью подключения к базе данных, созданные по соглашению.</span><span class="sxs-lookup"><span data-stu-id="4a90c-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="4a90c-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-113">For example:</span></span>  

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

<span data-ttu-id="4a90c-114">В этом примере DbContext использует полное имя пространства имен вашего производного контекста class—Demo.EF.BloggingContext—as имя базы данных и создает строку подключения для этой базы данных с помощью SQL Express или LocalDB.</span><span class="sxs-lookup"><span data-stu-id="4a90c-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="4a90c-115">Если установлены оба, SQL Express будут использоваться.</span><span class="sxs-lookup"><span data-stu-id="4a90c-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="4a90c-116">Visual Studio 2010 включает в себя SQL Express по умолчанию и Visual Studio 2012 и более поздних версий LocalDB.</span><span class="sxs-lookup"><span data-stu-id="4a90c-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="4a90c-117">Во время установки пакета EntityFramework NuGet проверяет, какой сервер базы данных доступна.</span><span class="sxs-lookup"><span data-stu-id="4a90c-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="4a90c-118">Пакет NuGet затем обновит файл конфигурации, задав сервер базы данных по умолчанию, использующий Code First, при создании подключения по соглашению.</span><span class="sxs-lookup"><span data-stu-id="4a90c-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="4a90c-119">Если выполняется SQL Express, он будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="4a90c-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="4a90c-120">Если SQL Express не доступен затем LocalDB будет зарегистрировано по умолчанию вместо этого.</span><span class="sxs-lookup"><span data-stu-id="4a90c-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="4a90c-121">Нет изменения вносятся в файл конфигурации, если он уже содержит параметр для фабрики подключения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4a90c-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="4a90c-122">Использовать Code First с подключением, соглашение о вызовах и указанное имя базы данных</span><span class="sxs-lookup"><span data-stu-id="4a90c-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="4a90c-123">Если вы не сделали любую другую конфигурацию в приложении, последующим вызовом конструктор строк на DbContext с именем базы данных, которую вы хотите использовать приведет к DbContext для запуска в режиме Code First с помощью подключения к базе данных, созданные по соглашению к базе данных Это имя.</span><span class="sxs-lookup"><span data-stu-id="4a90c-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="4a90c-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="4a90c-125">В этом примере DbContext использует «BloggingDatabase» в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express (устанавливается с Visual Studio 2010) или LocalDB (устанавливается с Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="4a90c-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="4a90c-126">Если установлены оба, SQL Express будут использоваться.</span><span class="sxs-lookup"><span data-stu-id="4a90c-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="4a90c-127">Использование Code First, строкой подключения в файле app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="4a90c-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="4a90c-128">Вы можете поместить строку подключения в файле app.config или web.config.</span><span class="sxs-lookup"><span data-stu-id="4a90c-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="4a90c-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="4a90c-130">Это простой способ сообщить DbContext, чтобы использовать сервер базы данных, отличных от SQL Express или LocalDB, приведенном выше примере указывает базу данных SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="4a90c-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="4a90c-131">Если имя строки подключения совпадает с именем контекста (с или без квалификации пространства имен) затем он будет найден DbContext при использовании конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="4a90c-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="4a90c-132">Если имя строки подключения отличается от имени контекста можно сказать DbContext, чтобы использовать это подключение в режиме Code First, передавая имя строки подключения для конструктора DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="4a90c-133">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="4a90c-134">Кроме того, можно использовать форму «имя =\<имя строки подключения\>"для строки, переданных конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="4a90c-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="4a90c-136">Эта форма позволяет явные, что предполагается, что строка подключения должна находиться в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4a90c-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="4a90c-137">Исключение возникает, если строку подключения с заданным именем не найден.</span><span class="sxs-lookup"><span data-stu-id="4a90c-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="4a90c-138">Database First или Model First со строкой подключения в файле app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="4a90c-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="4a90c-139">Модели, созданные в конструкторе EF, отличаются от Code First, в том, что модель уже существует и не создается из кода при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="4a90c-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="4a90c-140">Модель обычно существует как файл EDMX в проекте.</span><span class="sxs-lookup"><span data-stu-id="4a90c-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="4a90c-141">Конструктор добавляет строки подключения к EF в файл app.config или web.config.</span><span class="sxs-lookup"><span data-stu-id="4a90c-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="4a90c-142">Эта строка подключения отличается тем, что он содержит сведения о том, как найти сведения в файле EDMX.</span><span class="sxs-lookup"><span data-stu-id="4a90c-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="4a90c-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-143">For example:</span></span>  

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

<span data-ttu-id="4a90c-144">Конструктор EF также создаст код, который сообщает DbContext, чтобы использовать это подключение, передав имя строки подключения конструктору DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="4a90c-145">Пример:</span><span class="sxs-lookup"><span data-stu-id="4a90c-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="4a90c-146">DbContext знал о необходимости загрузить существующей модели (а не с помощью Code First для вычисления его из кода), так как строка подключения имеет строки подключения к EF, содержащий подробные сведения о модели для использования.</span><span class="sxs-lookup"><span data-stu-id="4a90c-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="4a90c-147">Другие параметры конструктора DbContext</span><span class="sxs-lookup"><span data-stu-id="4a90c-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="4a90c-148">Класс DbContext содержит другие конструкторы и шаблоны использования, которые обеспечивают некоторые более расширенные сценарии.</span><span class="sxs-lookup"><span data-stu-id="4a90c-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="4a90c-149">Ниже приведены некоторые из них.</span><span class="sxs-lookup"><span data-stu-id="4a90c-149">Some of these are:</span></span>  

- <span data-ttu-id="4a90c-150">Класс DbModelBuilder можно использовать для построения модели Code First без создания экземпляра экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="4a90c-151">В результате является объектом DbModel.</span><span class="sxs-lookup"><span data-stu-id="4a90c-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="4a90c-152">Затем можно передать этот объект DbModel для одного или конструкторов DbContext, когда будете готовы для создания экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="4a90c-153">Строка подключения целиком можно передать DbContext вместо базы данных или соединения строки имени.</span><span class="sxs-lookup"><span data-stu-id="4a90c-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="4a90c-154">По умолчанию эта строка подключения используется с поставщиком System.Data.SqlClient; Это можно изменить, задав другой реализации IConnectionFactory в контекст. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="4a90c-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="4a90c-155">Можно использовать существующий объект DbConnection, передавая его конструктора DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a90c-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="4a90c-156">Если объект подключения — это экземпляр EntityConnection, модели, указанное в соединении будет использоваться, а не вычисление модель с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="4a90c-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="4a90c-157">Если объект является экземпляром другого типа — например, SqlConnection, затем контекст будет использовать для режиме Code First.</span><span class="sxs-lookup"><span data-stu-id="4a90c-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="4a90c-158">Можно передать существующий ObjectContext DbContext конструктора для создания DbContext упаковки существующего контекста.</span><span class="sxs-lookup"><span data-stu-id="4a90c-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="4a90c-159">Это можно использовать для существующих приложений, использующие ObjectContext, но которым нужно воспользоваться преимуществами DbContext в некоторых частях приложения.</span><span class="sxs-lookup"><span data-stu-id="4a90c-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
