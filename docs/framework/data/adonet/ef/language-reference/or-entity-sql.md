---
title: '|| (ИЛИ) (Entity SQL)'
ms.date: 03/30/2017
ms.assetid: 8e649648-eb9a-4380-9d74-36e62260628c
ms.openlocfilehash: 89c0a92030f2f067d5e5d45b58d475414a224ce4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150808"
---
# <a name="-or-entity-sql"></a>|| (ИЛИ) (Entity SQL)

Объединяет два выражения типа `Boolean` .  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
boolean_expression OR boolean_expression  
-- or
boolean_expression || boolean_expression  
```  
  
## <a name="arguments"></a>Аргументы  

 `boolean_expression`  
 Любое допустимое выражение, возвращающее значение типа `Boolean`.  
  
## <a name="return-value"></a>Возвращаемое значение  

 `true` , если любое из условий есть `true`; в противном случае `false`.  
  
## <a name="remarks"></a>Remarks  

 OR - это логический оператор [!INCLUDE[esql](../../../../../../includes/esql-md.md)] . Он используется только для объединения двух условий. Если в инструкции используется более одного логического оператора, то операторы OR вычисляются после операторов AND. Однако порядок выполнения можно изменить с помощью скобок.  
  
 Двойные вертикальные черты (&#124;&#124;) имеют те же функциональные возможности, что и оператор OR.  
  
 В следующей таблице указаны возможные входные значения и возвращаемые типы.  
  
||`TRUE`|`FALSE`|`NULL`|  
|-|------------|-------------|------------|  
|`TRUE`|TRUE|TRUE|TRUE|  
|`FALSE`|TRUE|FALSE|NULL|  
|`NULL`|TRUE|NULL|NULL|  
  
## <a name="example"></a>Пример  

 Следующий запрос Entity SQL использует оператор OR, чтобы объединить два выражения типа `Boolean` . Запрос основан на модели AdventureWorks Sales. Для компиляции и запуска этого запроса выполните следующие шаги.  
  
1. Выполните процедуру из статьи [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md).  
  
2. Передайте следующий запрос в качестве аргумента методу `ExecuteStructuralTypeQuery` :  
  
 [!code-sql[DP EntityServices Concepts 2#OR](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#or)]  
  
## <a name="see-also"></a>См. также раздел

- [Справочник по Entity SQL](entity-sql-reference.md)
