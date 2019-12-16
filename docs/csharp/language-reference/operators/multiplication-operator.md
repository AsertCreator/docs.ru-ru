---
title: "* Оператор (ссылка C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- '*_CSharpKeyword'
dev_langs:
- CSharp
helpviewer_keywords:
- multiplication operator (*) [C#]
- '* operator [C#]'
ms.assetid: abd9a5f0-9b24-431e-971a-09ee1c45c50e
caps.latest.revision: 14
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
ms.openlocfilehash: 165ca8f797eb8d03ae1dec8c0ec5e1f4b31cb050
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="-operator-c-reference"></a><span data-ttu-id="8ae29-102">Оператор * (Справочник по C#)</span><span class="sxs-lookup"><span data-stu-id="8ae29-102">* Operator (C# Reference)</span></span>
<span data-ttu-id="8ae29-103">Оператор умножения (`*`), который вычисляет произведение двух операндов.</span><span class="sxs-lookup"><span data-stu-id="8ae29-103">The multiplication operator (`*`), which computes the product of its operands.</span></span>  <span data-ttu-id="8ae29-104">Кроме того, это оператор разыменования, позволяющий выполнять чтение и запись в указателе.</span><span class="sxs-lookup"><span data-stu-id="8ae29-104">Also, the dereference operator, which allows reading and writing to a pointer.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="8ae29-105">Примечания</span><span class="sxs-lookup"><span data-stu-id="8ae29-105">Remarks</span></span>  
 <span data-ttu-id="8ae29-106">Все числовые типы имеют предопределенные операторы умножения.</span><span class="sxs-lookup"><span data-stu-id="8ae29-106">All numeric types have predefined multiplication operators.</span></span>  
  
 <span data-ttu-id="8ae29-107">Оператор `*` также используется для объявления типов указателей и для разыменования указателей.</span><span class="sxs-lookup"><span data-stu-id="8ae29-107">The `*` operator is also used to declare pointer types and to dereference pointers.</span></span> <span data-ttu-id="8ae29-108">Этот оператор может использоваться только в небезопасных контекстах, обозначенных с помощью ключевого слова [unsafe](../../../csharp/language-reference/keywords/unsafe.md) и требующих параметр компилятора [/unsafe](../../../csharp/language-reference/compiler-options/unsafe-compiler-option.md).</span><span class="sxs-lookup"><span data-stu-id="8ae29-108">This operator can only be used in unsafe contexts, denoted by the use of the [unsafe](../../../csharp/language-reference/keywords/unsafe.md) keyword, and requiring the [/unsafe](../../../csharp/language-reference/compiler-options/unsafe-compiler-option.md) compiler option.</span></span>  <span data-ttu-id="8ae29-109">Оператор разыменования также известен как оператор косвенного обращения.</span><span class="sxs-lookup"><span data-stu-id="8ae29-109">The dereference operator is also known as the indirection operator.</span></span>  
  
 <span data-ttu-id="8ae29-110">Определяемые пользователем типы могут вызвать перегрузку бинарного оператора `*` (см. раздел [operator](../../../csharp/language-reference/keywords/operator.md)) .</span><span class="sxs-lookup"><span data-stu-id="8ae29-110">User-defined types can overload the binary `*` operator (see [operator](../../../csharp/language-reference/keywords/operator.md)).</span></span> <span data-ttu-id="8ae29-111">При перегрузке бинарного оператора соответствующий оператор присвоения (если таковой имеется) также неявно перегружается.</span><span class="sxs-lookup"><span data-stu-id="8ae29-111">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span>  
  
## <a name="example"></a><span data-ttu-id="8ae29-112">Пример</span><span class="sxs-lookup"><span data-stu-id="8ae29-112">Example</span></span>  
 <span data-ttu-id="8ae29-113">[!code-cs[csRefOperators#50](../../../csharp/language-reference/operators/codesnippet/CSharp/multiplication-operator_1.cs)]</span><span class="sxs-lookup"><span data-stu-id="8ae29-113">[!code-cs[csRefOperators#50](../../../csharp/language-reference/operators/codesnippet/CSharp/multiplication-operator_1.cs)]</span></span>  
  
## <a name="example"></a><span data-ttu-id="8ae29-114">Пример</span><span class="sxs-lookup"><span data-stu-id="8ae29-114">Example</span></span>  
 <span data-ttu-id="8ae29-115">[!code-cs[csRefOperators#51](../../../csharp/language-reference/operators/codesnippet/CSharp/multiplication-operator_2.cs)]</span><span class="sxs-lookup"><span data-stu-id="8ae29-115">[!code-cs[csRefOperators#51](../../../csharp/language-reference/operators/codesnippet/CSharp/multiplication-operator_2.cs)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="8ae29-116">См. также</span><span class="sxs-lookup"><span data-stu-id="8ae29-116">See Also</span></span>  
 <span data-ttu-id="8ae29-117">[Справочник по C#](../../../csharp/language-reference/index.md) </span><span class="sxs-lookup"><span data-stu-id="8ae29-117">[C# Reference](../../../csharp/language-reference/index.md) </span></span>  
 <span data-ttu-id="8ae29-118">[Руководство по программированию на C#](../../../csharp/programming-guide/index.md) </span><span class="sxs-lookup"><span data-stu-id="8ae29-118">[C# Programming Guide](../../../csharp/programming-guide/index.md) </span></span>  
 <span data-ttu-id="8ae29-119">[Небезопасный код и указатели](../../../csharp/programming-guide/unsafe-code-pointers/index.md) </span><span class="sxs-lookup"><span data-stu-id="8ae29-119">[Unsafe Code and Pointers](../../../csharp/programming-guide/unsafe-code-pointers/index.md) </span></span>  
 [<span data-ttu-id="8ae29-120">Операторы в C#</span><span class="sxs-lookup"><span data-stu-id="8ae29-120">C# Operators</span></span>](../../../csharp/language-reference/operators/index.md)
