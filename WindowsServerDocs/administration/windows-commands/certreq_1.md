---
title: certreq
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e3c98fd965fc6923c6af7893261bd1e0eee7d8b
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947595"
---
# <a name="certreq"></a>certreq



Certreq 可以用來向憑證授權單位單位（CA）要求憑證、從 CA 取出先前要求的回應、從 .inf 檔案建立新的要求、接受並安裝要求的回應、建立交叉認證，或來自現有 CA 憑證或要求的合格從屬要求，以及簽署交叉憑證或合格的從屬要求。

> [!WARNING]
> - 舊版的 certreq 可能不會提供本檔中所述的所有選項。 您可以藉由執行語法標記法一節中所示的命令，來查看特定 certreq 版本所提供的所有選項。
> - Certreq 不支援在 CEP/CES 環境中，根據金鑰證明範本來建立新的憑證要求

## <a name="BKMK_Contents"></a>目錄

本文中的主要章節如下所示：
1.  [之外](#BKMK_Verbs)
2.  [語法標記法](#BKMK_notation)
3.  [選項](#BKMK_Options)
4.  [格式](#BKMK_Formats)
5.  [其他 certreq 範例](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>之外

下表描述可搭配 certreq 命令使用的動詞

|切換|說明|
|------|-----------|
|-提交|將要求提交至 CA。 如需詳細資訊，請參閱[Certreq-submit](#BKMK_Submit)。|
|-取出*RequestID*|從 CA 抓取對先前要求的回應。 如需詳細資訊，請參閱[Certreq-取出](#BKMK_Retrieve)。|
|-新增|從 .inf 檔案建立新的要求。 如需詳細資訊，請參閱[Certreq-新增](#BKMK_New)。|
|-接受|接受並安裝憑證要求的回應。 如需詳細資訊，請參閱[Certreq-accept](#BKMK_accept)。|
|-原則|設定要求的原則。 如需詳細資訊，請參閱[Certreq-policy](#BKMK_policy)。|
|-Sign|簽署交叉認證或合格的從屬要求。 如需詳細資訊，請參閱[Certreq-sign](#BKMK_sign)。|
|-註冊|註冊或續約憑證。 如需詳細資訊，請參閱[Certreq-註冊](#BKMK_enroll)。|
|-?|顯示 certreq 語法、選項和描述的清單。|
|*\<動詞 >* -？|顯示指定之動詞命令的說明。|
|-v-？|顯示 certreq 語法、選項和描述的詳細資訊清單。|

返回[內容](#BKMK_Contents)

## <a name="BKMK_notation"></a>語法標記法

-   如需基本命令列語法，請執行 `certreq -?`
-   如需搭配特定動詞使用 certutil 的語法，請執行**certreq** *\<動詞 >* **-？**
-   若要將所有 certutil 語法傳送到文字檔，請執行下列命令：  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

下表說明用來指出命令列語法的表示法。

|表示法|說明|
|--------|-----------|
|不含括弧或大括弧的文字|必須依照顯示輸入的項目|
|\<角括弧內的文字 >|必須在其中提供值的保留區|
|[方括弧內的文字]|選擇性的項目|
|{大括弧內的文字}|必要的項目集；選擇一個|
|分隔號（&#124;）|互斥項目的分隔字元；選擇一個|
|省略符號 (…)|可重複的項目|

返回[內容](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-提交

這是預設的 certreq 參數，如果在命令提示字元中未明確指定任何選項，certreq 會嘗試將憑證要求提交給 CA。
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
您必須在使用 –submit 選項時指定憑證要求檔案。 假使忽略此參數，則會顯示一般的 [開啟舊檔] 視窗，讓您選取適當的憑證要求檔案。

您可以使用這些範例作為起點，以建立您的憑證提交要求：

若要提交簡單的憑證要求，請使用下列範例：
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
若要藉由指定 SAN 屬性來要求憑證，請參閱 Microsoft 知識庫文章931351中的詳細步驟：

返回[內容](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-取出

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   如果您未在-config CAComputerName\CANamea 中指定 [CAComputerName] 或 [CAName] 對話方塊，就會顯示所有可用的 Ca 清單。
-   如果使用 -config - 來代替 -config CAComputerName\CAName，則會以預設 CA 處理作業。
-   您可以使用 certreq-取出*RequestID* ，在 CA 實際發出憑證後加以取出。 *RequestID*PKC 可以是具有0x 前置詞的十進位或十六進位，而且它可以是不含0x 前置詞的憑證序號。 您也可以使用它來抓取 CA 曾經發行的任何憑證，包括已撤銷或過期的憑證，而不考慮憑證的要求是否曾經處於擱置狀態。
-   如果您將要求提交給 CA，CA 的原則模組可能會讓要求處於擱置狀態，並將*RequestID*傳回給 Certreq 呼叫端以供顯示。 最後，CA 的系統管理員會發出憑證或拒絕要求。

下列命令會抓取憑證識別碼20，並建立憑證檔案（.cer）：
```
certreq -retrieve 20 MyCertificate.cer 
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-新增

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
因為 INF 檔案允許指定一組豐富的參數和選項，所以很容易就能定義預設範本，供系統管理員用於所有目的。 因此，本節說明所有選項，讓您能夠建立專為您的特定需求而量身打造的 INF 檔案。 下列是用來描述 INF 檔案結構的關鍵字。
1.  *區段*是 INF 檔案中的一個區域，其中涵蓋了金鑰的邏輯群組。 區段一律會出現在 INF 檔案中的括弧內。
2.  「索引*鍵*」是位於等號左邊的參數。
3.  *值*是等號右邊的參數。

例如，最小的 INF 檔案看起來類似下面所示：
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
以下是一些可能新增至 INF 檔案的可能區段：

**[NewRequest]**

對於作為新憑證要求範本的 INF 檔案而言，這是必要區段。 此區段至少需要一個具有值的索引鍵。

|按鍵|定義|值|範例|
|---|----------|-----|-------|
|主體|有數個應用程式依賴憑證中的主體資訊。 因此，建議為此金鑰指定一個值。 假如此處沒有設定主體，建議在主體的別名憑證延伸中包括主體名稱。|相對辨別名稱字串值|Subject = "CN = computer1，contoso .com" Subject = "CN = John Smith，CN = Users，DC = Contoso，DC = com"|
|Exportable|如果此屬性設為 TRUE，則可與憑證一起匯出私密金鑰。 為了確保高層級的安全性，不應該匯出私密金鑰，不過在某些情況下，如果有數部電腦或使用者必須共用相同的私密金鑰，可能就有必要使私密金鑰可以匯出。|true、false|可匯出 = TRUE。 CNG 金鑰可以區分此和純文字匯出。 CAPI1 索引鍵不能。|
|ExportableEncrypted|指定是否應將私密金鑰設定為可匯出。|true、false|ExportableEncrypted = true</br>提示：並非所有公用金鑰大小和演算法都適用所有雜湊演算法。 Tamehe 指定的 CSP 也必須支援指定的雜湊演算法。 若要查看支援的雜湊演算法清單，您可以執行命令 <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|要用於此要求的雜湊演算法。|Sha256、sha384、sha512、sha1、md5、md4、md2|HashAlgorithm = sha1。 若要查看支援的雜湊演算法清單，請使用： certutil &#124; -oid &#124; 1 Findstr pwszCNGAlgid findstr/v CryptOIDInfo|
|KeyAlgorithm|服務提供者將用來產生公開和私密金鑰組的演算法。|RSA、DH、DSA、ECDH_P256、ECDH_P521、ECDSA_P256、ECDSA_P384、ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|不建議針對產生新金鑰材料的新要求設定此參數。 金鑰容器是由系統自動產生和維護。 對於應該使用現有金鑰材料的要求而言，可將此值設為現有金鑰的金鑰容器名稱。 使用 certutil –key 命令來顯示機器內容中可用的金鑰容器清單。 針對目前使用者的內容，請使用 certutil – key – user 命令。|隨機字串值</br>提示：您應該在具有空白或特殊字元的任何 INF 金鑰值前後加上雙引號，以避免可能的 INF 剖析問題。|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|定義公開和私密金鑰的長度。 金鑰長度會影響憑證的安全性層級。 較大的金鑰長度通常會提供較高的安全性層級;不過，某些應用程式可能會有關于金鑰長度的限制。|密碼編譯服務提供者所支援的任何有效金鑰長度。|KeyLength = 2048|
|KeySpec|決定金鑰是否可用於簽章、Exchange （加密）或兩者。|AT_NONE、AT_SIGNATURE、AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|KeyUsage|定義應使用的憑證金鑰。|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 （128）</br>提示：顯示的值是每個位定義的十六進位（十進位）值。 也可以使用較舊的語法：已設定多個位的單一十六進位值，而不是符號標記法。 例如，KeyUsage = 0xa0。</br>CERT_NON_REPUDIATION_KEY_USAGE--40 （64）</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 （32）</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 （16）</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 （32768）|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>提示：多個值使用分隔號（&#124;）符號分隔符號。 使用多個值時，請確定您使用雙引號來避免 INF 剖析問題。|
|KeyUsageProperty|抓取值，識別可使用私密金鑰的特定目的。|NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--ffffff （16777215）|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|當您需要建立由電腦所擁有而不是使用者所擁有的憑證時，此金鑰很重要。 產生的金鑰材料會保存在建立該要求的安全性主體 (使用者或電腦帳戶) 的安全性內容中。 當系統管理員代表電腦建立憑證要求時，必須在電腦的安全性內容中建立金鑰內容，而不是系統管理員的安全性內容。 否則，電腦無法存取其私密金鑰，因為它會在系統管理員的安全性內容中。|true、false|MachineKeySet = true</br>提示：預設值為 false。|
|NotBefore|指定無法發出要求的日期或日期和時間。 NotBefore 可以與 ValidityPeriod 和 ValidityPeriodUnits 搭配使用。|日期或日期和時間|NotBefore = "7/24/2012 10:31 AM"</br>提示： NotBefore 和 NotAfter 僅適用于 RequestType = cert。日期剖析嘗試區分地區設定。使用月份名稱會區分其意義，且應該在每個地區設定中使用。|
|NotAfter|指定無法發出要求的日期或日期和時間。 NotAfter 不能與 ValidityPeriod 或 ValidityPeriodUnits 搭配使用。|日期或日期和時間|NotAfter = "9/23/2014 10:31 AM"</br>提示： NotBefore 和 NotAfter 僅適用于 RequestType = cert。日期剖析嘗試區分地區設定。使用月份名稱會區分其意義，且應該在每個地區設定中使用。|
|PrivateKeyArchive|只有在對應的 RequestType 設定為 "CMC" 時，PrivateKeyArchive 設定才會運作，因為只有憑證管理訊息 over CMS （CMC）要求格式，才能安全地將要求者的私密金鑰傳送至 CA 進行金鑰保存。|true、false|PrivateKeyArchive = True|
|EncryptionAlgorithm|要使用的加密演算法。|視作業系統版本和安裝的密碼編譯提供者集而定，可能的選項會有所不同。 若要查看可用的演算法清單，請執行命令 <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> 指定的 CSP 必須同時支援指定的對稱式加密演算法和長度。|EncryptionAlgorithm = 3des|
|EncryptionLength|要使用的加密演算法長度。|指定 EncryptionAlgorithm 所允許的任何長度。|EncryptionLength = 128|
|ProviderName|提供者名稱是 CSP 的顯示名稱。|假如您不知道所使用的 CSP 的提供者名稱，請從命令列執行 certutil –csplist。 此命令會顯示本機系統上所有可用的 Csp 名稱|ProviderName = "Microsoft RSA SChannel 密碼編譯提供者"|
|ProviderType|提供者類型是用來根據特定的演算法功能（例如「RSA Full」）來選取特定的提供者。|假如您不知道所使用的 CSP 的提供者類型，請從命令列執行 certutil –csplist。 此命令會顯示本機系統上所有可用 Csp 的提供者類型。|ProviderType = 1|
|RenewalCert|如果您需要更新存在於產生憑證要求之系統上的憑證，您必須將其憑證雜湊指定為此金鑰的值。|在建立憑證要求的電腦上可用之任何憑證的憑證雜湊。 若您不知道憑證雜湊，請使用 [憑證] MMC 嵌入式管理單元，並尋找應該要更新的憑證。 開啟憑證內容，並查看憑證的「指紋」屬性。 憑證更新需要 PKCS # 7 或 CMC 要求格式。|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>注意：這會讓要求代表另一個使用者要求進行註冊。您也必須使用註冊代理程式憑證來簽署要求，否則 CA 將會拒絕要求。 使用-cert 選項來指定註冊代理程式憑證。|如果 RequestType 設定為 PKCS # 7 或 CMC，則可以為憑證要求指定要求者名稱。 如果 RequestType 設定為 PKCS # 10，則會略過此金鑰。 Requestername 只能設定為要求的一部分。 您無法在擱置中的要求中操作 Requestername。|採用|Requestername = "Contoso\BSmith"|
|RequestType|決定用來產生和傳送憑證要求的標準。|PKCS10--1</br>PKCS7--2</br>CMC--3</br>Cert--4</br>SCEP--fd00 （64768）</br>提示：此選項表示自我簽署或自我頒發證書。 它不會產生要求，而是會產生新的憑證，然後安裝憑證。[自我簽署] 是預設值。使用– cert 選項指定簽署憑證，以建立未自我簽署的自我發行憑證。|RequestType = CMC|
|SecurityDescriptor</br>提示：這僅與電腦內容非智慧卡金鑰有關。|包含與安全物件相關聯的安全性資訊。 對於大部分的安全物件，您可以在建立物件的函式呼叫中指定物件的安全描述項。|以[安全描述項定義語言](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx)為基礎的字串。|SecurityDescriptor = "D:P （A;;GA;;;SY）（A;;）GA;;;BA）」|
|AlternateSignatureAlgorithm|指定並抓取布林值，指出 PKCS # 10 要求或憑證簽章的簽章演算法物件識別元（OID）是否為離散或結合。|true、false|AlternateSignatureAlgorithm = false</br>提示：針對 RSA 簽章，false 表示 Pkcs1 v 1.5。 True 表示2.1 版簽章。|
|Silent|根據預設，此選項可讓 CSP 存取互動式使用者桌面，並向使用者要求資訊，例如智慧卡 PIN。 如果此金鑰設定為 TRUE，CSP 就不能與桌面互動，而且將會遭到封鎖而無法向使用者顯示任何使用者介面。|true、false|無訊息 = true|
|SMIME|如果此參數設定為 TRUE，則會將具有物件識別碼值1.2.840.113549.1.9.15 的延伸加入至要求中。 物件識別碼的數目取決於 [已安裝的作業系統版本] 和 [CSP] 功能，這是指安全多用途網際網路郵件延伸（S/MIME）應用程式（如 Outlook）可能使用的對稱式加密演算法。|true、false|SMIME = true|
|假如 useexistingkeyset|這個參數是用來指定建立憑證要求時，應該使用現有的金鑰組。 如果此機碼設為 TRUE，您也必須指定 RenewalCert 索引鍵或 KeyContainer 名稱的值。 您不得設定可匯出的金鑰，因為您無法變更現有金鑰的屬性。 在此例中，當建立憑證要求時，不會產生任何金鑰材料。|true、false|假如 useexistingkeyset = true|
|KeyProtection|指定一個值，指出私密金鑰在使用前如何受到保護。|XCN_NCRYPT_UI_NO_PROTCTION_FLAG--0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG--1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|指定布林值，指出要求中是否包含預設的擴充功能和屬性。 預設值是以其物件識別碼（Oid）來表示。|true、false|SuppressDefaults = true|
|FriendlyName|新憑證的易記名稱。|文字|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>注意：只有在要求類型 = cert 時，才會使用此憑證。|指定要與 ValidityPeriod 搭配使用的單位數。|數字|ValidityPeriodUnits = 3|
|ValidityPeriod</br>注意：只有在要求類型 = cert 時，才會使用此憑證。|VValidityPeriod 必須是美式英文複數的時間週期。|年、月、周、日、小時、分鐘、秒|ValidityPeriod = 年|

返回[內容](#BKMK_Contents)

**延伸模組**

本節為選擇性。


|  延伸 OID   | 定義 | 值 |                                                                                                                                                                                                                                                                                                                                                                                                                      範例                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "EMail =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS = 主機. domain .com &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DIRECTORYNAME = CN = NAME，Dc = DOMAIN，DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress = 10.0.0.1 &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1 = {Utf8} String &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2 = {八進位} AAECAwQFBgc = &"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2 = {八進位} {hex} 00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3 = {Asn} BAgAAQIDBAUGBw = = &"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3 = {hex} 04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7。                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{text} ca = 0pathlength = 3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     嚴重     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Critical = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE--0</br>AT_SIGNATURE--2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10--1</br>PKCS7--2</br>CMC--3</br>Cert--4</br>SCEP--fd00 （64768）                                                                                                                                                                                                                                                                                                                                                                                   |
|     KeyUsage     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 （128）</br>CERT_NON_REPUDIATION_KEY_USAGE--40 （64）</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 （32）</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 （16）</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 （32768）                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--ffffff （16777215）                                                                                                                                                                                                                                                                                                                                             |
|  KeyProtection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG--0</br>NCRYPT_UI_PROTECT_KEY_FLAG--1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  範本  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME--40000000 （1073741824）</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH--80000000 （2147483648）</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN--10000000 （268435456）</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL--20000000 （536870912）</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME--8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1000000 （16777216）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8000000 （134217728）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 （4194304）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL--4000000 （67108864）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN--800000 （8388608）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2000000 （33554432）                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE--0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG--40000000 （1073741824）</br>CERT_NAME_STR_NO_PLUS_FLAG--20000000 （536870912）</br>CERT_NAME_STR_NO_QUOTING_FLAG--10000000 （268435456）</br>CERT_NAME_STR_CRLF_FLAG--8000000 （134217728）</br>CERT_NAME_STR_COMMA_FLAG--4000000 （67108864）</br>CERT_NAME_STR_REVERSE_FLAG--2000000 （33554432）</br>CERT_NAME_STR_FORWARD_FLAG--1000000 （16777216）</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG--10000 （65536）</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG--20000 （131072）</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG--40000 （262144）</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG--80000 （524288）</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG--100000 （1048576）</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG--200000 （2097152） |

返回[內容](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags 可讓 INF 檔案根據目前的使用者或目前的電腦屬性，指定 certreq 自動填入的主旨和 SubjectAltName 延伸模組欄位： DNS 名稱、UPN 等等。 使用常值「範本」表示改用範本名稱旗標。 這可讓您在多個內容中使用單一 INF 檔案，以產生具有內容特定主體資訊的要求。
>
> 當主體 INF 金鑰值轉換成 asn.1 編碼的辨別名稱時，X500NameFlags 指定要直接傳遞至 CertStrToName API 的旗標。

若要使用 certreq （新增）來要求憑證，請使用下列範例中的步驟：

> [!WARNING]
> 本主題的內容是以 Windows Server 2008 AD CS 的預設設定為基礎;例如，將金鑰長度設定為2048、選取 Microsoft 軟體金鑰儲存提供者作為 CSP，並使用安全雜湊演算法1（SHA1）。 根據貴公司的安全性原則需求來評估這些選擇。

若要建立原則檔案（.inf），請在 [記事本] 中複製並儲存下列範例，並另存為 RequestConfig .inf：
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
在您要求憑證的電腦上，請輸入下列命令：
```
CertReq –New RequestConfig.inf CertRequest.req 
```
下列範例示範如何針對 Oid 和其他不容易解讀的資料，執行 [String] 區段語法。 EKU 延伸模組的新 {text} 語法範例，其使用以逗號分隔的 Oid 清單：
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

## <a name="BKMK_accept"></a>Certreq-接受

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
-Accept 參數會將之前產生的私密金鑰與已發行的憑證連結，並從要求憑證的系統中移除擱置中的憑證要求（如果有相符的要求）。

您可以使用此範例來手動接受憑證：
```
certreq -accept certnew.cer 
```

> [!WARNING]
> -Accept 動詞、-user 和– machine 選項會指出所安裝的憑證是否應安裝在使用者或電腦內容中。 如果其中一個內容中的未處理要求符合所安裝的公開金鑰，則不需要這些選項。 如果沒有未處理的要求，則必須指定其中一個。

返回[內容](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-原則

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   定義限定的部屬時，定義套用至 CA 憑證之條件約束的設定檔稱為「原則 .inf」。
-   在附錄 A 中，您可以在[規劃及實施跨認證和合格的部屬](https://technet.microsoft.com/library/cc738878(WS.10).aspx)白皮書中找到原則 .inf 檔案的範例。
-   如果您輸入不含任何其他參數的 certreq 原則，則會開啟對話方塊視窗，讓您可以選取要求的 fie （需求、cmc、txt、der、cer 或 crt）。 一旦您選取要求的檔案，然後按一下 [開啟] 按鈕，就會開啟另一個對話方塊視窗來選取 INF 檔案。

您可以使用此範例來建立跨憑證要求：
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   如果您輸入不含任何其他參數的 certreq 符號，則會開啟對話方塊視窗，讓您可以選取要求的檔案（必要、cmc、txt、der、cer 或 crt）。
-   簽署合格的從屬要求可能需要企業系統管理員認證。 這是針對合格的從屬發行簽署憑證的最佳作法。
-   用來簽署合格的從屬要求的憑證是使用合格的從屬範本來建立。 企業系統管理員必須簽署要求，或授與使用者許可權給將簽署憑證的個人。
-   當您簽署 CMC 要求時，您可能需要讓多個人員簽署此要求，視與合格的從屬關聯的保證層級而定。
-   如果您要安裝之合格次級 CA 的父系 CA 已離線，您必須從離線父系取得合格次級 CA 的 CA 憑證。 如果父系 CA 在線上，請在 [憑證服務安裝精靈] 期間指定合格次級 CA 的 CA 憑證。

下面的命令順序將示範如何建立新的憑證要求、簽署並提交它：
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-註冊

註冊憑證
```
certreq –enroll [Options] TemplateName
```
若要更新現有的憑證
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
您只能更新時間有效的憑證。 過期的憑證無法更新，必須以新憑證取代。

以下是使用其序號來更新憑證的範例：
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
以下範例示範如何使用星號（*）來註冊名為 Web 服務器的憑證範本，以透過 U/I 選取原則伺服器：
```
certreq -enroll –machine –policyserver * "WebServer"
```
返回[內容](#BKMK_Contents)

## <a name="BKMK_Options"></a>選項

|[選項]|說明|
|-------|-----------|
|-任何|強制執行 ICertRequest：： Submit 以判斷編碼類型。|
|-attrib \<AttributeString >|指定名稱和值字串配對，並以冒號分隔。</br>以 \n 分隔名稱和值字串組（例如，Name1： Value1\nName2： Value2）。|
|-binary|將輸出檔案格式化為二進位，而不是 base64 編碼。|
|-PolicyServer *\<PolicyServer >*|「ldap： *\<路徑 >* 」</br>為執行憑證註冊原則 Web 服務的電腦插入 URI 或唯一識別碼。</br>若要指定您想要透過流覽來使用要求檔案，請使用 *\<policyserver >* 的減號（-）符號。|
|-config \<ConfigString >|使用設定字串（也就是 CAHostName\CAName.）中指定的 CA 來處理作業。 若為 HTTPs 連線，請指定註冊伺服器 URI。 若為本機電腦存放區 CA，請使用減號（-）符號。|
|-Anonymous|將匿名認證用於憑證註冊 Web 服務。|
|-Kerberos|針對憑證註冊 Web 服務使用 Kerberos （網域）認證。|
|-ClientCertificate *\<ClientCertId >*|您可以使用憑證指紋、CN、EKU、範本、電子郵件、UPN 和新的 name = value 語法來取代 *\<ClientCertID >* 。|
|-UserName *\<使用者名稱 >*|與憑證註冊 Web 服務搭配使用。 您可以將 *\<UserName >* 替換成 SAM 名稱或 domain\user 此選項可搭配-p 選項使用。|
|-p *\<密碼 >*|與憑證註冊 Web 服務搭配使用。 以實際使用者的密碼取代 *\<密碼 >* 。 此選項可搭配-UserName 選項使用。|
|-使用者|為新的憑證要求設定-user 內容，或指定接受憑證的內容。 如果 INF 或範本中未指定任何內容，則此為預設內容。|
|-機器|設定新的憑證要求，或指定電腦內容接受憑證的內容。 針對新的要求，它必須與 MachineKeyset INF 金鑰和範本內容一致。 如果未指定此選項，且範本未設定內容，則預設值為使用者內容。|
|-crl|在 CertChainFileOut 指定之 base64 編碼的 PKCS #7 檔案的輸出中包含憑證撤銷清單（Crl），或是由 RequestFileOut 所指定的 base64 編碼檔案。|
|-rpc|指示 Active Directory 憑證服務（AD CS）使用遠端程序呼叫（RPC）伺服器連接，而不是分散式 COM。|
|-AdminForceMachine|使用金鑰服務或模擬，從本機系統內容提交要求。 需要叫用此選項的使用者必須是本機系統管理員的成員。|
|-RenewOnBehalfOf|代表簽署憑證中識別的主旨提交更新。 這會在呼叫[ICertRequest：： Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)時設定 CR_IN_ROBO|
|-f|強制覆寫現有的檔案。 這也會略過快取範本和原則。|
|-q|使用無訊息模式;隱藏所有互動式提示。|
|-Unicode|當標準輸出重新導向或輸送至另一個命令時，寫入 Unicode 輸出，這有助於從 Windows PowerShell®腳本叫用）。|
|-UnicodeText|將 base64 文字編碼的資料 blob 寫入檔案時，傳送 Unicode 輸出。|

返回[內容](#BKMK_Contents)

## <a name="BKMK_Formats"></a>格式

|格式|說明|
|-------|-----------|
|RequestFileIn|Base64 編碼或二進位輸入檔名稱： PKCS #10 憑證要求、CMS 憑證要求、PKCS #7 憑證更新要求、x.509 憑證，以進行交叉認證，或 KeyGen 的標記格式憑證要求。|
|RequestFileOut|Base64 編碼的輸出檔名稱|
|CertFileOut|Base64 編碼的 X-509 檔案名。|
|PKCS10FileOut|僅與[Certreq 原則](#BKMK_policy)動詞搭配使用。 Base64 編碼的 PKCS10 輸出檔名稱。|
|CertChainFileOut|Base64 編碼的 PKCS #7 檔案名。|
|FullResponseFileOut|Base64 編碼的完整回應檔名稱。|
|PolicyFileIn|僅與[Certreq 原則](#BKMK_policy)動詞搭配使用。 INF 檔案，其中包含用來限定要求的延伸模組文字標記法。|

## <a name="BKMK_Examples"></a>其他 certreq 範例

下列文章包含 certreq 使用方式的範例：
-   [如何使用自訂主體別名要求憑證](https://technet.microsoft.com/library/ff625722.aspx)
-   [測試實驗室指南：部署 AD CS 兩層式 PKI 階層](https://technet.microsoft.com/library/hh831348.aspx)
-   [附錄3： Certreq .exe 語法](https://technet.microsoft.com/library/cc736326.aspx)
-   [如何手動建立網頁伺服器 SSL 憑證](https://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [使用 Windows Server 2008 CA 要求 AMT 布建憑證](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [System Center Operations Manager 代理程式的憑證註冊](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [AD CS 逐步指南：兩層式 PKI 階層部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [如何使用協力廠商憑證授權單位單位啟用 LDAP over SSL](https://support.microsoft.com/kb/321051)

返回[內容](#BKMK_Contents)
