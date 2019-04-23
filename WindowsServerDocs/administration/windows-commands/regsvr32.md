---
title: regsvr32
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 87d9291755ddb4484e85248cb01ad78b01a25965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889989"
---
# <a name="regsvr32"></a>regsvr32



在登錄中的命令元件註冊.dll 檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/u|取消註冊伺服器。|
|/s|回合**Regsvr32**而不會顯示訊息。|
|/n|回合**Regsvr32**而不需呼叫**DllRegisterServer**。 (需要 **/i**參數。)|
|/i:\<cmdline>|傳遞的選擇性命令列字串 (*cmdline*) 來**DllInstall**。 如果您使用這個參數搭配 **/u**參數，它會呼叫**DllUninstall**。|
|\<DllName>|要註冊的.dll 檔案名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要登錄此.dll 檔的 Active Directory 架構中，輸入：
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)