---
title: 管理 bde 繼續
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ca3cd1ca-6f2c-4190-b68f-27816635facb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b4f655cf6e37df7397c45f1214fa8799776a758
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823129"
---
# <a name="manage-bde-resume"></a>管理 bde： 繼續



BitLocker 加密或解密之後繼續已暫停。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -resume [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive>|表示磁碟機代號，後面接著冒號。|
|-computername|指定 bde.exe 用以修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|\<名稱 >|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？|顯示在命令提示字元中，簡短說明。|
|-help 或-h|顯示在命令提示字元完成說明。|

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用 **-繼續**命令來繼續磁碟機 c 上的 BitLocker 加密
```
manage-bde –resume C:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)