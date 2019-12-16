---
title: "Оператор &amp;&amp; (справочник по C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- '&&_CSharpKeyword'
dev_langs:
- CSharp
helpviewer_keywords:
- '&& operator [C#]'
- logical AND operator [C#]
ms.assetid: 2e4f0a1c-92a3-40f8-8e3b-17b607f20c31
caps.latest.revision: 18
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 1da61947cbc85e5b3045513e99e013d1e4fae4b3
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="ampamp-operator-c-reference"></a><span data-ttu-id="2e84a-102">Оператор &amp;&amp; (справочник по C#)</span><span class="sxs-lookup"><span data-stu-id="2e84a-102">&amp;&amp; Operator (C# Reference)</span></span>
<span data-ttu-id="2e84a-103">Условный оператор И (`&&`) выполняет логическую операцию И применительно к своим операндам типа `bool`, но вычисляет только второй операнд при необходимости.</span><span class="sxs-lookup"><span data-stu-id="2e84a-103">The conditional-AND operator (`&&`) performs a logical-AND of its `bool` operands, but only evaluates its second operand if necessary.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="2e84a-104">Примечания</span><span class="sxs-lookup"><span data-stu-id="2e84a-104">Remarks</span></span>  
 <span data-ttu-id="2e84a-105">Операция</span><span class="sxs-lookup"><span data-stu-id="2e84a-105">The operation</span></span>  
  
```  
x && y  
```  
  
 <span data-ttu-id="2e84a-106">соответствует операции</span><span class="sxs-lookup"><span data-stu-id="2e84a-106">corresponds to the operation</span></span>  
  
```  
x & y  
```  
  
 <span data-ttu-id="2e84a-107">за исключением того, что если `x` имеет значение `false`, `y` не вычисляется, так как результат операции И — `false` независимо от того, какое значение имеет `y`.</span><span class="sxs-lookup"><span data-stu-id="2e84a-107">except that if `x` is `false`, `y` is not evaluated, because the result of the AND operation is `false` no matter what the value of `y`  is.</span></span> <span data-ttu-id="2e84a-108">Это называется сокращенным вычислением.</span><span class="sxs-lookup"><span data-stu-id="2e84a-108">This is known as "short-circuit" evaluation.</span></span>  
  
 <span data-ttu-id="2e84a-109">Оператор условного И не может быть перегружен, но перегрузки регулярных логических операторов и операторов [true](../../../csharp/language-reference/keywords/true.md) и [false](../../../csharp/language-reference/keywords/false.md) могут, с некоторыми ограничениями, считаться перегрузками условных логических операторов.</span><span class="sxs-lookup"><span data-stu-id="2e84a-109">The conditional-AND operator cannot be overloaded, but overloads of the regular logical operators and operators [true](../../../csharp/language-reference/keywords/true.md) and [false](../../../csharp/language-reference/keywords/false.md) are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span>  
  
## <a name="example"></a><span data-ttu-id="2e84a-110">Пример</span><span class="sxs-lookup"><span data-stu-id="2e84a-110">Example</span></span>  
 <span data-ttu-id="2e84a-111">В приведенном ниже примере условное выражение во втором операторе `if` вычисляет только первый операнд, так как операнд возвращает `false`.</span><span class="sxs-lookup"><span data-stu-id="2e84a-111">In the following example, the conditional expression in the second `if` statement evaluates only the first operand because the operand returns `false`.</span></span>  
  
 <span data-ttu-id="2e84a-112">[!code-cs[csRefOperators#48](../../../csharp/language-reference/operators/codesnippet/CSharp/conditional-and-operator_1.cs)]</span><span class="sxs-lookup"><span data-stu-id="2e84a-112">[!code-cs[csRefOperators#48](../../../csharp/language-reference/operators/codesnippet/CSharp/conditional-and-operator_1.cs)]</span></span>  
  
## <a name="c-language-specification"></a><span data-ttu-id="2e84a-113">Спецификация языка C#</span><span class="sxs-lookup"><span data-stu-id="2e84a-113">C# Language Specification</span></span>  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a><span data-ttu-id="2e84a-114">См. также</span><span class="sxs-lookup"><span data-stu-id="2e84a-114">See Also</span></span>  
 <span data-ttu-id="2e84a-115">[Справочник по C#](../../../csharp/language-reference/index.md) </span><span class="sxs-lookup"><span data-stu-id="2e84a-115">[C# Reference](../../../csharp/language-reference/index.md) </span></span>  
 <span data-ttu-id="2e84a-116">[Руководство по программированию на C#](../../../csharp/programming-guide/index.md) </span><span class="sxs-lookup"><span data-stu-id="2e84a-116">[C# Programming Guide](../../../csharp/programming-guide/index.md) </span></span>  
 [<span data-ttu-id="2e84a-117">Операторы в C#</span><span class="sxs-lookup"><span data-stu-id="2e84a-117">C# Operators</span></span>](../../../csharp/language-reference/operators/index.md)
