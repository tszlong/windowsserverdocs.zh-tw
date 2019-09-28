---
title: ktpass
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 608757b4e366693bf0b4d006ad866ecab4117805
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374775"
---
# <a name="ktpass"></a>ktpass

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定 active directory 網域服務（AD DS）中主機或服務的伺服器主體名稱，並產生包含服務之共用秘密金鑰的 keytab 檔案。 .keytab 檔案是根據 Kerberos 驗證通訊協定的 Massachusetts Institute of Technology (麻省理工學院，MIT) 實作。 Ktpass 命令列工具可讓支援 Kerberos 驗證的非 Windows 服務使用 Kerberos 金鑰發佈中心（KDC）服務所提供的互通性功能。 本主題適用于本主題開頭的**適用**物件清單中指定的作業系統版本。  

## <a name="syntax"></a>語法  
```  
ktpass  
[/out <FileName>]   
[/princ <PrincipalName>]   
[/mapuser <UserAccount>]   
[/mapop {add|set}] [{-|+}desonly] [/in <FileName>]  
[/pass {Password|*|{-|+}rndpass}]  
[/minpass]  
[/maxpass]  
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]  
[/itercount]  
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]  
[/kvno <KeyversionNum>]  
[/answer {-|+}]  
[/target]  
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <Password>]  [/?|/h|/help]  
```  
## <a name="parameters"></a>參數  

