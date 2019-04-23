---
title: 管理 bde forcerecovery
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bf7a913c9c496aebded349731ef965a53c98ac7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858639"
---
# <a name="manage-bde-forcerecovery"></a>管理 bde: forcerecovery



強制受 BitLocker 保護磁碟機進入修復模式在重新啟動。 此命令會刪除所有相關的信賴平台模組 TPM 金鑰保護裝置，從磁碟機。 當電腦重新啟動時，只能復原密碼或修復金鑰可以用來解除鎖定磁碟機。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde –forcerecovery <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
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

下列範例說明如何利用 **-forcerecovery**命令，會導致 BitLocker 開始進入修復模式，在 c 磁碟機。
```
manage-bde –forcerecovery C:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)