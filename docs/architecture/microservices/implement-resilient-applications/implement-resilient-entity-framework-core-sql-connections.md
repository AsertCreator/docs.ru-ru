---
title: Реализация устойчивых SQL-подключений Entity Framework Core
description: Сведения о реализации устойчивых SQL-подключений Entity Framework Core Этот прием особенно важен при использовании базы данных SQL Azure в облаке.
ms.date: 10/16/2018
ms.openlocfilehash: 3bf5c1827cee1da69aeccdc9f15573c301fc9363
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674561"
---
# <a name="implement-resilient-entity-framework-core-sql-connections"></a><span data-ttu-id="76eeb-104">Реализация устойчивых SQL-подключений Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="76eeb-104">Implement resilient Entity Framework Core SQL connections</span></span>

<span data-ttu-id="76eeb-105">Для Базы данных SQL Azure платформа Entity Framework (EF) Core уже предоставляет логику устойчивости и повторных попыток при подключении к внутренним базам данных.</span><span class="sxs-lookup"><span data-stu-id="76eeb-105">For Azure SQL DB, Entity Framework (EF) Core already provides internal database connection resiliency and retry logic.</span></span> <span data-ttu-id="76eeb-106">Но вам необходимо применить стратегию выполнения Entity Framework к каждому подключению <xref:Microsoft.EntityFrameworkCore.DbContext>, чтобы обеспечить [устойчивое подключение к EF Core](/ef/core/miscellaneous/connection-resiliency).</span><span class="sxs-lookup"><span data-stu-id="76eeb-106">But you need to enable the Entity Framework execution strategy for each <xref:Microsoft.EntityFrameworkCore.DbContext> connection if you want to have [resilient EF Core connections](/ef/core/miscellaneous/connection-resiliency).</span></span>

<span data-ttu-id="76eeb-107">Например, следующий код на уровне подключения к EF Core обеспечивает устойчивое SQL-подключение, которое устанавливается повторно при сбое.</span><span class="sxs-lookup"><span data-stu-id="76eeb-107">For instance, the following code at the EF Core connection level enables resilient SQL connections that are retried if the connection fails.</span></span>

```csharp
// Startup.cs from any ASP.NET Core Web API
public class Startup
{
    // Other code ...
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddDbContext<CatalogContext>(options =>
        {
            options.UseSqlServer(Configuration["ConnectionString"],
            sqlServerOptionsAction: sqlOptions =>
            {
                sqlOptions.EnableRetryOnFailure(
                maxRetryCount: 10,
                maxRetryDelay: TimeSpan.FromSeconds(30),
                errorNumbersToAdd: null);
            });
        });
    }
//...
}
```

## <a name="execution-strategies-and-explicit-transactions-using-begintransaction-and-multiple-dbcontexts"></a><span data-ttu-id="76eeb-108">Стратегии выполнения и явные транзакции с использованием BeginTransaction и нескольких DbContext</span><span class="sxs-lookup"><span data-stu-id="76eeb-108">Execution strategies and explicit transactions using BeginTransaction and multiple DbContexts</span></span>

<span data-ttu-id="76eeb-109">Если в подключениях к EF Core включены повторные попытки, каждая операция, выполняемая с помощью EF Core, будет предпринимать повторные попытки.</span><span class="sxs-lookup"><span data-stu-id="76eeb-109">When retries are enabled in EF Core connections, each operation you perform using EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="76eeb-110">Каждый запрос и каждый вызов к `SaveChanges` будет повторяться снова как единица в случае сбоя.</span><span class="sxs-lookup"><span data-stu-id="76eeb-110">Each query and each call to `SaveChanges` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="76eeb-111">Но если код инициирует транзакцию с помощью `BeginTransaction`, вы сами определяете группу операций, которые должны рассматриваться как единица.</span><span class="sxs-lookup"><span data-stu-id="76eeb-111">However, if your code initiates a transaction using `BeginTransaction`, you're defining your own group of operations that need to be treated as a unit.</span></span> <span data-ttu-id="76eeb-112">Все содержимое транзакции можно будет откатить в случае сбоя.</span><span class="sxs-lookup"><span data-stu-id="76eeb-112">Everything inside the transaction has to be rolled back if a failure occurs.</span></span>

