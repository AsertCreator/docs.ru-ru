---
title: Функция CreateDebuggingInterfaceFromVersion
ms.date: 03/30/2017
api_name:
- CreateDebuggingInterfaceFromVersion
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- CreateDebuggingInterfaceFromVersion
helpviewer_keywords:
- CreateDebuggingInterfaceFromVersion function [.NET Framework hosting]
ms.assetid: a746a849-463c-44f5-a2f0-9e812ed8bcc3
topic_type:
- apiref
ms.openlocfilehash: b68fbc713374642c9f55d49ee51a88c5785cf4b2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727878"
---
# <a name="createdebugginginterfacefromversion-function"></a>Функция CreateDebuggingInterfaceFromVersion

Создает объект [ICorDebug](../debugging/icordebug-interface.md) на основе указанных сведений о версии.  
  
 Эта функция является устаревшей в .NET Framework 4. Вместо этого для получения интерфейса для среды CLR 2,0 используйте метод [ICLRRuntimeInfo::](iclrruntimeinfo-getinterface-method.md) coclass и укажите идентификатор класса CLSID_CLRDebuggingLegacy и идентификатор интерфейса IID_ICorDebug. Чтобы получить интерфейс для CLR 4 или более поздней версии, вызовите функцию [клркреатеинстанце](clrcreateinstance-function.md) и укажите идентификатор класса CLSID_CLRDebugging и идентификатор интерфейса IID_ICLRDebugging.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT CreateDebuggingInterfaceFromVersion (  
    [in]  int      iDebuggerVersion,
    [in]  LPCWSTR  szDebuggeeVersion,
    [out] IUnknown **ppCordb  
);  
```  
  
## <a name="parameters"></a>Параметры  

 `iDebuggerVersion`  
 окне Версия `ICorDebug` , ожидаемая отладчиком. Допустимые значения см. в описании перечисления [CorDebugInterfaceVersion](../debugging/cordebuginterfaceversion-enumeration.md) .  
  
 `szDebuggeeVersion`  
 окне Версия среды CLR, связанная с приложением или процессом для отладки. Сведения о получении этого значения см. в описании метода [GetVersionFromProcess](getversionfromprocess-function.md) или [жетрекуестедрунтимеверсион](getrequestedruntimeversion-function.md) .  
  
 `ppCordb`  
 заполняет Расположение, которое получает указатель на `ICorDebug` объект.  
  
## <a name="return-value"></a>Возвращаемое значение  

 Этот метод возвращает стандартные коды ошибок COM, определенные в файле WinError. h, а также следующие значения.  
  
|Код возврата|Описание|  
|-----------------|-----------------|  
|S_OK|Метод завершился успешно.|  
|E_INVALIDARG|`szDebuggeeVersion` или `ppCordb` имеет значение null, или строка версии неверна.|  
  
## <a name="remarks"></a>Комментарии  

 `szDebuggeeVersion`Параметр сопоставляется с соответствующей версией MSCorDbi.dll.  
  
## <a name="requirements"></a>Требования  

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** MSCorEE. h  
  
 **Библиотека:** MSCorEE.dll  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>См. также раздел

- [Устаревшие функции размещения CLR](deprecated-clr-hosting-functions.md)
