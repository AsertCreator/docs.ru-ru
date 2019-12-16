---
title: "Операторы преобразования (Руководство по программированию в C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- C# language, conversion operators
- conversion operators [C#]
- operators [C#], conversion
- user-defined conversions [C#]
ms.assetid: c5ad73a3-d57b-4d2b-b4c9-24e3c2856efc
caps.latest.revision: 22
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: c12fd13d6526d79363f973ce2a944c4823bf4104
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="conversion-operators-c-programming-guide"></a><span data-ttu-id="3f324-102">Операторы преобразования (Руководство по программированию в C#)</span><span class="sxs-lookup"><span data-stu-id="3f324-102">Conversion Operators (C# Programming Guide)</span></span>
<span data-ttu-id="3f324-103">Разработчики на C# могут объявлять преобразования классов или структур в другие классы, структуры или базовые типы и обратно.</span><span class="sxs-lookup"><span data-stu-id="3f324-103">C# enables programmers to declare conversions on classes or structs so that classes or structs can be converted to and/or from other classes or structs, or basic types.</span></span> <span data-ttu-id="3f324-104">Преобразования определяются как операторы и называются по имени типа, в который осуществляется преобразование.</span><span class="sxs-lookup"><span data-stu-id="3f324-104">Conversions are defined like operators and are named for the type to which they convert.</span></span> <span data-ttu-id="3f324-105">В качестве содержащего типа должен выступать либо тип преобразуемого аргумента, либо тип результата преобразования, но не оба эти типа.</span><span class="sxs-lookup"><span data-stu-id="3f324-105">Either the type of the argument to be converted, or the type of the result of the conversion, but not both, must be the containing type.</span></span>  
  
 <span data-ttu-id="3f324-106">[!code-cs[csProgGuideStatements#10](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/conversion-operators_1.cs)]</span><span class="sxs-lookup"><span data-stu-id="3f324-106">[!code-cs[csProgGuideStatements#10](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/conversion-operators_1.cs)]</span></span>  
  
## <a name="conversion-operators-overview"></a><span data-ttu-id="3f324-107">Обзор операторов преобразования</span><span class="sxs-lookup"><span data-stu-id="3f324-107">Conversion Operators Overview</span></span>  
 <span data-ttu-id="3f324-108">Операторы преобразования имеют следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="3f324-108">Conversion operators have the following properties:</span></span>  
  
-   <span data-ttu-id="3f324-109">Преобразования, объявленные как `implicit`, выполняются автоматически при необходимости.</span><span class="sxs-lookup"><span data-stu-id="3f324-109">Conversions declared as `implicit` occur automatically when it is required.</span></span>  
  
-   <span data-ttu-id="3f324-110">Преобразования, объявленные как `explicit`, требуют вызова приведения.</span><span class="sxs-lookup"><span data-stu-id="3f324-110">Conversions declared as `explicit` require a cast to be called.</span></span>  
  
-   <span data-ttu-id="3f324-111">Все преобразования должны объявляться как `static`.</span><span class="sxs-lookup"><span data-stu-id="3f324-111">All conversions must be declared as `static`.</span></span>  
  
## <a name="related-sections"></a><span data-ttu-id="3f324-112">Связанные разделы</span><span class="sxs-lookup"><span data-stu-id="3f324-112">Related Sections</span></span>  
 <span data-ttu-id="3f324-113">Дополнительные сведения:</span><span class="sxs-lookup"><span data-stu-id="3f324-113">For more information:</span></span>  
  
-   [<span data-ttu-id="3f324-114">Использование операторов преобразования</span><span class="sxs-lookup"><span data-stu-id="3f324-114">Using Conversion Operators</span></span>](../../../csharp/programming-guide/statements-expressions-operators/using-conversion-operators.md)  
  
-   [<span data-ttu-id="3f324-115">Приведение и преобразование типов</span><span class="sxs-lookup"><span data-stu-id="3f324-115">Casting and Type Conversions</span></span>](../../../csharp/programming-guide/types/casting-and-type-conversions.md)  
  
-   [<span data-ttu-id="3f324-116">Практическое руководство. Реализация определенных пользователем преобразований между структурами</span><span class="sxs-lookup"><span data-stu-id="3f324-116">How to: Implement User-Defined Conversions Between Structs</span></span>](../../../csharp/programming-guide/statements-expressions-operators/how-to-implement-user-defined-conversions-between-structs.md)  
  
-   [<span data-ttu-id="3f324-117">explicit</span><span class="sxs-lookup"><span data-stu-id="3f324-117">explicit</span></span>](../../../csharp/language-reference/keywords/explicit.md)  
  
-   [<span data-ttu-id="3f324-118">implicit</span><span class="sxs-lookup"><span data-stu-id="3f324-118">implicit</span></span>](../../../csharp/language-reference/keywords/implicit.md)  
  
-   [<span data-ttu-id="3f324-119">static</span><span class="sxs-lookup"><span data-stu-id="3f324-119">static</span></span>](../../../csharp/language-reference/keywords/static.md)  
  
## <a name="see-also"></a><span data-ttu-id="3f324-120">См. также</span><span class="sxs-lookup"><span data-stu-id="3f324-120">See Also</span></span>  
 <span data-ttu-id="3f324-121"><xref:System.Convert></span><span class="sxs-lookup"><span data-stu-id="3f324-121"><xref:System.Convert></span></span>   
 <span data-ttu-id="3f324-122">[Руководство по программированию на C#](../../../csharp/programming-guide/index.md) </span><span class="sxs-lookup"><span data-stu-id="3f324-122">[C# Programming Guide](../../../csharp/programming-guide/index.md) </span></span>  
 [<span data-ttu-id="3f324-123">Связанные пользовательские явные преобразования в C#</span><span class="sxs-lookup"><span data-stu-id="3f324-123">Chained user-defined explicit conversions in C#</span></span>](http://go.microsoft.com/fwlink/?LinkId=112384)