<span data-ttu-id="76eeb-113">Если вы попытаетесь выполнить эту транзакцию при использовании стратегии выполнения EF (политика повтора) и вызываете `SaveChanges` из нескольких DbContext, отобразится исключение такого типа:</span><span class="sxs-lookup"><span data-stu-id="76eeb-113">If you try to execute that transaction when using an EF execution strategy (retry policy) and you call `SaveChanges` from multiple DbContexts, you'll get an exception like this one:</span></span>

> <span data-ttu-id="76eeb-114">"System.InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions" (System.InvalidOperationException: настроенная стратегия выполнения SqlServerRetryingExecutionStrategy не поддерживает запуск транзакций пользователем).</span><span class="sxs-lookup"><span data-stu-id="76eeb-114">System.InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="76eeb-115">Используйте стратегию выполнения, возвращенную 'DbContext.Database.CreateExecutionStrategy()', чтобы выполнить все операции в транзакции как повторяемую единицу.</span><span class="sxs-lookup"><span data-stu-id="76eeb-115">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="76eeb-116">Необходимо вручную вызвать стратегию выполнения EF с делегатом, который представляет все, что должно быть выполнено.</span><span class="sxs-lookup"><span data-stu-id="76eeb-116">The solution is to manually invoke the EF execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="76eeb-117">В случае временного сбоя стратегия выполнения будет снова вызывать делегат.</span><span class="sxs-lookup"><span data-stu-id="76eeb-117">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span> <span data-ttu-id="76eeb-118">Например, следующий код демонстрирует, как это реализуется в eShopOnContainers с двумя DbContext (\_catalogContext и IntegrationEventLogContext) при обновлении продукта и сохранении объекта ProductPriceChangedIntegrationEvent, который должен использовать другой DbContext.</span><span class="sxs-lookup"><span data-stu-id="76eeb-118">For example, the following code show how it's implemented in eShopOnContainers with two multiple DbContexts (\_catalogContext and the IntegrationEventLogContext) when updating a product and then saving the ProductPriceChangedIntegrationEvent object, which needs to use a different DbContext.</span></span>

```csharp
public async Task<IActionResult> UpdateProduct(
    [FromBody]CatalogItem productToUpdate)
{
    // Other code ...

    var oldPrice = catalogItem.Price;
    var raiseProductPriceChangedEvent = oldPrice != productToUpdate.Price;

    // Update current product
    catalogItem = productToUpdate;

    // Save product's data and publish integration event through the Event Bus
    // if price has changed
    if (raiseProductPriceChangedEvent)
    {
        //Create Integration Event to be published through the Event Bus
        var priceChangedEvent = new ProductPriceChangedIntegrationEvent(
          catalogItem.Id, productToUpdate.Price, oldPrice);

       // Achieving atomicity between original Catalog database operation and the
       // IntegrationEventLog thanks to a local transaction
       await _catalogIntegrationEventService.SaveEventAndCatalogContextChangesAsync(
           priceChangedEvent);

       // Publish through the Event Bus and mark the saved event as published
       await _catalogIntegrationEventService.PublishThroughEventBusAsync(
           priceChangedEvent);
    }
    // Just save the updated product because the Product's Price hasn't changed.
    else
    {
        await _catalogContext.SaveChangesAsync();
    }
}
```

<span data-ttu-id="76eeb-119">Первый <xref:Microsoft.EntityFrameworkCore.DbContext> — это `_catalogContext`, а второй `DbContext` находится в пределах объекта `_integrationEventLogService`.</span><span class="sxs-lookup"><span data-stu-id="76eeb-119">The first <xref:Microsoft.EntityFrameworkCore.DbContext> is `_catalogContext` and the second `DbContext` is within the `_integrationEventLogService` object.</span></span> <span data-ttu-id="76eeb-120">Действие фиксации выполняется во всех объектах `DbContext` с помощью стратегии выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="76eeb-120">The Commit action is performed across all `DbContext` objects using an EF execution strategy.</span></span>

