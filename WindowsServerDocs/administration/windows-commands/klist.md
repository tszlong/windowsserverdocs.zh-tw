---
title: klist
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f83a46f158cd83aa8393630aa0138271e8868575
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724758"
---
# <a name="klist"></a>klist



顯示目前快取的 Kerberos 票證清單。 這套資訊適用于 Windows Server 2012。

## <a name="syntax"></a>語法

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-lh|表示使用者的本機唯一識別碼（LUID）的最高部分，以十六進位表示。 如果-lh 或– li 皆不存在，此命令會預設為目前登入之使用者的 LUID。|
|-li|表示使用者的本機唯一識別碼（LUID）的低部分，以十六進位表示。 如果-lh 或– li 皆不存在，此命令會預設為目前登入之使用者的 LUID。|
|票證|列出目前快取的票證授權票證（Tgt），以及指定登入會話的服務票證。 這是預設選項。|
|tgt|顯示初始 Kerberos TGT。|
|清理|可讓您刪除指定登入會話的所有票證。|
|工作階段|顯示這部電腦上的登入會話清單。|
|kcd_cache|顯示 Kerberos 限制委派快取資訊。|
|get|可讓您向服務主體名稱（SPN）所指定的目的電腦要求票證。|
|add_bind|可讓您指定 Kerberos 驗證慣用的網域控制站。|
|query_bind|顯示 Kerberos 已連線之每個網域的快取慣用網域控制站清單。|
|purge_bind|移除指定網域的快取慣用網域控制站。|
|kdcoptions|顯示 RFC 4120 中指定的金鑰發佈中心（KDC）選項。|
|/?|顯示此命令的說明。|

## <a name="remarks"></a>備註

若要執行此命令的所有參數，至少需要**Domain Admins**的成員資格或同等許可權。

如果未提供任何參數，Klist 會取得目前登入之使用者的所有票證。

參數會顯示下列資訊：
-   **門票**

    列出自登入後，您已驗證的服務目前已快取的票證。 顯示所有快取票證的下列屬性：  
    -   LogonID： LUID
    -   用戶端：用戶端名稱和用戶端功能變數名稱的串連
    -   伺服器：服務名稱和服務功能變數名稱的串連
    -   KerbTicket 加密類型：用來加密 Kerberos 票證的加密類型
    -   票證旗標： Kerberos 票證旗標
    -   開始時間：票證生效的時間
    -   結束時間：票證變得不再有效的時間。 當票證過期時，就無法再用來驗證服務或用於更新
    -   更新時間：需要新的初始驗證的時間
    -   工作階段金鑰類型：用於工作階段金鑰的加密演算法
-   **tgt**

    列出目前快取票證的初始 Kerberos TGT 和下列屬性：  
    -   LogonID：以十六進位識別
    -   ServiceName： krbtgt
    -   TargetName \<SPN>： krbtgt
    -   DomainName：發行 TGT 的網功能變數名稱稱
    -   TargetDomainName： TGT 發出的網域
    -   AltTargetDomainName： TGT 發出的網域
    -   票證旗標：位址和目標動作和類型
    -   工作階段金鑰：金鑰長度和加密演算法
    -   StartTime：要求票證的本機電腦時間
    -   EndTime：票證變得不再有效的時間。 當票證在這段時間之後，就無法再用來向服務進行驗證。
    -   RenewUntil：票證更新的期限
    -   TimeSkew：與金鑰發佈中心（KDC）的時間差
    -   EncodedTicket：編碼的票證
-   **清理**

    可讓您刪除特定的票證。 清除票證會損毀您已快取的所有票證，因此請小心使用此屬性。 這可能會讓您無法向資源進行驗證。 如果發生這種情況，您就必須登出再登入一次。  
    -   LogonID：以十六進位識別
-   **討論會**

    可讓您列出和顯示這部電腦上所有登入會話的資訊。  
    -   LogonID：如果指定，只會顯示指定值的登入會話。 如果未指定，會顯示這部電腦上的所有登入會話。
-   **kcd_cache**

    可讓您顯示 Kerberos 限制委派快取資訊。  
    -   LogonID：如果指定，會依指定的值顯示登入會話的快取資訊。 如果未指定，則會顯示目前使用者登入會話的快取資訊。
-   **get**

    可讓您向 SPN 指定的目標要求票證。  
    -   LogonID：如果指定，則會依指定的值使用登入會話來要求票證。 如果未指定，則會使用目前使用者的登入會話來要求票證。
    -   kdcoptions：使用指定的 KDC 選項要求票證
-   **add_bind**

    可讓您指定 Kerberos 驗證慣用的網域控制站。
-   **query_bind**

    可讓您顯示網域的快取、慣用的網域控制站。
-   **purge_bind**

    可讓您移除網域的快取、慣用的網域控制站。
-   **kdcoptions**

    如需目前的選項清單及其說明，請參閱[RFC 4120](http://www.ietf.org/rfc/rfc4120.txt)。

**其他考量**
-   Klist 可在 Windows Server 2012 和 Windows 8 中使用，而且不需要進行任何特殊安裝。

## <a name="examples"></a>範例

1. 當您在處理目標伺服器的票證授權服務（TGS）要求時診斷事件識別碼27時，該帳戶沒有適當的金鑰可產生 Kerberos 票證。 您可以使用 Klist 來查詢 Kerberos 票證快取，以判斷是否有任何票證遺失、目標伺服器或帳戶是否有錯誤，或加密類型不受支援。  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. 當您診斷錯誤，而且想要知道電腦上針對登入會話快取的每個票證授權票證的細節時，可以使用 Klist 來顯示 TGT 資訊。  
   ```
   klist tgt
   ```  
3. 如果您無法建立連接，而且診斷可能需要太長的時間，您可以清除 Kerberos 票證快取、登出，然後重新登入。  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. 當您想要診斷使用者或服務的登入會話時，您可以使用下列命令來尋找在其他 Klist 命令中使用的 LogonID。  
   ```
   klist sessions
   ```  
5. 當您想要診斷 Kerberos 限制委派失敗時，您可以使用下列命令來尋找最後發生的錯誤。  
   ```
   klist kcd_cache
   ```  
6. 當您想要診斷使用者或服務是否可以取得伺服器的票證時，您可以使用此命令來要求特定 SPN 的票證。  
   ```
   klist get host/%computername%
   ```  
7. 診斷網域控制站之間的複寫問題時，您通常需要用戶端電腦以特定網域控制站為目標。 在這些情況下，您可以使用下列命令，將用戶端電腦的目標設為該特定網域控制站。  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. 若要查詢這部電腦最近接觸過的網域控制站，您可以使用下列命令。  
   ```
   klist query_bind
   ```  
9. 當您想要讓 Kerberos 重新探索網域控制站時，您可以使用下列命令。 此命令也可以在使用 klist add_bind 建立新的網域控制站系結之前，用來排清快取。  
   ```
   klist purge_bind
   ```

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)