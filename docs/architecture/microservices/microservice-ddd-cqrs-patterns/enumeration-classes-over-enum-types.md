---
title: Использование классов перечисления вместо типов перечисления
description: Архитектура микрослужб .NET для упакованных в контейнеры приложений .NET | Сведения о том, как можно использовать классы перечисления вместо перечислений для преодоления некоторых ограничений последних.
ms.date: 10/08/2018
ms.openlocfilehash: 10b4c2f7b9f079ed535111e65b8154791f6575cd
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674191"
---
# <a name="use-enumeration-classes-instead-of-enum-types"></a><span data-ttu-id="bcfd1-103">Использование классов перечисления вместо типов перечисления</span><span class="sxs-lookup"><span data-stu-id="bcfd1-103">Use enumeration classes instead of enum types</span></span>

<span data-ttu-id="bcfd1-104">[Перечисления](../../../../docs/csharp/language-reference/keywords/enum.md) (или *типы перечисления*) — это тонкая языковая оболочка вокруг целочисленного типа.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-104">[Enumerations](../../../../docs/csharp/language-reference/keywords/enum.md) (or *enum types* for short) are a thin language wrapper around an integral type.</span></span> <span data-ttu-id="bcfd1-105">Лучше всего использовать их при сохранении одного значения из закрытого набора значений.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-105">You might want to limit their use to when you are storing one value from a closed set of values.</span></span> <span data-ttu-id="bcfd1-106">Хорошим примером является классификация, основанная на размерах (небольшой, средний, большой).</span><span class="sxs-lookup"><span data-stu-id="bcfd1-106">Classification based on sizes (small, medium, large) is a good example.</span></span> <span data-ttu-id="bcfd1-107">Использование перечислений для потока управления или более устойчивых абстракций может быть [признаком плохого кода](https://deviq.com/code-smells/).</span><span class="sxs-lookup"><span data-stu-id="bcfd1-107">Using enums for control flow or more robust abstractions can be a [code smell](https://deviq.com/code-smells/).</span></span> <span data-ttu-id="bcfd1-108">При таком использовании код будет недолговечным, поскольку множество операторов потока управления будут проверять значения перечисления.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-108">This type of usage leads to fragile code with many control flow statements checking values of the enum.</span></span>

<span data-ttu-id="bcfd1-109">Вместо этого можно создавать классы перечисления, позволяющие использовать широкие возможности объектно-ориентированного языка.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-109">Instead, you can create Enumeration classes that enable all the rich features of an object-oriented language.</span></span>

<span data-ttu-id="bcfd1-110">Как правило, это не критично, и для простоты вы можете по-прежнему использовать обычные [типы перечисления](../../../csharp/language-reference/keywords/enum.md), если вам так удобно.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-110">However, this isn't a critical topic and in many cases, for simplicity, you can still use regular [enum types](../../../csharp/language-reference/keywords/enum.md) if that's your preference.</span></span> <span data-ttu-id="bcfd1-111">В любом случае использование классов перечисления больше связано с бизнес-концепциями.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-111">Anyway, the use of enumeration classes is more related to business-related concepts.</span></span>

## <a name="implement-an-enumeration-base-class"></a><span data-ttu-id="bcfd1-112">Применение базового класса перечисления</span><span class="sxs-lookup"><span data-stu-id="bcfd1-112">Implement an Enumeration base class</span></span>

<span data-ttu-id="bcfd1-113">Микрослужба для заказа в eShopOnContainers содержит образец базового класса перечисления, как показано на следующем примере:</span><span class="sxs-lookup"><span data-stu-id="bcfd1-113">The ordering microservice in eShopOnContainers provides a sample Enumeration base class implementation, as shown in the following example:</span></span>

```csharp
public abstract class Enumeration : IComparable
{
    public string Name { get; private set; }

    public int Id { get; private set; }

    protected Enumeration(int id, string name) 
    {
        Id = id; 
        Name = name; 
    }

    public override string ToString() => Name;

    public static IEnumerable<T> GetAll<T>() where T : Enumeration
    {
        var fields = typeof(T).GetFields(BindingFlags.Public | 
                                         BindingFlags.Static | 
                                         BindingFlags.DeclaredOnly); 

        return fields.Select(f => f.GetValue(null)).Cast<T>();
    }

    public override bool Equals(object obj) 
    {
        var otherValue = obj as Enumeration; 

        if (otherValue == null) 
            return false;

        var typeMatches = GetType().Equals(obj.GetType());
        var valueMatches = Id.Equals(otherValue.Id);

        return typeMatches && valueMatches;
    }

    public int CompareTo(object other) => Id.CompareTo(((Enumeration)other).Id); 

    // Other utility methods ... 
}
```

<span data-ttu-id="bcfd1-114">Этот класс можно использовать как тип в любой сущности или любом объекте значения, как в следующем классе `CardType` : `Enumeration`:</span><span class="sxs-lookup"><span data-stu-id="bcfd1-114">You can use this class as a type in any entity or value object, as for the following `CardType` : `Enumeration` class:</span></span>

```csharp
public class CardType : Enumeration
{
    public static CardType Amex = new CardType(1, "Amex");
    public static CardType Visa = new CardType(2, "Visa");
    public static CardType MasterCard = new CardType(3, "MasterCard");

    public CardType(int id, string name)
        : base(id, name)
    {
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="bcfd1-115">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bcfd1-115">Additional resources</span></span>

- <span data-ttu-id="bcfd1-116">**Перечисления нам не друзья (обновление)**  </span><span class="sxs-lookup"><span data-stu-id="bcfd1-116">**Enum’s are evil—update** </span></span>\
  <https://www.planetgeek.ch/2009/07/01/enums-are-evil/>

- <span data-ttu-id="bcfd1-117">**Дэниэл Хардман (Daniel Hardman). Какие болезни переносят перечисления и как их вылечить** </span><span class="sxs-lookup"><span data-stu-id="bcfd1-117">**Daniel Hardman. How Enums Spread Disease — And How To Cure It** </span></span>\
  <https://codecraft.co/2012/10/29/how-enums-spread-disease-and-how-to-cure-it/>

- <span data-ttu-id="bcfd1-118">**Джимми Богард (Jimmy Bogard). Классы перечислений** </span><span class="sxs-lookup"><span data-stu-id="bcfd1-118">**Jimmy Bogard. Enumeration classes** </span></span>\
  <https://lostechies.com/jimmybogard/2008/08/12/enumeration-classes/>

- <span data-ttu-id="bcfd1-119">**Стив Смит (Steve Smith). Альтернативы перечислениям в C#**  </span><span class="sxs-lookup"><span data-stu-id="bcfd1-119">**Steve Smith. Enum Alternatives in C#** </span></span>\
  <https://ardalis.com/enum-alternatives-in-c>

- <span data-ttu-id="bcfd1-120">**Enumeration.cs.**</span><span class="sxs-lookup"><span data-stu-id="bcfd1-120">**Enumeration.cs.**</span></span> <span data-ttu-id="bcfd1-121">Базовый класс перечисления в eShopOnContainers </span><span class="sxs-lookup"><span data-stu-id="bcfd1-121">Base Enumeration class in eShopOnContainers </span></span>\
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/SeedWork/Enumeration.cs>

- <span data-ttu-id="bcfd1-122">**CardType.cs**.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-122">**CardType.cs**.</span></span> <span data-ttu-id="bcfd1-123">Пример класса перечисления в eShopOnContainers.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-123">Sample Enumeration class in eShopOnContainers.</span></span> \
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/BuyerAggregate/CardType.cs>
    
- <span data-ttu-id="bcfd1-124">**SmartEnum**.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-124">**SmartEnum**.</span></span> <span data-ttu-id="bcfd1-125">Ardalis — классы, помогающие создавать более эффективные строго типизированные перечисления в .NET.</span><span class="sxs-lookup"><span data-stu-id="bcfd1-125">Ardalis - Classes to help produce strongly typed smarter enums in .NET.</span></span> \
  <https://www.nuget.org/packages/Ardalis.SmartEnum/>

>[!div class="step-by-step"]
><span data-ttu-id="bcfd1-126">[Назад](implement-value-objects.md)
>[Вперед](domain-model-layer-validations.md)</span><span class="sxs-lookup"><span data-stu-id="bcfd1-126">[Previous](implement-value-objects.md)
[Next](domain-model-layer-validations.md)</span></span>