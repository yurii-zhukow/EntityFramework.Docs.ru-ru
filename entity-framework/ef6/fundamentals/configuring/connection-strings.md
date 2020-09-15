---
title: Строки подключения и модели — EF6
description: Строки подключения и модели в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/fundamentals/configuring/connection-strings
ms.openlocfilehash: 45db461b18cde3bc1f1fccadec3c8ece6dd16832
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90070787"
---
# <a name="connection-strings-and-models"></a>Строки подключения и модели
В этом разделе описано, как Entity Framework обнаруживает, какое подключение к базе данных следует использовать, и как его можно изменить. В этом разделе рассматриваются модели, созданные с помощью Code First и конструктора EF.  

Обычно Entity Framework приложение использует класс, производный от DbContext. Этот производный класс будет вызывать один из конструкторов базового класса DbContext для управления:  

- Как контекст будет подключаться к базе данных, то есть как строка подключения будет найдена или использована  
- Будет ли контекст использовать вычисление модели с помощью Code First или загрузить модель, созданную с помощью конструктора EF  
- Дополнительные дополнительные параметры  

В следующих фрагментах показаны некоторые способы использования конструкторов DbContext.  

## <a name="use-code-first-with-connection-by-convention"></a>Использование Code First с подключением по соглашению  

Если вы еще не выполнили какие-либо другие настройки в приложении, то вызов конструктора без параметров в DbContext приведет к запуску DbContext в режиме Code First с подключением к базе данных, созданным по соглашению. Пример:  

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

В этом примере DbContext использует полное имя пространства имен производного класса контекста — Demo. EF. BloggingContext — в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express или LocalDB. Если установлены оба варианта, будет использоваться SQL Express.  

Visual Studio 2010 включает SQL Express по умолчанию, а Visual Studio 2012 и более поздней версии включает LocalDB. Во время установки пакет NuGet EntityFramework проверяет, какой сервер базы данных доступен. Пакет NuGet обновит файл конфигурации, задав сервер базы данных по умолчанию, который Code First использует при создании соединения по соглашению. Если SQL Express работает, он будет использоваться. Если SQL Express недоступен, то вместо этого будет зарегистрирована LocalDB в качестве значения по умолчанию. В файл конфигурации не вносятся изменения, если он уже содержит параметр для фабрики подключений по умолчанию.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Использование Code First с подключением по соглашению и указанным именем базы данных  

Если вы не выполнили какие-либо другие настройки в приложении, то вызов конструктора строк для DbContext с именем базы данных, которое вы хотите использовать, приведет к запуску DbContext в режиме Code First с подключением к базе данных, созданным по соглашению с базой данных этого имени. Пример:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

В этом примере DbContext использует "Блоггингдатабасе" в качестве имени базы данных и создает строку подключения для этой базы данных с помощью SQL Express (устанавливается вместе с Visual Studio 2010) или LocalDB (устанавливается вместе с Visual Studio 2012). Если установлены оба варианта, будет использоваться SQL Express.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Использование Code First со строкой подключения в файле app.config или web.config  

Вы можете указать строку подключения в app.config или web.config файле. Пример:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Это простой способ указать DbContext использовать сервер базы данных, отличный от SQL Express или LocalDB. в приведенном выше примере указывается база данных SQL Server Compact Edition.  

Если имя строки подключения совпадает с именем контекста (с квалификатором пространства имен или без него), то при использовании конструктора без параметров он будет найден с помощью DbContext. Если имя строки подключения отличается от имени контекста, можно указать DbContext использовать это соединение в режиме Code First, передав имя строки подключения конструктору DbContext. Пример:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Кроме того, можно использовать форму "Name = \<connection string name\> " для строки, передаваемой конструктору DbContext. Пример:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Эта форма явно делает то, что строка подключения будет найдена в файле конфигурации. Если строка подключения с данным именем не найдена, будет выдано исключение.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>База данных/Model First со строкой подключения в файле app.config или web.config  

Модели, созданные с помощью конструктора EF, отличаются от Code First тем, что модель уже существует и не создается из кода при запуске приложения. Модель обычно существует в файле EDMX в проекте.  

Конструктор добавит строку подключения EF в app.config или web.config файл. Эта строка подключения является особой в том, что она содержит сведения о том, как найти информацию в файле EDMX. Пример:  

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

Конструктор EF также создаст код, который сообщает DbContext использовать это соединение, передав имя строки подключения конструктору DbContext. Пример:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext знает о загрузке существующей модели (вместо использования Code First для ее вычисления из кода), так как строка подключения является строкой соединения EF, содержащей сведения о модели для использования.  

## <a name="other-dbcontext-constructor-options"></a>Другие параметры конструктора DbContext  

Класс DbContext содержит другие конструкторы и шаблоны использования, которые позволяют использовать некоторые более сложные сценарии. Вот некоторые из них:  

- Класс Дбмоделбуилдер можно использовать для создания модели Code First без создания экземпляра DbContext. Результатом этого является объект Дбмодел. Затем этот объект Дбмодел можно передать в один из конструкторов DbContext, когда вы будете готовы создать экземпляр DbContext.  
- Можно передать полную строку подключения в DbContext, а не только имя базы данных или строки подключения. По умолчанию эта строка подключения используется с поставщиком System. Data. SqlClient. Это можно изменить, задав другую реализацию Иконнектионфактори на контекст. База данных. Дефаултконнектионфактори.  
- Можно использовать существующий объект DbConnection, передав его в конструктор DbContext. Если объект Connection является экземпляром EntityConnection, то модель, указанная в соединении, будет использоваться вместо вычисления модели с помощью Code First. Если объект является экземпляром какого-либо другого типа, например SqlConnection, то контекст будет использовать его для Code Firstного режима.  
- Вы можете передать существующий контекст ObjectContext в конструктор DbContext, чтобы создать DbContext, который создает оболочку для существующего контекста. Это можно использовать для существующих приложений, использующих ObjectContext, которые хотят использовать преимущества DbContext в некоторых частях приложения.  
