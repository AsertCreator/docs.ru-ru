---
title: Перечисление COR_PRF_GC_ROOT_FLAGS
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_ROOT_FLAGS
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_ROOT_FLAGS
helpviewer_keywords:
- COR_PRF_GC_ROOT_FLAGS enumeration [.NET Framework profiling]
ms.assetid: 4611ee6f-0f05-4d84-91e1-e83d5e7dd7e4
topic_type:
- apiref
ms.openlocfilehash: 6b4c71a099e1ddb03b8a5287b56b750f7119e34e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95682361"
---
# <a name="cor_prf_gc_root_flags-enumeration"></a>Перечисление COR_PRF_GC_ROOT_FLAGS

Указывает свойство корня сборки мусора.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
typedef enum {  
    COR_PRF_GC_ROOT_PINNING = 0x1,  
    COR_PRF_GC_ROOT_WEAKREF = 0x2,  
    COR_PRF_GC_ROOT_INTERIOR = 0x4,  
    COR_PRF_GC_ROOT_REFCOUNTED = 0x8,  
} COR_PRF_GC_ROOT_FLAGS;  
```  
  
## <a name="members"></a>Члены  
  
|Член|Описание|  
|------------|-----------------|  
|`COR_PRF_GC_ROOT_PINNING`|Корень предотвращает перемещение объекта в сборку мусора.|  
|`COR_PRF_GC_ROOT_WEAKREF`|Корень не препятствует сбору мусора.|  
|`COR_PRF_GC_ROOT_INTERIOR`|Корень ссылается на поле объекта, а не на сам объект.|  
|`COR_PRF_GC_ROOT_REFCOUNTED`|Корень предотвращает сборку мусора, если значение счетчика ссылок объекта является определенным значением.|  
  
## <a name="remarks"></a>Комментарии  

 `COR_PRF_GC_ROOT_FLAGS` Битовая маска, которая предоставляет дополнительные сведения о специальных корневых элементах. Однако не все корни являются специальными. Например, некоторые корни не являются слабыми ссылками, внутренними указателями, закрепленными или подсчитаны ссылки. Для таких корней нет флагов для передачи. Таким образом, методы, использующие это перечисление, например метод [ICorProfilerCallback2:: RootReferences2](icorprofilercallback2-rootreferences2-method.md) , отправляют 0 для битовой маски флагов, что означает, что все флаги отключены.  
  
## <a name="requirements"></a>Требования  

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** CorProf.idl, CorProf.h  
  
 **Библиотека:** CorGuids.lib  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>См. также раздел

- [Перечисления профилирования](profiling-enumerations.md)
