---
title: regsvr32
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b775b0c49e4191a9fee6dc9e2e91f968142085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836211"
---
# <a name="regsvr32"></a>regsvr32



將 .dll 檔案註冊為登錄中的命令元件。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/u|取消註冊伺服器。|
|/s|執行**Regsvr32**而不顯示訊息。|
|/n|執行**Regsvr32**而不呼叫**DllRegisterServer**。 （需要 **/i**參數）。|
|/i：\<cmdline >|將選擇性的命令列字串（*cmdline*）傳遞至**DllInstall**。 如果您搭配 **/u**參數使用此參數，則會呼叫**DllUninstall**。|
|\<DllName >|將註冊的 .DLL 檔案名。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要註冊 Active Directory 架構的 .dll，請輸入：
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)