---
title: "Практическое руководство. Анализ строк с помощью метода String.Split (руководство по программированию на языке C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- splitting strings [C#]
- Split method [C#]
- strings [C#], splitting
- parse strings
ms.assetid: 729c2923-4169-41c6-9c90-ef176c1e2953
caps.latest.revision: 17
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
ms.sourcegitcommit: d74c1d0760d4e776c2cf4c7dea1dac060c85a83c
ms.openlocfilehash: e25dbf6b86c82808622377c0618cd956541c6e09
ms.contentlocale: ru-ru
ms.lasthandoff: 09/05/2017

---
# <a name="how-to-parse-strings-using-stringsplit-c-programming-guide"></a><span data-ttu-id="0937d-102">Практическое руководство. Анализ строк с помощью метода String.Split (руководство по программированию на языке C#)</span><span class="sxs-lookup"><span data-stu-id="0937d-102">How to: Parse Strings Using String.Split (C# Programming Guide)</span></span>
<span data-ttu-id="0937d-103">В приведенном ниже примере кода демонстрируется возможность анализа строки с помощью метода <xref:System.String.Split%2A?displayProperty=fullName> .</span><span class="sxs-lookup"><span data-stu-id="0937d-103">The following code example demonstrates how a string can be parsed using the <xref:System.String.Split%2A?displayProperty=fullName> method.</span></span> <span data-ttu-id="0937d-104">В качестве входных данных метод <xref:System.String.Split%2A> принимает массив символов, который определяет, какие символы разделяют нужные подстроки целевой строки.</span><span class="sxs-lookup"><span data-stu-id="0937d-104">As input, <xref:System.String.Split%2A> takes an array of characters that indicate which characters separate interesting sub strings of the target string.</span></span>  <span data-ttu-id="0937d-105">Функция возвращает массив подстрок.</span><span class="sxs-lookup"><span data-stu-id="0937d-105">The function returns an array of the sub strings.</span></span>  
  
 <span data-ttu-id="0937d-106">В этом примере в качестве символов-разделителей используются пробелы, запятые, точки, двоеточия и символы табуляции, которые передаются в метод <xref:System.String.Split%2A>в виде массива.</span><span class="sxs-lookup"><span data-stu-id="0937d-106">This example uses spaces, commas, periods, colons, and tabs, all passed in an array containing these separating characters to <xref:System.String.Split%2A>.</span></span>  <span data-ttu-id="0937d-107">Каждое слово в целевой строке отображается отдельно от полученного в результате массива строк.</span><span class="sxs-lookup"><span data-stu-id="0937d-107">Each word in the target string's sentence displays separately from the resulting array of strings.</span></span>  
  
## <a name="example"></a><span data-ttu-id="0937d-108">Пример</span><span class="sxs-lookup"><span data-stu-id="0937d-108">Example</span></span>  
 <span data-ttu-id="0937d-109">[!code-cs[csProgGuideStrings#16](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-parse-strings-using-string-split_1.cs)]</span><span class="sxs-lookup"><span data-stu-id="0937d-109">[!code-cs[csProgGuideStrings#16](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-parse-strings-using-string-split_1.cs)]</span></span>  
  
## <a name="example"></a><span data-ttu-id="0937d-110">Пример</span><span class="sxs-lookup"><span data-stu-id="0937d-110">Example</span></span>  
 <span data-ttu-id="0937d-111">По умолчанию метод String.Split возвращает пустые строки, если в целевой строке два символа-разделителя следуют друг за другом.</span><span class="sxs-lookup"><span data-stu-id="0937d-111">By default, String.Split returns empty strings when two separating characters appear contiguously in the target string.</span></span>  <span data-ttu-id="0937d-112">Чтобы исключить из выходных данных все пустые строки, можно передать необязательный параметр StringSplitOptions.RemoveEmptyEntries.</span><span class="sxs-lookup"><span data-stu-id="0937d-112">You can pass an optional StringSplitOptions.RemoveEmptyEntries parameter to exclude any empty strings in the output.</span></span>  
  
 <span data-ttu-id="0937d-113">Метод String.Split может принимать массив строк (в этом случае в качестве разделителей при анализе целевой строки используются последовательности символов, а не отдельные символы).</span><span class="sxs-lookup"><span data-stu-id="0937d-113">String.Split can take an array of strings (character sequences that act as separators for parsing the target string, instead of single characters).</span></span>  
  
```csharp  
class TestStringSplit  
{  
    static void Main()  
    {  
        string[] separatingChars = { "<<", "..." };  
  
        string text = "one<<two......three<four";  
        System.Console.WriteLine("Original text: '{0}'", text);  
  
        string[] words = text.Split(separatingChars, System.StringSplitOptions.RemoveEmptyEntries );  
        System.Console.WriteLine("{0} substrings in text:", words.Length);  
  
        foreach (string s in words)  
        {  
            System.Console.WriteLine(s);  
        }  
  
        // Keep the console window open in debug mode.  
        System.Console.WriteLine("Press any key to exit.");  
        System.Console.ReadKey();  
    }  
}  
/* Output:  
    Original text: 'one<<two......three<four'  
    3 words in text:  
    one  
    two  
    three<four  
*/  
```  
  
## <a name="see-also"></a><span data-ttu-id="0937d-114">См. также</span><span class="sxs-lookup"><span data-stu-id="0937d-114">See Also</span></span>  
 <span data-ttu-id="0937d-115">[Руководство по программированию на C#](../../../csharp/programming-guide/index.md) </span><span class="sxs-lookup"><span data-stu-id="0937d-115">[C# Programming Guide](../../../csharp/programming-guide/index.md) </span></span>  
 <span data-ttu-id="0937d-116">[Строки](../../../csharp/programming-guide/strings/index.md) </span><span class="sxs-lookup"><span data-stu-id="0937d-116">[Strings](../../../csharp/programming-guide/strings/index.md) </span></span>  
 [<span data-ttu-id="0937d-117">Регулярные выражения в .NET Framework</span><span class="sxs-lookup"><span data-stu-id="0937d-117">.NET Framework Regular Expressions</span></span>](https://msdn.microsoft.com/library/hs600312)
