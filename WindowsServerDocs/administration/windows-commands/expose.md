---
title: 公開 （expose)
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51cc744bc2b61862ed05ca2e7d0aaa8f70d38692
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886659"
---
# <a name="expose"></a>公開 （expose)



會公開為磁碟機代號、 共用或掛接點的持續性的陰影複製。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|ShadowID|指定您想要公開 （expose） 的陰影複製的陰影識別碼。|
|\<磁碟機： >|會將指定的陰影複製公開為磁碟機代號 （例如，p:）。|
|\<Share>|會公開指定的陰影複製，在共用資源 (例如\\ \\ *MachineName*\)。|
|\<MountPoint>|會公開指定的陰影複製到掛接點 (例如 C:\shadowcopy\)。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數的位置*ShadowID*。 使用**新增**不含參數，若要查看現有的別名。

## <a name="BKMK_examples"></a>範例

若要公開為磁碟機 X 相關聯 VSS_SHADOW_1 環境變數的持續性的陰影複製，請輸入：
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)