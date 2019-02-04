---
title: Подключение устойчивости и повторного выполнения логики - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 7d6aa870cc32a2b344457fbb04525a7c2c8d1c61
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668769"
---
# <a name="connection-resiliency-and-retry-logic"></a>Логику устойчивости и повторите попытку подключения
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

Приложений, подключающихся к серверу базы данных всегда были уязвимы для разрывов соединений из-за внутренних сбоев и нестабильностью сети. Тем не менее в среде локальной сети на основе работе для выделенной базы данных серверов эти ошибки можно достаточно редко, что дополнительную логику для обработки этих ошибок не часто требуется. С ростом облачных основаны менее надежной сети, которые теперь чаще разрывы подключения серверов базы данных, таких как база данных SQL Windows Azure и подключения. Это может быть вызвано защитных техник, облачной базы данных используют, чтобы обеспечить распределение ресурсов службы, например, регулирование подключения, либо к нестабильной работе в сети, приводит к времени ожидания периодических и других временных ошибок.  

Устойчивость подключений относится к возможности для Entity FRAMEWORK, автоматический повтор любых команд, которые не отвечают из-за этих разрывов соединения.  

## <a name="execution-strategies"></a>Стратегии выполнения  

Повторную попытку подключения возложить на реализацию интерфейса IDbExecutionStrategy. Реализации IDbExecutionStrategy будет отвечать за принятие операции и, при возникновении исключения, определение, если Повтор может быть выполнен и повторять их в случае. Существуют четыре стратегии выполнения, входящие в состав EF.  

1. **DefaultExecutionStrategy**: Эта стратегия выполнения не выполняет повторную попытку запуска любых операций, это значение по умолчанию для баз данных, отличных от sql server.  
2. **DefaultSqlExecutionStrategy**: это стратегия внутреннего выполнения, которая используется по умолчанию. Эта стратегия не пытается повторно вообще, тем не менее, она будет разбита любые исключения, которые может быть временной для информирования пользователей, которые они хотели включить устойчивость подключения.  
3. **DbExecutionStrategy**: этот класс подходит как базовый класс для других стратегий выполнения, включая собственные пользовательские. Она реализует экспоненциальную политику повторов, где начальной повторная попытка происходит с нулевой задержки и задержка увеличивается экспоненциально пока не достигается максимальное число повторных попыток. Этот класс содержит абстрактный метод ShouldRetryOn, который может быть реализован в производном выполнения стратегии для управления, какие исключения следует повторить.  
4. **SqlAzureExecutionStrategy**: Эта стратегия выполнения наследует от DbExecutionStrategy и повторит попытку для исключений, которые заведомо возможно временных при работе с базой данных SQL Azure.

> [!NOTE]
> Стратегии выполнения, 2 и 4, включаются в поставщике Sql Server, поставляемый с EF, которая находится в сборке EntityFramework.SqlServer и предназначены для работы с SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Включение стратегия выполнения  

Самый простой способ сообщить EF для использования стратегии выполнения — с помощью метода SetExecutionStrategy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) класса:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Этот код указывает EF для использования SqlAzureExecutionStrategy при подключении к SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Настройка стратегии выполнения  

Конструктор SqlAzureExecutionStrategy может принимать два параметра, значение параметра MaxRetryCount и MaxDelay. Число MaxRetry — это максимальное число попыток повтора стратегии. MaxDelay — это интервал времени, представляющий максимальную задержку между повторными попытками, которые будут использовать стратегии выполнения.  

Чтобы задать максимальное число повторных попыток 1 и Максимальная задержка до 30 секунд можно execue следующее:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

SqlAzureExecutionStrategy повторит попытку мгновенной превышено в первый раз, временная ошибка возникает, но будет больше задержка между попытками, пока либо максимальное предельное число повторов или общее время достигает максимальный интервал.  

Стратегии выполнения повторит только ограниченное число исключений, которые обычно являются tansient, по-прежнему необходимо будет обрабатывать другие ошибки, а также перехват исключения RetryLimitExceeded в случае когда ошибка не временная, или занимает слишком много времени для решения сам.  

Существуют некоторые известные ограничения при использовании стратегии выполнения повторной попытки:  

## <a name="streaming-queries-are-not-supported"></a>Потоковой передачи запросы не поддерживаются  

По умолчанию EF6 или более поздней версии будет буфера, результаты запроса, а не их streaming. Если требуется, чтобы результаты потоковую передачу вы можно использовать метод AsStreaming изменение LINQ to Entities в запрос для потоковой передачи.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Не поддерживается потоковая передача, при регистрации стратегию выполнения повторной попытки. Это ограничение существует, так как подключение удалось удалить части через возвращаемых результатов. В этом случае EF, потребуется повторно выполнить весь запрос, но имеет надежного способа узнать, какие результаты уже были возвращены (данные могут измениться после начального запроса было отправлено, результаты могут вернуться в другом порядке, результаты могут не иметь уникальный идентификатор и т.д.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Инициированной пользователем транзакции не поддерживаются  

Когда вы настроили стратегия выполнения, которая приводит к повторных попыток, существуют некоторые ограничения в использовании транзакций.  

По умолчанию EF будет выполнять все обновления базы данных в рамках транзакции. Не нужно ничего делать, чтобы включить эту, EF всегда выполняет это автоматически.  

Например в следующем коде SaveChanges выполняется автоматически в рамках транзакции. Если SaveChanges ошибкой после вставки одного нового узла, а затем был бы выполнен откат транзакции и никакие изменения не применены к базе данных. Контекст также остается в состоянии, допускающем SaveChanges вызываться снова, чтобы повторить попытку применения изменений.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Если не используется Повтор стратегию выполнения, можно включить несколько операций в рамках одной транзакции. Например следующий код создает оболочку двух вызовов SaveChanges в одной транзакции. Если любой части либо операция завершается ошибкой затем изменения применяются.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Это не поддерживается при использовании стратегии выполнения повторной попытки, поскольку EF не будет учитывать любых предыдущих операций и как повторить их. Например если второй SaveChanges неудачей, EF больше не имеет сведения, необходимые для повторите первый вызов метода SaveChanges.  

### <a name="workaround-suspend-execution-strategy"></a>Решение Приостановка выполнения стратегии  

Возможным решением является Приостановка Повтор стратегия выполнения для фрагмент программного кода, который должен использовать пользователь инициировал транзакции. Самый простой способ сделать это является добавление флага SuspendExecutionStrategy в код на основе класса конфигурации и изменить лямбда-выражения стратегия выполнения для возврата стратегия выполнения по умолчанию (не retying), если флаг установлен.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy") ?? false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Обратите внимание на то, что мы используем CallContext для хранения значения флага. Это предоставляет аналогичную функциональность в локальное хранилище потока, но безопасен для использования асинхронного кода — включая асинхронный запрос и сохранить с Entity Framework.  

Теперь мы может приостановить стратегия выполнения для раздела кода, использующего инициированной пользователем транзакции.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a>Решение Вручную вызвать стратегию выполнения  

Другой вариант — вручную используйте стратегию выполнения и присвойте ему всего набора логики для выполнения, таким образом, чтобы его можно повторить все, что при сбое одной из операций. Нам еще нужно приостановить стратегия выполнения — с применением технологии показанный выше -, так что любой контекст, используемые внутри блока кода повторной попытки не пытайтесь повторить попытку.  

Обратите внимание на то, что все контексты должно быть создано в блоке кода, для повтора. Это гарантирует, что мы начинаем с чистого состояния для каждой повторной попытки.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
