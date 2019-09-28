---
title: manage-bde changepassword
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b174e152-8442-4fba-8b33-56a81ff4f547
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2dee55aba2f5439b2902072a8e6fe2da2707ce29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374094"
---
# <a name="manage-bde-changepassword"></a>manage-bde： changepassword



修改資料磁片磁碟機的密碼。 系統會提示使用者輸入新密碼。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -changepassword [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive >|表示後面接著冒號的磁碟機號。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="BKMK_Examples"></a>典型

下列範例說明如何使用 **-changepassword**命令來變更用來解除鎖定資料磁片磁碟機 D 上 BitLocker 的密碼。
```
manage-bde –changepassword D:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)