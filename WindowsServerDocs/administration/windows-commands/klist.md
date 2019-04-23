---
title: klist
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b3d0591f9feb12782d0c77b6c786cfe17656ab2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831159"
---
# <a name="klist"></a>klist



顯示目前快取 Kerberos 票證的清單。 這項資訊適用於 Windows Server 2012。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-lh|代表使用者的本機唯一識別碼 (LUID)，高部分，以十六進位表示。 如果 – lh 或 – li 都不會出現，此命令會預設為目前登入之使用者的 LUID。|
|-li|代表使用者的本機唯一識別碼 (LUID)，低部分，以十六進位表示。 如果 – lh 或 – li 都不會出現，此命令會預設為目前登入之使用者的 LUID。|
|tickets|列出目前快取的票證授與-票證 (Tgt)，以及指定的登入工作階段的服務票證。 此為預設選項。|
|tgt|顯示初始的 Kerberos tgt。|
|清除|可讓您刪除指定的登入工作階段的所有票證。|
|工作階段|在此電腦上，會顯示一份登入工作階段。|
|kcd_cache|會顯示 Kerberos 限制委派快取資訊。|
|取得|可讓您要求的服務主體名稱 (SPN) 所指定的目標電腦的票證。|
|add_bind|可讓您指定 Kerberos 驗證的慣用的網域控制站。|
|query_bind|會顯示一份已連絡 Kerberos 的每個網域的快取慣用的網域控制站。|
|purge_bind|移除快取慣用的網域指定的網域控制站。|
|kdcoptions|顯示在 RFC 4120 中指定的金鑰發佈中心 (KDC) 選項。|
|/?|此命令，會顯示說明。|

## <a name="remarks"></a>備註

中的成員資格**Domain Admins**，或同等權限，才能執行此命令的所有參數的最小值。

如果未不提供任何參數，Klist 會擷取目前登入使用者的所有票證。

參數會顯示下列資訊：
-   **tickets**

    列出您已通過驗證之後登入服務的目前快取的票證。 會顯示所有已快取的票證的下列屬性：  
    -   LogonID:LUID
    -   用戶端：用戶端名稱和用戶端的網域名稱的串連
    -   伺服器：服務名稱和服務的網域名稱的串連
    -   KerbTicket 加密類型：用來加密的 Kerberos 票證加密類型
    -   票證旗標：Kerberos 票證旗標
    -   開始時間：從中票證就會是有效的時間
    -   結束時間：票證不再生效的時間。 超過這個時間票證時，它不再可用來向服務進行驗證，或用於更新
    -   更新時間：新的初始驗證所需的時間
    -   工作階段金鑰類型：用於工作階段金鑰加密演算法
-   **tgt**

    列出初始的 Kerberos TGT 與目前快取的票證的下列屬性：  
    -   LogonID:以十六進位方式識別
    -   ServiceName: krbtgt
    -   TargetName \<SPN >: krbtgt
    -   網域名稱：發出的 TGT 的網域名稱
    -   TargetDomainName:若要發出的 TGT 的網域
    -   AltTargetDomainName:若要發出的 TGT 的網域
    -   票證旗標：地址和目標的動作和類型
    -   工作階段金鑰：金鑰長度和加密演算法
    -   StartTime:票證已要求的本機電腦時間
    -   結束時間：票證不再生效的時間。 超過這個時間票證時，它不再可用來向服務進行驗證。
    -   RenewUntil:票證更新的期限
    -   TimeSkew:使用金鑰發佈中心 (KDC) 的時間差異
    -   EncodedTicket:編碼的票證
-   **purge**

    可讓您刪除特定的票證。 清除票證終結已快取的所有票證，因此請謹慎使用這個屬性。 它可能會阻止您能夠對資源進行驗證。 如果發生這種情況，您必須先登出再重新登入。  
    -   LogonID:以十六進位方式識別
-   **工作階段**

    可讓您列出並顯示在此電腦上的所有登入工作階段的資訊。  
    -   LogonID:如果指定，會顯示登入工作階段，只要指定的值。 如果未指定，會顯示所有登入工作階段，在此電腦上。
-   **kcd_cache**

    可讓您顯示 Kerberos 限制委派的快取資訊。  
    -   LogonID:如果指定，會顯示 登入工作階段的快取資訊，以所指定的值。 如果未指定，會顯示目前使用者的登入工作階段的快取資訊。
-   **get**

    可讓您要求的目標，由 SPN 的票證。  
    -   LogonID:如果指定，要求的票證使用登入工作階段所指定的值。 如果未指定，請使用目前使用者的登入工作階段要求的票證。
    -   kdcoptions:要求的票證使用給定的 KDC 選項
-   **add_bind**

    可讓您指定 Kerberos 驗證的慣用的網域控制站。
-   **query_bind**

    可讓您顯示定義域的快取、 慣用的網域控制站。
-   **purge_bind**

    可讓您移除定義域的快取、 慣用的網域控制站。
-   **kdcoptions**

    目前的選項和其說明清單，請參閱 < [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt)。

**其他考量**
-   Klist.exe 適用於 Windows Server 2012 和 Windows 8 中，而且需要任何特殊的安裝。

## <a name="BKMK_Examples"></a>範例

1.  在診斷時處理事件識別碼 27 時票證授權服務 (TGS) 要求目標伺服器的帳戶沒有合適的索引鍵，用於產生 Kerberos 票證。 您可以使用 Klist 查詢來判斷任何票證不見了，如果目標伺服器或帳戶不正確，或不支援加密類型的 Kerberos 票證快取。  
    ```
    klist 
    ```  
    ```
    klist –li 0x3e7
    ```  
2.  當您診斷錯誤和您想要知道每個票證授權票證快取電腦上登入工作階段的詳細資訊時，您可以使用 Klist 的 TGT 資訊顯示。  
    ```
    klist tgt
    ```  
3.  如果您是無法建立連線，而且診斷可能會花太長的時間，可以清除 Kerberos 票證快取、 登出，然後再重新登入。  
    ```
    klist purge
    ```  
    ```
    klist purge –li 0x3e7
    ```  
4.  當您想要診斷使用者或服務的登入工作階段時，您可以使用下列命令來尋找使用 LogonID 其他 Klist 命令。  
    ```
    klist sessions
    ```  
5.  當您想要診斷 Kerberos 限制委派失敗時，您可以使用下列命令來尋找上次發生的錯誤。  
    ```
    klist kcd_cache
    ```  
6.  當您想要診斷使用者或服務可以取得票證至伺服器時，您可以使用此命令，以要求票證的特定 SPN。  
    ```
    klist get host/%computername%
    ```  
7.  當診斷網域控制站之間的複寫問題，您通常需要用戶端電腦為目標的特定網域控制站。 在這些情況下，您可以使用下列命令，以該特定網域控制站是用戶端電腦為目標。  
    ```
    klist add_bind CONTOSO KDC.CONTOSO.COM
    
    ```  
    ```
    klist add_bind CONTOSO.COM KDC.CONTOSO.COM
    ```  
8.  若要查詢哪些網域控制站最近連絡這台電腦，您可以使用下列命令。  
    ```
    klist query_bind
    ```  
9.  當您想要重新探索網域控制站的 Kerberos 時，您可以使用下列命令。 此命令也可用來建立新的網域控制站繫結與 klist add_bind 之前先清除快取。  
    ```
    klist purge_bind
    ```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)