---
title: certreq
description: Certreq 命令的參考主題，會向憑證授權單位單位（CA）要求憑證、抓取 CA 對先前要求的回應、從 .inf 檔案建立新的要求、接受並安裝對要求的回應、從現有的 CA 憑證或要求來構造跨憑證或合格的從屬要求，以及簽署交叉憑證或合格的從屬要求。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14fc717ad49a676387206692af32842f212c4296
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719650"
---
# <a name="certreq"></a>certreq

Certreq 命令可以用來向憑證授權單位單位（CA）要求憑證，若要從 CA 取出先前要求的回應，請從 .inf 檔案建立新的要求，以接受並安裝要求的回應、從現有的 CA 憑證或要求來建立跨憑證或合格的從屬要求，以及簽署交叉憑證或合格的從屬要求。

> [!IMPORTANT]
> 舊版的 certreq 命令可能無法提供這裡所述的所有選項。 若要查看根據特定 certreq 版本支援的選項，請執行命令列說明選項`certreq -v -?`。
>
> Certreq 命令不支援在 CEP/CES 環境中，根據金鑰證明範本來建立新的憑證要求。

> [!WARNING]
> 本主題的內容是以 Windows Server 的預設設定為基礎;例如，將金鑰長度設定為2048、選取 Microsoft 軟體金鑰儲存提供者作為 CSP，並使用安全雜湊演算法1（SHA1）。 根據貴公司的安全性原則需求來評估這些選擇。

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
| -取出`<requestid>` | 從憑證授權單位單位抓取先前要求的回應。 |
| -新增 | 從 .inf 檔案建立新的要求。 |
| -接受 | 接受並安裝憑證要求的回應。 |
| -原則 | 設定要求的原則。 |
| -sign | 簽署交叉認證或合格的從屬要求。 |
| -註冊 | 註冊或續約憑證。 |
| -? | 顯示 certreq 語法、選項和描述的清單。 |
| `<parameter>` -? | 顯示指定之參數的說明。 |
| -v-？ | 顯示 certreq 語法、選項和描述的詳細資訊清單。 |

## <a name="examples"></a>範例

### <a name="certreq--submit"></a>certreq-提交

若要提交簡單憑證要求：

```
certreq –submit certrequest.req certnew.cer certnew.pfx
```

#### <a name="remarks"></a>備註

- 這是預設的 certreq 參數。 如果在命令提示字元中未指定任何選項，則 certreq 會嘗試將憑證要求提交給憑證授權單位單位。 使用 **– submit**選項時，您必須指定憑證要求檔案。 如果省略此參數，則會出現通用的 [**開啟**檔案] 視窗，讓您選取適當的憑證要求檔案。

