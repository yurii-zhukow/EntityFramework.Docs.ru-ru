---
title: Ведение журнала и перехвата операций базы данных - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
caps.latest.revision: 3
ms.openlocfilehash: 1d0e953309f3c81a2941d6850e169aaa31ae8de0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122797"
---
# <a name="logging-and-intercepting-database-operations"></a>Ведение журнала и перехвата операций базы данных
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

Начиная с Entity Framework 6, каждый раз, когда Entity Framework отправляет команду базе данных этой команды могут быть перехвачены кодом приложения. Это чаще всего используется для ведения журнала SQL, но можно также использовать для изменения или прервите команду.  

В частности платформа EF предусматривает:  

- Свойство Log для контекста, аналогичную DataContext.Log в LINQ to SQL  
- Механизм для настройки содержимого и форматирования выходных данных, отправляются в журнал  
- Низкоуровневые строительные блоки для перехвата, обеспечивая гибкость управления /  

## <a name="context-log-property"></a>Свойство контекста журнала  

Свойство DbContext.Database.Log можно задать делегат для любого метода, который принимает строку. Чаще всего он используется с любой TextWriter при установке для него метод «Запись», TextWriter. Все SQL, созданных в текущем контексте будет регистрироваться для этого средства записи. Например следующий код, регистрируются SQL в консоли:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Обратите внимание, что этот контекст. Database.Log имеет значение "Console.Write". Это все, что требуется для входа SQL на консоль.  

### <a name="example-output"></a>Пример результата  

Давайте добавим немного простого кода запроса и вставки или обновления, чтобы мы могли видеть некоторые выходные данные:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Это создаст следующие выходные данные:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Обратите внимание, что это выходные данные, при условии, что уже произошло инициализацию базы данных. Если инициализация базы данных не уже была применена, то будет гораздо больше результат, показаны всю работу миграции не на самом деле поиск или создать новую базу данных.)  

### <a name="what-gets-logged"></a>Новые журналы?  

Когда все указанные ниже имеет значение свойства Log будет записан в журнал:  

- SQL для всех различных типов команд. Пример:  
    - Запросы, включая обычные запросы LINQ, запросов eSQL и необработанные запросы из методов, таких как SQL-запрос  
    - Операции вставки, обновления и удаления, созданным как часть SaveChanges  
    - Отношение, загрузке запросов, которые созданы при отложенной загрузки  
- Параметры  
- Ли команда выполняется асинхронно  
- Меткой начала выполнения команды  
- Независимо от того, имеется ли команда выполнена успешно, сбой, создается исключение, или для асинхронных, была отменена  
- Некоторые указания результирующего значения  
- Приблизительное количество времени, которое потребовалось для выполнения команды. Обратите внимание, что это время с отправкой команды для получения объекта результат обратно. Не включает время считать результаты.  

Глядя в примере выходных данных выше, каждый из четырех команд в журнал:  

- Запрос, полученный из вызова к контексту. Blogs.First  
    - Обратите внимание, что метод ToString получения SQL не были бы выполнены для этого запроса, так как "First" не поддерживает IQueryable, для которого может быть вызван ToString  
- Запрос, полученный в результате отложенной загрузки блога. Сообщения  
    - Обратите внимание, что сведения о параметрах для значения ключа для какой отложенная загрузка происходит  
    - Регистрируются только свойства параметра, которые присваиваются значения по умолчанию. Например свойство размера отображается только если оно не равно нулю.  
- Две команды, полученный в результате SaveChangesAsync; для обновления изменить заголовок post, другой для инструкции insert для добавления новой записи  
    - Обратите внимание, что сведения о параметрах для свойства внешнего ключа и заголовок  
    - Обратите внимание на то, что эти команды выполняются асинхронно  

### <a name="logging-to-different-places"></a>Ведение журнала для различных мест  

