---
title: Соглашения Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: c5fa580879a4b53fed34d94b737988875f38c62c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995530"
---
# <a name="code-first-conventions"></a>Первый соглашения о коде
Во-первых, код позволяет описания модели с помощью классов C# или Visual Basic .NET. Основные фигуры модели определяется с использованием соглашений. Соглашения являются наборы правил, которые используются для автоматической настройки концептуальной модели на основе определений класса при работе в режиме Code First. Правила определяются в пространстве имен System.Data.Entity.ModelConfiguration.Conventions.  

Можно продолжить настройку модели с помощью заметок к данным или текучего API. Преимущество отдается конфигурацию с помощью текучего API, следуют заметок к данным и соглашения. Дополнительные сведения см. в разделе [заметок к данным](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - связи](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - типов и свойств,](~/ef6/modeling/code-first/fluent/types-and-properties.md) и [Fluent API с использованием VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Подробный список соглашения Code First доступен в [документации по API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Здесь представлен обзор соглашения, используемые механизмом Code First.  

## <a name="type-discovery"></a>Обнаружение типа  

При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен). Помимо определения классов, необходимо также разрешить **DbContext** знать, какие типы, которые вы хотите включить в модель. Чтобы сделать это, вы определяется класс контекста, производный от **DbContext** и предоставляет **DbSet** свойств для типов, которые должны быть частью модели. Код сначала будет включать эти типы и также будет получать в любой ссылочных типов, даже если ссылочных типов определяются в другой сборке.  

Если типы участвуют в иерархии наследования, его достаточно, чтобы определить **DbSet** свойство для базового класса и производных типов, будут автоматически включены, если они находятся в той же сборке, в качестве базового класса.  

В следующем примере имеется только один **DbSet** свойство, определенное для **SchoolEntities** класс (**отделов**). Код сначала использует это свойство для обнаружения и по запросу в любой ссылочных типов.  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

Если вы хотите исключить тип из модели, используйте **NotMapped** атрибут или **DbModelBuilder.Ignore** текучего API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Соглашение первичного ключа  

Код сначала определяет, что свойство является первичным ключом, если свойство класса, называется «ID» (без учета регистра), или имя класса, добавив «ID». Если тип со свойством первичного ключа является числом или GUID будет настроен в качестве столбца идентификаторов.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Соглашение о связи  

В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей. Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует. Свойства навигации позволяют перейти связей и управление ими в обоих направлениях, возвращая объект ссылки (если атрибут кратности имеет один или ноль или один) или коллекции (Если кратность равна много). Код сначала определяет связи, основанные на свойства навигации, определенные для типов.  

В дополнение к свойствам навигации рекомендуется включить свойства внешнего ключа на типы, представляющие зависимые объекты. Любое свойство с тем же типом данных, как основной свойство первичного ключа и с именем, которое следует одной из следующих форматов представляет внешний ключ для связи: "\<имя свойства навигации\>\<участника Имя свойство первичного ключа\>','\<имя основного класса\>\<имя свойство первичного ключа\>", или"\<имя участника-свойство первичного ключа\>". Если найдено несколько совпадений, преимущество отдается в порядке, указанном выше. Обнаружение внешнего ключа не учитывается регистр символов. При обнаружении свойство внешнего ключа, Code First определяет кратность связи, в зависимости от того, поддерживает ли внешний ключ. Если свойство имеет значение NULL затем связь регистрируется как необязательный; в противном случае — регистрируется связи при необходимости.  

Если внешний ключ для зависимой сущности не допускает значения NULL, то Code First устанавливает каскадное удаление в отношении. Если внешний ключ для зависимой сущности допускает значения NULL, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ устанавливается в значение null. Поведение, обнаруживаемые при удалении кратность и cascade соглашение можно переопределить с помощью текучего API.  

В следующем примере свойства навигации и внешний ключ используются для определения связи между классами отдела и курса.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> Если у вас есть несколько связей между теми же типами (предположим, например, можно определить **Person** и **книги** классы, где **Person** класс содержит  **ReviewedBooks** и **AuthoredBooks** свойства навигации и **книги** класс содержит **автор** и  **Редактор** свойства навигации) необходимо вручную настроить отношения с помощью заметок к данным или текучего API. Дополнительные сведения см. в разделе [заметок к данным - связи](~/ef6/modeling/code-first/data-annotations.md) и [Fluent API - связи](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Соглашение о сложных типов  

Когда Code First обнаруживает определение класса, где не может быть определен первичный ключ, а первичный ключ не зарегистрирован при помощи заметки к данным или fluent API, тип автоматически регистрируется как сложный тип. Определение сложных типов также требует, что тип не имеет свойства, которые ссылаются на типы сущностей и нет ссылок из свойства коллекции другого типа. Учитывая следующие определения класса Code First бы получен, **сведения** — это сложный тип, так как он не имеет первичного ключа.  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a>Соглашение о строке подключения  

Дополнительные сведения о соглашениях об что DbContext использует для обнаружения соединение для использования см. в разделе [подключений и модели](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Удаление соглашения  

Вы можете удалить правилам, определенным в пространстве имен System.Data.Entity.ModelConfiguration.Conventions. В следующем примере удаляется **PluralizingTableNameConvention**.  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a>Соглашения об именовании  

Соглашения об именовании, поддерживаются в EF6 и выше. Дополнительные сведения см. в разделе [первый соглашения о написании пользовательского кода](~/ef6/modeling/code-first/conventions/custom.md).
