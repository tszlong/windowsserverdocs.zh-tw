---
title: manage-bde 鎖定
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8858e61-3a7e-4d03-8c98-5c09853f35e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84d883ddbf75c2e605afcbb761a54fcb5b0d7713
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820728"
---
# <a name="manage-bde-lock"></a>manage-bde： lock



鎖定受 BitLocker 保護的磁片磁碟機以防止其存取，除非提供解除鎖定金鑰。

## <a name="syntax"></a>語法

```
manage-bde -lock [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|\<磁片磁碟機>|表示後面接著冒號的磁碟機號。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<Name>|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="examples"></a>範例

說明如何使用 **-lock**命令來鎖定資料磁片磁碟機 D。
```
manage-bde –lock D:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)