- 若要藉由指定 SAN 屬性來要求憑證，請參閱 Microsoft 知識庫文章931351如何*使用 certreq 建立和提交憑證要求*一節，[如何將主體別名新增至安全的 LDAP 憑證](https://support.microsoft.com/kb/931351)。

### <a name="certreq--retrieve"></a>certreq-取出

若要取出憑證識別碼20並建立憑證檔案（.cer），請將其命名為*我憑證*：

```
certreq -retrieve 20 MyCertificate.cer
```

#### <a name="remarks"></a>備註

- 在憑證授權單位單位發行後，使用 certreq-取出*requestid*來取得憑證。 *Requestid* PKC 可以是具有0x 前置詞的十進位或十六進位，而且它可以是不含0x 前置詞的憑證序號。 您也可以使用它來抓取憑證授權單位單位曾經發行的任何憑證，包括已撤銷或過期的憑證，而不考慮憑證的要求是否曾經處於擱置狀態。

- 如果您向憑證授權單位單位提交要求，憑證授權單位單位的原則模組可能會讓要求處於擱置狀態，並將*requestid*傳回給 certreq 呼叫端以供顯示。 最後，憑證授權單位單位的系統管理員會發出憑證或拒絕要求。

### <a name="certreq--new"></a>certreq-新增

若要建立新的要求：

```
[newrequest]
; At least one value must be set in this section
subject = CN=W2K8-BO-DC.contoso2.com
```

以下是一些可能新增至 INF 檔案的可能區段：

#### <a name="newrequest"></a>[newrequest]

這個 INF 檔案的區域對於任何新的憑證要求範本都是必要的，而且必須包含至少一個具有值的參數。

| 金鑰<sup>1</sup> | 描述 | 值<sup>2</sup> | 範例 |
| --- | ---------- | ----- | ------- |
| 主體 | 有數個應用程式依賴憑證中的主體資訊。 我們建議您為此金鑰指定一個值。 如果此處未設定主旨，建議您在主體別名憑證延伸中包含主體名稱。 | 相對辨別名稱字串值 | Subject = CN = computer1。 contoso .com Subject = CN = John Smith，CN = Users，DC = Contoso，DC = com |
| Exportable | 如果設定為 TRUE，就可以使用憑證來匯出私密金鑰。 為了確保高層級的安全性，私密金鑰不能匯出;不過，在某些情況下，如果有數部電腦或使用者必須共用相同的私密金鑰，可能就會需要此金鑰。 | `true | false` | `Exportable = TRUE`. CNG 金鑰可以區分此和純文字匯出。 CAPI1 索引鍵不能。 |
| ExportableEncrypted | 指定是否應將私密金鑰設定為可匯出。 | `true | false` | `ExportableEncrypted = true`<p>**秘訣：** 並非所有的公用金鑰大小和演算法都適用所有雜湊演算法。 指定的 CSP 也必須支援指定的雜湊演算法。 若要查看支援的雜湊演算法清單，您可以執行命令：`certutil -oid 1 | findstr pwszCNGAlgid | findstr /v CryptOIDInfo` |
| HashAlgorithm | 要用於此要求的雜湊演算法。 | `Sha256, sha384, sha512, sha1, md5, md4, md2` | `HashAlgorithm = sha1`. 若要查看支援的雜湊演算法清單，請使用： certutil-oid 1 | findstr pwszCNGAlgid | findstr/v CryptOIDInfo|
| KeyAlgorithm| 服務提供者將用來產生公開和私密金鑰組的演算法。| `RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521` | `KeyAlgorithm = RSA` |
| KeyContainer | 我們不建議針對產生新金鑰材料的新要求，設定此參數。 金鑰容器是由系統自動產生和維護。<p>對於應該使用現有金鑰材料的要求，此值可以設定為現有金鑰的金鑰容器名稱。 使用`certutil –key`命令來顯示機器內容可用的金鑰容器清單。 針對目前`certutil –key –user`使用者的內容使用命令。| 隨機字串值<p>**秘訣：** 使用雙引號括住具有空白或特殊字元的任何 INF 金鑰值，以避免可能的 INF 剖析問題。 | `KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}` |
| KeyLength | 定義公開和私密金鑰的長度。 金鑰長度會影響憑證的安全性層級。 較大的金鑰長度通常會提供較高的安全性層級;不過，某些應用程式可能會有關于金鑰長度的限制。 | 密碼編譯服務提供者所支援的任何有效金鑰長度。 | `KeyLength = 2048` |
| KeySpec | 決定金鑰是否可用於簽章、Exchange （加密）或兩者。 | `AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE` | `KeySpec = AT_KEYEXCHANGE` |
| KeyUsage | 定義應使用的憑證金鑰。 | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> | `KeyUsage = CERT_DIGITAL_SIGNATURE_KEY_USAGE | CERT_KEY_ENCIPHERMENT_KEY_USAGE`<p>**秘訣：** 多個值使用分隔號（|）符號分隔符號。 使用多個值時，請確定您使用雙引號來避免 INF 剖析問題。 所顯示的值是每個位定義的十六進位（十進位）值。 也可以使用較舊的語法：已設定多個位的單一十六進位值，而不是符號標記法。 例如： `KeyUsage = 0xa0` 。 |
| KeyUsageProperty | 抓取值，識別可使用私密金鑰的特定目的。 | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> | `KeyUsageProperty = NCRYPT_ALLOW_DECRYPT_FLAG | NCRYPT_ALLOW_SIGNING_FLAG` |
| MachineKeySet | 當您需要建立由電腦所擁有而不是使用者所擁有的憑證時，此金鑰很重要。 產生的金鑰內容會保留在已建立要求之安全性主體（使用者或電腦帳戶）的安全性內容中。 當系統管理員代表電腦建立憑證要求時，必須在電腦的安全性內容中建立金鑰內容，而不是系統管理員的安全性內容。 否則，電腦無法存取其私密金鑰，因為它會在系統管理員的安全性內容中。 | `true | false`. 預設值為 false。 | `MachineKeySet = true` |
| NotBefore | 指定無法發出要求的日期或日期和時間。 `NotBefore`可以與`ValidityPeriod`和`ValidityPeriodUnits`搭配使用。 | 日期或日期和時間 | `NotBefore = 7/24/2012 10:31 AM`<p>**提示：** `NotBefore`和`NotAfter`僅適用于`equestType=cert` R。 日期剖析嘗試區分地區設定。 使用月份名稱會區分其意義，且應該在每個地區設定中使用。 |
| NotAfter | 指定無法發出要求的日期或日期和時間。 `NotAfter`不能與`ValidityPeriod`或`ValidityPeriodUnits`搭配使用。 | 日期或日期和時間 | `NotAfter = 9/23/2014 10:31 AM`<p>**提示：** `NotBefore`和`NotAfter`僅適用`RequestType=cert`于。 日期剖析嘗試區分地區設定。 使用月份名稱會區分其意義，且應該在每個地區設定中使用。 |
| PrivateKeyArchive | PrivateKeyArchive 設定只適用于對應的 RequestType 設定為 CMC 的情況，因為只有透過 CMS （CMC）要求格式的憑證管理訊息，才允許安全地將要求者的私密金鑰傳送至 CA，以進行金鑰保存。 | `true | false` | `PrivateKeyArchive = true` |
| EncryptionAlgorithm | 要使用的加密演算法。 | 視作業系統版本和安裝的密碼編譯提供者集而定，可能的選項會有所不同。 若要查看可用的演算法清單，請執行下列命令`certutil -oid 2 | findstr pwszCNGAlgid`：。 所使用的指定 CSP 也必須支援指定的對稱式加密演算法和長度。 | `EncryptionAlgorithm = 3des` |
| EncryptionLength | 要使用的加密演算法長度。 | 指定 EncryptionAlgorithm 所允許的任何長度。 | `EncryptionLength = 128` |
| ProviderName | 提供者名稱是 CSP 的顯示名稱。 | 如果您不知道所使用之 CSP 的提供者名稱，請從`certutil –csplist`命令列執行。 此命令會顯示本機系統上所有可用的 Csp 名稱 | `ProviderName = Microsoft RSA SChannel Cryptographic Provider` |
| ProviderType | 提供者類型是用來根據特定的演算法功能（例如 RSA Full）來選取特定的提供者。 | 如果您不知道所使用之 CSP 的提供者類型，請從命令`certutil –csplist`提示字元執行。 此命令會顯示本機系統上所有可用 Csp 的提供者類型。 | `ProviderType = 1` |
| RenewalCert | 如果您需要更新存在於產生憑證要求之系統上的憑證，您必須將其憑證雜湊指定為此金鑰的值。 | 在建立憑證要求的電腦上可用之任何憑證的憑證雜湊。 如果您不知道憑證雜湊，請使用 [憑證] MMC 嵌入式管理單元，並查看應更新的憑證。 開啟憑證屬性，並查看憑證`Thumbprint`的屬性。 憑證更新需要`PKCS#7`或`CMC`要求格式。 | `RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D` |
| RequesterName | 提出要求以代表另一個使用者要求進行註冊。您也必須使用註冊代理程式憑證來簽署要求，否則 CA 將會拒絕要求。 使用`-cert`選項來指定註冊代理程式憑證。 如果`RequestType`設定為`PKCS#7`或`CMC`，則可以指定憑證要求的要求者名稱。 `RequestType`如果設定為`PKCS#10`，則會忽略此索引鍵。 `Requestername`只能設定為要求的一部分。 您無法在擱置`Requestername`中的要求中操作。 | `Domain\User` | `Requestername = Contoso\BSmith` |
| RequestType | 決定用來產生和傳送憑證要求的標準。 | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul>**秘訣：** 此選項表示自我簽署或自我頒發證書。 它不會產生要求，而是會產生新的憑證，然後安裝憑證。 [自我簽署] 是預設值。 使用– cert 選項指定簽署憑證，以建立未自我簽署的自我發行憑證。 | `RequestType = CMC` |
| SecurityDescriptor | 包含與安全物件相關聯的安全性資訊。 對於大部分的安全物件，您可以在建立物件的函式呼叫中指定物件的安全描述項。以[安全描述項定義語言](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx)為基礎的字串。<p>**秘訣：** 這僅與電腦內容非智慧卡金鑰有關。 | `SecurityDescriptor = D:P(A;;GA;;;SY)(A;;GA;;;BA)` |
| AlternateSignatureAlgorithm | 指定並抓取布林值，指出 PKCS # 10 要求或憑證簽章的簽章演算法物件識別元（OID）是否為離散或結合。 | `true | false` | `AlternateSignatureAlgorithm = false`<p>針對 RSA 簽章， `false`表示`Pkcs1 v1.5`，而`true`則表示`v2.1`簽章。 |
| 無訊息 | 根據預設，此選項可讓 CSP 存取互動式使用者桌面，並向使用者要求資訊，例如智慧卡 PIN。 如果此金鑰設定為 TRUE，CSP 就不能與桌面互動，而且將會遭到封鎖而無法向使用者顯示任何使用者介面。 | `true | false` | `Silent = true` |
| SMIME | 如果此參數設定為 TRUE，則會將具有物件識別碼值1.2.840.113549.1.9.15 的延伸加入至要求中。 物件識別碼的數目取決於 [已安裝的作業系統版本] 和 [CSP] 功能，這是指安全多用途網際網路郵件延伸（S/MIME）應用程式（如 Outlook）可能使用的對稱式加密演算法。 | `true | false` | `SMIME = true` |
| 假如 useexistingkeyset | 這個參數是用來指定建立憑證要求時，應該使用現有的金鑰組。 如果此機碼設為 TRUE，您也必須指定 RenewalCert 索引鍵或 KeyContainer 名稱的值。 您不得設定可匯出的金鑰，因為您無法變更現有金鑰的屬性。 在此情況下，建立憑證要求時，不會產生任何金鑰材料。 | `true | false` | `UseExistingKeySet = true` |
| KeyProtection | 指定一個值，指出私密金鑰在使用前如何受到保護。 | <ul><li>`XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0`</li><li>`XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> | `KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG` |
| SuppressDefaults | 指定布林值，指出要求中是否包含預設的擴充功能和屬性。 預設值是以其物件識別碼（Oid）來表示。 | `true | false` | `SuppressDefaults = true` |
| FriendlyName | 新憑證的易記名稱。 | Text | `FriendlyName = Server1` |
| ValidityPeriodUnits | 指定要與 ValidityPeriod 搭配使用的單位數。 注意：只有在時，才會`request type=cert`使用此。 | 數值 | `ValidityPeriodUnits = 3` |
| ValidityPeriod | ValidityPeriod 必須是美式英文複數的時間週期。 注意：只有在要求類型 = cert 時，才會使用此憑證。 | `Years |  Months | Weeks | Days | Hours | Minutes | Seconds` | `ValidityPeriod = Years` |

<sup>1</sup>在等號左邊的參數（=）

<sup>2</sup>等號（=）右邊的參數

#### <a name="extensions"></a>延伸模組

此為選擇性區段。

| 延伸 OID | 定義 | 範例 |
| ------------- | ---------- | ----- | ------- |
| 2.5.29.17 | | 2.5.29.17 = {text} |
| *continue* | | `continue = UPN=User@Domain.com&` |
| *continue* | | `continue = EMail=User@Domain.com&` |
| *continue* | | `continue = DNS=host.domain.com&` |
| *continue* | | `continue = DirectoryName=CN=Name,DC=Domain,DC=com&` |
| *continue* | | `continue = URL=<http://host.domain.com/default.html&>` |
| *continue* | | `continue = IPAddress=10.0.0.1&` |
| *continue* | | `continue = RegisteredId=1.2.3.4.5&` |
| *continue* | | `continue = 1.2.3.4.6.1={utf8}String&` |
| *continue* | | `continue = 1.2.3.4.6.2={octet}AAECAwQFBgc=&` |
| *continue* | | `continue = 1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&` |
| *continue* | | `continue = 1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&` |
| *continue* | | `continue = 1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07` |
| 2.5.29.37 | | `2.5.29.37={text}` |
| *continue* | | `continue = 1.3.6.1.5.5.7` |
| *continue* | | `continue = 1.3.6.1.5.5.7.3.1` |
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
> `SubjectNameFlags`允許 INF 檔案根據目前的使用者或**目前的電腦**屬性，指定應自動填入的主旨和**SubjectAltName**延伸模組欄位： DNS 名稱、UPN 等等。 使用常值範本表示會改為使用範本名稱旗標。 這可讓您在多個內容中使用單一 INF 檔案，以產生具有內容特定主體資訊的要求。
>
> `X500NameFlags`指定當`Subject INF keys`值轉換成 asn.1 編碼的`CertStrToName`辨別**名稱**時，要直接傳遞給 API 的旗標。

#### <a name="example"></a>範例

若要在 [記事本] 中建立原則檔（.inf），並將它儲存為*requestconfig*：

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

在您要求憑證的電腦上：

```
certreq –new requestconfig.inf certrequest.req
```

表示對 Oid 使用 [String] 區段語法，而其他則不容易解讀資料。 EKU 延伸模組的新 {text} 語法範例，其使用以逗號分隔的 Oid 清單：

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

`–accept`參數會將先前產生的私密金鑰與已發行的憑證連結，並從要求憑證的系統中移除擱置中的憑證要求（如果有相符的要求）。

若要手動接受憑證：

```
certreq -accept certnew.cer
```

> [!WARNING]
> 使用`-accept` `-user`參數搭配和選項， `–machine`指出安裝憑證是否應該安裝在**使用者**或**電腦**內容中。 如果其中一個內容中的未處理要求符合所安裝的公開金鑰，則不需要這些選項。 如果沒有未處理的要求，則必須指定其中一個。

### <a name="certreq--policy"></a>certreq-原則

當定義限定的部屬時，原則 .inf 檔案是定義套用至 CA 憑證之條件約束的設定檔。

若要建立交叉憑證要求：

```
certreq -policy certsrv.req policy.inf newcertsrv.req
```

使用`certreq -policy`不含任何其他參數的會開啟對話方塊視窗，讓您選取要求的 fie （. 需求、cmc、.txt、der、.cer 或 crt）。 在您選取要求的檔案並按一下 [**開啟**] 之後，另一個對話方塊視窗隨即開啟，讓您可以選取 [原則 .inf] 檔案。

#### <a name="examples"></a>範例

在[capolicy.inf](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/prepare-the-capolicy-inf-file)中尋找原則 .inf 檔案的範例。

### <a name="certreq--sign"></a>certreq-sign

若要建立新的憑證要求，請將其簽署並提交：

```
certreq -new policyfile.inf myrequest.req
certreq -sign myrequest.req myrequest.req
certreq -submit myrequest_sign.req myrequest_cert.cer
```

#### <a name="remarks"></a>備註

- 使用`certreq -sign`時，如果沒有任何其他參數，則會開啟對話方塊視窗，讓您可以選取要求的檔案（需求、cmc、txt、der、cer 或 crt）。

- 簽署合格的從屬要求可能需要**企業系統管理員**認證。 這是針對合格的從屬發行簽署憑證的最佳作法。

- 用來簽署合格的從屬要求的憑證會使用合格的從屬範本。 企業系統管理員必須簽署要求，或授與使用者對簽署憑證之人員的許可權。

- 在您之後，您可能需要讓其他人員簽署 CMC 要求。 這將取決於與合格的從屬關聯的保證層級。

- 如果您要安裝之合格次級 CA 的父系 CA 已離線，您必須從離線父系取得合格次級 CA 的 CA 憑證。 如果父系 CA 在線上，請在 [**憑證服務安裝**嚮導] 期間指定合格次級 CA 的 ca 憑證。

### <a name="certreq--enroll"></a>certreq-註冊

您可以使用此批註來註冊或更新您的憑證。

#### <a name="examples"></a>範例

若要註冊憑證、使用*web*伺服器範本，以及使用 U/I 選取原則伺服器：

```
certreq -enroll –machine –policyserver * WebServer
```

若要使用序號更新憑證：

```
certreq –enroll -machine –cert 61 2d 3c fe 00 00 00 00 00 05 renew
```

您只能更新有效的憑證。 過期的憑證無法更新，必須以新憑證取代。

## <a name="options"></a>選項。

| 選項。 | 描述 |
| ------- | ----------- |
| -任何 | `Force ICertRequest::Submit`判斷編碼類型。|
| -attrib`<attributestring>` | 指定**名稱**和**值**字串配對，並以冒號分隔。<p>使用`\n`分隔**名稱**和**值**字串配對（例如，Name1： value1\nName2： value2）。 |
| -binary | 將輸出檔案格式化為二進位，而不是 base64 編碼。 |
| -policyserver`<policyserver>` | ldap`<path>`<br>針對執行憑證註冊原則 web 服務的電腦，插入其 URI 或唯一識別碼。<p>若要指定您想要藉由流覽來使用要求檔案，只要使用的減號（-）符號即可`<policyserver>`。 |
| -config`<ConfigString>` | 使用設定字串中所指定的 CA （也就是**CAHostName\CAName**）來處理作業。 針對 HTTPs：\\\ 連接，指定註冊伺服器 URI。 若為本機電腦存放區 CA，請使用減號（-）符號。 |
| -匿名 | 將匿名認證用於憑證註冊 web 服務。 |
| -kerberos | 針對憑證註冊 web 服務使用 Kerberos （網域）認證。 |
| -clientcertificate`<ClientCertId>` | 您可以將取代`<ClientCertId>`為憑證指紋、CN、EKU、範本、電子郵件、UPN 或新`name=value`的語法。 |
| -username`<username>` | 與憑證註冊 web 服務搭配使用。 您可以使用`<username>` SAM 名稱或「網域**\ 使用者**」值來取代。 此選項可搭配`-p`選項使用。 |
| -p`<password>` | 與憑證註冊 web 服務搭配使用。 以`<password>`實際使用者的密碼取代。 此選項可搭配`-username`選項使用。 |
| -使用者 | 設定新`-user`憑證要求的內容，或指定接受憑證的內容。 如果 INF 或範本中未指定任何內容，則此為預設內容。 |
| -機器 | 設定新的憑證要求，或指定電腦內容接受憑證的內容。 針對新的要求，它必須與 MachineKeyset INF 金鑰和範本內容一致。 如果未指定此選項，且範本未設定內容，則預設值為使用者內容。 |
| -crl | 在所指定的 base64 編碼 PKCS #7 檔案的輸出中，包含憑證撤銷清單（Crl） `certchainfileout` ，或是所指定的 base64 編碼檔案`requestfileout`。 |
| -rpc | 指示 Active Directory 憑證服務（AD CS）使用遠端程序呼叫（RPC）伺服器連接，而不是分散式 COM。 |
| -adminforcemachine | 使用金鑰服務或模擬，從本機系統內容提交要求。 需要叫用此選項的使用者必須是本機系統管理員的成員。 |
| -renewonbehalfof | 代表簽署憑證中識別的主旨提交更新。 這會在呼叫[ICertRequest：： Submit 方法](https://docs.microsoft.com/windows/win32/api/certcli/nf-certcli-icertrequest-submit)時設定 CR_IN_ROBO |
| -f | 強制覆寫現有的檔案。 這也會略過快取範本和原則。 |
| -Q | 使用無訊息模式;隱藏所有互動式提示。 |
| -unicode | 當標準輸出重新導向或輸送至另一個命令時，寫入 Unicode 輸出，這有助於從 Windows PowerShell 腳本叫用。 |
| -unicodetext | 將 base64 文字編碼的資料 blob 寫入檔案時，傳送 Unicode 輸出。 |

## <a name="formats"></a>格式

| 格式 | 描述 |
| ------- | ----------- |
| requestfilein | Base64 編碼或二進位輸入檔名稱： PKCS #10 憑證要求、CMS 憑證要求、PKCS #7 憑證更新要求、x.509 憑證，以進行交叉認證，或 KeyGen 的標記格式憑證要求。 |
| requestfileout | Base64 編碼的輸出檔名稱。 |
| certfileout | Base64 編碼的 X-509 檔案名。 |
| PKCS10fileout | 僅與`certreq -policy`參數搭配使用。 Base64 編碼的 PKCS10 輸出檔名稱。 |
| certchainfileout | Base64 編碼的 PKCS #7 檔案名。 |
| fullresponsefileout | Base64 編碼的完整回應檔名稱。 |
| policyfilein | 僅與`certreq -policy`參數搭配使用。 INF 檔案，其中包含用來限定要求的延伸模組文字標記法。 |

## <a name="additional-resources"></a>其他資源

下列文章包含 certreq 使用方式的範例：

- [如何將主體替代名稱新增至安全的 LDAP 憑證](https://support.microsoft.com/help/931351/how-to-add-a-subject-alternative-name-to-a-secure-ldap-certificate)

- [Test Lab Guide: Deploying an AD CS Two-Tier PKI Hierarchy](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831348(v=ws.11))

- [附錄3： Certreq .exe 語法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc736326(v=ws.10))

- [如何手動建立網頁伺服器 SSL 憑證](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/how-to-create-a-web-server-ssl-certificate-manually/ba-p/1128529)

- [使用 Windows Server 2008 CA 要求 AMT 布建憑證](https://social.technet.microsoft.com/wiki/contents/articles/548.request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)

- [System Center Operations Manager 代理程式的憑證註冊](https://social.technet.microsoft.com/wiki/contents/articles/2017.certificate-enrollment-for-system-center-operations-manager-agent.aspx)

- [AD CS 逐步指南：兩層式 PKI 階層部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)

- [如何使用協力廠商憑證授權單位單位啟用 LDAP over SSL](https://support.microsoft.com/help/321051/how-to-enable-ldap-over-ssl-with-a-third-party-certification-authority)
