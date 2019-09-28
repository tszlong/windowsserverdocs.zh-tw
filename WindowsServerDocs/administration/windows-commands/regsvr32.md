---
title: regsvr32
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 444af0ccf7c9bbe21c013f32b396997b7cb2e00f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371626"
---
# <a name="regsvr32"></a>regsvr32



將 .dll 檔案註冊為登錄中的命令元件。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|u|取消註冊伺服器。|
|/s|執行**Regsvr32**而不顯示訊息。|
|/n|執行**Regsvr32**而不呼叫**DllRegisterServer**。 （需要 **/i**參數）。|
|/i： \<cmdline >|將選擇性的命令列字串（*cmdline*）傳遞至**DllInstall**。 如果您搭配 **/u**參數使用此參數，則會呼叫**DllUninstall**。|
|\<DllName >|將註冊的 .DLL 檔案名。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>典型

若要註冊 Active Directory 架構的 .dll，請輸入：
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)