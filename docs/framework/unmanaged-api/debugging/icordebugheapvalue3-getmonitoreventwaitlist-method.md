---
title: Метод ICorDebugHeapValue3::GetMonitorEventWaitList
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue3.GetMonitorEventWaitList
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue3::GetMonitorEventWaitList
helpviewer_keywords:
- ICorDebugHeapValue3::GetMonitorEventWaitList method [.NET Framework debugging]
- GetMonitorEventWaitList method [.NET Framework debugging]
ms.assetid: 035a9035-ac66-4953-b48a-99652b42b7fe
topic_type:
- apiref
ms.openlocfilehash: 21bf0122039a720ff8a1d38d62e77c2560dcc435
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726539"
---
# <a name="icordebugheapvalue3getmonitoreventwaitlist-method"></a>Метод ICorDebugHeapValue3::GetMonitorEventWaitList

Предоставляет упорядоченный список потоков, поставленных в очередь на событие, связанное с блокировкой монитора.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetMonitorEventWaitList (  
    [out] ICorDebugThreadEnum **ppThreadEnum  
);  
```  
  
## <a name="parameters"></a>Параметры  

 `ppThreadEnum`  
 заполняет Перечислитель Икордебугсреаденум, предоставляющий упорядоченный список потоков.  
  
## <a name="return-value"></a>Возвращаемое значение  

 Этот метод возвращает следующие конкретные результаты HRESULT, а также ошибки HRESULT, которые указывают на сбой метода.  
  
|HRESULT|Описание:|  
|-------------|-----------------|  
|S_OK|Список не пуст.|  
|S_FALSE|Список пуст.|  
  
## <a name="exceptions"></a>Исключения  
  
## <a name="remarks"></a>Remarks  

 Первый поток в списке является первым потоком, который освобождается при следующем вызове метода <xref:System.Threading.Monitor.Pulse%28System.Object%29?displayProperty=nameWithType> . Следующий поток в списке освобождается в следующем вызове и т. д.  
  
 Если список не пуст, этот метод возвращает S_OK. Если список пуст, метод возвращает S_FALSE; в этом случае перечисление по-прежнему является допустимым, хотя оно пустое.  
  
 В любом случае интерфейс перечисления можно использовать только в течение текущего синхронизированного состояния. Тем не менее, высветка интерфейсов потока является допустимой, пока поток не завершит работу.  
  
 Если не `ppThreadEnum` является допустимым указателем, результат не определен.  
  
 Если возникает ошибка, которая не может определить, какие потоки ожидают монитор, метод возвращает значение HRESULT, указывающее на сбой.  
  
## <a name="requirements"></a>Требования  

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** CorDebug.idl, CorDebug.h  
  
 **Библиотека:** CorGuids.lib  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>См. также раздел

- [Интерфейсы отладки](debugging-interfaces.md)
- [Отладка](index.md)
