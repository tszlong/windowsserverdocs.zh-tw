---
title: certreq
description: Certreq 命令的參考文章，此命令會向憑證授權單位單位 (CA) 取得憑證、從 .inf 檔案建立新的要求、接受並安裝對要求的回應、從現有的 CA 憑證或要求中建立跨憑證或限定的從屬要求，以及簽署跨憑證或限定的從屬要求。
ms.topic: reference
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1f2cdc1123595dae9c0c72bcdc77c2f55382c760
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629945"
---
# <a name="certreq"></a>certreq

Certreq 命令可用於向憑證授權單位單位要求憑證 (CA) ，若要向 CA 取出先前要求的回應，從 .inf 檔案建立新的要求、接受並安裝對要求的回應、從現有的 CA 憑證或要求中建立跨憑證或限定的從屬要求，以及簽署跨憑證或限定的從屬要求。

> [!IMPORTANT]
> 舊版的 certreq 命令可能無法提供此處所述的所有選項。 若要查看根據特定 certreq 版本支援的選項，請執行命令列說明選項 `certreq -v -?` 。
>
> 在 CEP/CES 環境中，certreq 命令不支援根據金鑰證明範本建立新的憑證要求。

> [!WARNING]
> 本主題的內容是根據 Windows Server 的預設設定;例如，將金鑰長度設定為2048、選取 Microsoft 軟體金鑰儲存提供者作為 CSP，以及使用安全雜湊演算法 1 (SHA1) 。 根據公司安全性原則的需求來評估這些選項。

## <a name="syntax"></a>語法