<span data-ttu-id="76eeb-121">Для достижения такой фиксации нескольких `DbContext` `SaveEventAndCatalogContextChangesAsync` использует класс `ResilientTransaction`, как показано в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="76eeb-121">To achieve this multiple `DbContext` commit, the `SaveEventAndCatalogContextChangesAsync` uses a `ResilientTransaction` class, as shown in the following code:</span></span>

```csharp
public class CatalogIntegrationEventService : ICatalogIntegrationEventService
{
    //…
    public async Task SaveEventAndCatalogContextChangesAsync(
        IntegrationEvent evt)
    {
        // Use of an EF Core resiliency strategy when using multiple DbContexts
        // within an explicit BeginTransaction():
        // https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
        await ResilientTransaction.New(_catalogContext).ExecuteAsync(async () =>
        {
            // Achieving atomicity between original catalog database 
            // operation and the IntegrationEventLog thanks to a local transaction
            await _catalogContext.SaveChangesAsync();
            await _eventLogService.SaveEventAsync(evt,
                _catalogContext.Database.CurrentTransaction.GetDbTransaction());
        });
    }
}
```

<span data-ttu-id="76eeb-122">Метод `ResilientTransaction.ExecuteAsync`, по сути, начинает транзакцию из переданного `DbContext` (`_catalogContext`) и затем указывает `EventLogService` использовать эту транзакцию для сохранения изменений из `IntegrationEventLogContext`, после чего фиксирует всю транзакцию.</span><span class="sxs-lookup"><span data-stu-id="76eeb-122">The `ResilientTransaction.ExecuteAsync` method basically begins a transaction from the passed `DbContext` (`_catalogContext`) and then makes the `EventLogService` use that transaction to save changes from the `IntegrationEventLogContext` and then commits the whole transaction.</span></span>

```csharp
public class ResilientTransaction
{
    private DbContext _context;
    private ResilientTransaction(DbContext context) =>
        _context = context ?? throw new ArgumentNullException(nameof(context));

    public static ResilientTransaction New (DbContext context) =>
        new ResilientTransaction(context);

    public async Task ExecuteAsync(Func<Task> action)
    {
        // Use of an EF Core resiliency strategy when using multiple DbContexts 
        // within an explicit BeginTransaction():
        // https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
        var strategy = _context.Database.CreateExecutionStrategy();
        await strategy.ExecuteAsync(async () =>
        {
            using (var transaction = _context.Database.BeginTransaction())
            {
                await action();
                transaction.Commit();
            }
        });
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="76eeb-123">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="76eeb-123">Additional resources</span></span>

- <span data-ttu-id="76eeb-124">**Устойчивость подключений и перехват команд c помощью EF в приложении ASP.NET MVC** </span><span class="sxs-lookup"><span data-stu-id="76eeb-124">**Connection Resiliency and Command Interception with EF in an ASP.NET MVC Application** </span></span>\
  [https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)

- <span data-ttu-id="76eeb-125">**Сезар де ла Торре (Cesar de la Torre). Using Resilient Entity Framework Core SQL Connections and Transactions** \ (Использование устойчивых SQL-подключений Entity Framework Core и транзакций)</span><span class="sxs-lookup"><span data-stu-id="76eeb-125">**Cesar de la Torre. Using Resilient Entity Framework Core SQL Connections and Transactions** \</span></span>
  <https://devblogs.microsoft.com/cesardelatorre/using-resilient-entity-framework-core-sql-connections-and-transactions-retries-with-exponential-backoff/>

>[!div class="step-by-step"]
><span data-ttu-id="76eeb-126">[Назад](implement-retries-exponential-backoff.md)
>[Вперед](explore-custom-http-call-retries-exponential-backoff.md)</span><span class="sxs-lookup"><span data-stu-id="76eeb-126">[Previous](implement-retries-exponential-backoff.md)
[Next](explore-custom-http-call-retries-exponential-backoff.md)</span></span>