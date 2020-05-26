---
title: manage-bde changekey
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d50abbe40232987045713c5eadf43607aeb6a81
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820708"
---
# <a name="manage-bde-changekey"></a>manage-bde： changekey



修改作業系統磁片磁碟機的啟動金鑰。

## <a name="syntax"></a>語法

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|\<磁片磁碟機>|表示後面接著冒號的磁碟機號。|
|\<PathToExternalKeyDirectory>|代表用來儲存外部啟動金鑰檔案的目錄位置，此檔案可用來解除鎖定磁片磁碟機。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<Name>|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="examples"></a>範例

說明如何使用 **-changekey**命令，在 C 磁片磁碟機上建立新的啟動金鑰，以搭配使用 C 磁片磁碟機上的 BitLocker 加密。
```
manage-bde -changekey C: E:\
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
