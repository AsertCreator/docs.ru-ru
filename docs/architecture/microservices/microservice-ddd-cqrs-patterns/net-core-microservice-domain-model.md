---
title: Реализация модели предметной области микрослужбы с помощью .NET Core
description: Архитектура микрослужб .NET для контейнерных приложений .NET | Сведения о реализации модели предметной области, ориентированной на DDD.
ms.date: 10/08/2018
ms.openlocfilehash: b2ad62c2a16dd3993b9624ec14f0070e934ac2de
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68676591"
---
# <a name="implement-a-microservice-domain-model-with-net-core"></a><span data-ttu-id="e14e4-103">Реализация модели предметной области микрослужбы с помощью .NET Core</span><span class="sxs-lookup"><span data-stu-id="e14e4-103">Implement a microservice domain model with .NET Core</span></span>

<span data-ttu-id="e14e4-104">В предыдущем разделе были представлены основные принципы и шаблоны для проектирования модели предметной области.</span><span class="sxs-lookup"><span data-stu-id="e14e4-104">In the previous section, the fundamental design principles and patterns for designing a domain model were explained.</span></span> <span data-ttu-id="e14e4-105">Теперь настало время рассмотреть возможные способы реализации модели предметной области с помощью .NET Core (обычного кода C\#) и EF Core.</span><span class="sxs-lookup"><span data-stu-id="e14e4-105">Now it is time to explore possible ways to implement the domain model by using .NET Core (plain C\# code) and EF Core.</span></span> <span data-ttu-id="e14e4-106">Обратите внимание на то, что модель предметной области будет состоять только из вашего кода.</span><span class="sxs-lookup"><span data-stu-id="e14e4-106">Note that your domain model will be composed simply of your code.</span></span> <span data-ttu-id="e14e4-107">В ней будут реализованы требования модели EF Core, но не реальные зависимости от EF.</span><span class="sxs-lookup"><span data-stu-id="e14e4-107">It will have just the EF Core model requirements, but not real dependencies on EF.</span></span> <span data-ttu-id="e14e4-108">В модели предметной области не должно быть строгих зависимостей или ссылок на EF Core или любой другой сопоставитель ORM.</span><span class="sxs-lookup"><span data-stu-id="e14e4-108">You should not have hard dependencies or references to EF Core or any other ORM in your domain model.</span></span>

## <a name="domain-model-structure-in-a-custom-net-standard-library"></a><span data-ttu-id="e14e4-109">Структура модели предметной области в пользовательской библиотеке .NET Standard</span><span class="sxs-lookup"><span data-stu-id="e14e4-109">Domain model structure in a custom .NET Standard Library</span></span>

<span data-ttu-id="e14e4-110">Структура папок в образце приложения eShopOnContainers демонстрирует модель DDD для приложения.</span><span class="sxs-lookup"><span data-stu-id="e14e4-110">The folder organization used for the eShopOnContainers reference application demonstrates the DDD model for the application.</span></span> <span data-ttu-id="e14e4-111">В вашем случае требованиям проекта более точно может отвечать другая структура папок.</span><span class="sxs-lookup"><span data-stu-id="e14e4-111">You might find that a different folder organization more clearly communicates the design choices made for your application.</span></span> <span data-ttu-id="e14e4-112">Как видно на рис. 7-10, в модели предметной области размещения заказов имеются два агрегата: агрегат заказа и агрегат покупателя.</span><span class="sxs-lookup"><span data-stu-id="e14e4-112">As you can see in Figure 7-10, in the ordering domain model there are two aggregates, the order aggregate and the buyer aggregate.</span></span> <span data-ttu-id="e14e4-113">Каждый агрегат представляет собой группу сущностей предметной области и объектов значений, хотя агрегат может состоять и из одной сущности предметной области (корневой сущности агрегата).</span><span class="sxs-lookup"><span data-stu-id="e14e4-113">Each aggregate is a group of domain entities and value objects, although you could have an aggregate composed of a single domain entity (the aggregate root or root entity) as well.</span></span>

![<span data-ttu-id="e14e4-114">Представление обозревателя решений для проекта Ordering.Domain, в котором отображается папка AggregatesModel, содержащая папки BuyerAggregate и OrderAggregate, каждая из которых содержит классы сущности, файлы объектов значений и т. д.</span><span class="sxs-lookup"><span data-stu-id="e14e4-114">The Solution Explorer view for the Ordering.Domain project, showing the AggregatesModel folder containing the BuyerAggregate and OrderAggregate folders, each one containing it's entity classes, value object files and so on.</span></span> ](./media/image11.png)

<span data-ttu-id="e14e4-115">**Рис. 7-10**.</span><span class="sxs-lookup"><span data-stu-id="e14e4-115">**Figure 7-10**.</span></span> <span data-ttu-id="e14e4-116">Структура модели предметной области для микрослужбы размещения заказов в eShopOnContainers</span><span class="sxs-lookup"><span data-stu-id="e14e4-116">Domain model structure for the ordering microservice in eShopOnContainers</span></span>

<span data-ttu-id="e14e4-117">Кроме того, уровень модели предметной области включает в себя контракты репозиториев (интерфейсы), которые представляют требования к инфраструктуре, предъявляемые моделью предметной области.</span><span class="sxs-lookup"><span data-stu-id="e14e4-117">Additionally, the domain model layer includes the repository contracts (interfaces) that are the infrastructure requirements of your domain model.</span></span> <span data-ttu-id="e14e4-118">Иными словами, эти интерфейсы описывают то, какие репозитории и методы должен реализовывать уровень инфраструктуры.</span><span class="sxs-lookup"><span data-stu-id="e14e4-118">In other words, these interfaces express what repositories and the methods the infrastructure layer must implement.</span></span> <span data-ttu-id="e14e4-119">Важным требованием является реализация репозиториев вне уровня модели предметной области, а именно в библиотеке уровня инфраструктуры, чтобы уровень модели предметной области не "засорялся" интерфейсами API или классами, связанными с технологиями инфраструктуры, такими как Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e14e4-119">It is critical that the implementation of the repositories be placed outside of the domain model layer, in the infrastructure layer library, so the domain model layer is not “contaminated” by API or classes from infrastructure technologies, like Entity Framework.</span></span>

<span data-ttu-id="e14e4-120">Обратите также внимание на папку [SeedWork](https://martinfowler.com/bliki/Seedwork.html), содержащую пользовательские базовые классы, которые можно использовать в качестве основы для сущностей предметной области и объектов значений, чтобы избежать избыточного кода в каждом классе объекта предметной области.</span><span class="sxs-lookup"><span data-stu-id="e14e4-120">You can also see a [SeedWork](https://martinfowler.com/bliki/Seedwork.html) folder that contains custom base classes that you can use as a base for your domain entities and value objects, so you do not have redundant code in each domain’s object class.</span></span>

## <a name="structure-aggregates-in-a-custom-net-standard-library"></a><span data-ttu-id="e14e4-121">Структура агрегатов в пользовательской библиотеке .NET Standard</span><span class="sxs-lookup"><span data-stu-id="e14e4-121">Structure aggregates in a custom .NET Standard library</span></span>

<span data-ttu-id="e14e4-122">Под агрегатом понимается совокупность объектов предметной области, сгруппированных с целью обеспечения согласованности транзакций.</span><span class="sxs-lookup"><span data-stu-id="e14e4-122">An aggregate refers to a cluster of domain objects grouped together to match transactional consistency.</span></span> <span data-ttu-id="e14e4-123">Эти объекты могут представлять собой сущности (одна из которых является корневой сущностью агрегата), а также дополнительные объекты значений.</span><span class="sxs-lookup"><span data-stu-id="e14e4-123">Those objects could be instances of entities (one of which is the aggregate root or root entity) plus any additional value objects.</span></span>

<span data-ttu-id="e14e4-124">Согласованность транзакций гарантирует согласованность и актуальность агрегата по завершении бизнес-действия.</span><span class="sxs-lookup"><span data-stu-id="e14e4-124">Transactional consistency means that an aggregate is guaranteed to be consistent and up to date at the end of a business action.</span></span> <span data-ttu-id="e14e4-125">Например, агрегат заказа из модели предметной области микрослужбы размещения заказов eShopOnContainers состоит из компонентов, показанных на рис. 7-11.</span><span class="sxs-lookup"><span data-stu-id="e14e4-125">For example, the order aggregate from the eShopOnContainers ordering microservice domain model is composed as shown in Figure 7-11.</span></span>

![Подробное представление папки OrderAggregate: Address.cs — объект значения, IOrderRepository — интерфейс репозитория, Order.cs — корень агрегации, OrderItem.cs — дочерняя сущность, а OrderStatus.cs — класс перечисления.](./media/image12.png)

<span data-ttu-id="e14e4-127">**Рис. 7-11**.</span><span class="sxs-lookup"><span data-stu-id="e14e4-127">**Figure 7-11**.</span></span> <span data-ttu-id="e14e4-128">Агрегат заказа в решении Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14e4-128">The order aggregate in Visual Studio solution</span></span>

<span data-ttu-id="e14e4-129">Если открыть любой файл в папке агрегата, можно увидеть, что он помечен как пользовательский базовый класс или интерфейс, например сущность или объект значения. Пример реализации см. в папке [SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork).</span><span class="sxs-lookup"><span data-stu-id="e14e4-129">If you open any of the files in an aggregate folder, you can see how it is marked as either a custom base class or interface, like entity or value object, as implemented in the [SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) folder.</span></span>

## <a name="implement-domain-entities-as-poco-classes"></a><span data-ttu-id="e14e4-130">Реализация сущностей предметной области в виде классов POCO</span><span class="sxs-lookup"><span data-stu-id="e14e4-130">Implement domain entities as POCO classes</span></span>

<span data-ttu-id="e14e4-131">Модель предметной области реализуется в .NET путем создания классов POCO, которые реализуют сущности предметной области.</span><span class="sxs-lookup"><span data-stu-id="e14e4-131">You implement a domain model in .NET by creating POCO classes that implement your domain entities.</span></span> <span data-ttu-id="e14e4-132">В приведенном ниже примере класс Order определен как сущность, а также как корневая сущность агрегата.</span><span class="sxs-lookup"><span data-stu-id="e14e4-132">In the following example, the Order class is defined as an entity and also as an aggregate root.</span></span> <span data-ttu-id="e14e4-133">Так как класс Order является производным от базового класса Entity, в нем можно использовать код, общий для всех сущностей.</span><span class="sxs-lookup"><span data-stu-id="e14e4-133">Because the Order class derives from the Entity base class, it can reuse common code related to entities.</span></span> <span data-ttu-id="e14e4-134">Имейте в виду, что эти базовые классы и интерфейсы определяются вами в проекте модели предметной области, поэтому это ваш код, а не код инфраструктуры из ORM, например EF.</span><span class="sxs-lookup"><span data-stu-id="e14e4-134">Bear in mind that these base classes and interfaces are defined by you in the domain model project, so it is your code, not infrastructure code from an ORM like EF.</span></span>

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE 2.0
// Entity is a custom base class with the ID
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId;

    public OrderStatus OrderStatus { get; private set; }
    private int _orderStatusId;

    private string _description;
    private int? _paymentMethodId;

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    public Order(string userId, Address address, int cardTypeId, string cardNumber, string cardSecurityNumber,
            string cardHolderName, DateTime cardExpiration, int? buyerId = null, int? paymentMethodId = null)
    {
        _orderItems = new List<OrderItem>();
        _buyerId = buyerId;
        _paymentMethodId = paymentMethodId;
        _orderStatusId = OrderStatus.Submitted.Id;
        _orderDate = DateTime.UtcNow;
        Address = address;

        // ...Additional code ...
    }

    public void AddOrderItem(int productId, string productName,
                            decimal unitPrice, decimal discount,
                            string pictureUrl, int units = 1)
    {
        //...
        // Domain rules/logic for adding the OrderItem to the order
        // ...

        var orderItem = new OrderItem(productId, productName, unitPrice, discount, pictureUrl, units);

        _orderItems.Add(orderItem);

    }
    // ...
    // Additional methods with domain rules/logic related to the Order aggregate
    // ...
}
```

<span data-ttu-id="e14e4-135">Важно отметить, что это сущность предметной области, реализованная в виде класса POCO.</span><span class="sxs-lookup"><span data-stu-id="e14e4-135">It is important to note that this is a domain entity implemented as a POCO class.</span></span> <span data-ttu-id="e14e4-136">Она не имеет прямой зависимости от Entity Framework Core или любой другой платформы инфраструктуры.</span><span class="sxs-lookup"><span data-stu-id="e14e4-136">It does not have any direct dependency on Entity Framework Core or any other infrastructure framework.</span></span> <span data-ttu-id="e14e4-137">Именно такая реализация должна использоваться в DDD — просто код C\#, реализующий модель предметной области.</span><span class="sxs-lookup"><span data-stu-id="e14e4-137">This implementation is as it should be in DDD, just C\# code implementing a domain model.</span></span>

<span data-ttu-id="e14e4-138">Кроме того, класс снабжен интерфейсом с именем IAggregateRoot.</span><span class="sxs-lookup"><span data-stu-id="e14e4-138">In addition, the class is decorated with an interface named IAggregateRoot.</span></span> <span data-ttu-id="e14e4-139">Это пустой интерфейс, иногда называемый *интерфейсом-маркером*, который служит лишь для указания на то, что этот класс сущности также является корневой сущностью агрегата.</span><span class="sxs-lookup"><span data-stu-id="e14e4-139">That interface is an empty interface, sometimes called a *marker interface*, that is used just to indicate that this entity class is also an aggregate root.</span></span>

<span data-ttu-id="e14e4-140">Интерфейсы-маркеры порой считают антишаблонами; однако они также позволяют ясно помечать классы, особенно если интерфейс может развиваться.</span><span class="sxs-lookup"><span data-stu-id="e14e4-140">A marker interface is sometimes considered as an anti-pattern; however, it is also a clean way to mark a class, especially when that interface might be evolving.</span></span> <span data-ttu-id="e14e4-141">В качестве маркеров можно также применять атрибуты, однако базовый класс (Entity) проще заметить рядом с интерфейсом IAggregate, чем атрибут-маркер Aggregate, размещенный над классом.</span><span class="sxs-lookup"><span data-stu-id="e14e4-141">An attribute could be the other choice for the marker, but it is quicker to see the base class (Entity) next to the IAggregate interface instead of putting an Aggregate attribute marker above the class.</span></span> <span data-ttu-id="e14e4-142">В любом случае это зависит от предпочтений.</span><span class="sxs-lookup"><span data-stu-id="e14e4-142">It is a matter of preferences, in any case.</span></span>

<span data-ttu-id="e14e4-143">Наличие корневой сущности агрегата означает, что большая часть кода, связанного с обеспечением согласованности и бизнес-правилами сущностей агрегата, должна реализовываться в виде методов класса корневой сущности агрегата Order (например, AddOrderItem при добавлении объекта OrderItem в агрегат).</span><span class="sxs-lookup"><span data-stu-id="e14e4-143">Having an aggregate root means that most of the code related to consistency and business rules of the aggregate’s entities should be implemented as methods in the Order aggregate root class (for example, AddOrderItem when adding an OrderItem object to the aggregate).</span></span> <span data-ttu-id="e14e4-144">Объекты OrderItems не следует создавать или изменять отдельно или напрямую; класс AggregateRoot должен контролировать все операции изменения его дочерних сущностей и поддерживать их согласованность.</span><span class="sxs-lookup"><span data-stu-id="e14e4-144">You should not create or update OrderItems objects independently or directly; the AggregateRoot class must keep control and consistency of any update operation against its child entities.</span></span>

## <a name="encapsulate-data-in-the-domain-entities"></a><span data-ttu-id="e14e4-145">Инкапсуляция данных в сущностях предметной области</span><span class="sxs-lookup"><span data-stu-id="e14e4-145">Encapsulate data in the Domain Entities</span></span>

<span data-ttu-id="e14e4-146">Распространенной проблемой моделей сущностей является то, что они предоставляют свойства навигации коллекции как общедоступные типы списка.</span><span class="sxs-lookup"><span data-stu-id="e14e4-146">A common problem in entity models is that they expose collection navigation properties as publicly accessible list types.</span></span> <span data-ttu-id="e14e4-147">Это позволяет любому разработчику управлять содержимым этих типов в коллекции в процессе совместной работы. В результате могут обходиться важные бизнес-правила, связанные с коллекцией, из-за чего объект может оказаться в недопустимом состоянии.</span><span class="sxs-lookup"><span data-stu-id="e14e4-147">This allows any collaborator developer to manipulate the contents of these collection types, which may bypass important business rules related to the collection, possibly leaving the object in an invalid state.</span></span> <span data-ttu-id="e14e4-148">Решение заключается в предоставлении доступа только для чтения к связанным коллекциям и явном предоставлении методов, которые определяют возможные способы работы клиентов с этими коллекциями.</span><span class="sxs-lookup"><span data-stu-id="e14e4-148">The solution to this is to expose read-only access to related collections and explicitly provide methods that define ways in which clients can manipulate them.</span></span>

<span data-ttu-id="e14e4-149">В предыдущем коде обратите внимание на то, что многие атрибуты доступны только для чтения или являются закрытыми и могут изменяться только методами класса, поэтому при любом изменении учитываются инварианты и логика предметной области, определенные в методах класса.</span><span class="sxs-lookup"><span data-stu-id="e14e4-149">In the previous code, note that many attributes are read-only or private and are only updatable by the class methods, so any update considers business domain invariants and logic specified within the class methods.</span></span>

<span data-ttu-id="e14e4-150">Например, согласно шаблонам DDD ***не* следует выполнять следующую операцию** из любого обработчика команд или класса прикладного уровня (фактически, сделать это будет невозможно):</span><span class="sxs-lookup"><span data-stu-id="e14e4-150">For example, following DDD patterns, **you should *not* do the following** from any command handler method or application layer class (actually, it should be impossible for you to do so):</span></span>

```csharp
// WRONG ACCORDING TO DDD PATTERNS – CODE AT THE APPLICATION LAYER OR
// COMMAND HANDLERS
// Code in command handler methods or Web API controllers
//... (WRONG) Some code with business logic out of the domain classes ...
OrderItem myNewOrderItem = new OrderItem(orderId, productId, productName,
    pictureUrl, unitPrice, discount, units);

//... (WRONG) Accessing the OrderItems collection directly from the application layer // or command handlers
myOrder.OrderItems.Add(myNewOrderItem);
//...
```

<span data-ttu-id="e14e4-151">В этом случае метод Add представляет собой исключительно операцию добавления данных с прямым доступом к коллекции OrderItems.</span><span class="sxs-lookup"><span data-stu-id="e14e4-151">In this case, the Add method is purely an operation to add data, with direct access to the OrderItems collection.</span></span> <span data-ttu-id="e14e4-152">Поэтому большая часть логики предметной области, правил или проверок, связанных с этой операцией с дочерними сущностями, будет распределена по прикладному уровню (обработчикам команд и контроллерам веб-интерфейса API).</span><span class="sxs-lookup"><span data-stu-id="e14e4-152">Therefore, most of the domain logic, rules, or validations related to that operation with the child entities will be spread across the application layer (command handlers and Web API controllers).</span></span>

<span data-ttu-id="e14e4-153">Если обойти корневую сущность агрегата, ее инвариантность, допустимость или согласованность не может быть гарантирована.</span><span class="sxs-lookup"><span data-stu-id="e14e4-153">If you go around the aggregate root, the aggregate root cannot guarantee its invariants, its validity, or its consistency.</span></span> <span data-ttu-id="e14e4-154">В конечном итоге получится спагетти-код или код транзакционного скрипта.</span><span class="sxs-lookup"><span data-stu-id="e14e4-154">Eventually you will have spaghetti code or transactional script code.</span></span>

<span data-ttu-id="e14e4-155">Если следовать шаблонам DDD, сущности не должны иметь открытых методов задания ни в одном свойстве сущности.</span><span class="sxs-lookup"><span data-stu-id="e14e4-155">To follow DDD patterns, entities must not have public setters in any entity property.</span></span> <span data-ttu-id="e14e4-156">Изменения в сущности должны производиться явными методами, в которых с помощью единого языка формулируются производимые ими изменения.</span><span class="sxs-lookup"><span data-stu-id="e14e4-156">Changes in an entity should be driven by explicit methods with explicit ubiquitous language about the change they are performing in the entity.</span></span>

<span data-ttu-id="e14e4-157">Кроме того, коллекции в сущности (например, элементы заказа) должны быть свойствами, доступными только для чтения (см. описание метода AsReadOnly далее).</span><span class="sxs-lookup"><span data-stu-id="e14e4-157">Furthermore, collections within the entity (like the order items) should be read-only properties (the AsReadOnly method explained later).</span></span> <span data-ttu-id="e14e4-158">Возможность их изменения должна быть доступна только из методов класса корневой сущности агрегата или из методов дочерних сущностей.</span><span class="sxs-lookup"><span data-stu-id="e14e4-158">You should be able to update it only from within the aggregate root class methods or the child entity methods.</span></span>

<span data-ttu-id="e14e4-159">Как видно в коде корневой сущности агрегата Order, все методы задания должны быть закрытыми либо по крайней мере доступными только для чтения извне, чтобы любая операция с данными сущности или ее дочерними сущностями выполнялась посредством методов в классе сущности.</span><span class="sxs-lookup"><span data-stu-id="e14e4-159">As you can see in the code for the Order aggregate root, all setters should be private or at least read-only externally, so that any operation against the entity’s data or its child entities has to be performed through methods in the entity class.</span></span> <span data-ttu-id="e14e4-160">Это позволяет обеспечивать согласованность контролируемым и объектно-ориентированным образом вместо реализации кода транзакционного скрипта.</span><span class="sxs-lookup"><span data-stu-id="e14e4-160">This maintains consistency in a controlled and object-oriented way instead of implementing transactional script code.</span></span>

<span data-ttu-id="e14e4-161">В приведенном ниже фрагменте кода показан правильный способ реализации задачи для добавления объекта OrderItem в агрегат Order.</span><span class="sxs-lookup"><span data-stu-id="e14e4-161">The following code snippet shows the proper way to code the task of adding an OrderItem object to the Order aggregate.</span></span>

```csharp
// RIGHT ACCORDING TO DDD--CODE AT THE APPLICATION LAYER OR COMMAND HANDLERS
// The code in command handlers or WebAPI controllers, related only to application stuff
// There is NO code here related to OrderItem object’s business logic
myOrder.AddOrderItem(productId, productName, pictureUrl, unitPrice, discount, units);

// The code related to OrderItem params validations or domain rules should
// be WITHIN the AddOrderItem method.

//...
```

<span data-ttu-id="e14e4-162">В этом фрагменте большая часть проверок и логики, связанных с созданием объекта OrderItem, контролируется корневой сущностью агрегата Order в методе AddOrderItem, в частности, проверки и логика, связанные с другими элементами агрегата.</span><span class="sxs-lookup"><span data-stu-id="e14e4-162">In this snippet, most of the validations or logic related to the creation of an OrderItem object will be under the control of the Order aggregate root—in the AddOrderItem method—especially validations and logic related to other elements in the aggregate.</span></span> <span data-ttu-id="e14e4-163">Например, один и тот же элемент продукта может быть получен в результате нескольких вызовов метода AddOrderItem.</span><span class="sxs-lookup"><span data-stu-id="e14e4-163">For instance, you might get the same product item as the result of multiple calls to AddOrderItem.</span></span> <span data-ttu-id="e14e4-164">В этом методе можно проверять элементы продуктов и объединять одинаковые элементы в один объект OrderItem с несколькими единицами.</span><span class="sxs-lookup"><span data-stu-id="e14e4-164">In that method, you could examine the product items and consolidate the same product items into a single OrderItem object with several units.</span></span> <span data-ttu-id="e14e4-165">Кроме того, если имеется несколько разных размеров скидок, но идентификатор продукта один и тот же, скорее всего, следует применить наибольшую скидку.</span><span class="sxs-lookup"><span data-stu-id="e14e4-165">Additionally, if there are different discount amounts but the product ID is the same, you would likely apply the higher discount.</span></span> <span data-ttu-id="e14e4-166">Этот принцип применим к любой другой логике предметной области для объекта OrderItem.</span><span class="sxs-lookup"><span data-stu-id="e14e4-166">This principle applies to any other domain logic for the OrderItem object.</span></span>

<span data-ttu-id="e14e4-167">Кроме того, операция new OrderItem(params) также будет контролироваться и выполняться методом AddOrderItem из корневой сущности агрегата Order.</span><span class="sxs-lookup"><span data-stu-id="e14e4-167">In addition, the new OrderItem(params) operation will also be controlled and performed by the AddOrderItem method from the Order aggregate root.</span></span> <span data-ttu-id="e14e4-168">Поэтому большая часть логики или проверок, связанных с этой операцией (особенно все, что влияет на согласованность дочерних сущностей), будет находиться в одном месте в корневой сущности агрегата.</span><span class="sxs-lookup"><span data-stu-id="e14e4-168">Therefore, most of the logic or validations related to that operation (especially anything that impacts the consistency between other child entities) will be in a single place within the aggregate root.</span></span> <span data-ttu-id="e14e4-169">Это конечная цель шаблона корневой сущности агрегата.</span><span class="sxs-lookup"><span data-stu-id="e14e4-169">That is the ultimate purpose of the aggregate root pattern.</span></span>

<span data-ttu-id="e14e4-170">В Entity Framework Core 1.1 или более поздней версии сущность DDD можно описать лучше благодаря возможности [сопоставления с полями](https://docs.microsoft.com/ef/core/modeling/backing-field), помимо свойств.</span><span class="sxs-lookup"><span data-stu-id="e14e4-170">When you use Entity Framework Core 1.1 or later, a DDD entity can be better expressed because it allows [mapping to fields](https://docs.microsoft.com/ef/core/modeling/backing-field) in addition to properties.</span></span> <span data-ttu-id="e14e4-171">Это полезно при защите коллекций дочерних сущностей или объектов значений.</span><span class="sxs-lookup"><span data-stu-id="e14e4-171">This is useful when protecting collections of child entities or value objects.</span></span> <span data-ttu-id="e14e4-172">Благодаря этому улучшению вы можете использовать простые закрытые поля вместо свойств и реализовать любое изменение коллекции полей в открытых методах, предоставив доступ только для чтения посредством метода AsReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e14e4-172">With this enhancement, you can use simple private fields instead of properties and you can implement any update to the field collection in public methods and provide read-only access through the AsReadOnly method.</span></span>

<span data-ttu-id="e14e4-173">В рамках DDD изменять сущность желательно только с помощью методов самой сущности (или конструктора), чтобы контролировать инварианты и согласованность данных, поэтому свойства определяются только с помощью метода доступа get.</span><span class="sxs-lookup"><span data-stu-id="e14e4-173">In DDD you want to update the entity only through methods in the entity (or the constructor) in order to control any invariant and the consistency of the data, so properties are defined only with a get accessor.</span></span> <span data-ttu-id="e14e4-174">Свойства поддерживаются закрытыми полями.</span><span class="sxs-lookup"><span data-stu-id="e14e4-174">The properties are backed by private fields.</span></span> <span data-ttu-id="e14e4-175">Закрытые члены доступны только внутри класса.</span><span class="sxs-lookup"><span data-stu-id="e14e4-175">Private members can only be accessed from within the class.</span></span> <span data-ttu-id="e14e4-176">Но есть одно исключение: платформе EF Core необходимо также задать эти поля (поэтому она может возвращать объект с надлежащими значениями).</span><span class="sxs-lookup"><span data-stu-id="e14e4-176">However, there one exception: EF Core needs to set these fields as well (so it can return the object with the proper values).</span></span>

### <a name="map-properties-with-only-get-accessors-to-the-fields-in-the-database-table"></a><span data-ttu-id="e14e4-177">Сопоставление свойств только с методами доступа get с полями в таблице базы данных</span><span class="sxs-lookup"><span data-stu-id="e14e4-177">Map properties with only get accessors to the fields in the database table</span></span>

<span data-ttu-id="e14e4-178">За сопоставление свойств со столбцами в таблице базы данных отвечает не предметная область, а инфраструктура и уровень хранения данных.</span><span class="sxs-lookup"><span data-stu-id="e14e4-178">Mapping properties to database table columns is not a domain responsibility but part of the infrastructure and persistence layer.</span></span> <span data-ttu-id="e14e4-179">Мы упоминаем это здесь, чтобы вы знали о новых возможностях, связанных с моделированием сущностей, в EF Core 1.1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="e14e4-179">We mention this here just so you are aware of the new capabilities in EF Core 1.1 or later related to how you can model entities.</span></span> <span data-ttu-id="e14e4-180">Дополнительные сведения по этой теме приводятся в разделе, посвященном инфраструктуре и хранению данных.</span><span class="sxs-lookup"><span data-stu-id="e14e4-180">Additional details on this topic are explained in the infrastructure and persistence section.</span></span>

<span data-ttu-id="e14e4-181">При использовании EF Core 1.0 или более поздней версии в классе DbContext необходимо сопоставлять свойства, определенные только с методами задания, с фактическими полями в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="e14e4-181">When you use EF Core 1.0 or later, within the DbContext you need to map the properties that are defined only with getters to the actual fields in the database table.</span></span> <span data-ttu-id="e14e4-182">Для этого служит метод HasField класса PropertyBuilder.</span><span class="sxs-lookup"><span data-stu-id="e14e4-182">This is done with the HasField method of the PropertyBuilder class.</span></span>

### <a name="map-fields-without-properties"></a><span data-ttu-id="e14e4-183">Сопоставление полей без свойств</span><span class="sxs-lookup"><span data-stu-id="e14e4-183">Map fields without properties</span></span>

<span data-ttu-id="e14e4-184">Благодаря возможности сопоставления столбцов с полями в EF Core 1.1 и более поздних версиях можно также не использовать свойства.</span><span class="sxs-lookup"><span data-stu-id="e14e4-184">With the feature in EF Core 1.1 or later to map columns to fields, it is also possible to not use properties.</span></span> <span data-ttu-id="e14e4-185">Вместо этого можно просто сопоставить столбцы таблицы с полями.</span><span class="sxs-lookup"><span data-stu-id="e14e4-185">Instead, you can just map columns from a table to fields.</span></span> <span data-ttu-id="e14e4-186">Распространенным сценарием применения этой возможности являются закрытые поля для хранения внутреннего состояния, доступ к которым не должен осуществляться извне сущности.</span><span class="sxs-lookup"><span data-stu-id="e14e4-186">A common use case for this is private fields for an internal state that does not need to be accessed from outside the entity.</span></span>

<span data-ttu-id="e14e4-187">Так, в предыдущем примере кода OrderAggregate есть несколько закрытых полей, таких как `_paymentMethodId`, которые не имеют связанного свойства для метода задания или метода получения.</span><span class="sxs-lookup"><span data-stu-id="e14e4-187">For example, in the preceding OrderAggregate code example, there are several private fields, like the  `_paymentMethodId` field, that have no related property for either a setter or getter.</span></span> <span data-ttu-id="e14e4-188">Такое поле могло бы вычисляться в бизнес-логике заказа и использоваться из методов заказа, но оно также должно сохраняться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e14e4-188">That field could also be calculated within the order’s business logic and used from the order’s methods, but it needs to be persisted in the database as well.</span></span> <span data-ttu-id="e14e4-189">Поэтому в EF Core (начиная с версии 1.1) есть возможность сопоставления поля, с которым не связано свойство, со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e14e4-189">So in EF Core (since v1.1) there is a way to map a field without a related property to a column in the database.</span></span> <span data-ttu-id="e14e4-190">Об этом также рассказывается в разделе [Уровень инфраструктуры](ddd-oriented-microservice.md#the-infrastructure-layer) этого руководства.</span><span class="sxs-lookup"><span data-stu-id="e14e4-190">This is also explained in the [Infrastructure layer](ddd-oriented-microservice.md#the-infrastructure-layer) section of this guide.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="e14e4-191">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e14e4-191">Additional resources</span></span>

- <span data-ttu-id="e14e4-192">**Вон Вернон (Vaughn Vernon). Моделирование агрегатов с помощью DDD и Entity Framework.**</span><span class="sxs-lookup"><span data-stu-id="e14e4-192">**Vaughn Vernon. Modeling Aggregates with DDD and Entity Framework.**</span></span> <span data-ttu-id="e14e4-193">Обратите внимание, что речь идет *не* об Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e14e4-193">Note that this is *not* Entity Framework Core.</span></span> \
  <https://kalele.io/blog-posts/modeling-aggregates-with-ddd-and-entity-framework/>

- <span data-ttu-id="e14e4-194">**Julie Lerman (Джули Лерман). Точки данных — создание кода при проблемно-ориентированном проектировании: советы для разработчиков, ориентированных на данные** </span><span class="sxs-lookup"><span data-stu-id="e14e4-194">**Julie Lerman. Data Points - Coding for Domain-Driven Design: Tips for Data-Focused Devs** </span></span>\
  <https://msdn.microsoft.com/magazine/dn342868.aspx>

- <span data-ttu-id="e14e4-195">**Уди Дахан (Udi Dahan). Создание полностью инкапсулированных моделей предметной области** </span><span class="sxs-lookup"><span data-stu-id="e14e4-195">**Udi Dahan. How to create fully encapsulated Domain Models** </span></span>\
  <http://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

> [!div class="step-by-step"]
> <span data-ttu-id="e14e4-196">[Назад](microservice-domain-model.md)
> [Вперед](seedwork-domain-model-base-classes-interfaces.md)</span><span class="sxs-lookup"><span data-stu-id="e14e4-196">[Previous](microservice-domain-model.md)
[Next](seedwork-domain-model-base-classes-interfaces.md)</span></span>