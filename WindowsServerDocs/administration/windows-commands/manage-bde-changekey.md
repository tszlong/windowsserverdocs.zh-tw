---
title: manage-bde changekey
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 436d4bbec0dbb31fd9cdfb4fc29057e32d87888a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374115"
---
# <a name="manage-bde-changekey"></a>manage-bde： changekey



修改作業系統磁片磁碟機的啟動金鑰。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive >|表示後面接著冒號的磁碟機號。|
|\<PathToExternalKeyDirectory >|代表用來儲存外部啟動金鑰檔案的目錄位置，此檔案可用來解除鎖定磁片磁碟機。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="BKMK_Examples"></a>典型

下列範例說明如何使用 **-changekey**命令，在磁片磁碟機 C 上建立新的啟動金鑰，以搭配使用 C 磁片磁碟機上的 BitLocker 加密。
```
manage-bde –changekey C: E:\
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)