---
title: certreq
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b947a231228d66084bc61146f7347a76cf41406
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434515"
---
# <a name="certreq"></a>certreq



Certreq 可用來向憑證授權單位 (CA) 要求憑證，來擷取 CA，以建立新的要求從.inf 檔案，以接受並安裝要求，來建構跨憑證的回應中的前一個要求的回應或合格的分類要求，從現有的 CA 憑證或要求，並簽署跨憑證或完整的分類要求。

> [!WARNING]
> - 舊版的 certreq 可能不會提供所有這份文件中所述的選項。 您可以看到特定版本的 certreq 執行語法標記法一節中所示的命令所提供的所有選項。
> - Certreq 不支援建立新的憑證要求，以在 CEP/CES 的環境中的金鑰證明範本為基礎

## <a name="BKMK_Contents"></a>內容

這篇文章中的主要區段如下所示：
1.  [Verbs](#BKMK_Verbs)
2.  [語法標記法](#BKMK_notation)
3.  [選項](#BKMK_Options)
4.  [格式](#BKMK_Formats)
5.  [其他 certreq 範例](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>動詞命令

那里下表描述可以搭配 certreq 命令的動詞命令

|參數|描述|
|------|-----------|
|-Submit|提交要求給 CA。 如需詳細資訊，請參閱 < [Certreq-提交](#BKMK_Submit)。|
|-擷取*RequestID*|擷取來自 CA 的前一個要求的回應。 如需詳細資訊，請參閱 < [Certreq-擷取](#BKMK_Retrieve)。|
|-New|從.inf 檔案建立新的要求。 如需詳細資訊，請參閱 < [Certreq-新](#BKMK_New)。|
|-Accept|接受並安裝憑證要求的回應。 如需詳細資訊，請參閱 < [Certreq-接受](#BKMK_accept)。|
|-Policy|設定要求的原則。 如需詳細資訊，請參閱 < [Certreq-原則](#BKMK_policy)。|
|-Sign|簽署跨憑證或完整的分類要求。 如需詳細資訊，請參閱 < [Certreq-登](#BKMK_sign)。|
|-Enroll|註冊或更新憑證。 如需詳細資訊，請參閱 < [Certreq-註冊](#BKMK_enroll)。|
|-?|會顯示一份 certreq 語法、 選項及描述。|
|*\<verb>* -?|顯示說明所指定的動詞。|
|-v -?|顯示 certreq 語法選項及描述的詳細資訊清單。|

返回[內容](#BKMK_Contents)

## <a name="BKMK_notation"></a>語法標記法

-   如需執行的基本命令列語法 `certreq -?`
-   如需有關使用使用特定的動詞命令的 certutil 語法，請執行**certreq** *\<動詞 >* **-嗎？**
-   若要傳送的 certutil 語法的所有文字檔案，請執行下列命令：  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

下表描述用來表示命令列語法標記法。

|標記法|描述|
|--------|-----------|
|文字，而不需要方括號或大括號|您必須依照顯示輸入的項目|
|\<角括弧內的文字 >|您必須提供值的預留位置|
|[方括號內的文字]|選擇性的項目|
|{大括號內的文字}|一組必要的項目;選擇其中一個|
|分隔號 (&#124;)|分隔符號是互斥的項目;選擇其中一個|
|省略符號 （...）|可重複的項目|

返回[內容](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-提交

如果在命令列提示字元中明確不指定任何選項，這是預設 certreq.exe 參數，certreq.exe 會嘗試提交憑證要求到 CA。
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
使用時，您必須指定憑證要求檔案 – submit 選項。 如果省略這個參數，則會顯示常見的檔案開啟視窗，您可以在其中選取適當的憑證要求檔案。

您可以使用這些範例做為起點，來建置您的憑證提交要求：

若要提交簡單的憑證要求，請使用下列範例：
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
若要指定 SAN 屬性，藉以要求憑證，請參閱 Microsoft 知識庫文章 931351 中的詳細的步驟[如何將主旨替代名稱加入至安全的 LDAP 憑證](https://support.microsoft.com/kb/931351)中的 < 如何使用 Certreq.exe 公用程式建立並提交憑證要求，其中包含 SAN 」 一節。

返回[內容](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-擷取

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   如果您未指定 CAComputerName 或 CAName，在-config CAComputerName\CANamea 對話方塊隨即出現，並會顯示一份可用的所有 Ca。
-   如果您使用-config-而不是-config CAComputerName\CAName 時，使用預設 CA 處理作業。
-   您可以使用 certreq-擷取*RequestID* CA 已實際發行它之後，請擷取憑證。 *RequestID*PKC 可以是十進位或十六進位的 0x 前置詞和它可以是憑證序號，沒有 0x 前置詞。 您也可以使用它來擷取以往發出 ca，包括已撤銷或過期的憑證，而不考慮憑證的要求是否曾在暫止狀態中的任何憑證。
-   如果您提交要求到 CA，CA 的原則模組可能會導致在暫止狀態並傳回要求*RequestID* Certreq 呼叫端，以供顯示。 最後，CA 的系統管理員將會發行憑證，或拒絕要求。

下列命令會擷取憑證識別碼 20，並建立憑證檔案 (.cer):
```
certreq -retrieve 20 MyCertificate.cer 
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-new

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
INF 檔案允許一組豐富的參數和指定的選項，因為它很難定義系統管理員應該用於所有用途的預設範本。 因此，本節會描述可讓您建立您的特定需求來量身打造的 INF 檔案的所有選項。 下列關鍵字用來描述 INF 檔案結構。
1.  A*一節*是 INF 檔案中涵蓋的索引鍵的邏輯群組中的區域。 區段一律會出現在 INF 檔案中的括號中。
2.  A*金鑰*是等號左邊的參數。
3.  A*值*是等號右邊的參數。

例如，最小的 INF 檔案看起來會如下所示：
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
以下是一些可能會加入 INF 檔案中的可能區段：

**[NewRequest]**

此區段是做為新的憑證要求的範本的 INF 檔案的必要項目。 此區段會需要至少一個索引鍵的值。

|Key|定義|值|範例|
|---|----------|-----|-------|
|Subject|數個應用程式依賴於憑證的主體資訊。 因此，建議指定這個機碼的值。 如果此處沒有設定主體，則建議的主體名稱是主體的別名憑證延伸模組的一部分。|相對辨別名稱的字串值|Subject = "CN=computer1.contoso.com" Subject="CN=John Smith,CN=Users,DC=Contoso,DC=com"|
|Exportable|如果這個屬性設定為 TRUE，就可以匯出私密金鑰的憑證。 為了確保高層級的安全性，私密金鑰不應該匯出;不過，在某些情況下，它可能需要進行的私密金鑰可匯出，如果數個電腦或使用者必須共用相同的私密金鑰。|true、 false|可匯出 = TRUE。 CNG 金鑰可以區別這與純文字可匯出。 CAPI1 金鑰不能。|
|ExportableEncrypted|指定是否應該設定私密金鑰可以匯出。|true、 false|ExportableEncrypted = true</br>秘訣：並非所有的公用金鑰的大小和演算法會使用所有的雜湊演算法。 Tamehe 指定 CSP 也必須支援指定的雜湊演算法。 若要查看支援的雜湊演算法的清單，您可以執行命令 <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|要用於此要求的雜湊演算法。|Sha256，sha384、 sha512、 sha1、 md5、 md4、 md2|HashAlgorithm = sha1。 若要查看使用支援的雜湊演算法清單： certutil-oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo|
|KeyAlgorithm|將服務提供者用來產生公開和私密金鑰組演算法。|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|它建議不要設定此參數，新的要求產生新的金鑰內容的位置。 自動產生及維護系統的金鑰容器。 其中應該使用現有的金鑰材料的要求，這個值可以設為現有金鑰的金鑰容器名稱。 使用 certutil – key 命令來顯示機器內容的可用金鑰容器的清單。 使用 certutil – key – user 命令目前使用者的內容。|隨機的字串值</br>秘訣：您應該使用雙引號括住有空格或特殊字元，以避免發生潛在的 INF 剖析問題任何 INF 值。|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|定義公用和私用金鑰的長度。 金鑰長度會影響憑證的安全性層級上。 較長的金鑰長度通常提供較高的安全性層級;不過，有些應用程式可能會有相關的金鑰長度限制。|密碼編譯服務提供者支援任何有效金鑰長度。|KeyLength = 2048|
|KeySpec|判斷簽章、 交換 （加密），或兩者，是否可以使用的金鑰。|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|KeyUsage|定義憑證金鑰應該用途。|CERT_DIGITAL_SIGNATURE_KEY_USAGE-80 (128)</br>秘訣：所顯示的值是十六進位 （十進位） 的值，每個元定義。 也可以用較舊的語法： 單一與多個十六進位值的位元集，而不是符號表示。 比方說，KeyUsage = 0xa0。</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>秘訣：多個值會使用管道 (&#124;) 符號分隔。 請確定您使用雙引號，當使用多個值，以避免 INF 剖析問題。|
|KeyUsageProperty|擷取值，識別用於私用金鑰的特定目的。|NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|當您需要建立的機器而不是使用者所擁有的憑證時，這個金鑰是很重要。 產生的金鑰材料會維護的安全性內容中的安全性主體 （使用者或電腦帳戶），建立該要求。 當系統管理員會建立代表電腦的憑證要求時，金鑰的內容必須建立電腦的安全性內容和系統管理員的安全性內容中。 否則，因為它會是系統管理員的安全性內容中電腦會無法存取其私密金鑰。|true、 false|MachineKeySet = true</br>秘訣：預設值為 false。|
|NotBefore|指定日期或日期和時間之前無法發出要求。 NotBefore 可以搭配 ValidityPeriod 和 ValidityPeriodUnits。|日期或日期和時間|NotBefore ="2012 年 7 月 24 日上午 10:31 」</br>秘訣：NotBefore 和 NotAfter 適用於 RequestType = 只有憑證。剖析日期會嘗試將會區分地區設定。使用名稱將釐清，而且用於每個地區設定的月份。|
|NotAfter|指定日期或日期和時間之後，無法發出要求。 NotAfter 無法搭配 ValidityPeriod 或 ValidityPeriodUnits。|日期或日期和時間|NotAfter = 「 2014 年 9 月 23 日上午 10:31 」</br>秘訣：NotBefore 和 NotAfter 適用於 RequestType = 只有憑證。剖析日期會嘗試將會區分地區設定。使用名稱將釐清，而且用於每個地區設定的月份。|
|PrivateKeyArchive|只有在相對應的 RequestType 會設定為 「 CMC 」，因為只有憑證管理訊息 over CMS (CMC) 要求格式允許安全地傳輸到 CA 進行金鑰保存的 要求者的私密金鑰，就會運作 PrivateKeyArchive 設定。|true、 false|PrivateKeyArchive = True|
|EncryptionAlgorithm|要使用的加密演算法。|可能的選項而異的作業系統版本和已安裝的密碼編譯提供者的集合。 若要查看可用的演算法清單，請執行命令<code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code>指定使用的 CSP 也必須支援指定的對稱加密演算法和長度。|EncryptionAlgorithm = 3des|
|EncryptionLength|若要使用的加密演算法的長度。|任何指定 EncryptionAlgorithm 所允許的長度。|EncryptionLength = 128|
|ProviderName|提供者名稱是 CSP 的顯示名稱...|如果您不知道所使用的 CSP 的提供者名稱，請從命令列執行 certutil – csplist。 該命令會顯示所有本機系統可用的 Csp 的名稱|ProviderName = "Microsoft RSA SChannel Cryptographic Provider"|
|ProviderType|提供者類型用來選取特定的提供者根據特定的演算法功能，例如 「 RSA Full"。|如果您不知道所使用的 CSP 的提供者類型，請從命令列執行 certutil – csplist。 此命令會顯示所有本機系統可用的 Csp 的提供者類型。|ProviderType = 1|
|RenewalCert|如果您需要更新存在於的系統產生憑證要求的位置的憑證，您必須指定其憑證雜湊做為此索引鍵的值。|可在其中建立憑證要求電腦的任何憑證的憑證雜湊。 如果您不知道憑證雜湊，使用憑證 MMC 嵌入式管理單元，並看看應該要更新的憑證。 開啟 [憑證內容]，請參閱 < 憑證的 「 指紋 」 屬性。 憑證的更新需要有 pkcs#7 或 CMC 要求格式。|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>注意:這可讓另一個使用者要求代表註冊要求。要求也必須使用註冊代理 憑證來簽署或 CA 將會拒絕要求。 使用指定的註冊代理程式憑證-憑證選項。|如果 RequestType 設為 pkcs#7 或 CMC 要求者名稱可以指定為憑證要求。 如果 RequestType 設 PKCS #10 中，將會忽略此金鑰。 Requestername 只能設定為要求的一部分。 您無法操作暫止的要求中 Requestername。|網域 \ 使用者|Requestername = "Contoso\BSmith"|
|RequestType|決定用來產生並傳送憑證要求的標準。|PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>憑證-4</br>SCEP -- fd00 (64768)</br>秘訣：這個選項表示的自我簽署或自行發出的憑證。 它不會產生要求，但而不是新的憑證，並安裝憑證。自我簽署是預設值。使用指定的簽署憑證 – 建立自行發出的憑證不是自我簽署的憑證選項。|RequestType = CMC|
|SecurityDescriptor</br>秘訣：這是只對電腦內容非智慧卡金鑰相關。|包含與安全性實體物件相關聯的安全性資訊。 最安全的物件，您可以指定物件的安全性描述元中會建立物件的函式呼叫。|字串依[安全性描述元定義語言](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx)。|SecurityDescriptor = "D:P(A;;GA;;;SY)(A;;GA;;;BA)"|
|AlternateSignatureAlgorithm|指定並擷取布林值，表示 PKCS #10 要求或憑證簽章的簽章演算法物件識別碼 (OID) 是離散或合併。|true、 false|AlternateSignatureAlgorithm = false</br>秘訣：RSA 簽章，false 表示 Pkcs1 v1.5。 True 表示 v2.1 簽章。|
|無訊息|根據預設，此選項可讓使用者從 CSP 存取互動式使用者桌面和要求資訊，例如智慧卡 PIN。 假如此金鑰設為 TRUE，CSP 必須與桌面互動，而且將無法向使用者顯示任何使用者介面。|true、 false|無訊息 = true|
|SMIME|如果此參數設定為 TRUE，則會將物件識別項值 1.2.840.113549.1.9.15 的延伸模組加入至要求。 物件識別項的數目取決於安裝作業系統版本和 CSP 的容量，這可能會由 Secure Multipurpose Internet Mail Extensions (S/MIME) 應用程式，例如 Outlook 的對稱式加密演算法，請參閱。|true、 false|SMIME = true|
|UseExistingKeySet|這個參數用來指定現有的金鑰組應該用於建置憑證要求。 如果此機碼設為 TRUE 時，您也必須指定 RenewalCert 金鑰或 KeyContainer 名稱的值。 因為您無法變更現有金鑰的屬性不可設定成可匯出金鑰。 在此情況下，建立憑證要求時，會不產生任何金鑰材料。|true、 false|UseExistingKeySet = true|
|KeyProtection|指定值，指出在使用前先保護私密金鑰的方式。|XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|指定布林值，指出是否會將預設擴充功能和屬性包含在要求中。 預設值是由其物件識別碼 (Oid) 代表。|true、 false|SuppressDefaults = true|
|FriendlyName|新憑證的易記名稱。|Text|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>注意:這只使用要求的類型時 = 憑證。|指定的數字的單位來搭配 ValidityPeriod。|Numeric|ValidityPeriodUnits = 3|
|ValidityPeriod</br>注意:這只使用要求的類型時 = 憑證。|VValidityPeriod 必須是英文 （美國） 複數時間週期。|年、 月數週，數天、 小時、 分鐘秒|ValidityPeriod = 年|

返回[內容](#BKMK_Contents)

**[Extensions]**

此區段是選擇性的。


|  延伸模組的 OID   | 定義 | 值 |                                                                                                                                                                                                                                                                                                                                                                                                                      範例                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN=User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "EMail=User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS=host.domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DirectoryName=CN=Name,DC=Domain,DC=com&"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL=<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress=10.0.0.1&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId=1.2.3.4.5&"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1={utf8}String&"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2={octet}AAECAwQFBgc=&"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37="{text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{text}ca=0pathlength=3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     嚴重     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Critical=2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE -- 0</br>AT_SIGNATURE-2</br>AT_KEYEXCHANGE -- 1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>憑證-4</br>SCEP -- fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     KeyUsage     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE-80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  KeyProtection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG -- 0</br>NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  範本  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME-40000000 (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME-8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE -- 0</br>CERT_OID_NAME_STR -- 2</br>CERT_X500_NAME_STR -- 3</br>CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG-10000000 (268435456)</br>CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)</br>CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)</br>CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)</br>CERT_NAME_STR_FORWARD_FLAG-1000000 (設為 16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152) |

返回[內容](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags 可讓您指定的主旨和 SubjectAltName 延伸模組欄位應該會自動填入 certreq 根據目前的使用者或目前的機器內容的 INF 檔案：DNS 名稱、 UPN 和等等。 使用常值 「 範本 」 表示改為使用範本名稱旗標。 這可讓單一的 INF 檔案來產生與特定內容的主體資訊的要求，在多個內容中使用。
>
> X500NameFlags 指定的旗標來直接傳遞給 CertStrToName API 時的主旨 INF 金鑰值會轉換成 ASN.1 編碼的辨別名稱。

若要要求憑證，以使用 certreq-new 使用下列範例中的步驟：

> [!WARNING]
> 本主題的內容根據 Windows Server 2008 AD CS; 的預設設定例如，設定金鑰長度為 2048，身為 CSP，選取 Microsoft 軟體金鑰儲存提供者，並使用安全雜湊演算法 1 (SHA1)。 評估這些選取項目，根據貴公司的安全性原則的需求。

若要建立的原則檔案 (.inf) 複製並儲存下的面的範例 [記事本] 中並儲存為 RequestConfig.inf:
```
[NewRequest] 
Subject = "CN=<FQDN of computer you are creating the certificate>" 
Exportable = TRUE 
KeyLength = 2048 
KeySpec = 1 
KeyUsage = 0xf0 
MachineKeySet = TRUE 
[RequestAttributes]
CertificateTemplate="WebServer"
[Extensions] 
OID = 1.3.6.1.5.5.7.3.1 
OID = 1.3.6.1.5.5.7.3.2  
```
您要求憑證的電腦上輸入下列命令：
```
CertReq –New RequestConfig.inf CertRequest.req 
```
下列範例會示範實作 [Strings] 區段語法，Oid 和其他難以解譯資料。 新的 {text} 語法範例，EKU 延伸模組，使用逗號分隔 Oid 的清單：
```
[Version]
Signature="$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1"
szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

[NewRequest]
Subject = "CN=TestSelfSignedCert"
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%="{text}%szOID_PKIX_KP_SERVER_AUTH%,"
_continue_ = "%szOID_PKIX_KP_CLIENT_AUTH%"
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq -accept

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– 接受參數連結所發出的憑證與先前產生的私密金鑰和憑證要求 （如果沒有相符的要求），系統會移除擱置的憑證要求。

您可以使用此範例中手動接受憑證：
```
certreq -accept certnew.cer 
```

> [!WARNING]
> -接受動詞命令、-使用者和 – 電腦的選項會指出是否應該在安裝憑證安裝在使用者或電腦內容。 如果其中一個符合所安裝的公用金鑰的內容中沒有未完成的要求，就不會需要這些選項。 如果沒有任何未處理的要求，則其中一個必須指定。

返回[內容](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-原則

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   定義合格的分類定義時，會套用到 CA 憑證的條件約束的組態檔稱為 Policy.inf...
-   您可以找到 Policy.inf 檔案中的範例[附錄 A 的規劃和實作交互憑證及合格的分類](https://technet.microsoft.com/library/cc738878(WS.10).aspx)白皮書 （英文）。
-   如果您鍵入 certreq-原則，而不需要任何額外的參數它將會開啟對話方塊視窗，因此您可以選取要求檔案 （要求、 cmc、 txt、 d、 cer 或 crt）。 一旦您選取要求的檔案，並按一下 [開啟] 按鈕，另一個對話方塊視窗會開啟，就可以選取 INF 檔案中。

您可以使用此範例來建置交互憑證要求：
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-標誌

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   如果您鍵入 certreq-符號而不需要任何額外的參數它將會開啟對話方塊視窗讓您可以選取要求的檔案 （要求、 cmc、 txt、 d、 cer 或 crt）。
-   簽署合格的分類要求，可能需要企業系統管理員認證。 這是發行合格的分類的簽署憑證的最佳作法。
-   使用合格的分類範本來建立用來簽署合格的分類要求的憑證。 企業系統管理員必須登入要求，或授與將要簽署憑證之個人的使用者權限。
-   當您簽署 CMC 要求時，您可能需要有多個登入此要求，根據與合格的分類相關聯的保證層級的人員。
-   如果合格的次級 CA，您要安裝父系 CA 離線，您必須為合格的次級 CA 取得 CA 憑證，從離線的父項目。 如果父系 CA 在線上時，指定合格的次級 CA 的 CA 憑證，憑證服務安裝精靈執行期間。

下列命令序列會顯示如何建立新的憑證要求、 簽署及提交：
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-註冊

若要註冊的憑證
```
certreq –enroll [Options] TemplateName
```
若要更新現有的憑證
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
您只能更新有效時間的憑證。 過期的憑證無法更新，而且必須以新的憑證取代。

以下是範例的更新憑證，使用它的序號：
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
這裡註冊至憑證範本的範例會呼叫 web 伺服器選取原則伺服器，透過 U/i： 使用星號 （*）
```
certreq -enroll –machine –policyserver * "WebServer"
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_Options"></a>選項

|選項。|描述|
|-------|-----------|
|-任何|若要判斷編碼類型的強制 ICertRequest::Submit。|
|-attrib \<AttributeString>|指定名稱和值字串組，並以冒號分隔。</br>不同的名稱和值字串組 \n (比方說，Name1:Value1\nName2:Value2)。|
|-二進位|格式為而不是 base64 編碼的二進位輸出檔案。|
|-PolicyServer *\<PolicyServer>*|"ldap: *\<路徑 >* "</br>插入的 URI 或執行憑證註冊原則 Web 服務的電腦的唯一識別碼。</br>若要指定您想要使用要求檔案，藉由瀏覽，只要使用減號 （-） 符號 *\<policyserver >* 。|
|-config \<ConfigString>|使用組態字串中，也就是 CAHostName\CAName 中指定的 CA 處理作業。 針對 https 連線，指定註冊的伺服器 URI。 針對本機電腦存放區 CA，請使用減號 （-） 符號。|
|-Anonymous|憑證註冊 Web 服務使用匿名認證。|
|-Kerberos|使用 Kerberos （網域） 認證的憑證註冊 Web 服務。|
|-ClientCertificate *\<ClientCertId>*|您可以取代 *\<ClientCertID >* 具有憑證指紋、 CN、 EKU、 範本、 電子郵件、 UPN 和新名稱 = 值語法。|
|-UserName *\<使用者名稱 >*|與憑證註冊 Web 服務搭配使用。 您可以使用替代 *\<使用者名稱 >* SAM 名稱或網域 \ 使用者。 此選項是與-p 選項搭配使用。|
|-p *\<密碼 >*|與憑證註冊 Web 服務搭配使用。 Substitute *\<密碼 >* 取代實際使用者的密碼。 此選項僅供搭配-UserName 選項的使用。|
|-使用者|設定新的憑證要求的-使用者內容或指定的內容憑證接受。 如果未指定 INF 或範本中，這是預設的內容。|
|-電腦|設定新的憑證要求，或指定的內容憑證接受機器內容。 新的要求必須符合 MachineKeyset INF 金鑰和範本的內容。 如果沒有指定這個選項，而且範本不會設定內容，預設值是使用者內容。|
|-crl|包含憑證撤銷清單 (Crl)，在輸出中，以 base64 編碼的 PKCS #7 檔案，而此一 CertChainFileOut 所指定或指定 RequestFileOut base64 編碼的檔案。|
|-rpc|指示 Active Directory 憑證服務 (AD CS) 若要使用遠端程序呼叫 (RPC) 伺服器連接，而不是分散式 COM|
|-AdminForceMachine|要提交要求的本機系統內容中使用的金鑰服務或模擬。 需要叫用此選項的使用者是本機系統管理員的成員。|
|-RenewOnBehalfOf|代表簽章憑證中所識別的主體中提交更新。 呼叫時，這會設定 CR_IN_ROBO [ICertRequest::Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|強制覆寫現有的檔案。 這也會略過快取的範本和原則。|
|-q|使用無訊息模式;抑制所有的互動式提示。|
|-Unicode|Unicode 輸出時寫入標準輸出重新導向或使用管線傳送至另一個命令，可協助從 Windows PowerShell® 指令碼叫用時）。|
|-UnicodeText|將 base64 文字寫入編碼的資料檔案的 blob 時，會將 Unicode 輸出傳送。|

返回[內容](#BKMK_Contents)

## <a name="BKMK_Formats"></a>格式

|格式|描述|
|-------|-----------|
|RequestFileIn|Base64 編碼或二進位的輸入的檔案名稱：PKCS #10 憑證要求、 CMS 憑證要求，PKCS #7 憑證更新要求，是交互檢定的 X.509 憑證或 KeyGen 標記格式的憑證要求。|
|RequestFileOut|Base64 編碼的輸出檔名稱|
|CertFileOut|Base64 編碼的 x-509 檔案名稱。|
|PKCS10FileOut|搭配[Certreq-原則](#BKMK_policy)只動詞命令。 Base64 編碼 PKCS10 輸出檔案名稱。|
|CertChainFileOut|Base64 編碼的 PKCS #7 檔案名稱。|
|FullResponseFileOut|Base64 編碼完整的回應檔案名稱。|
|PolicyFileIn|搭配[Certreq-原則](#BKMK_policy)只動詞命令。 包含用來限定要求的擴充功能的文字表示的 INF 檔案。|

## <a name="BKMK_Examples"></a>其他 certreq 範例

下列文章包含 certreq 使用方式的範例：
-   [如何要求含有自訂主體替代名稱的憑證](https://technet.microsoft.com/library/ff625722.aspx)
-   [測試實驗室指南：部署 AD CS 兩層式 PKI 階層](https://technet.microsoft.com/library/hh831348.aspx)
-   [附錄 3:Certreq.exe 語法](https://technet.microsoft.com/library/cc736326.aspx)
-   [如何手動建立的 web 伺服器 SSL 憑證](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [要求 AMT 佈建使用 Windows Server 2008 CA 的憑證](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [System Center Operations Manager 代理程式的憑證註冊](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [AD CS 逐步解說指南：部署兩層式 PKI 階層](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [如何使用第三方憑證授權單位啟用 LDAP over SSL](https://support.microsoft.com/kb/321051)

返回[內容](#BKMK_Contents)
