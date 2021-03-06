---
title: Практическое руководство. Как разрешать конфликты параллелизма путем сохранения значений базы данных
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b475cf72-9e64-4f6e-99c1-af7737bc85ef
ms.openlocfilehash: b6f9b0308bcbf53a89ae0690ed44db0a364aef0c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191701"
---
# <a name="how-to-resolve-conflicts-by-retaining-database-values"></a>Практическое руководство. Как разрешать конфликты параллелизма путем сохранения значений базы данных

Чтобы согласовать различия между ожидаемыми и фактическими значениями базы данных до повторной отправки изменений, можно воспользоваться <xref:System.Data.Linq.RefreshMode.OverwriteCurrentValues> для сохранения значений, найденных в базе данных. Текущие значения в объектной модели при этом перезаписываются. Дополнительные сведения см. в разделе [Оптимистическая блокировка: обзор](optimistic-concurrency-overview.md).  
  
> [!NOTE]
> Во всех случаях запись на клиенте сначала обновляется путем извлечения обновленных данных из базы данных. Это действие гарантирует успешное выполнение следующей попытки обновления при тех же проверках параллелизма.  
  
## <a name="example"></a>Пример  

 В данном сценарии, когда Пользователь1 пытается отправить изменения, возникает исключение <xref:System.Data.Linq.ChangeConflictException>, поскольку Пользователь2 в это время изменил столбцы "Помощник" и "Отдел". Эта ситуация представлена в следующей таблице.  
  
||Manager|Помощник|отдел;|  
|------|-------------|---------------|----------------|  
|Исходное состояние базы данных при отправке запросов Пользователем1 и Пользователем2.|Алексеи|Мария|Sales|  
|Пользователь1 готовится отправить изменения.|Алексей||Marketing|  
|Пользователь2 уже отправил изменения.||Инна|Служба|  
  
 Пользователь1 решает устранить этот конфликт путем перезаписи текущих значений из объектной модели более новыми значениями базы данных.  
  
 При устранении Пользователем1 конфликта с помощью <xref:System.Data.Linq.RefreshMode.OverwriteCurrentValues> результат в базе данных будет соответствовать данным в следующей таблице.  
  
||Manager|Помощник|отдел;|  
|------|-------------|---------------|----------------|  
|Новое состояние после устранения конфликта.|Алексеи<br /><br /> (исходное значение)|Инна<br /><br /> (от Пользователя2)|Служба<br /><br /> (от Пользователя2)|  
  
 В следующем примере кода показано, как перезаписать текущие значения из объектной модели значениями базы данных (проверка или пользовательская обработка конфликтов отдельных членов не выполняется).  
  
 [!code-csharp[System.Data.Linq.RefreshMode#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.refreshmode/cs/program.cs#1)]
 [!code-vb[System.Data.Linq.RefreshMode#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.refreshmode/vb/module1.vb#1)]  
  
## <a name="see-also"></a>См. также раздел

- [Практическое руководство. Как управлять конфликтами изменений](how-to-manage-change-conflicts.md)
