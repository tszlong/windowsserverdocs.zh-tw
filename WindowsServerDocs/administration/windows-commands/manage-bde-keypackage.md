---
title: manage-bde KeyPackage
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0a1b4fd0fff1153a0f778eca105ecfc618a4689
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374050"
---
# <a name="manage-bde-keypackage"></a>manage-bde.wsfKeyPackage



產生磁片磁碟機的金鑰封裝。 金鑰套件可以與修復工具搭配使用，以修復損毀的磁片磁碟機。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive >|表示後面接著冒號的磁碟機號。|
|-ID|使用金鑰保護裝置建立金鑰封裝，此識別碼是由此 ID 值所指定。|
|-path|要在其中儲存所建立之金鑰封裝的位置。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="BKMK_Examples"></a>典型

下列範例說明如何使用 **-KeyPackage**命令，根據 GUID 所識別的金鑰保護裝置來建立磁片磁碟機 C 的金鑰封裝，並將金鑰套件儲存至 F:\Folder。
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path "f:\Folder"
```

> [!TIP]
> 使用 manage-bde **–保護裝置–取得**您想要建立金鑰套件的磁碟機號，以取得可用的 guid 清單做為識別碼值。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)