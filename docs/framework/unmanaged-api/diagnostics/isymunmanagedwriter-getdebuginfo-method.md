---
title: Метод ISymUnmanagedWriter::GetDebugInfo
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.GetDebugInfo
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::GetDebugInfo
helpviewer_keywords:
- ISymUnmanagedWriter::GetDebugInfo method [.NET Framework debugging]
- GetDebugInfo method [.NET Framework debugging]
ms.assetid: dd31c210-6829-45eb-927e-cc53932638b7
topic_type:
- apiref
ms.openlocfilehash: dcab63b603d4a9a8e1430228043d2a5e597bbf4f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678296"
---
# <a name="isymunmanagedwritergetdebuginfo-method"></a>Метод ISymUnmanagedWriter::GetDebugInfo

Возвращает сведения, необходимые компилятору для записи записи каталога отладки в заголовке переносимого исполняемого файла (PE). Средство записи символов заполняет все поля, за исключением `TimeDateStamp` и `PointerToRawData` . (Компилятор отвечает за установку этих двух полей соответствующим образом.)  
  
 Компилятор должен вызывать этот метод, выдавать большой двоичный объект данных в PE-файл, устанавливать `PointerToRawData` поле в IMAGE_DEBUG_DIRECTORY, чтобы указывать на выпущенные данные, и записывать IMAGE_DEBUG_DIRECTORY в PE-файл. Компилятор также должен задать `TimeDateStamp` для поля значение, равное значению `TimeDateStamp` создаваемого PE.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetDebugInfo(  
    [in, out] IMAGE_DEBUG_DIRECTORY *pIDD,  
    [in]  DWORD cData,  
    [out] DWORD *pcData,  
    [out, size_is(cData),  
        length_is(*pcData)] BYTE data[]);  
```  
  
## <a name="parameters"></a>Параметры  

 `pIDD`  
 [вход, выход] Указатель на IMAGE_DEBUG_DIRECTORY, который средство записи символов будет заполнять.  
  
 `cData`  
 окне Значение типа `DWORD` , содержащее размер данных отладки.  
  
 `pcData`  
 заполняет Указатель на объект `DWORD` , который получает размер буфера, необходимого для хранения данных отладки.  
  
 `data`  
 заполняет Указатель на буфер, достаточно большой для хранения отладочных данных для хранилища символов.  
  
## <a name="return-value"></a>Возвращаемое значение  

 S_OK, если метод выполнен. в противном случае E_FAIL или другой код ошибки.  
  
## <a name="requirements"></a>Требования  

 **Заголовок:** Корсим. idl, Корсим. h  
  
## <a name="see-also"></a>См. также раздел

- [Интерфейс ISymUnmanagedWriter](isymunmanagedwriter-interface.md)