```
certreq [-submit] [options] [requestfilein [certfileout [certchainfileout [fullresponsefileOut]]]]
certreq -retrieve [options] requestid [certfileout [certchainfileout [fullresponsefileOut]]]
certreq -new [options] [policyfilein [requestfileout]]
certreq -accept [options] [certchainfilein | fullresponsefilein | certfilein]
certreq -sign [options] [requestfilein [requestfileout]]
certreq –enroll [options] templatename
certreq –enroll –cert certId [options] renew [reusekeys]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------- | ----------- |
| -提交 | 將要求提交給憑證授權單位單位。 |
| -取得 `<requestid>` | 從憑證授權單位單位抓取對前一個要求的回應。 |
| -新增 | 從 .inf 檔案建立新的要求。 |
| -接受 | 接受並安裝對憑證要求的回應。 |
| -原則 | 設定要求的原則。 |
| -sign | 簽署跨憑證或限定的從屬要求。 |
| -註冊 | 註冊或更新憑證。 |
| -? | 顯示 certreq 語法、選項和描述的清單。 |
| `<parameter>` -? | 顯示指定參數的說明。 |
| -v-？ | 顯示 certreq 語法、選項和描述的詳細資訊清單。 |

## <a name="examples"></a>範例

### <a name="certreq--submit"></a>certreq-提交

若要提交簡單的憑證要求：

```
certreq –submit certrequest.req certnew.cer certnew.pfx
```

#### <a name="remarks"></a>備註

- 這是預設的 certreq.exe 參數。 如果在命令提示字元中未指定任何選項，certreq.exe 會嘗試將憑證要求提交給憑證授權單位單位。 使用 **– submit** 選項時，您必須指定憑證要求檔案。 如果省略此參數，則會顯示一般的 [ **開啟** 檔案] 視窗，讓您選取適當的憑證要求檔案。

- 若要藉由指定 SAN 屬性來要求憑證，請參閱 Microsoft 知識庫文章 931351[如何將主體別名新增至安全 LDAP 憑證](https://support.microsoft.com/kb/931351)中的*如何使用 certreq.exe 公用程式建立並提交憑證要求*一節。

### <a name="certreq--retrieve"></a>certreq-取得

若要取出憑證識別碼20並建立憑證檔案 ( .cer) ，名為 *MyCertificate*：

```
certreq -retrieve 20 MyCertificate.cer
```

#### <a name="remarks"></a>備註

- 當憑證授權單位單位發出憑證之後，請使用 certreq-取出 *requestid* 來取得憑證。 *Requestid* PKC 可以是具有0x 前置詞的十進位或十六進位，也可以是不含0x 前置詞的憑證序號。 您也可以使用它來取得憑證授權單位單位所發行的任何憑證（包括已撤銷或過期的憑證），而不考慮憑證的要求是否曾經處於擱置狀態。

- 如果您將要求提交給憑證授權單位單位，憑證授權單位單位的原則模組可能會讓要求處於擱置狀態，並將 *requestid* 傳回給 certreq 呼叫端以供顯示。 最後，憑證授權單位單位的系統管理員會發行憑證或拒絕要求。

### <a name="certreq--new"></a>certreq-新增

若要建立新的要求：

```
[newrequest]
; At least one value must be set in this section
subject = CN=W2K8-BO-DC.contoso2.com
```

以下是一些可能會新增至 INF 檔案的可能區段：

#### <a name="newrequest"></a>[newrequest]

INF 檔案的這個區域對於任何新的憑證要求範本是必要的，而且必須包含至少一個具有值的參數。

| 金鑰<sup>1</sup> | 描述 | 值<sup>2</sup> | 範例 |
| --- | ---------- | ----- | ------- |
| 主體 | 有幾個應用程式依賴憑證中的主體資訊。 建議您為此金鑰指定一個值。 如果此處未設定主體，建議您在主體別名憑證延伸中包括主體名稱。 | 相對辨別名稱字串值 | Subject = CN = computer1 .com Subject = CN = John Smith，CN = Users，DC = Contoso，DC = com |
| Exportable | 如果設定為 TRUE，則可以使用憑證匯出私密金鑰。 為了確保高層級的安全性，不應該匯出私密金鑰;不過，在某些情況下，如果有數部電腦或使用者必須共用相同的私密金鑰，可能就需要用到此金鑰。 | `true | false` | `Exportable = TRUE`. CNG 金鑰可以區分此和純文字可匯出。 CAPI1 索引鍵不能。 |
| ExportableEncrypted | 指定是否應將私密金鑰設定為可匯出。 | `true | false` | `ExportableEncrypted = true`<p>**秘訣：** 並非所有的公開金鑰大小和演算法都適用于所有的雜湊演算法。 指定的 CSP 也必須支援指定的雜湊演算法。 若要查看支援的雜湊演算法清單，您可以執行命令： `certutil -oid 1 | findstr pwszCNGAlgid | findstr /v CryptOIDInfo` |
| HashAlgorithm | 要用於此要求的雜湊演算法。 | `Sha256, sha384, sha512, sha1, md5, md4, md2` | `HashAlgorithm = sha1`. 若要查看支援的雜湊演算法清單，請使用： certutil-oid 1 | findstr pwszCNGAlgid | findstr/v CryptOIDInfo|
| KeyAlgorithm| 服務提供者將用來產生公開和私密金鑰組的演算法。| `RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521` | `KeyAlgorithm = RSA` |
| KeyContainer | 我們不建議針對新的金鑰材料產生的新要求設定此參數。 系統會自動產生及維護金鑰容器。<p>對於應該使用現有金鑰材料的要求，可以將此值設定為現有金鑰的金鑰容器名稱。 使用 `certutil –key` 命令來顯示機器內容的可用金鑰容器清單。 `certutil –key –user`針對目前使用者的內容使用命令。| 隨機字串值<p>**秘訣：** 使用雙引號括住有空白或特殊字元的任何 INF 金鑰值，以避免潛在的 INF 剖析問題。 | `KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}` |
| KeyLength | 定義公開金鑰和私密金鑰的長度。 金鑰長度會影響憑證的安全性層級。 較高的金鑰長度通常會提供較高的安全性層級;不過，有些應用程式可能會有關于金鑰長度的限制。 | 密碼編譯服務提供者所支援的任何有效金鑰長度。 | `KeyLength = 2048` |
| KeySpec | 判斷金鑰是否可用於簽章、Exchange (加密) 或兩者。 | `AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE` | `KeySpec = AT_KEYEXCHANGE` |
| KeyUsage | 定義應使用的憑證金鑰。 | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> | `KeyUsage = CERT_DIGITAL_SIGNATURE_KEY_USAGE | CERT_KEY_ENCIPHERMENT_KEY_USAGE`<p>**秘訣：** 多個值使用管道 (|) 符號分隔符號。 使用多個值時，請務必使用雙引號來避免 INF 剖析問題。 所顯示的值是每個位定義的十六進位 (十進位) 值。 也可以使用較舊的語法：已設定多個位的單一十六進位值，而不是符號標記法。 例如： `KeyUsage = 0xa0` 。 |
| KeyUsageProperty | 取得值，這個值會識別可以使用私密金鑰的特定用途。 | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> | `KeyUsageProperty = NCRYPT_ALLOW_DECRYPT_FLAG | NCRYPT_ALLOW_SIGNING_FLAG` |
| MachineKeySet | 當您需要建立電腦所擁有的憑證，而不是使用者所擁有的憑證時，此索引鍵是很重要的。 產生的金鑰材料會保存在安全性主體的安全性內容中， (使用者或電腦帳戶) 已建立要求。 當系統管理員代表電腦建立憑證要求時，必須在電腦的安全性內容中建立金鑰內容，而不是在系統管理員的安全性內容中建立。 否則，電腦無法存取其私密金鑰，因為它會在系統管理員的安全性內容中。 | `true | false`. 預設為 false。 | `MachineKeySet = true` |
| NotBefore | 指定無法發出要求的日期或日期和時間。 `NotBefore` 可以搭配和使用 `ValidityPeriod` `ValidityPeriodUnits` 。 | 日期或日期和時間 | `NotBefore = 7/24/2012 10:31 AM`<p>**秘訣：** `NotBefore` 和 `NotAfter` 僅適用于 R `equestType=cert` 。 日期剖析嘗試區分地區設定。 使用月份名稱將會區分其意義，而且應該會在每個地區設定中運作。 |
| NotAfter | 指定無法發出要求的日期或日期和時間。 `NotAfter` 無法與或搭配 `ValidityPeriod` 使用 `ValidityPeriodUnits` 。 | 日期或日期和時間 | `NotAfter = 9/23/2014 10:31 AM`<p>**秘訣：** `NotBefore` 和 `NotAfter` 僅適用于 `RequestType=cert` 。 日期剖析嘗試區分地區設定。 使用月份名稱將會區分其意義，而且應該會在每個地區設定中運作。 |
| PrivateKeyArchive | PrivateKeyArchive 設定僅適用于對應的 RequestType 設定為 CMC 的情況，因為只有透過 CMS 的憑證管理訊息 (CMC) 要求格式可將要求者的私密金鑰安全地傳輸至 CA 以進行金鑰保存。 | `true | false` | `PrivateKeyArchive = true` |
| EncryptionAlgorithm | 要使用的加密演算法。 | 可能的選項會視作業系統版本和已安裝的密碼編譯提供者集合而有所不同。 若要查看可用演算法的清單，請執行下列命令： `certutil -oid 2 | findstr pwszCNGAlgid` 。 使用的指定 CSP 也必須支援指定的對稱式加密演算法和長度。 | `EncryptionAlgorithm = 3des` |
| EncryptionLength | 要使用的加密演算法長度。 | 指定之 EncryptionAlgorithm 允許的任何長度。 | `EncryptionLength = 128` |
| ProviderName | 提供者名稱是 CSP 的顯示名稱。 | 如果您不知道所使用的 CSP 提供者名稱，請 `certutil –csplist` 從命令列執行。 此命令會顯示本機系統上所有可用的 Csp 名稱。 | `ProviderName = Microsoft RSA SChannel Cryptographic Provider` |
| ProviderType | 提供者類型是用來根據特定的演算法功能（例如 RSA Full）來選取特定的提供者。 | 如果您不知道所使用的 CSP 提供者類型，請 `certutil –csplist` 從命令列提示字元執行。 此命令會顯示本機系統上所有可用 Csp 的提供者類型。 | `ProviderType = 1` |
| RenewalCert | 如果您需要更新的憑證存在於產生憑證要求的系統上，您必須指定其憑證雜湊做為此金鑰的值。 | 在建立憑證要求的電腦上可用之任何憑證的憑證雜湊。 如果您不知道憑證雜湊，請使用 [憑證] MMC 嵌入式管理單元，並查看應該更新的憑證。 開啟憑證內容，並查看 `Thumbprint` 憑證的屬性。 憑證更新需要 `PKCS#7` 或 `CMC` 要求格式。 | `RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D` |
| RequesterName | 提出要求以代表其他使用者要求進行註冊。要求也必須使用註冊代理憑證來簽署，否則 CA 將會拒絕要求。 使用 `-cert` 選項來指定註冊代理程式憑證。 如果設定為或，則可以指定憑證要求的要求者名稱 `RequestType` `PKCS#7` `CMC` 。 如果 `RequestType` 設定為，則 `PKCS#10` 會忽略此索引鍵。 `Requestername`只能設定為要求的一部分。 您無法 `Requestername` 在暫止的要求中操作。 | `Domain\User` | `Requestername = Contoso\BSmith` |
| RequestType | 決定用來產生和傳送憑證要求的標準。 | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul>**秘訣：** 此選項表示自我簽署或自我發行的憑證。 它不會產生要求，而是會產生新的憑證，然後安裝憑證。 預設值為自我簽署。 使用– cert 選項指定簽署憑證，以建立未自我簽署的自我簽發憑證。 | `RequestType = CMC` |
| SecurityDescriptor | 包含與安全物件相關聯的安全性資訊。 對於大部分的安全物件而言，您可以在建立物件的函式呼叫中指定物件的安全描述項。以 [安全描述項定義語言](/windows/win32/secauthz/security-descriptor-definition-language)為基礎的字串。<p>**秘訣：** 這只與機器內容非智慧卡金鑰有關。 | `SecurityDescriptor = D:P(A;;GA;;;SY)(A;;GA;;;BA)` |
| AlternateSignatureAlgorithm | 指定並取得布林值，這個值會指出 PKCS # 10 要求或憑證簽章的簽章演算法物件識別碼 (OID) 是否為離散或合併。 | `true | false` | `AlternateSignatureAlgorithm = false`<p>針對 RSA 簽章， `false` 表示 `Pkcs1 v1.5` ，而表示簽章 `true` `v2.1` 。 |
| 無訊息 | 依預設，此選項可讓 CSP 存取互動式使用者桌面，以及要求使用者提供的資訊，例如智慧卡 PIN。 如果此機碼設為 TRUE，則 CSP 不能與桌面互動，而且將會遭到封鎖而無法向使用者顯示任何使用者介面。 | `true | false` | `Silent = true` |
| SMIME | 如果此參數設定為 TRUE，則會在要求中加入具有物件識別碼值1.2.840.113549.1.9.15 的延伸模組。 物件識別碼的數目取決於已安裝作業系統版本的和 CSP 功能，其指的是可由安全多用途網際網路郵件延伸 (S/MIME) 應用程式（例如 Outlook）使用的對稱式加密演算法。 | `true | false` | `SMIME = true` |
| 假如 useexistingkeyset | 這個參數是用來指定要在建立憑證要求時使用現有的金鑰組。 如果此機碼設為 TRUE，您也必須指定 RenewalCert 索引鍵或 KeyContainer 名稱的值。 您不得設定可匯出的金鑰，因為您無法變更現有金鑰的屬性。 在此情況下，建立憑證要求時不會產生任何金鑰材料。 | `true | false` | `UseExistingKeySet = true` |
| KeyProtection | 指定值，這個值會指出如何在使用之前保護私密金鑰。 | <ul><li>`XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0`</li><li>`XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> | `KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG` |
| SuppressDefaults | 指定布林值，指出要求中是否包含預設的擴充功能和屬性。 預設值是以 (Oid) 的物件識別碼來表示。 | `true | false` | `SuppressDefaults = true` |
| FriendlyName | 新憑證的易記名稱。 | Text | `FriendlyName = Server1` |
| ValidityPeriodUnits | 指定要搭配 ValidityPeriod 使用的單位數。 注意：這只適用于 `request type=cert` 。 | 數字 | `ValidityPeriodUnits = 3` |
| ValidityPeriod | ValidityPeriod 必須是 US 英文複數的時間週期。 注意：只有在要求類型 = cert 時，才會使用這個。 | `Years |  Months | Weeks | Days | Hours | Minutes | Seconds` | `ValidityPeriod = Years` |

<sup>1</sup>等號 (=) 左邊的參數

<sup>2</sup>等號 (=) 右邊的參數

#### <a name="extensions"></a>extensions

此為選擇性區段。

| 延伸模組 OID | 定義 | 範例 |
| ------------- | ---------- | ----- | ------- |
| 2.5.29.17 | | 2.5.29.17 = {text} |
| *繼續* | | `continue = UPN=User@Domain.com&` |
| *繼續* | | `continue = EMail=User@Domain.com&` |
| *繼續* | | `continue = DNS=host.domain.com&` |
| *繼續* | | `continue = DirectoryName=CN=Name,DC=Domain,DC=com&` |
| *繼續* | | `continue = URL=<http://host.domain.com/default.html&>` |
| *繼續* | | `continue = IPAddress=10.0.0.1&` |
| *繼續* | | `continue = RegisteredId=1.2.3.4.5&` |
| *繼續* | | `continue = 1.2.3.4.6.1={utf8}String&` |
| *繼續* | | `continue = 1.2.3.4.6.2={octet}AAECAwQFBgc=&` |
| *繼續* | | `continue = 1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&` |
| *繼續* | | `continue = 1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&` |
| *繼續* | | `continue = 1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07` |
| 2.5.29.37 | | `2.5.29.37={text}` |
| *繼續* | | `continue = 1.3.6.1.5.5.7` |
| *繼續* | | `continue = 1.3.6.1.5.5.7.3.1` |
| 2.5.29.19 | | `{text}ca=0pathlength=3` |
| 重大 | | `Critical=2.5.29.19` |
| KeySpec | | <ul><li>`AT_NONE -- 0`</li><li>`AT_SIGNATURE -- 2`</li><li>`AT_KEYEXCHANGE -- 1`</ul></li> |
| RequestType | | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul> |
| KeyUsage | | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> |
| KeyUsageProperty | | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> |
| KeyProtection | | <ul><li>`NCRYPT_UI_NO_PROTECTION_FLAG -- 0`</li><li>`NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> |
| SubjectNameFlags | template | <ul><li>`CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)`</li><li>`CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)`</li></ul> |
| X500NameFlags | | <ul><li>`CERT_NAME_STR_NONE -- 0`</li><li>`CERT_OID_NAME_STR -- 2`</li><li>`CERT_X500_NAME_STR -- 3`</li><li>`CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)`</li><li>`CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)`</li><li>`CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)`</li><li>`CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)`</li><li>`CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)`</li><li>`CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)`</li><li>`CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)`</li><li>`CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)`</li><li>`CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)`</li><li>`CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)`</li><li>`CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)`</li><li>`CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)`</li><li>`CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)`</li></ul> |

> [!NOTE]
> `SubjectNameFlags` 允許 INF 檔案根據目前的使用者或 **目前的電腦** 屬性，指定 certreq 自動填入的主旨和 **SubjectAltName** 延伸模組欄位： DNS 名稱、UPN 等等。 使用常值範本表示會改為使用範本名稱旗標。 這可讓您在多個內容中使用單一 INF 檔案，以產生具有特定內容之主體資訊的要求。
>
> `X500NameFlags` 指定將值轉換為 asn.1 編碼的 `CertStrToName` `Subject INF keys` 辨別 **名稱**時，要直接傳遞至 API 的旗標。

#### <a name="example"></a>範例

若要在 [記事本] 中 ( .inf) 建立原則檔，並將它儲存為 *requestconfig*：

```
[NewRequest]
Subject = CN=<FQDN of computer you are creating the certificate>
Exportable = TRUE
KeyLength = 2048
KeySpec = 1
KeyUsage = 0xf0
MachineKeySet = TRUE
[RequestAttributes]
CertificateTemplate=WebServer
[Extensions]
OID = 1.3.6.1.5.5.7.3.1
OID = 1.3.6.1.5.5.7.3.2
```

在您要要求憑證的電腦上：

```
certreq –new requestconfig.inf certrequest.req
```

若要使用 Oid 的 [字串] 區段語法，以及其他難以解讀的資料。 適用于 EKU 延伸模組的新 {text} 語法範例，其使用以逗號分隔的 Oid 清單：

```
[Version]
Signature=$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = 2.5.29.37
szOID_PKIX_KP_SERVER_AUTH = 1.3.6.1.5.5.7.3.1
szOID_PKIX_KP_CLIENT_AUTH = 1.3.6.1.5.5.7.3.2

