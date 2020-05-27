---
title: klist
description: Klist 命令的參考主題，它會顯示目前快取的 Kerberos 票證清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58a3ae0ffc223710f62355180ba590ff6b34b188
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818138"
---
# <a name="klist"></a>klist

顯示目前快取的 Kerberos 票證清單。

> [!IMPORTANT]
> 您必須至少是網域系統**管理員**或同等許可權，才能執行此命令的所有參數。

## <a name="syntax"></a>語法

```
klist [-lh <logonID.highpart>] [-li <logonID.lowpart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -lh | 表示使用者的本機唯一識別碼（LUID）的最高部分，以十六進位表示。 如果 **-lh**和 **– li**皆不存在，此命令會預設為目前登入之使用者的 LUID。 |
| -li | 表示使用者的本機唯一識別碼（LUID）的低部分，以十六進位表示。 如果 **-lh**和 **– li**皆不存在，此命令會預設為目前登入之使用者的 LUID。 |
| 票證 | 列出目前快取的票證授權票證（Tgt），以及指定登入會話的服務票證。 這是預設選項。 |
| tgt | 顯示初始 Kerberos TGT。 |
| 清理 | 可讓您刪除指定登入會話的所有票證。 |
| 工作階段 | 顯示這部電腦上的登入會話清單。 |
| kcd_cache | 顯示 Kerberos 限制委派快取資訊。 |
| get | 可讓您向服務主體名稱（SPN）所指定的目的電腦要求票證。 |
| add_bind | 可讓您指定 Kerberos 驗證慣用的網域控制站。 |
| query_bind | 顯示 Kerberos 已連線之每個網域的快取慣用網域控制站清單。 |
| purge_bind | 移除指定網域的快取慣用網域控制站。 |
| kdcoptions | 顯示 RFC 4120 中指定的金鑰發佈中心（KDC）選項。 |
| /? | 顯示此命令的說明。 |

#### <a name="remarks"></a>備註

- 如果未提供任何參數， **klist**會抓取目前登入之使用者的所有票證。

- 參數會顯示下列資訊：

  - **票證**-列出自登入後，您已驗證的服務目前已快取的票證。 顯示所有快取票證的下列屬性：

    - **LogonID：** LUID。

    - **用戶端：** 用戶端名稱和用戶端功能變數名稱的串連。

    - **伺服器：** 服務名稱和服務功能變數名稱的串連。

    - **KerbTicket 加密類型：** 用來加密 Kerberos 票證的加密類型。

    - **票證旗標：** Kerberos 票證旗標。

    - **開始時間：** 票證有效的時間。

    - **結束時間：** 票證變得不再有效的時間。 當票證過期時，就無法再用來向服務驗證或用於更新。

    - **更新時間：** 需要新的初始驗證的時間。

    - **工作階段金鑰類型：** 用於工作階段金鑰的加密演算法。

  - **tgt** -列出目前快取票證的初始 Kerberos tgt 和下列屬性：

    - **LogonID：** 以十六進位識別。

    - **ServiceName：** krbtgt

    - **TargetName `<SPN>` ：** krbtgt

    - **DomainName：** 發行 TGT 的網功能變數名稱稱。

    - **TargetDomainName：** TGT 發出的網域。

    - **AltTargetDomainName：** TGT 發出的網域。

    - **票證旗標：**[位址] 和 [目標動作] 和 [類型]。

    - **工作階段金鑰：** 金鑰長度和加密演算法。

    - **StartTime：** 要求票證的本機電腦時間。

    - **EndTime：** 票證變得不再有效的時間。 當票證在這段時間之後，就無法再用來向服務進行驗證。

    - **RenewUntil：** 票證更新的期限。

    - **TimeSkew：** 金鑰發佈中心（KDC）的時間差。

    - **EncodedTicket：** 編碼的票證。

  - **清除**-可讓您刪除特定的票證。 清除票證會損毀您已快取的所有票證，因此請小心使用此屬性。 這可能會讓您無法向資源進行驗證。 如果發生這種情況，您必須登出後再登入一次。

    - **LogonID：** 以十六進位識別。

  - **會話**-可讓您列出並顯示這部電腦上所有登入會話的資訊。

    - **LogonID：** 如果指定，則只會顯示指定值的登入會話。 如果未指定，會顯示這部電腦上的所有登入會話。

  - **kcd_cache** -允許您顯示 Kerberos 限制委派快取資訊。

    - **LogonID：** 如果指定，則會依指定的值顯示登入會話的快取資訊。 如果未指定，則會顯示目前使用者登入會話的快取資訊。

  - **get** -可讓您向 SPN 指定的目標要求票證。

    - **LogonID：** 如果指定，則會依指定的值使用登入會話來要求票證。 如果未指定，則會使用目前使用者的登入會話來要求票證。

    - **kdcoptions：** 使用指定的 KDC 選項要求票證

  - **add_bind** -可讓您指定慣用的網域控制站來進行 Kerberos 驗證。

  - **query_bind** -允許您顯示網域的快取、慣用的網域控制站。

  - **purge_bind** -可讓您移除網域的快取、慣用的網域控制站。

  - **kdcoptions** -如需目前的選項清單及其說明，請參閱[RFC 4120](http://www.ietf.org/rfc/rfc4120.txt)。

### <a name="examples"></a>範例

若要查詢 Kerberos 票證快取以判斷是否遺漏任何票證、目標伺服器或帳戶是否有錯誤，或因為事件識別碼27錯誤而不支援加密類型，請輸入：

```
klist
```

```
klist –li 0x3e7
```

若要瞭解電腦上針對登入會話快取的每個票證授權票證的細節，請輸入：

```
klist tgt
```

若要清除 Kerberos 票證快取、登出，然後重新登入，請輸入：

```
klist purge
```

```
klist purge –li 0x3e7
```

若要診斷登入會話，並找出使用者或服務的 logonID，請輸入：

```
klist sessions
```

若要診斷 Kerberos 限制委派失敗，並找出最後發生的錯誤，請輸入：

```
klist kcd_cache
```

若要診斷使用者或服務是否可以取得伺服器的票證，或要求特定 SPN 的票證，請輸入：

```
klist get host/%computername%
```

若要診斷跨網域控制站的複寫問題，您通常需要用戶端電腦以特定網域控制站為目標。 若要將用戶端電腦的目標設為特定網域控制站，請輸入：

```
klist add_bind CONTOSO KDC.CONTOSO.COM
```

```
klist add_bind CONTOSO.COM KDC.CONTOSO.COM
```

若要查詢這部電腦最近接觸過的網域控制站，請輸入：

```
klist query_bind
```

若要重新探索網域控制站，或在建立新的網域控制站系結之前清除快取 `klist add_bind` ，請輸入：

```
klist purge_bind
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)