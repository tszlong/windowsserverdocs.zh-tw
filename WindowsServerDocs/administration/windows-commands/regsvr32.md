---
title: regsvr32
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: beadc9e9e614e2fe4cffad5dc263cfb1d4aecf67
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722481"
---
# <a name="regsvr32"></a>regsvr32



將 .dll 檔案註冊為登錄中的命令元件。



## <a name="syntax"></a>語法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/U|取消註冊伺服器。|
|/s|執行**Regsvr32**而不顯示訊息。|
|/n|執行**Regsvr32**而不呼叫**DllRegisterServer**。 （需要 **/i**參數）。|
|/i：\<cmdline>|將選擇性的命令列字串（*cmdline*）傳遞至**DllInstall**。 如果您搭配 **/u**參數使用此參數，則會呼叫**DllUninstall**。|
|\<DllName>|將註冊的 .DLL 檔案名。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要註冊 Active Directory 架構的 .dll，請輸入：
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)