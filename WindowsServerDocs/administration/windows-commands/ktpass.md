---
title: ktpass
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ae7508aaefd136a91bd19e547b407020293ca484
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437919"
---
# <a name="ktpass"></a>ktpass

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 active directory 網域服務 (AD DS) 中設定的主機或服務的伺服器主體名稱，並產生包含服務的共用祕密金鑰的.keytab 檔案。 .keytab 檔案是根據 Kerberos 驗證通訊協定的 Massachusetts Institute of Technology (麻省理工學院，MIT) 實作。 Ktpass 命令列工具可讓支援 Kerberos 驗證使用 Kerberos 金鑰發佈中心 (KDC) 服務所提供的互通性功能的非 Windows 服務。 本主題適用於中指定的作業系統版本**套用至**本主題開頭的清單。  

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
|                                          /out <FileName>                                           |                                                                                                                                                                        指定要產生的 Kerberos 版本 5.keytab 檔案名稱。 **注意：** 這是.keytab 檔案傳送到未執行 Windows 作業系統的電腦和之後要取代或合併，以您現存.keytab 檔案，/Etc/Krb5.keytab。                                                                                                                                                                        |
|                                       /princ <PrincipalName>                                       |                                                                                                                                                                                                                   指定的主體名稱格式host/computer.contoso.com@CONTOSO.COM。 **警告：** 此參數會區分大小寫。 請參閱[備註](#BKMK_remarks)如需詳細資訊。                                                                                                                                                                                                                    |
|                                       /mapuser <UserAccount>                                       |                                                                                                                                                                                                                                                將所指定的 Kerberos 主體名稱對應**princ**參數，以指定的網域帳戶。                                                                                                                                                                                                                                                |
|                                       /mapop {add&#124;set}                                        |                                                                                                                                                                             指定的對應屬性的設定方式。<br /><br />-   **新增**加入指定的本機使用者名稱的值。 這是預設值。<br />-   **設定**設定值的資料加密標準 (DES)-只加密已針對指定的本機使用者名稱。                                                                                                                                                                             |
|                                         {-&#124;+}desonly                                          |                                                                                                                                                            僅 DES 加密是預設設定。<br /><br />-    **+** 設定僅 DES 加密的帳戶。<br />-    **-** 僅 DES 加密帳戶上的版本限制。 **重要：** 從 Windows 7 和 Windows Server 2008 R2 開始，Windows 不支援 DES 預設。                                                                                                                                                            |
|                                           /in <FileName>                                           |                                                                                                                                                                                                                                                       指定要讀取從主機電腦未執行 Windows 作業系統的.keytab 檔案。                                                                                                                                                                                                                                                        |
|                          /pass {Password&#124;\*&#124;{-&#124;+}rndpass}                           |                                                                                                                                                                                                                                           指定主體的使用者名稱所指定的密碼**princ**參數。 使用 「\*"提示輸入密碼。                                                                                                                                                                                                                                            |
|                                              /minpass                                              |                                                                                                                                                                                                                                                                            設定為 15 個字元的隨機密碼的最小長度。                                                                                                                                                                                                                                                                            |
|                                              /maxpass                                              |                                                                                                                                                                                                                                                                           設定隨機密碼的最大長度為 256 個字元。                                                                                                                                                                                                                                                                            |
| /crypto {DES-CBC-CRC&#124;DES-CBC-MD5&#124;RC4-HMAC-NT&#124;AES256-SHA1&#124;AES128-SHA1&#124;All} | 指定的索引鍵產生 keytab 檔案中：<br /><br />-   **DES-CBC-CRC**用於相容性。<br />-   **DES CBC MD5**更密切符合 MIT 實作，用於相容性。<br />-   **RC4-HMAC-NT**採用 128 位元加密。<br />-   **AES256-SHA1**採用 AES256-CTS-HMAC-SHA1-96 加密。<br />-   **AES128 SHA1**採用 AES128-CTS-HMAC-SHA1-96 加密。<br />-   **所有**可以用於所有支援的加密類型的狀態。 **注意：** 預設設定根據 MIT 上的舊版。 因此，`/crypto`一定要指定。 |
|                                             /itercount                                             |                                                                                                                                                                                                                        指定用於 AES 加密的反覆項目計數。 預設值是**itercount**會忽略非 AES 加密，而且在 4,096 AES 加密設定。                                                                                                                                                                                                                         |
|               /ptype {KRB5_NT_PRINCIPAL&#124;KRB5_NT_SRV_INST&#124;KRB5_NT_SRV_HST}                |                                                                                                                                                                                         指定主體的類型。<br /><br />-   **KRB5_NT_PRINCIPAL**是一般的主體類型 （建議選項）。<br />-   **KRB5_NT_SRV_INST**是使用者的服務執行個體。<br />-   **KRB5_NT_SRV_HST**主控件服務執行個體。                                                                                                                                                                                         |
|                                       /kvno <KeyversionNum>                                        |                                                                                                                                                                                                                                                                               指定的索引鍵的版本號碼。 預設值為 1。                                                                                                                                                                                                                                                                                |
|                                         回答 / {-&#124;+}                                         |                                                                                                                                                                                                                    設定背景回應模式：<br /><br />**-** 答案重設密碼的提示，自動與 [否]。<br /><br />**+** 答案重設密碼的提示，自動與 [是]。                                                                                                                                                                                                                     |
|                                              /target                                               |                                                                                                                                                                                           設定要使用哪一個網域控制站。 預設值是網域控制站偵測到，根據主體的名稱。 如果網域控制站名稱無法解決，對話方塊會提示有效的網域控制站。                                                                                                                                                                                           |
|                                              /rawsalt                                              |                                                                                                                                                                                                                                                           強制 ktpass rawsalt 演算法產生時要使用的索引鍵。 不需要此參數。                                                                                                                                                                                                                                                            |
|                                         {-&#124;+}dumpsalt                                         |                                                                                                                                                                                                                                                           此參數的輸出會顯示用來產生金鑰 MIT salt 演算法。                                                                                                                                                                                                                                                            |
|                                          {-&#124;+}setupn                                          |                                                                                                                                                                                                                                          設定服務主要名稱 (SPN) 除了使用者主體名稱 (UPN)。 .Keytab 檔案中同時設定為預設值。                                                                                                                                                                                                                                           |
|                                    {-&#124;+}setpass <Password>                                    |                                                                                                                                                                                                                                                          設定使用者的密碼時提供。 如果使用 rndpass，則會改為產生隨機密碼。                                                                                                                                                                                                                                                           |
|                                       /?&#124;/h&#124;/help                                        |                                                                                                                                                                                                                                                                                         Ktpass 命令列的顯示說明。                                                                                                                                                                                                                                                                                         |

## <a name="BKMK_remarks"></a>註解  
使用 active directory 網域服務中的服務執行個體帳戶，可以設定服務未執行 Windows 作業系統的系統上執行。 這可讓任何 Kerberos 用戶端服務未執行 Windows 作業系統使用 Windows Kdc 進行驗證。  
/Princ 參數不會評估 ktpass 並且會使用所提供。 不會檢查參數是否符合的大小寫完全相符**userPrincipalName**產生 Keytab 檔案時，屬性值。 確切的大小寫相符項目時，使用此 Keytab 檔案區分大小寫的 Kerberos 散發套件可能會遇到問題，因此無法在預先驗證時。 檢查並擷取正確**userPrincipalName** LDifDE 匯出檔案中的屬性值。 例如:  
```  
ldifde /f keytab_user.ldf /d "CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com" /p base /l samaccountname,userprincipalname  
```  
## <a name="BKMK_examples"></a>範例  
下列範例說明如何建立 Kerberos.keytab 檔案，而 machine.keytab，使用者 Sample1 目前目錄中。 （您會將合併 Krb5.keytab 檔案不執行 Windows 作業系統的主機電腦上使用此檔案）。Kerberos.keytab 檔案將會建立一般的主體類型的所有支援的加密類型。  
若要產生.keytab 檔案未執行 Windows 作業系統的主機電腦，請對應到帳戶的主體，並設定的主機主體的密碼使用下列步驟：  
1.  在 active directory 使用者和電腦嵌入式管理單元來建立服務的使用者帳戶未執行 Windows 作業系統的電腦上使用。 例如，具有名稱 Sample1 建立帳戶。  
2.  若要設定的使用者帳戶的身分識別對應，在命令提示字元中輸入下列使用 ktpass:  
    ```  
    ktpass /princ host/Sample1.contoso.com@CONTOSO.COM /mapuser Sample1 /pass MyPas$w0rd /out Sample1.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set   
    ```  

    > [!NOTE]  
    > 您無法將多個服務執行個體對應至相同的使用者帳戶。  

3.  合併與 /Etc/Krb5.keytab 檔案不執行 Windows 作業系統的主機電腦上的.keytab 檔案。 

#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
