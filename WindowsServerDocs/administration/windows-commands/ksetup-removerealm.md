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
ms.openlocfilehash: 579b0772e4642389b90aa370dad80a3eebea9d34
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564723"
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

領域名稱會儲存在登錄中的兩個地方：**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001**並 **\CurrentControlSet\Control\Lsa\Kerberos**。

您無法從網域控制站移除預設領域名稱，因為這將會重設其 DNS 資訊，並將它移除可能會導致網域控制站無法使用。

## <a name="BKMK_Examples"></a>範例

不小心設定領域名稱拼錯".COM"的本機電腦上為 CORP.CONTOSO。CON
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