|                                             參數                                              |                                                                                                                                                                                                                                                                                                      描述                                                                                                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                          /out <FileName>                                           |                                                                                                                                                                        指定要產生的 Kerberos 版本 5. keytab 檔案的名稱。 **注意：** 這是您要傳送到未執行 Windows 作業系統之電腦的 keytab 檔案，然後以您現有的 keytab 檔案（/Etc/Krb5.keytab.）取代或合併。                                                                                                                                                                        |
|                                       /princ <PrincipalName>                                       |                                                                                                                                                                                                                   以 host/computer.contoso.com@CONTOSO.COM 的格式指定主體名稱。 **警告：** 這個參數會區分大小寫。 如需詳細資訊，請參閱[備註](#BKMK_remarks)。                                                                                                                                                                                                                    |
|                                       /mapuser <UserAccount>                                       |                                                                                                                                                                                                                                                將**princ**參數所指定的 Kerberos 主體名稱對應至指定的網域帳戶。                                                                                                                                                                                                                                                |
|                                       /mapop {加入&#124;set}                                        |                                                                                                                                                                             指定如何設定對應屬性。<br /><br />-   **add**會新增指定之本機使用者名稱的值。 這是預設值。<br />-   **設定**會為指定的本機使用者名稱設定「資料加密標準（DES）-僅加密」的值。                                                                                                                                                                             |
|                                         {-&#124;+} desonly                                          |                                                                                                                                                            預設會設定 [僅限 DES 加密]。<br /><br />-    **+** 會為僅限 DES 的加密設定帳戶。<br />-    **-** 會針對僅限 DES 加密的帳戶釋放限制。 **重大**從 Windows 7 和 Windows Server 2008 R2 開始，Windows 預設不支援 DES。                                                                                                                                                            |
|                                           /in <FileName>                                           |                                                                                                                                                                                                                                                       指定要從不是執行 Windows 作業系統的主機電腦讀取的 keytab 檔案。                                                                                                                                                                                                                                                        |
|                          /pass {Password&#124;\*&#124;{-&#124;+} rndpass}                           |                                                                                                                                                                                                                                           指定**princ**參數所指定之主體使用者名稱的密碼。 使用 "\*" 來提示輸入密碼。                                                                                                                                                                                                                                            |
|                                              /minpass                                              |                                                                                                                                                                                                                                                                            將隨機密碼的最小長度設定為15個字元。                                                                                                                                                                                                                                                                            |
|                                              /maxpass                                              |                                                                                                                                                                                                                                                                           將隨機密碼的最大長度設定為256個字元。                                                                                                                                                                                                                                                                            |
| /crypto {DES-CBC-CRC&#124;DES-CBC-MD5&#124;RC4-HMAC-NT&#124;AES256-sha1&#124;AES128-sha1&#124;All} | 指定在 keytab 檔案中產生的金鑰：<br /><br />-   **DES-CBC-CRC**用於相容性。<br />-   **DES-CBC-MD5**會更緊密地遵循 MIT 的執行，並用於相容性。<br />-   **RC4-HMAC-NT**採用128位加密。<br />-   **AES256-sha1**採用 AES256-CTS-HMAC-sha1-96 加密。<br />-   **AES128-sha1**採用 AES128-CTS-HMAC-sha1-96 加密。<br />-   **所有**狀態都可以使用所有支援的密碼編譯類型。 **注意：** 預設設定是以較舊的 MIT 版本為基礎。 因此，應一律指定 `/crypto`。 |
|                                             /itercount                                             |                                                                                                                                                                                                                        指定用於 AES 加密的反復專案計數。 預設值是針對非 AES 加密忽略**itercount** ，並在4096設定 aes 加密。                                                                                                                                                                                                                         |
|               /ptype {KRB5_NT_PRINCIPAL&#124;KRB5_NT_SRV_INST&#124;KRB5_NT_SRV_HST}                |                                                                                                                                                                                         指定主體類型。<br /><br />-   **KRB5_NT_PRINCIPAL**是一般主體類型（建議選項）。<br />-   **KRB5_NT_SRV_INST**是使用者服務實例。<br />-   **KRB5_NT_SRV_HST**是主機服務實例。                                                                                                                                                                                         |
|                                       /kvno <KeyversionNum>                                        |                                                                                                                                                                                                                                                                               指定金鑰版本號碼。 預設值為 1。                                                                                                                                                                                                                                                                                |
|                                         /answer {-&#124;+}                                         |                                                                                                                                                                                                                    設定背景回應模式：<br /><br />**-** 自動使用 [否] 回應重設密碼提示。<br /><br />**+** 使用 [是] 自動回答 [重設密碼] 提示。                                                                                                                                                                                                                     |
|                                              /target                                               |                                                                                                                                                                                           設定要使用的網域控制站。 預設值是根據主體名稱來偵測網域控制站。 如果網域控制站名稱無法解析，則會出現一個對話方塊，提示您輸入有效的網域控制站。                                                                                                                                                                                           |
|                                              /rawsalt                                              |                                                                                                                                                                                                                                                           強制 ktpass 在產生金鑰時使用 rawsalt 演算法。 不需要這個參數。                                                                                                                                                                                                                                                            |
|                                         {-&#124;+} dumpsalt                                         |                                                                                                                                                                                                                                                           此參數的輸出會顯示用來產生金鑰的 MIT salt 演算法。                                                                                                                                                                                                                                                            |
|                                          {-&#124;+} setupn                                          |                                                                                                                                                                                                                                          除了服務主體名稱（SPN）之外，設定使用者主體名稱（UPN）。 預設值是在 keytab 檔案中設定兩者。                                                                                                                                                                                                                                           |
|                                    {-&#124;+} setpass <Password>                                    |                                                                                                                                                                                                                                                          在提供時設定使用者的密碼。 如果使用 rndpass，則會改為產生隨機密碼。                                                                                                                                                                                                                                                           |
|                                       /?&#124;/h&#124;/help                                        |                                                                                                                                                                                                                                                                                         顯示 ktpass 的命令列說明。                                                                                                                                                                                                                                                                                         |

## <a name="BKMK_remarks"></a>標記  
在不是執行 Windows 作業系統的系統上執行的服務，可以使用 active directory 網域服務中的服務實例帳戶進行設定。 這可讓任何 Kerberos 用戶端使用 Windows Kdc 向不是執行 Windows 作業系統的服務進行驗證。  
/Princ 參數不會由 ktpass 評估，並會依照提供的方式使用。 產生 Keytab 檔案時，並不會檢查參數是否符合**userPrincipalName**屬性值的確切大小寫。 使用這個 Keytab 檔的區分大小寫 Kerberos 散發，可能會在沒有完全相符的大小寫，而且在預先驗證期間可能會發生問題。 從 LDifDE 匯出檔案檢查並取出正確的**userPrincipalName**屬性值。 例如:  
```  
ldifde /f keytab_user.ldf /d "CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com" /p base /l samaccountname,userprincipalname  
```  
## <a name="BKMK_examples"></a>典型  
下列範例說明如何在使用者 Sample1 的目前目錄中，建立 keytab 檔案 keytab。 （您會將此檔案與未執行 Windows 作業系統的主機電腦上的 Krb5. keytab 檔案合併）。系統會針對一般主體類型的所有支援的加密類型，建立 keytab 檔案。  
若要為不是執行 Windows 作業系統的主機電腦產生 keytab 檔案，請使用下列步驟將主體對應至帳戶，並設定主機主體密碼：  
1.  使用 [active directory 使用者和電腦] 嵌入式管理單元，在未執行 Windows 作業系統的電腦上建立服務的使用者帳戶。 例如，建立名稱為 Sample1 的帳戶。  
2.  使用 ktpass 來設定使用者帳戶的身分識別對應，方法是在命令提示字元中輸入下列內容：  
    ```  
    ktpass /princ host/Sample1.contoso.com@CONTOSO.COM /mapuser Sample1 /pass MyPas$w0rd /out Sample1.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set   
    ```  

    > [!NOTE]  
    > 您不能將多個服務實例對應到相同的使用者帳戶。  

3.  在未執行 Windows 作業系統的主機電腦上，將 keytab 檔案與/Etc/Krb5.keytab 檔案合併。 

#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