[NewRequest]
Subject = CN=TestSelfSignedCert
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%={text}%szOID_PKIX_KP_SERVER_AUTH%,
_continue_ = %szOID_PKIX_KP_CLIENT_AUTH%
```

### <a name="certreq--accept"></a>certreq-接受

`–accept`參數會連結先前產生的私密金鑰與已發行的憑證，並從要求憑證的系統中移除擱置的憑證要求 (如果有相符的要求) 。

若要手動接受憑證：

```
certreq -accept certnew.cer
```

> [!WARNING]
> 使用 `-accept` 參數搭配 `-user` 和 `–machine` 選項會指出安裝憑證是否應該安裝在 **使用者** 或 **電腦** 內容中。 如果任何一個內容中的未完成要求符合所要安裝的公開金鑰，則不需要這些選項。 如果沒有未完成的要求，則必須指定其中一個。

### <a name="certreq--policy"></a>certreq-原則

原則 .inf 檔案是定義限定的從屬定義時，套用至 CA 憑證之條件約束的設定檔。

若要建立交叉憑證要求：

```
certreq -policy certsrv.req policy.inf newcertsrv.req
```

使用 `certreq -policy` 沒有任何額外參數的會開啟對話方塊視窗，讓您選取要求的 fie ( 必要、cmc、.txt、der、.cer 或 .crt) 。 當您選取要求的檔案，並按一下 [ **開啟**] 後，另一個對話方塊視窗便會開啟，讓您可以選取原則 .inf 檔案。

#### <a name="examples"></a>範例

在 [capolicy.inf .Inf 語法](../../networking/core-network-guide/cncg/server-certs/prepare-the-capolicy-inf-file.md)中尋找原則 .inf 檔案的範例。

### <a name="certreq--sign"></a>certreq-sign

若要建立新的憑證要求，請加以簽署並提交：

```
certreq -new policyfile.inf myrequest.req
certreq -sign myrequest.req myrequest.req
certreq -submit myrequest_sign.req myrequest_cert.cer
```

#### <a name="remarks"></a>備註

- `certreq -sign`如果不使用任何額外的參數，它會開啟對話方塊視窗，讓您可以選取要求的檔案 (要求、cmc、txt、der、cer 或 crt) 。

- 簽署合格的從屬要求可能需要 **企業系統管理員** 認證。 這是針對合格的從屬發行簽署憑證的最佳作法。

- 用來簽署合格的從屬要求的憑證會使用合格的從屬範本。 企業系統管理員必須簽署要求或授與使用者許可權給簽署憑證的個人。

- 在您之後，您可能需要讓其他人員簽署 CMC 要求。 這將取決於與合格的從屬關係相關聯的保證層級。

- 如果您要安裝之合格次級 CA 的父系 CA 離線，您必須從離線父系取得合格次級 CA 的 CA 憑證。 如果父系 CA 在線上，請在 **憑證服務安裝** 嚮導期間指定合格次級 CA 的 CA 憑證。

### <a name="certreq--enroll"></a>certreq-註冊

您可以使用此批註來註冊或更新您的憑證。

#### <a name="examples"></a>範例

若要註冊憑證，請使用 *web* 伺服器範本，然後選取使用 U/I 的原則伺服器：

```
certreq -enroll –machine –policyserver * WebServer
```

若要使用序號來更新憑證：

```
certreq –enroll -machine –cert 61 2d 3c fe 00 00 00 00 00 05 renew
```

您只能更新有效的憑證。 過期的憑證無法更新，必須取代為新的憑證。

## <a name="options"></a>選項。

| 選項。 | 描述 |
| ------- | ----------- |
| -任何 | `Force ICertRequest::Submit` 判斷編碼類型。|
| -attrib `<attributestring>` | 指定 **名稱** 和 **值** 字串組（以冒號分隔）。<p>使用 (分隔 **名稱** 和 **值** 字串組 `\n` ，例如 Name1： value1\nName2： value2) 。 |
| -binary | 將輸出檔格式化為二進位檔，而不是 base64 編碼。 |
| -policyserver `<policyserver>` | Ldap： `<path>`<br>為執行憑證註冊原則 web 服務的電腦插入 URI 或唯一識別碼。<p>若要指定您想要透過流覽來使用要求檔案，只需使用的減號 ( ) 號 `<policyserver>` 。 |
| -config `<ConfigString>` | 使用設定字串（也就是 **CAHostName\CAName**）中指定的 CA 來處理作業。 若是 HTTPs： \\ \ 連接，請指定註冊伺服器 URI。 若為本機電腦存放區 CA，請使用減號 (-) 號。 |
| -匿名 | 針對憑證註冊 web 服務使用匿名認證。 |
| -kerberos | 使用 Kerberos (網域) 憑證註冊 web 服務的認證。 |
| -clientcertificate `<ClientCertId>` | 您可以 `<ClientCertId>` 使用憑證指紋、CN、EKU、範本、電子郵件、UPN 或新的語法來取代 `name=value` 。 |
| -username `<username>` | 與憑證註冊 web 服務搭配使用。 您可以 `<username>` 用 SAM 名稱或 **domain\user** 值取代。 此選項可搭配 `-p` 選項使用。 |
| -p `<password>` | 與憑證註冊 web 服務搭配使用。 `<password>`以實際使用者的密碼取代。 此選項可搭配 `-username` 選項使用。 |
| -使用者 | 設定 `-user` 新憑證要求的內容，或指定憑證接受的內容。 如果在 INF 或範本中未指定任何內容，這就是預設內容。 |
| -電腦 | 設定新的憑證要求，或指定機器內容的憑證接受內容。 對於新的要求，它必須與 MachineKeyset INF 金鑰和範本內容一致。 如果未指定此選項，且範本未設定內容，則預設值為使用者內容。 |
| -crl | 包含憑證撤銷清單 (Crl) 在所指定之 base64 編碼 PKCS #7 檔的輸出中， `certchainfileout` 或寫入至所指定之 base64 編碼檔案的輸出中 `requestfileout` 。 |
| -rpc | 指示 Active Directory 憑證服務 (AD CS) 使用遠端程序呼叫 (RPC) 伺服器連接，而不是分散式 COM。 |
| -adminforcemachine | 使用金鑰服務或模擬，從本機系統內容提交要求。 要求叫用這個選項的使用者必須是本機系統管理員的成員。 |
| -renewonbehalfof | 代表簽署憑證中識別的主旨提交續約。 這會在呼叫[ICertRequest：： Submit 方法](/windows/win32/api/certcli/nf-certcli-icertrequest-submit)時設定 CR_IN_ROBO |
| -f | 強制覆寫現有的檔案。 這也會略過快取範本和原則。 |
| -Q | 使用無訊息模式;隱藏所有互動式提示。 |
| -unicode | 當標準輸出重新導向或輸送至另一個命令時，會寫入 Unicode 輸出，這有助於從 Windows PowerShell 腳本叫用時。 |
| -unicodetext | 將 base64 文字編碼資料 blob 寫入檔案時，傳送 Unicode 輸出。 |

## <a name="formats"></a>格式

| 格式 | 描述 |
| ------- | ----------- |
| requestfilein | Base64 編碼或二進位輸入檔名稱： PKCS #10 憑證要求、CMS 憑證要求、PKCS #7 憑證更新要求、x.509 憑證以進行交叉認證或 KeyGen 標記格式的憑證要求。 |
| requestfileout | Base64 編碼的輸出檔名稱。 |
| certfileout | Base64 編碼的509檔案名。 |
| PKCS10fileout | `certreq -policy`僅用於參數。 Base64 編碼的 PKCS10 輸出檔名稱。 |
| certchainfileout | Base64 編碼 PKCS #7 的檔案名。 |
| fullresponsefileout | Base64 編碼的完整回應檔名稱。 |
| policyfilein | `certreq -policy`僅用於參數。 INF 檔案，其中包含用來限定要求之擴充功能的文字標記法。 |

## <a name="additional-resources"></a>其他資源

下列文章包含 certreq 使用方式的範例：

- [如何將主體替代名稱新增至安全 LDAP 憑證](https://support.microsoft.com/help/931351/how-to-add-a-subject-alternative-name-to-a-secure-ldap-certificate)

- [Test Lab Guide: Deploying an AD CS Two-Tier PKI Hierarchy](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831348(v=ws.11))

- [附錄3： Certreq.exe 語法](/previous-versions/windows/it-pro/windows-server-2003/cc736326(v=ws.10))

- [如何手動建立網頁伺服器 SSL 憑證](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/how-to-create-a-web-server-ssl-certificate-manually/ba-p/1128529)

- [System Center Operations Manager 代理程式的憑證註冊](/system-center/scom/plan-planning-agent-deployment?view=sc-om-2019)

- [Active Directory 憑證服務概觀](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))

- [如何使用協力廠商憑證授權單位單位啟用 LDAP over SSL](https://support.microsoft.com/help/321051/how-to-enable-ldap-over-ssl-with-a-third-party-certification-authority)