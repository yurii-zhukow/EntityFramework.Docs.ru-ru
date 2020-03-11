---
title: Соглашения Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415949"
---
# <a name="code-first-conventions"></a>Соглашения Code First
Code First позволяет описать модель с помощью C# среды или Visual Basic классов .NET. Базовая форма модели определяется с помощью соглашений. Соглашения — это наборы правил, которые используются для автоматической настройки концептуальной модели на основе определений классов при работе с Code First. Соглашения определяются в пространстве имен System. Data. Entity. Моделконфигуратион. Conventions.  

Вы можете дополнительно настроить модель с помощью заметок к данным или API Fluent. Приоритет предоставляется в конфигурации через API Fluent, за которым следуют заметки к данным, а затем соглашения. Дополнительные сведения см. в статье [заметки к данным](~/ef6/modeling/code-first/data-annotations.md), [интерфейсы API Fluent](~/ef6/modeling/code-first/fluent/relationships.md), [типы API Fluent & свойства](~/ef6/modeling/code-first/fluent/types-and-properties.md) и [fluent API с помощью VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Подробный список соглашений Code First можно найти в документации по [API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). В этом разделе приводятся общие сведения о соглашениях, используемых Code First.  

## <a name="type-discovery"></a>Обнаружение типов  

При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель. Помимо определения классов необходимо также разрешить **DbContext** знать, какие типы необходимо включить в модель. Для этого необходимо определить класс контекста, который является производным от **DbContext** , и предоставляет свойства **DbSet** для типов, которые должны быть частью модели. Code First будут включать эти типы, а также будут запрашивать ссылочные типы, даже если типы, на которые имеются ссылки, определены в другой сборке.  

Если типы участвуют в иерархии наследования, достаточно определить свойство **DbSet** для базового класса, и производные типы будут автоматически включены в сборку, если они находятся в той же сборке, что и базовый класс.  

В следующем примере для класса **пункт SchoolEntities** определено только одно свойство **DbSet** (**Departments**). Code First использует это свойство для обнаружения и извлечения любых ссылочных типов.  

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

Если необходимо исключить тип из модели, используйте атрибут **нотмаппед** или API **дбмоделбуилдер. Ignore** Fluent.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Соглашение о первичном ключе  

Code First выводит, что свойство является первичным ключом, если свойство класса имеет имя "ID" (без учета регистра), или имя класса, за которым следует "ID". Если тип свойства первичного ключа является числовым или идентификатором GUID, он будет настроен в качестве столбца идентификаторов.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Соглашение о связях  

В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей. Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует. Свойства навигации позволяют перемещать связи и управлять ими в обоих направлениях, возвращая либо ссылочный объект (если кратность — либо одна, либо нулевая или-одна), либо коллекция (если количество элементов равно многим). Code First определяет связи на основе свойств навигации, определенных в типах.  

В дополнение к свойствам навигации рекомендуется включить свойства внешнего ключа для типов, представляющих зависимые объекты. Любое свойство с тем же типом данных, что и основное свойство первичного ключа, и имя, следующее за одним из следующих форматов, представляют внешний ключ для связи: "\<имя свойства навигации\>\<имя свойства основного ключа\>", "\<имя основного класса\>\<имя свойства первичного ключа\>", или "\<имя свойства основного ключа\>". Если найдено несколько совпадений, приоритет передается в указанном выше порядке. При обнаружении внешнего ключа не учитывается регистр. При обнаружении свойства внешнего ключа Code First определяет количество элементов связи на основе допустимости значений NULL внешнего ключа. Если свойство допускает значение null, то связь регистрируется как необязательнаая. в противном случае связь регистрируется по мере необходимости.  

Если внешний ключ в зависимой сущности не допускает значения NULL, то Code First задает каскадное удаление для связи. Если внешний ключ в зависимой сущности допускает значение null, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ будет установлен в значение null. Поведение количества элементов и каскадного удаления, обнаруженное соглашением, может быть переопределено с помощью API Fluent.  

В следующем примере свойства навигации и внешний ключ используются для определения связи между классами Department и Course.  

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
> При наличии нескольких связей между одними и теми же типами (например, предположим, что вы определили классы **Person** и **Book** , где класс **Person** содержит свойства навигации **ревиеведбукс** и **Аусоредбукс** , а класс **Book** содержит свойства навигации " **Автор** " и " **проверяющий** "), необходимо вручную настроить связи с помощью заметок к данным или API-интерфейса Fluent. Дополнительные сведения см. в статье связи [аннотаций данных](~/ef6/modeling/code-first/data-annotations.md) и [интерфейсов API Fluent](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Соглашение о сложных типах  

Когда Code First обнаруживает определение класса, в котором не удается вывести первичный ключ и не зарегистрирован первичный ключ с помощью аннотаций данных или API-интерфейса Fluent, тип автоматически регистрируется как сложный тип. Для обнаружения сложного типа также требуется, чтобы тип не имел свойств, ссылающихся на типы сущностей и на которые нет ссылок из свойства коллекции другого типа. Учитывая следующие определения классов Code First выводит, что **сведения** являются сложным типом, так как у него нет первичного ключа.  

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

## <a name="connection-string-convention"></a>Соглашение строки соединения  

Чтобы узнать о соглашениях, которые DbContext использует для обнаружения подключения для использования, см. раздел [подключения и модели](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Удаление соглашений  

Можно удалить любое из соглашений, определенных в пространстве имен System. Data. Entity. Моделконфигуратион. Conventions. В следующем примере удаляется **плурализингтабленамеконвентион**.  

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

## <a name="custom-conventions"></a>Настраиваемые соглашения  

Пользовательские соглашения поддерживаются в EF6. Дополнительные сведения см. в разделе [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).
