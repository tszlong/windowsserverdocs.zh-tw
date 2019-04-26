---
title: ksetup:removerealm
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3f62208d6576890529be80b1c6cb3cc073a2b4e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853359"
---
# <a name="ksetupremoverealm"></a>ksetup:removerealm



從登錄中刪除指定的領域的所有資訊。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM，並列為預設領域時**ksetup**執行。|

## <a name="remarks"></a>備註

領域名稱會儲存在登錄中的兩個地方：**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001**並**\CurrentControlSet\Control\Lsa\Kerberos**。

您無法從網域控制站移除預設領域名稱，因為這將會重設其 DNS 資訊，並將它移除可能會導致網域控制站無法使用。

## <a name="BKMK_Examples"></a>範例

不小心設定領域名稱拼字錯誤 」。COM 嗎？ CORP.本機電腦上CONTOSO。CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
從本機電腦中移除該錯誤的領域名稱：
```
ksetup /removerealm CORP.CONTOSO.CON
```
確認所執行的移除**ksetup**並檢閱輸出。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [命令列語法關鍵](command-line-syntax-key.md)