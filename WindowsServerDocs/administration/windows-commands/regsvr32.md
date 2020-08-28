---
title: regsvr32
description: Regsvr32 命令的參考文章，會在登錄中將 .dll 檔案註冊為命令元件。
ms.topic: reference
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bb39070ba1744ca261419e5b996144f89b17b11
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027418"
---
# <a name="regsvr32"></a>regsvr32

將 .dll 檔案註冊為登錄中的命令元件。

## <a name="syntax"></a>語法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <Dllname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /U | 取消註冊伺服器。 |
| /s | 防止顯示訊息。 |
| /n | 防止呼叫 **DllRegisterServer**。 此參數需要您同時使用 **/i** 參數。 |
| /i`<cmdline>` | 將選擇性的命令列字串 (*cmdline*) 傳遞至 **DllInstall**。 如果您搭配 **/u** 參數使用此參數，它會呼叫 **DllUninstall**。 |
| `<Dllname>` | 將註冊的 .DLL 檔案名。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要註冊 Active Directory 架構的 .dll，請輸入：

```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
