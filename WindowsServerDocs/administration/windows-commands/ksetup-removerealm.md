---
title: ksetup： removerealm
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1465ce08c0cf45de828683324b29fb2df8d0e893
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841451"
---
# <a name="ksetupremoverealm"></a>ksetup： removerealm



從登錄中刪除指定領域的所有資訊。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，在執行**ksetup**時，它會列為預設領域。|

## <a name="remarks"></a>備註

領域名稱會儲存在登錄中的兩個位置： **HKEY_LOCAL_MACHINE \system\controlset001**和 **\CurrentControlSet\Control\Lsa\Kerberos**。

您無法從網域控制站移除預設的領域名稱，因為這將會重設其 DNS 資訊，而將它移除可能會使網域控制站無法使用。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

錯誤地在本機電腦上將領域名稱設定為 CORP。CONTOSO.CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
從本機電腦移除該錯誤的領域名稱：
```
ksetup /removerealm CORP.CONTOSO.CON
```
執行**ksetup**並檢查輸出，以確認移除。

## <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)