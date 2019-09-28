---
title: ksetup： removerealm
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11858d8a24d4f125c83b3e4092ac48f336a9ef0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374952"
---
# <a name="ksetupremoverealm"></a>ksetup： removerealm



從登錄中刪除指定領域的所有資訊。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，在執行**ksetup**時，它會列為預設領域。|

## <a name="remarks"></a>備註

領域名稱會儲存在登錄中的兩個位置：**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001**和 **\CurrentControlSet\Control\Lsa\Kerberos**。

您無法從網域控制站移除預設的領域名稱，因為這將會重設其 DNS 資訊，而將它移除可能會使網域控制站無法使用。

## <a name="BKMK_Examples"></a>典型

將本機電腦上的 ".COM" 拼錯，以誤設定領域名稱為 CORP。CONTOSO.CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
從本機電腦移除該錯誤的領域名稱：
```
ksetup /removerealm CORP.CONTOSO.CON
```
執行**ksetup**並檢查輸出，以確認移除。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [命令列語法關鍵](command-line-syntax-key.md)