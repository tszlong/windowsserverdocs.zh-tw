---
title: rundll32
description: Rundll32.exe 命令的參考文章，此命令會載入並執行32位動態連結程式庫 (Dll) 。
ms.topic: reference
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 95bcb4984c3488b2c00a588d972941b4eb840fdb
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388412"
---
# <a name="rundll32"></a>rundll32

載入並執行32位動態連結程式庫 (Dll) 。 沒有可設定的 Rundll32.exe 設定。 系統會提供您使用 **rundll32.exe** 命令執行之特定 DLL 的說明資訊。

您必須從提升許可權的命令提示字元中執行 **rundll32.exe** 命令。 若要開啟提高許可權的命令提示字元，請按一下 [ **開始**]，以滑鼠右鍵按一下 **命令提示**字元，然後按一下 [以 **系統管理員身分執行**]

## <a name="syntax"></a>語法

```
rundll32 <DLLname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [Rundll32.exe printui.dll、PrintUIEntry](rundll32-printui.md) | 顯示印表機使用者介面。 |

## <a name="remarks"></a>備註

Rundll32.exe 只能從明確撰寫的 DLL 呼叫函式，以由 Rundll32.exe 呼叫。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