Как показано выше для ведения журнала консоли — очень легко. Это средство легко входить в памяти, файл, и т.д., с помощью различных типов из [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Если вы знакомы с помощью LINQ to SQL, следует отметить, что в LINQ to SQL, имеет значение свойства Log на фактический TextWriter объект (например, Console.Out) во время в EF журнала задано значение метод, который принимает строку (например Console.Write или Console.Out.Write). Причина этого — отделить EF из TextWriter, принимая любому делегату, может выступать в качестве приемника для строк. Например, представьте, что у вас уже есть некоторые платформы ведения журналов, и определяет метод ведения журнала следующим образом:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Это может привязать к свойству EF журнала следующим образом:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

### <a name="result-logging"></a>Записи результатов  

Средство ведения журнала по умолчанию текст команды (SQL), параметры и журналов в строке «Executing» с меткой времени перед отправкой команды в базу данных. «Завершено» строки, содержащей затраченное время — журнал выполнения следующие команды.  

Обратите внимание на то, что для асинхронных команд «завершено» строки не регистрируется до асинхронной задачи фактически завершении, сбое или отмене.  

Строка «завершено» содержит различные сведения в зависимости от типа команды и ли выполнение прошло успешно.  

#### <a name="successful-execution"></a>Успешное выполнение  

Для команд, которые успешно завершить выходные данные — «выполнено в x ms с результатом: "следуют некоторые указания результаты. Для команд, которые возвращают результат модуля чтения данных — это значение, указывающее тип [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) возвращается. Для команд, которые возвращают значение типа integer, таких как обновление команде, показанной выше показан результат является целого числа.  

#### <a name="failed-execution"></a>Сбой при выполнении  

Для команд, которые не удается, создается исключение выходные данные содержат сообщение из исключения. Например с помощью SQL-запрос для запроса к таблице, которая существует будет результат в журнале выходные данные примерно следующее:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

#### <a name="canceled-execution"></a>Выполнение отменено  

Асинхронные команды, где задача отменяется, результатом может стать сбой с исключением, так как это и базового поставщика ADO.NET часто делает при попытке отменить. Если этого не происходит, если задача отменена аккуратно выходных данных будет выглядеть примерно следующим образом:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Изменение содержимого журнала и форматирование  

### <a name="databaselogformatter"></a>DatabaseLogFormatter  

На самом деле Database.Log, делает свойство использовать DatabaseLogFormatter объекта. Этот объект фактически привязывает делегат, который принимает строки и DbContext реализацию IDbCommandInterceptor (см. ниже). Это означает, что методы DatabaseLogFormatter вызываются до и после выполнения команды EF. Эти методы DatabaseLogFormatter сбора и отформатировать выходные данные журнала и отправить его на делегат.  

### <a name="customizing-databaselogformatter"></a>Настройка DatabaseLogFormatter  

Изменение регистрируемых и форматирования можно сделать, создав новый класс, производный от DatabaseLogFormatter и переопределяет соответствующие методы. Ниже приведены наиболее распространенных методов для переопределения.  

- LogCommand — переопределить изменение процессом регистрации команд до их выполнения. По умолчанию LogCommand вызывает LogParameter для каждого параметра; Вы можете сделать то же самое в переопределении или обработки параметров вместо этого по-разному.  
- LogResult — переопределить изменение регистрируется как результат выполнения команды.  
- LogParameter — переопределить это, чтобы изменить форматирование и содержимое параметра ведения журнала.  

Например предположим, что мы хотели бы журнала лишь одна строка, перед отправкой каждой команды в базу данных. Это можно сделать с помощью переопределения:  

- Переопределить LogCommand для форматирования и записи одной строки SQL  
- Переопределите LogResult, чтобы не выполнять никаких действий.  

Код будет выглядеть примерно так:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

В журнал выходные простым вызовом метод Write, которая будет отправлять выходные данные к делегату настроенные записи.  

(Обратите внимание, что этот код выполняет упрощенную удаления разрывы строк так же, как пример. Он скорее всего не будет работать для просмотра сложных SQL.)  

### <a name="setting-the-databaselogformatter"></a>Параметр DatabaseLogFormatter  

Создав новый класс DatabaseLogFormatter он должен быть зарегистрирован с EF. Это делается с помощью конфигурации на основе кода. По сути это означает, создается новый класс, производный от DbConfiguration в ту же сборку класса DbContext и последующего вызова SetDatabaseLogFormatter в конструкторе этого нового класса. Пример:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>С помощью нового DatabaseLogFormatter  

Теперь этот новый DatabaseLogFormatter будет использоваться каждый раз, когда Database.Log имеет значение. Таким образом выполнение кода из части 1 теперь приведет следующие выходные данные:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Стандартные блоки перехвата  

Пока мы рассматривали как DbContext.Database.Log для ведения журнала SQL, созданного EF. Но этот код предоставляются фактически относительно тонкой через некоторые низкоуровневые строительные блоки для более общих перехвата.  

### <a name="interception-interfaces"></a>Перехват интерфейсы  

Перехват кода построена вокруг концепция перехвата интерфейсов. Этих интерфейсов наследуют от IDbInterceptor и определять методы, которые вызываются, когда EF выполняет определенное действие. Целью является один интерфейс по типам перехватываемым объектом. Например IDbCommandInterceptor интерфейс определяет методы, которые вызываются до EF вызывает метод ExecuteNonQuery, ExecuteScalar, ExecuteReader и связанные с ним методы. Аналогичным образом интерфейс определяет методы, вызываемые при завершении каждой из этих операций. Класс DatabaseLogFormatter, который мы рассмотрели выше реализует этот интерфейс, чтобы регистрировать в журнале команды.  

### <a name="the-interception-context"></a>Контекст перехвата  

Просмотрев методы, определенные на любой из этих интерфейсов перехватчик его очевидным, что при каждом вызове является заданный объект типа DbInterceptionContext или какого-либо типа производным от этого например DbCommandInterceptionContext\<\>. Этот объект содержит контекстные сведения о действии, занимает EF. Например если это действие выполняется от имени DbContext, DbContext включается в DbInterceptionContext. Аналогичным образом для команд, которые выполняются асинхронно, IsAsync установлен флаг DbCommandInterceptionContext.  

### <a name="result-handling"></a>Обработка результатов  

DbCommandInterceptionContext\< \> класс содержит свойства, результат, OriginalResult, исключения и первоначальное исключение. Эти свойства устанавливаются в null/zero для перехвата методов, которые вызываются до выполнения операции, то есть для... Выполнение методов. Если операция выполняется и завершается успешно, результат и OriginalResult задаются на результат операции. Затем эти значения можно наблюдать в перехвата методы, которые вызываются после выполнения операции, то есть на... Выполненных методов. Аналогично Если операция создает исключение, то свойства исключения и первоначальное исключение будет происходить.  

#### <a name="suppressing-execution"></a>Подавление выполнения  

Если перехватчик свойства результата до выполнения команды (в одном из... Выполнение методов) затем EF не будет пытаться фактически выполнить команду, но вместо этого просто будет использовать результирующий набор. Другими словами перехватчик можно подавить выполнение команды, но имеют EF по-прежнему так, как если была выполнена команда.  

Команды пакетной обработки, традиционно было сделано с помощью упаковки поставщика — пример того, как это может использовать. Перехватчик будет храниться команды для последующего выполнения в пакетном режиме, но будет «» выступить в роли EF, что команда выполнена в обычном режиме. Обратите внимание, что больше, чем это требуется для реализации пакетной обработки, но ниже приведен пример того, как может использовать изменения перехвата результатов.  

Выполнение можно также подавить, задав свойство Exception в одном из... Выполнение методов. В этом случае EF продолжить, как будто выполнение операции прервано возникновения данного исключения. Это Конечно, приложение дает сбой, но также может временного исключения или других исключений, который обрабатывается EF. Например это может использоваться в тестовой среде для тестирования поведения приложения, когда происходит сбой выполнения команды.  

#### <a name="changing-the-result-after-execution"></a>Изменения результатов после выполнения  

Если перехватчик свойства результат после выполнения команды (в одном из... Выполняется методы), а затем EF будет использовать измененные результат вместо результата, который фактически был возвращен из операции. Аналогично Если перехватчик задает свойство исключения после выполнения команды, затем EF будет исключение набор так, как если бы операция исключение.  

Перехватчик может также присвоено свойство Exception значение null, указывающее, что исключение не должно вызываться. Это может быть полезно, если не удалось выполнить операцию, но перехватчик намерена EF, чтобы продолжить, как если бы операция выполнена успешно. Это обычно также включает в себя установку результат, чтобы EF другому значению результат для работы с текущего сеанса.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult и первоначальное исключение  

После выполнения операции EF метод установит результат и OriginalResult свойства, если выполнение не завершилось сбоем или свойства исключения и первоначальное исключение если выполнение завершилось сбоем с исключением.  

Свойства OriginalResult и первоначальное исключение доступны только для чтения и только задаются EF после фактического выполнения операции. Эти свойства задаются не перехватчики. Это означает, что любой перехватчик может различать исключения или результат, который был задан некоторые перехватчик, в отличие от реальных исключение или результат, возникшей при выполнении операции.  

### <a name="registering-interceptors"></a>Регистрация перехватчики  

После создания класса, который реализует один или несколько интерфейсов перехват его можно зарегистрировать с помощью класса DbInterception EF. Пример:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Перехватчики также может быть зарегистрирован на уровне домена приложения с помощью механизма DbConfiguration конфигурация на основе кода.  

### <a name="example-logging-to-nlog"></a>Пример: Ведение журнала для NLog  

Давайте рассмотрим все это вместе в пример, при помощи IDbCommandInterceptor и [NLog](http://nlog-project.org/) для:  

- Журнал предупреждение для любой команды, который выполняется не в асинхронном режиме  
- Регистрирует ошибку для любой команды, который создает исключение при выполнении  

Ниже показан класс, который выполняет ведение журнала, который должен быть зарегистрирован как показано выше:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Обратите внимание на то, как этот код использует контекст перехвата для обнаружения при выполнении команды не асинхронно и обнаружения, когда произошла ошибка при выполнении команды.  
