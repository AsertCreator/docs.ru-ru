---
title: Функция CreateALink
ms.date: 03/30/2017
api_name:
- CreateALink
api_location:
- alink.dll
api_type:
- DLLExport
f1_keywords:
- CreateALink
helpviewer_keywords:
- CreateALink function
- Alink API, CreateALink function
ms.assetid: fc73bcb9-6af6-44d8-bc39-2f4400325dae
topic_type:
- apiref
ms.openlocfilehash: 98c6ed4657dc69554a9fcca27145f65c621492f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683735"
---
# <a name="createalink-function"></a>Функция CreateALink

Создает экземпляр компоновщика сборок и задает указатель на указанный интерфейс.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT CreateALink (  
   REFIID riid,  
   IUnknown **ppInterface  
);  
```  
  
## <a name="parameters"></a>Параметры  
  
|Параметр|Описание|  
|---------------|-----------------|  
|`riid`|Физическое имя одного из интерфейсов компоновщика сборок.|  
|`ppInterface`|Расположение, которое в случае успешного завершения содержит указатель на `riid` интерфейс.|  
  
## <a name="requirements"></a>Требования  

 **Библиотека**: alink.dll  
  
## <a name="see-also"></a>См. также раздел

- [Al.exe (компоновщик сборок)](../../tools/al-exe-assembly-linker.md)
