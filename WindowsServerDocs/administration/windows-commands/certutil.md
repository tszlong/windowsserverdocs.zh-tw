---
title: certutil
description: Certutil 命令的參考文章，這是一種命令列程式，會傾印並顯示憑證授權單位單位 (CA) 設定資訊、設定憑證服務、備份和還原 CA 元件，以及驗證憑證、金鑰組和憑證鏈。
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afaf0c75350cfb4121d0ebc664469f4494afe8c7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992946"
---
# <a name="certutil"></a>certutil

Certutil.exe 是一種命令列程式，已安裝為憑證服務的一部分。 您可以使用 certutil.exe 來傾印和顯示憑證授權單位單位 (CA) 設定資訊、設定憑證服務、備份和還原 CA 元件，以及驗證憑證、金鑰組和憑證鏈。

如果 certutil 是在沒有其他參數的憑證授權單位單位上執行，則會顯示目前的憑證授權單位單位設定。 如果 certutil 是在非憑證授權單位單位上執行，此命令會預設為執行 `certutil [-dump]` 命令。

> [!IMPORTANT]
> 較早版本的 certutil 可能無法提供本檔中所述的所有選項。 您可以藉由執行或，查看特定版本的 certutil 所提供的所有選項 `certutil -?` `certutil <parameter> -?` 。

## <a name="parameters"></a>參數

### <a name="-dump"></a>-傾印

傾印設定資訊或檔案。

```
certutil [options] [-dump]
certutil [options] [-dump] file
```

```
[-f] [-silent] [-split] [-p password] [-t timeout]
```

### <a name="-asn"></a>-asn

剖析 asn.1 檔案。

```
certutil [options] -asn file [type]
```

`[type]`：數值 CRYPT_STRING_ * 解碼類型

### <a name="-decodehex"></a>-decodehex

將十六進位編碼的檔案解碼。

```
certutil [options] -decodehex infile outfile [type]
```

`[type]`：數值 CRYPT_STRING_ * 編碼類型

```
[-f]
```

### <a name="-decode"></a>-解碼

將 Base64 編碼的檔案解碼。

```
certutil [options] -decode infile outfile
```

```
[-f]
```

### <a name="-encode"></a>-編碼

將檔案編碼為 Base64。

```
certutil [options] -encode infile outfile
```

```
[-f] [-unicodetext]
```

### <a name="-deny"></a>-拒絕

拒絕擱置中的要求。

```
certutil [options] -deny requestID
```

```
[-config Machine\CAName]
```

### <a name="-resubmit"></a>-重新提交

重新提交待決的要求。

```
certutil [options] -resubmit requestId
```

```
[-config Machine\CAName]
```

### <a name="-setattributes"></a>-setattributes

設定擱置中憑證要求的屬性。

```
certutil [options] -setattributes RequestID attributestring
```

其中：

- **requestID**是待決要求的數值要求識別碼。

- **attributestring**是要求屬性名稱和值配對。

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>備註

- 名稱和值必須以冒號分隔，而多個名稱，值組必須以分行符號分隔。 例如： `CertificateTemplate:User\nEMail:User@Domain.com` `\n` 序列轉換成分行符號的位置。

### <a name="-setextension"></a>-setextension

設定擱置中憑證要求的延伸模組。

```
certutil [options] -setextension requestID extensionname flags {long | date | string | \@infile}
```

其中：

- **requestID**是待決要求的數值要求識別碼。

- **extensionname**是延伸模組的 ObjectId 字串。

- **旗標**會設定擴充功能的優先順序。 `0`建議使用，而 `1` 將延伸模組設為 [重大]、 `2` 停用延伸模組，並 `3` 同時執行這兩項工作。

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>備註

- 如果最後一個參數是數值，則會將其視為**Long**。

- 如果最後一個參數可剖析為日期，則會被視為**日期**。

- 如果最後一個參數以為開頭 `\@` ，則會以二進位資料或 ascii 文字十六進位傾印的形式，將其餘的標記當做檔案名。

- 如果最後一個參數是任何其他參數，則會將它視為字串。

### <a name="-revoke"></a>-revoke

撤銷憑證。

```
certutil [options] -revoke serialnumber [reason]
```

其中：

- **serialnumber**是要撤銷的憑證序號清單（以逗號分隔）。

- **原因**是撤銷原因的數位或符號標記法，包括：

  - **0. CRL_REASON_UNSPECIFIED**未指定 (預設) 

  - **1. CRL_REASON_KEY_COMPROMISE**金鑰洩露

  - **2. CRL_REASON_CA_COMPROMISE** -憑證授權單位單位洩露

  - **3. CRL_REASON_AFFILIATION_CHANGED** -關係聯盟已變更

  - **4. CRL_REASON_SUPERSEDED** -已取代

  - **5. CRL_REASON_CESSATION_OF_OPERATION**作業的哈

  - **6. CRL_REASON_CERTIFICATE_HOLD** -憑證保存

  - **8. CRL_REASON_REMOVE_FROM_CRL** -從 CRL 移除

  - **1. 解除**吊銷-解除吊銷

```
[-config Machine\CAName]
```

### <a name="-isvalid"></a>-isvalid

顯示目前憑證的處置。

```
certutil [options] -isvalid serialnumber | certhash
```

```
[-config Machine\CAName]
```

### <a name="-getconfig"></a>-getconfig

取得預設設定字串。

```
certutil [options] -getconfig
```

```
[-config Machine\CAName]
```

### <a name="-ping"></a>-ping

嘗試聯絡 Active Directory 憑證服務要求介面。

```
certutil [options] -ping [maxsecondstowait | camachinelist]
```

其中：

- **camachinelist**是以逗號分隔的 CA 電腦名稱稱清單。 若為單一電腦，請使用結尾的逗號。 此選項也會顯示每部 CA 電腦的網站成本。

```
[-config Machine\CAName]
```

### <a name="-cainfo"></a>-cainfo

顯示憑證授權單位單位的相關資訊。

```
certutil [options] -cainfo [infoname [index | errorcode]]
```

其中：

- **infoname**會根據下列 infoname 引數語法，指出要顯示的 CA 屬性：

  - 檔案**檔版本**

  - **產品**產品版本

  - **exitcount** -Exit 模組計數

  - **結束 `[index]` **-Exit 模組描述

  - **原則**-原則模組描述

  - **名稱**-CA 名稱

  - **sanitizedname** -已清理的 CA 名稱

  - **dsname** -已清理的 CA 簡短名稱 (DS 名稱) 

  - **sharedfolder** -共用資料夾

  - **Error1 ErrorCode** -錯誤訊息正文

  - **Error2 相關說明 ErrorCode** -錯誤訊息正文和錯誤碼

  - **類型**-CA 類型

  - **資訊**-CA 資訊

  - **父系**-父系 CA

  - **certcount** -CA 憑證計數

  - **xchgcount** -CA 交換憑證計數

  - **kracount** -KRA 憑證計數

  - **kraused** -KRA cert 已使用計數

  - **propidmax** -CA PropId 上限

  - **certstate `[index]` **-CA 憑證

  - **certversion `[index]` **-CA 憑證版本

  - **certstatuscode `[index]` **-CA 憑證驗證狀態

  - **crlstate `[index]` **-CRL

  - **krastate `[index]` **-KRA cert

  - **crossstate + `[index]` **-正向交叉憑證

  - **crossstate- `[index]` **-後向交叉 cert

  - **cert `[index]` **-CA 憑證

  - **certchain `[index]` **-CA 憑證鏈

  - **certcrlchain `[index]` **-具有 Crl 的 CA 憑證鏈

  - **xchg `[index]` **-CA exchange 憑證

  - **xchgchain `[index]` **-CA exchange 憑證鏈

  - **xchgcrlchain `[index]` **-具有 Crl 的 CA exchange 憑證鏈

  - **kra `[index]` **-KRA cert

  - **跨 + `[index]` **-正向交叉憑證

  - **跨 `[index]` **-後向交叉 cert

  - **CRL `[index]` **-基底 CRL

  - **deltacrl `[index]` **-Delta CRL

  - **crlstatus `[index]` **-CRL 發佈狀態

  - **deltacrlstatus `[index]` **-Delta CRL 發佈狀態

  - **dns** -dns 名稱

  - **角色**-角色隔離

  - **ads** -Advanced Server

  - **範本**-範本

  - **csp `[index]` **-OCSP Url

  - **aia `[index]` **-AIA Url

  - **cdp `[index]` **-CDP Url

  - **localename** -CA 地區設定名稱

  - **subjecttemplateoids** -主旨範本 oid

  - **&#42;** -顯示所有屬性

- **index**是選擇性的以零為基底的屬性索引。

- **errorcode**是數值的錯誤碼。

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cacert"></a>-ca cert

取得憑證授權單位單位的憑證。

```
certutil [options] -ca.cert outcacertfile [index]
```

其中：

- **outcacertfile**是輸出檔案。

- [**索引**] 是 CA 憑證更新索引 (預設為最新的) 。

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cachain"></a>-ca. 鏈

取得憑證授權單位單位的憑證鏈。

```
certutil [options] -ca.chain outcacertchainfile [index]
```

其中：

- **outcacertchainfile**是輸出檔案。

- [**索引**] 是 CA 憑證更新索引 (預設為最新的) 。

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-getcrl"></a>-getcrl

取得 (CRL) 的憑證撤銷清單。

certutil [options]-getcrl outfile [index] [delta]

其中：

- **索引**是 crl 索引或金鑰索引 (預設為最新金鑰) 的 crl。

- **delta**是 delta crl (預設值是基底 crl) 。

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-crl"></a>-crl

發佈新的憑證撤銷清單 (Crl) 或 delta Crl。

```
certutil [options] -crl [dd:hh | republish] [delta]
```

其中：

- **dd： hh**是新的 CRL 有效期間（以天和小時為單位）。

- 重新**發佈**將最新的 crl。

- **差異**只會發佈 delta crl (預設值為「基底」和「delta crl」) 。

```
[-split] [-config Machine\CAName]
```

### <a name="-shutdown"></a>-關機

關閉 Active Directory 憑證服務。

```
certutil [options] -shutdown
```

```
[-config Machine\CAName]
```

### <a name="-installcert"></a>-installcert

安裝憑證授權單位單位憑證。

```
certutil [options] -installcert [cacertfile]
```

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-renewcert"></a>-renewcert

更新憑證授權單位單位憑證。

```
certutil [options] -renewcert [reusekeys] [Machine\ParentCAName]
```

- 使用 `-f` 來忽略未處理的更新要求，並產生新的要求。

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-schema"></a>-架構

傾印憑證的架構。

```
certutil [options] -schema [ext | attrib | cRL]
```
其中：

- 此命令預設為要求和憑證資料表。

- **ext**是延伸模組資料表。

- **attribute**是屬性資料表。

- **crl**是 crl 資料表。

```
[-split] [-config Machine\CAName]
```

### <a name="-view"></a>-view

傾印憑證視圖。

```
certutil [options] -view [queue | log | logfail | revoked | ext | attrib | crl] [csv]
```

其中：

- **佇列**會傾印特定的要求佇列。

- **記錄**會傾印發行或撤銷的憑證，再加上任何失敗的要求。

- **logfail**傾印失敗的要求。

- 已**撤銷**傾印已撤銷的憑證。

- **ext**傾印延伸模組資料表。

- **屬性**會傾印屬性資料表。

- **crl**會傾印 crl 資料表。

- **csv**提供使用逗號分隔值的輸出。

```
[-silent] [-split] [-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

#### <a name="remarks"></a>備註

- 若要顯示所有專案的**StatusCode**資料行，請輸入`-out StatusCode`

- 若要顯示最後一個專案的所有資料行，請輸入：`-restrict RequestId==$`

- 若要顯示三個**要求的** **RequestID**和配置，請輸入：`-restrict requestID>37,requestID<40 -out requestID,disposition`

- 若要顯示所有基底 Crl 的資料列識別碼資料**列識別碼**和**CRL 號碼**，請輸入：`-restrict crlminbase=0 -out crlrowID,crlnumber crl`

- 若要顯示，請輸入：`-v -restrict crlminbase=0,crlnumber=3 -out crlrawcrl crl`

- 若要顯示整個 CRL 資料表，請輸入：`CRL`

- 使用 `Date[+|-dd:hh]` 日期限制。

- `now+dd:hh`針對與目前時間相關的日期使用。

### <a name="-db"></a>-db

傾印原始資料庫。

```
certutil [options] -db
```

```
[-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

### <a name="-deleterow"></a>-deleterow

刪除伺服器資料庫中的資料列。

```
certutil [options] -deleterow rowID | date [request | cert | ext | attrib | crl]
```

其中：

- [**要求**] 會根據提交日期來刪除失敗和待決的要求。

- **cert**會根據到期日刪除過期和撤銷的憑證。

- **ext**會刪除延伸模組資料表。

- **屬性**會刪除屬性資料表。

- **crl**會刪除 crl 資料表。

```
[-f] [-config Machine\CAName]
```

#### <a name="examples"></a>範例

- 若要刪除2001年1月22日提交的失敗和擱置要求，請輸入：`1/22/2001 request`

- 若要刪除2001年1月22日過期的所有憑證，請輸入：`1/22/2001 cert`

- 若要刪除 RequestID 37 的憑證資料列、屬性和延伸模組，請輸入：`37`

- 若要刪除2001年1月22日到期的 Crl，請輸入：`1/22/2001 crl`

### <a name="-backup"></a>-backup

備份 Active Directory 憑證服務。

```
certutil [options] -backup backupdirectory [incremental] [keeplog]
```

其中：

- **backupdirectory**是用來儲存備份資料的目錄。

- **增量**只會執行增量備份 (預設為完整備份) 。

- **keeplog**會保留資料庫記錄檔 (預設為截斷) 的記錄檔。

```
[-f] [-config Machine\CAName] [-p Password]
```

### <a name="-backupdb"></a>-backupdb

備份憑證服務資料庫 Active Directory。

```
certutil [options] -backupdb backupdirectory [incremental] [keeplog]
```

其中：

- **backupdirectory**是用來儲存備份資料庫檔案的目錄。

- **增量**只會執行增量備份 (預設為完整備份) 。

- **keeplog**會保留資料庫記錄檔 (預設為截斷) 的記錄檔。

```
[-f] [-config Machine\CAName]
```

### <a name="-backupkey"></a>-backupkey

備份憑證服務憑證和私密金鑰 Active Directory。

```
certutil [options] -backupkey backupdirectory
```

其中：

- **backupdirectory**是用來儲存備份 PFX 檔案的目錄。

```
[-f] [-config Machine\CAName] [-p password] [-t timeout]
```

### <a name="-restore"></a>-restore

還原 Active Directory 憑證服務。

```
certutil [options] -restore backupdirectory
```

其中：

- **backupdirectory**是包含要還原之資料的目錄。

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-restoredb"></a>-restoredb

還原 Active Directory 憑證服務資料庫。

```
certutil [options] -restoredb backupdirectory
```

其中：

- **backupdirectory**是包含要還原之資料庫檔案的目錄。

```
[-f] [-config Machine\CAName]
```

### <a name="-restorekey"></a>-restorekey

還原 Active Directory 憑證服務憑證和私密金鑰。

```
certutil [options] -restorekey backupdirectory | pfxfile
```

其中：

- **backupdirectory**是包含要還原之 PFX 檔案的目錄。

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-importpfx"></a>-importpfx

匯入憑證和私密金鑰。 如需詳細資訊，請參閱本文 `-store` 中的參數。

```
certutil [options] -importpfx [certificatestorename] pfxfile [modifiers]
```

其中：

- **certificatestorename**是憑證存放區的名稱。

- 修飾**詞是以**逗號分隔的清單，其中可以包含下列一或多項：

  1. **AT_SIGNATURE** -將 keyspec 變更為 SIGNATURE

  2. **AT_KEYEXCHANGE** -將 keyspec 變更為金鑰交換

  3. **NoExport** -使私密金鑰無法匯出

  4. **NoCert** -不匯入憑證

  5. **NoChain** -不匯入憑證鏈

  6. **NoRoot** -不匯入根憑證

  7. **保護**-使用密碼保護金鑰

  8. **NoProtect** -不使用密碼來保護金鑰

```
[-f] [-user] [-p password] [-csp provider]
```

#### <a name="remarks"></a>備註

- 預設為個人電腦存放區。

### <a name="-dynamicfilelist"></a>-dynamicfilelist

顯示動態檔案清單。

```
certutil [options] -dynamicfilelist
```

```
[-config Machine\CAName]
```

### <a name="-databaselocations"></a>-databaselocations

顯示資料庫位置。

```
certutil [options] -databaselocations
```

```
[-config Machine\CAName]
```

### <a name="-hashfile"></a>-hashfile

產生並顯示檔案的密碼編譯雜湊。

```
certutil [options] -hashfile infile [hashalgorithm]
```

### <a name="-store"></a>-store

傾印證書存儲。

```
certutil [options] -store [certificatestorename [certID [outputfile]]]
```

其中：

- **certificatestorename**是憑證存放區名稱。 例如：

  - `My, CA (default), Root,`

  - `ldap:///CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?one?objectClass=certificationAuthority (View Root Certificates)`

  - `ldap:///CN=CAName,CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Modify Root Certificates)`

  - `ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?certificateRevocationList?base?objectClass=cRLDistributionPoint (View CRLs)`

  - `ldap:///CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Enterprise CA Certificates)`

  - `ldap: (AD computer object certificates)`

  - `-user ldap: (AD user object certificates)`

- **certID**是憑證或 CRL 相符 token。 這可以是序號、SHA-1 憑證、CRL、CTL 或公開金鑰雜湊、數值憑證索引 (0、1等等) 、數位 CRL 索引 ( .0、.1 等等，) 的數值 CTL 索引 (.。0、.。1，因此在) 、公用金鑰、簽章或延伸 ObjectId、憑證主體一般名稱、電子郵件地址、UPN 或 DNS 名稱、金鑰容器名稱或 CSP 名稱、範本名稱或 ObjectId、EKU 或應用程式原則 ObjectId，或 CRL 簽發者一般名稱。 其中有許多可能會導致多個相符專案。

- **outputfile**是用來儲存相符憑證的檔案。

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-silent] [-split] [-dc DCName]
```

#### <a name="options"></a>選項。

- `-user`選項會存取使用者存放區，而不是電腦存放區。

- `-enterprise`選項會存取機器企業商店。

- `-service`選項會存取機器服務存放區。

- `-grouppolicy`選項會存取電腦群組策略存放區。

例如：

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-addstore"></a>-addstore

將憑證新增至存放區。 如需詳細資訊，請參閱本文 `-store` 中的參數。

```
certutil [options] -addstore certificatestorename infile
```

其中：

- **certificatestorename**是憑證存放區名稱。

- **infile**是您想要新增至存放區的憑證或 CRL 檔案。

```
[-f] [-user] [-enterprise] [-grouppolicy] [-dc DCName]
```

### <a name="-delstore"></a>-delstore

從存放區中刪除憑證。 如需詳細資訊，請參閱本文 `-store` 中的參數。

```
certutil [options] -delstore certificatestorename certID
```

其中：

- **certificatestorename**是憑證存放區名稱。

- **certID**是憑證或 CRL 相符 token。

```
[-enterprise] [-user] [-grouppolicy] [-dc DCName]
```

### <a name="-verifystore"></a>-verifystore

驗證存放區中的憑證。 如需詳細資訊，請參閱本文 `-store` 中的參數。

```
certutil [options] -verifystore certificatestorename [certID]
```

其中：

- **certificatestorename**是憑證存放區名稱。

- **certID**是憑證或 CRL 相符 token。

```
[-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-dc DCName] [-t timeout]
```

### <a name="-repairstore"></a>-repairstore

修復金鑰關聯或更新憑證屬性或金鑰安全描述項。 如需詳細資訊，請參閱本文 `-store` 中的參數。

```
certutil [options] -repairstore certificatestorename certIDlist [propertyinffile | SDDLsecuritydescriptor]
```

其中：

- **certificatestorename**是憑證存放區名稱。

- **certIDlist**是以逗號分隔的憑證或 CRL 相符權杖清單。 如需詳細資訊，請參閱本文 `-store certID` 中的描述。

- **propertyinffile**是包含外部屬性的 INF 檔案，包括：

  ```
  [Properties]
      19 = Empty ; Add archived property, OR:
      19 =       ; Remove archived property

      11 = {text}Friendly Name ; Add friendly name property

      127 = {hex} ; Add custom hexadecimal property
          _continue_ = 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
          _continue_ = 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f

      2 = {text} ; Add Key Provider Information property
        _continue_ = Container=Container Name&
        _continue_ = Provider=Microsoft Strong Cryptographic Provider&
        _continue_ = ProviderType=1&
        _continue_ = Flags=0&
        _continue_ = KeySpec=2

      9 = {text} ; Add Enhanced Key Usage property
        _continue_ = 1.3.6.1.5.5.7.3.2,
        _continue_ = 1.3.6.1.5.5.7.3.1,
  ```

```
[-f] [-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-csp provider]
```

### <a name="-viewstore"></a>-viewstore

傾印證書存儲。 如需詳細資訊，請參閱本文 `-store` 中的參數。

```
certutil [options] -viewstore [certificatestorename [certID [outputfile]]]
```

其中：

- **certificatestorename**是憑證存放區名稱。

- **certID**是憑證或 CRL 相符 token。

- **outputfile**是用來儲存相符憑證的檔案。

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>選項。

- `-user`選項會存取使用者存放區，而不是電腦存放區。

- `-enterprise`選項會存取機器企業商店。

- `-service`選項會存取機器服務存放區。

- `-grouppolicy`選項會存取電腦群組策略存放區。

例如：

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-viewdelstore"></a>-viewdelstore

從存放區中刪除憑證。

```
certutil [options] -viewdelstore [certificatestorename [certID [outputfile]]]
```

其中：

- **certificatestorename**是憑證存放區名稱。

- **certID**是憑證或 CRL 相符 token。

- **outputfile**是用來儲存相符憑證的檔案。

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>選項。

- `-user`選項會存取使用者存放區，而不是電腦存放區。

- `-enterprise`選項會存取機器企業商店。

- `-service`選項會存取機器服務存放區。

- `-grouppolicy`選項會存取電腦群組策略存放區。

例如：

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-dspublish"></a>-dspublish

將憑證或憑證撤銷清單 (CRL) 發佈至 Active Directory。

```
certutil [options] -dspublish certfile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | User | Machine]
```

```
certutil [options] -dspublish CRLfile [DSCDPContainer [DSCDPCN]]
```

其中：

- **certfile**是要發佈之憑證檔案的名稱。

- **NTAuthCA**會將憑證發佈至 DS Enterprise store。

- **Rootca.cer**會將憑證發佈至 DS 受信任的根存放區。

- **SubCA**會將 CA 憑證發佈至 DS CA 物件。

- **CrossCA**會將交互憑證發佈至 DS CA 物件。

- **KRA**會將憑證發佈至 DS 金鑰復原代理物件。

- **使用者**將憑證發佈至使用者 DS 物件。

- **電腦**會將憑證發佈至電腦 DS 物件。

- **Crlfile.crl**是要發行之 CRL 檔案的名稱。

- **DSCDPContainer**是 DS CDP 容器 CN，通常是 CA 電腦名稱稱。

- **DSCDPCN**是 DS CDP 物件 CN，通常是以已清理的 CA 簡短名稱和金鑰索引為基礎。

- 使用 `-f` 來建立新的 DS 物件。

```
[-f] [-user] [-dc DCName]
```

### <a name="-adtemplate"></a>-adtemplate

顯示 Active Directory 範本。

```
certutil [options] -adtemplate [template]
```

```
[-f] [-user] [-ut] [-mt] [-dc DCName]
```

### <a name="-template"></a>-範本

顯示憑證範本。

```
certutil [options] -template [template]
```

```
[-f] [-user] [-silent] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-templatecas"></a>-templatecas

顯示憑證範本 (Ca) 的憑證授權單位單位。

```
certutil [options] -templatecas template
```

```
[-f] [-user] [-dc DCName]
```

### <a name="-catemplates"></a>-之 catemplates.txt 檔

顯示憑證授權單位單位的範本。

```
certutil [options] -catemplates [template]
```

```
[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]
```

### <a name="-setcasites"></a>-setcasites

管理網站名稱，包括設定、驗證和刪除憑證授權單位單位網站名稱

```
certutil [options] -setcasites [set] [sitename]
certutil [options] -setcasites verify [sitename]
certutil [options] -setcasites delete
```

其中：

- 只有在以單一憑證授權單位單位為目標時，才允許**sitename** 。

```
[-f] [-config Machine\CAName] [-dc DCName]
```

#### <a name="remarks"></a>備註

- `-config`選項的目標是單一憑證授權單位單位 (預設值是所有 ca) 。

- `-f`選項可以用來覆寫指定**sitename**的驗證錯誤，或刪除所有 CA sitenames。

> [!NOTE]
> 如需有關為 Active Directory Domain Services (AD DS) 網站感知設定 Ca 的詳細資訊，請參閱[AD DS AD CS 和 PKI 用戶端的網站感知](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))。

### <a name="-enrollmentserverurl"></a>-enrollmentserverURL

顯示、新增或刪除與 CA 相關聯的註冊伺服器 Url。

```
certutil [options] -enrollmentServerURL [URL authenticationtype [priority] [modifiers]]
certutil [options] -enrollmentserverURL URL delete
```

其中：

- **authenticationtype**指定下列其中一個用戶端驗證方法，同時新增 URL：

  1. **kerberos** -使用 kerberos SSL 認證。

  2. 使用者**名稱**-針對 SSL 認證使用已命名的帳戶。

  3. **clientcertificate**：-使用 X.509 憑證 SSL 認證。

  4. **匿名**-使用匿名 SSL 認證。

- [**刪除**] 會刪除與 CA 相關聯的指定 URL。

- **priority** `1` 如果在新增 URL 時未指定，則優先順序預設為。

- 修飾**詞是以**逗號分隔的清單，其中包含下列一或多項：

1. **allowrenewalsonly** -只有續訂要求可以透過此 URL 提交給此 CA。

2. **allowkeybasedrenewal** -允許使用在 AD 中沒有相關聯帳戶的憑證。 這僅適用于 clientcertificate 和 allowrenewalsonly 模式

```
[-config Machine\CAName] [-dc DCName]
```

### <a name="-adca"></a>-adca

顯示 Active Directory 憑證授權單位單位。

```
certutil [options] -adca [CAName]
```

```
[-f] [-split] [-dc DCName]
```

### <a name="-ca"></a>-ca

顯示註冊原則憑證授權單位單位。

```
certutil [options] -CA [CAName | templatename]
```

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policy"></a>-原則

顯示註冊原則。

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policycache"></a>-policycache

顯示或刪除註冊原則快取專案。

```
certutil [options] -policycache [delete]
```

其中：

- [**刪除**] 會刪除原則伺服器快取專案。

- **-f**刪除所有快取專案

```
[-f] [-user] [-policyserver URLorID]
```

### <a name="-credstore"></a>-credstore

顯示、新增或刪除認證存放區專案。

```
certutil [options] -credstore [URL]
certutil [options] -credstore URL add
certutil [options] -credstore URL delete
```

其中：

- **Url**是目標 url。 您也可以使用 `*` 來比對所有專案，或符合 `https://machine*` URL 前置詞。

- **新增**新增認證存放區專案。 使用此選項時，也需要使用 SSL 認證。

- **delete**刪除認證存放區專案。

- **-f**會覆寫單一專案或刪除多個專案。

```
[-f] [-user] [-silent] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-installdefaulttemplates"></a>-installdefaulttemplates 動作

安裝預設憑證範本。

```
certutil [options] -installdefaulttemplates
```

```
[-dc DCName]
```

### <a name="-urlcache"></a>-URLcache

顯示或刪除 URL 快取專案。

```
certutil [options] -URLcache [URL | CRL | * [delete]]
```

其中：

- **Url**是快取的 url。

- **CRL**只會在所有快取的 CRL url 上執行。

- **&#42;** 會在所有快取的 url 上運作。

- [**刪除**] 會從目前使用者的本機快取中刪除相關的 url。

- **-f**會強制提取特定的 URL，並更新快取。

```
[-f] [-split]
```

### <a name="-pulse"></a>-脈衝

脈衝自動註冊事件。

```
certutil [options] -pulse
```

```
[-user]
```

### <a name="-machineinfo"></a>-machineinfo

顯示 Active Directory 機物件的相關資訊。

```
certutil [options] -machineinfo domainname\machinename$
```

### <a name="-dcinfo"></a>-DCInfo

顯示網域控制站的相關資訊。 預設會顯示 DC 憑證而不進行驗證。

```
certutil [options] -DCInfo [domain] [verify | deletebad | deleteall]
```

```
[-f] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

> [!TIP]
> 在 Windows Server 2012 中已新增 Active Directory Domain Services (AD DS) 網域 **[網域]** 和指定網域控制站 (**-dc**) 的功能。 若要成功執行命令，您必須使用屬於**Domain admins**或**Enterprise admins**成員的帳戶。 此命令的行為修改如下所示：<ol><li>1. 如果未指定網域，而且未指定特定網域控制站，則此選項會傳回要從預設網域控制站處理的網域控制站清單。</li><li>2. 如果未指定網域，但指定網域控制站，則會產生指定網域控制站上的憑證報告。</li><li>3. 如果指定網域，但未指定網域控制站，則會在清單中的每個網域控制站的憑證上產生網域控制站清單，以及報告。</li><li>4. 如果指定網域和網域控制站，則會從目標網域控制站產生網域控制站清單。 也會產生清單中每個網域控制站的憑證報告。</li></ol>
>
>例如，假設有一個名為 CPANDL 的網域，且名為 CPANDL 的網域控制站。 您可以執行下列命令，從 CPANDL-DC1 取得網域控制站及其憑證的清單：`certutil -dc cpandl-dc1 -DCInfo cpandl`

### <a name="-entinfo"></a>-entinfo

顯示企業憑證授權單位單位的相關資訊。

```
certutil [options] -entinfo domainname\machinename$
```

```
[-f] [-user]
```

### <a name="-tcainfo"></a>-tcainfo

顯示憑證授權單位單位的相關資訊。

```
certutil [options] -tcainfo [domainDN | -]
```

```
[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

### <a name="-scinfo"></a>-scinfo

顯示智慧卡的相關資訊。

```
certutil [options] -scinfo [readername [CRYPT_DELETEKEYSET]]
```

其中：

- **CRYPT_DELETEKEYSET**刪除智慧卡上的所有金鑰。

```
[-silent] [-split] [-urlfetch] [-t timeout]
```

### <a name="-scroots"></a>-scroots

管理智慧卡的根憑證。

```
certutil [options] -scroots update [+][inputrootfile] [readername]
certutil [options] -scroots save \@in\\outputrootfile [readername]
certutil [options] -scroots view [inputrootfile | readername]
certutil [options] -scroots delete [readername]
```

```
[-f] [-split] [-p Password]
```

### <a name="-verifykeys"></a>-verifykeys

驗證公用或私用金鑰組。

```
certutil [options] -verifykeys [keycontainername cacertfile]
```

其中：

- **cspparameters.keycontainername**是要驗證之金鑰的金鑰容器名稱。 此選項預設為電腦金鑰。 若要切換至使用者金鑰，請使用 `-user` 。

- **cacertfile**簽署或加密憑證檔案。

```
[-f] [-user] [-silent] [-config Machine\CAName]
```

#### <a name="remarks"></a>備註

- 如果未指定任何引數，則會根據其私密金鑰來驗證每個簽署 CA 憑證。

- 這項作業只能針對本機 CA 或本機密鑰來執行。

### <a name="-verify"></a>-驗證

驗證憑證、憑證撤銷清單 (CRL) 或憑證鏈。

```
certutil [options] -verify certfile [applicationpolicylist | - [issuancepolicylist]]
certutil [options] -verify certfile [cacertfile [crossedcacertfile]]
certutil [options] -verify CRLfile cacertfile [issuedcertfile]
certutil [options] -verify CRLfile cacertfile [deltaCRLfile]
```

其中：

- **certfile**是要驗證之憑證的名稱。

- **applicationpolicylist**是必要的應用程式原則 objectid 的選擇性逗號分隔清單。

- **issuancepolicylist**是必要發佈原則 objectid 的選擇性逗號分隔清單。

- **cacertfile**是要驗證的選擇性發行 CA 憑證。

- **crossedcacertfile**是**certfile**所交叉認證的選擇性憑證。

- **Crlfile.crl**是用來驗證**cacertfile**的 CRL 檔案。

- **issuedcertfile**是 crlfile.crl 所涵蓋的選擇性發行憑證。

- **deltaCRLfile**是選擇性的 delta CRL 檔案。

```
[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t timeout]
```

#### <a name="remarks"></a>備註

- 使用**applicationpolicylist**會將鏈建築物限制為僅適用于指定之應用程式原則的有效鏈。

- 使用**issuancepolicylist**會將鏈建築物限制為僅適用于指定發佈原則的連結。

- 使用**cacertfile**會根據**certfile**或**crlfile.crl**來驗證檔案中的欄位。

- 使用**issuedcertfile**會針對**crlfile.crl**驗證檔案中的欄位。

- 使用 deltaCRLfile 會針對**certfile**驗證檔案中的欄位。

- 如果未指定**cacertfile** ，則會針對**certfile**建立並驗證完整鏈。

- 如果同時指定**cacertfile**和**crossedcacertfile** ，這兩個檔案中的欄位都會針對**certfile**進行驗證。

### <a name="-verifyctl"></a>-verifyCTL

驗證 AuthRoot 或不允許的憑證 CTL。

```
certutil [options] -verifyCTL CTLobject [certdir] [certfile]
```

其中：

- **CTLobject**會識別要驗證的 CTL，包括：

  - **AuthRootWU** -從 URL 快取讀取 AuthRoot CAB 和相符的憑證。 請改用 `-f` ，改為從 Windows Update 下載。

  - **DisallowedWU** -從 URL 快取讀取不允許的憑證 CAB 和不允許的憑證存放區檔案。 請改用 `-f` ，改為從 Windows Update 下載。

  - **AuthRoot** -讀取登錄快取的 AuthRoot CTL。 使用搭配 `-f` 和不受信任的**certfile** ，以強制登錄快取的 AuthRoot 和不允許的憑證 ctl 進行更新。

  - 不**允許**-讀取登錄-快取的不允許憑證 CTL。 使用搭配 `-f` 和不受信任的**certfile** ，以強制登錄快取的 AuthRoot 和不允許的憑證 ctl 進行更新。

- **CTLfilename**指定 CTL 或 CAB 檔案的檔案或 HTTP 路徑。

- **certdir**指定包含符合 CTL 專案之憑證的資料夾。 預設為與**CTLobject**相同的資料夾或網站。 使用 HTTP 資料夾路徑時，結尾必須有路徑分隔符號。 如果您未指定**AuthRoot**或不**允許**，則會搜尋多個位置以尋找相符的憑證，包括本機憑證存放區、crypt32.dll 資源和本機 URL 快取。 `-f`視需要使用從 Windows Update 下載。

- **certfile**指定) 要驗證的憑證 (。 憑證會與 CTL 專案進行比對，並顯示結果。 此選項會隱藏大部分的預設輸出。

```
[-f] [-user] [-split]
```

### <a name="-sign"></a>-sign

 (CRL) 或憑證重新簽署憑證撤銷清單。

```
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [startdate+dd:hh] [+serialnumberlist | -serialnumberlist | -objectIDlist | \@extensionfile]
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [#hashalgorithm] [+alternatesignaturealgorithm | -alternatesignaturealgorithm]
```

其中：

- **infilelist**是要修改和重新簽署的憑證或 CRL 檔案清單（以逗號分隔）。

- **serialnumber**是要建立之憑證的序號。 不能有有效期間和其他選項。

- **Crl**會建立空的 crl。 不能有有效期間和其他選項。

- **outfilelist**是已修改的憑證或 CRL 輸出檔案清單（以逗號分隔）。 檔案數目必須符合 infilelist。

- 開始**日期 + dd： hh**是憑證或 CRL 檔案的新有效期間，包括：

  - 選擇性的日期加上

  - 選擇性的天數和時數有效期間

  如果同時指定這兩者，您就必須使用加號 (+) 分隔符號。 使用從 `now[+dd:hh]` 目前的時間開始。 請使用 `never` ，讓 crl 沒有到期日 (僅) 。

- **serialnumberlist**是要新增或移除之檔案的逗點分隔序號清單。

- **objectIDlist**是要移除之檔案的逗號分隔擴充功能 ObjectId 清單。

- ** \@ EXTENSIONFILE**是 INF 檔案，其中包含要更新或移除的延伸模組。 例如：

  ```
  [Extensions]
      2.5.29.31 = ; Remove CRL Distribution Points extension
      2.5.29.15 = {hex} ; Update Key Usage extension
      _continue_=03 02 01 86
  ```

- **hashalgorithm**是雜湊演算法的名稱。 這必須是前面加上 `#` 正負號的文字。

- **alternatesignaturealgorithm**是替代簽章演算法規範。

```
[-nullsign] [-f] [-silent] [-cert certID]
```

#### <a name="remarks"></a>備註

- 使用負號 (-) 會移除序號和延伸模組。

- 使用加號 (+) 會將序號新增至 CRL。

- 您可以使用清單同時從 CRL 移除序號和 Objectid。

- 在**alternatesignaturealgorithm**之前使用減號，可讓您使用舊版簽章格式。 使用加號可讓您使用替代簽章格式。 如果您未指定**alternatesignaturealgorithm**，則會使用憑證或 CRL 中的簽章格式。

### <a name="-vroot"></a>-vroot

建立或刪除 web 虛擬根目錄和檔案共用。

```
certutil [options] -vroot [delete]
```

### <a name="-vocsproot"></a>-vocsproot

建立或刪除 OCSP Web Proxy 的 web 虛擬根目錄。

```
certutil [options] -vocsproot [delete]
```

### <a name="-addenrollmentserver"></a>-addenrollmentserver

如有必要，請為指定的憑證授權單位單位新增註冊伺服器應用程式和應用程式集區。 此命令不會安裝二進位檔或封裝。

```
certutil [options] -addenrollmentserver kerberos | username | clientcertificate [allowrenewalsonly] [allowkeybasedrenewal]
```

其中：

- **addenrollmentserver**會要求您針對與憑證註冊伺服器的用戶端連線使用驗證方法，包括：

  - **kerberos**使用 kerberos SSL 認證。

  - 使用者**名稱使用命名**帳戶作為 SSL 認證。

  - **clientcertificate**使用 X.509 憑證 SSL 認證。

- **allowrenewalsonly**只允許透過 URL 向憑證授權單位單位提交更新要求。

- **allowkeybasedrenewal**允許在 Active Directory 中使用沒有相關聯帳戶的憑證。 這適用于搭配**clientcertificate**和**allowrenewalsonly**模式使用時。

```
[-config Machine\CAName]
```

### <a name="-deleteenrollmentserver"></a>-deleteenrollmentserver

必要時，為指定的憑證授權單位單位刪除註冊伺服器應用程式和應用程式集區。 此命令不會安裝二進位檔或封裝。

```
certutil [options] -deleteenrollmentserver kerberos | username | clientcertificate
```

其中：

- **deleteenrollmentserver**會要求您針對與憑證註冊伺服器的用戶端連線使用驗證方法，包括：

  - **kerberos**使用 kerberos SSL 認證。

  - 使用者**名稱使用命名**帳戶作為 SSL 認證。

  - **clientcertificate**使用 X.509 憑證 SSL 認證。

```
[-config Machine\CAName]
```

### <a name="-addpolicyserver"></a>-addpolicyserver

視需要新增原則伺服器應用程式和應用程式集區。 此命令不會安裝二進位檔或封裝。

```
certutil [options] -addpolicyserver kerberos | username | clientcertificate [keybasedrenewal]
```

其中：

- **addpolicyserver**會要求您針對與憑證原則伺服器的用戶端連線使用驗證方法，包括：

  - **kerberos**使用 kerberos SSL 認證。

  - 使用者**名稱使用命名**帳戶作為 SSL 認證。

  - **clientcertificate**使用 X.509 憑證 SSL 認證。

- **keybasedrenewal**允許使用傳回給包含 keybasedrenewal 範本之用戶端的原則。 此選項只適用于使用者**名稱**和**clientcertificate**驗證。

### <a name="-deletepolicyserver"></a>-deletepolicyserver

視需要刪除原則伺服器應用程式和應用程式集區。 此命令不會移除二進位檔或封裝。

certutil [options]-deletePolicyServer kerberos |使用者名稱 |clientcertificate [keybasedrenewal]

其中：

- **deletepolicyserver**會要求您針對與憑證原則伺服器的用戶端連線使用驗證方法，包括：

  - **kerberos**使用 kerberos SSL 認證。

  - 使用者**名稱使用命名**帳戶作為 SSL 認證。

  - **clientcertificate**使用 X.509 憑證 SSL 認證。

- **keybasedrenewal**允許使用 keybasedrenewal 原則伺服器。

### <a name="-oid"></a>-oid

顯示物件識別碼或設定顯示名稱。

```
certutil [options] -oid objectID [displayname | delete [languageID [type]]]
certutil [options] -oid groupID
certutil [options] -oid agID | algorithmname [groupID]
```

其中：

- **objectID**顯示或加入顯示名稱。

- **groupid**是 objectid 列舉 (decimal) 的 groupid 數位。

- **algID**是 objectID 所查詢的十六進位識別碼。

- **algorithmname**是 objectID 所查詢的演算法名稱。

- **displayname**會顯示要儲存在 DS 中的名稱。

- **delete**刪除顯示名稱。

- **LanguageId**是語言識別項值 (預設為目前的： 1033) 。

- **類型**是要建立的 DS 物件類型，包括：

  - `1`-範本 (預設) 

  - `2`-發行原則

  - `3`-應用程式原則

- `-f`建立 DS 物件。

### <a name="-error"></a>-錯誤

顯示與錯誤碼相關聯的郵件內文。

```
certutil [options] -error errorcode
```

### <a name="-getreg"></a>-getreg

顯示登錄值。

```
certutil [options] -getreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

其中：

- **ca**會使用憑證授權單位單位的登錄機碼。

- **restore**會使用憑證授權單位單位的 restore 登錄機碼。

- **原則**會使用原則模組的登錄機碼。

- **exit**會使用第一個結束模組的登錄機碼。

- **範本**會使用範本登錄機碼， (用於 `-user`) 的使用者範本。

- **註冊**會使用註冊登錄機碼 (用於 `-user` 使用者內容) 。

- **鏈**會使用連鎖設定登錄機碼。

- **policyservers**會使用原則伺服器登錄機碼。

- **progID**會使用原則或結束模組的 progID (登錄子機碼名稱) 。

- **registryvaluename**會使用登錄值名稱， (用 `Name*` 來) 前置詞相符。

- **值**使用新的數值、字串或日期登錄值或檔案名。 如果數值開頭為 `+` 或，則 `-` 會在現有的登錄值中設定或清除在新值中指定的位。

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>備註

- 如果字串值的開頭為 `+` 或 `-` ，且現有的值為 `REG_MULTI_SZ` 值，則會在現有的登錄值中加入或移除字串。 若要強制建立 `REG_MULTI_SZ` 值，請將新增 `\n` 至字串值的結尾。

- 如果值以開頭 `\@` ，則值的其餘部分會是包含二進位值之十六進位文字表示的檔案名。 如果未參考有效的檔案，則會改為將其剖析為 `[Date][+|-][dd:hh]` 選擇性的日期加或減去選擇性的日和小時。 如果同時指定這兩者，請使用加號 (+) 或負號 ( ) 分隔符號。 `now+dd:hh`針對與目前時間相關的日期使用。

- 用 `chain\chaincacheresyncfiletime \@now` 來有效地清除快取的 crl。

### <a name="-setreg"></a>-setreg

設定登錄值。

```
certutil [options] -setreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]]registryvaluename value
```

其中：

- **ca**會使用憑證授權單位單位的登錄機碼。

- **restore**會使用憑證授權單位單位的 restore 登錄機碼。

- **原則**會使用原則模組的登錄機碼。

- **exit**會使用第一個結束模組的登錄機碼。

- **範本**會使用範本登錄機碼， (用於 `-user`) 的使用者範本。

- **註冊**會使用註冊登錄機碼 (用於 `-user` 使用者內容) 。

- **鏈**會使用連鎖設定登錄機碼。

- **policyservers**會使用原則伺服器登錄機碼。

- **progID**會使用原則或結束模組的 progID (登錄子機碼名稱) 。

- **registryvaluename**會使用登錄值名稱， (用 `Name*` 來) 前置詞相符。

- **值**使用新的數值、字串或日期登錄值或檔案名。 如果數值開頭為 `+` 或，則 `-` 會在現有的登錄值中設定或清除在新值中指定的位。

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>備註

- 如果字串值的開頭為 `+` 或 `-` ，且現有的值為 `REG_MULTI_SZ` 值，則會在現有的登錄值中加入或移除字串。 若要強制建立 `REG_MULTI_SZ` 值，請將新增 `\n` 至字串值的結尾。

- 如果值以開頭 `\@` ，則值的其餘部分會是包含二進位值之十六進位文字表示的檔案名。 如果未參考有效的檔案，則會改為將其剖析為 `[Date][+|-][dd:hh]` 選擇性的日期加或減去選擇性的日和小時。 如果同時指定這兩者，請使用加號 (+) 或負號 ( ) 分隔符號。 `now+dd:hh`針對與目前時間相關的日期使用。

- 用 `chain\chaincacheresyncfiletime \@now` 來有效地清除快取的 crl。

### <a name="-delreg"></a>-delreg

刪除登錄值。

```
certutil [options] -delreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

其中：

- **ca**會使用憑證授權單位單位的登錄機碼。

- **restore**會使用憑證授權單位單位的 restore 登錄機碼。

- **原則**會使用原則模組的登錄機碼。

- **exit**會使用第一個結束模組的登錄機碼。

- **範本**會使用範本登錄機碼， (用於 `-user`) 的使用者範本。

- **註冊**會使用註冊登錄機碼 (用於 `-user` 使用者內容) 。

- **鏈**會使用連鎖設定登錄機碼。

- **policyservers**會使用原則伺服器登錄機碼。

- **progID**會使用原則或結束模組的 progID (登錄子機碼名稱) 。

- **registryvaluename**會使用登錄值名稱， (用 `Name*` 來) 前置詞相符。

- **值**使用新的數值、字串或日期登錄值或檔案名。 如果數值開頭為 `+` 或，則 `-` 會在現有的登錄值中設定或清除在新值中指定的位。

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>備註

- 如果字串值的開頭為 `+` 或 `-` ，且現有的值為 `REG_MULTI_SZ` 值，則會在現有的登錄值中加入或移除字串。 若要強制建立 `REG_MULTI_SZ` 值，請將新增 `\n` 至字串值的結尾。

- 如果值以開頭 `\@` ，則值的其餘部分會是包含二進位值之十六進位文字表示的檔案名。 如果未參考有效的檔案，則會改為將其剖析為 `[Date][+|-][dd:hh]` 選擇性的日期加或減去選擇性的日和小時。 如果同時指定這兩者，請使用加號 (+) 或負號 ( ) 分隔符號。 `now+dd:hh`針對與目前時間相關的日期使用。

- 用 `chain\chaincacheresyncfiletime \@now` 來有效地清除快取的 crl。

### <a name="-importkms"></a>-importKMS

將使用者金鑰和憑證匯入至伺服器資料庫，以進行金鑰保存。

```
certutil [options] -importKMS userkeyandcertfile [certID]
```

其中：

- **userkeyandcertfile**是一個資料檔案，其中包含要封存的使用者私密金鑰和憑證。 這個檔案可以是：

  -  (KMS) 匯出檔案的 Exchange 金鑰管理伺服器。

  - PFX 檔案。

- certID 是一項 KMS 匯出檔案解密憑證比對權杖。 如需詳細資訊，請參閱本文 `-store` 中的參數。

- `-f`匯入不是由憑證授權單位單位所發行的憑證。

```
[-f] [-silent] [-split] [-config Machine\CAName] [-p password] [-symkeyalg symmetrickeyalgorithm[,keylength]]
```

### <a name="-importcert"></a>-importcert

將憑證檔案匯入資料庫。

```
certutil [options] -importcert certfile [existingrow]
```

其中：

- **existingrow**會匯入憑證，以取代相同金鑰的暫止要求。

- `-f`匯入不是由憑證授權單位單位所發行的憑證。

```
[-f] [-config Machine\CAName]
```

#### <a name="remarks"></a>備註

憑證授權單位單位也可能需要設定為支援外部憑證。 若要這麼做，請輸入 `import - certutil -setreg ca\KRAFlags +KRAF_ENABLEFOREIGN` 。

### <a name="-getkey"></a>-getkey

抓取封存的私密金鑰修復 blob、產生復原腳本，或復原封存的金鑰。

```
certutil [options] -getkey searchtoken [recoverybloboutfile]
certutil [options] -getkey searchtoken script outputscriptfile
certutil [options] -getkey searchtoken retrieve | recover outputfilebasename
```

其中：

- 如果找到多個相符的復原候選項目，或者如果找不到指定的輸出檔) ，**腳本**會產生腳本來抓取和復原金鑰 (預設行為。

- 如果只找到一個符合的復原候選，則抓取會抓取一或多個金鑰修復 Blob (預設行為，而且如果) 指定輸出檔案，則會**取得**。 使用此選項會截斷任何延伸模組，並為每個金鑰修復 blob 附加憑證特定的字串和 rec 副檔名。  每個檔案都包含憑證鏈和相關聯的私密金鑰，但仍加密為一或多個金鑰復原代理憑證。

- **復原** (需要) 金鑰復原代理憑證和私密金鑰，以在一個步驟中抓取和復原私密金鑰。 使用此選項會截斷任何延伸模組，並附加 p12 副檔名。  每個檔案都包含已復原的憑證鏈和相關聯的私密金鑰，並儲存為 PFX 檔案。

- **searchtoken**會選取要復原的金鑰和憑證，包括：

  - 1. 憑證一般名稱

  - 2. 憑證序號

  - 3. 憑證 SHA-1 雜湊 (指紋) 

  - 4. 憑證 KeyId SHA-1 雜湊 (主體金鑰識別碼) 

  - 5. 要求者名稱 (domain\user) 

  - 6. UPN (使用者 \@ 網域) 

- **recoverybloboutfile**會使用憑證鏈和相關聯的私密金鑰來輸出檔案，仍然加密為一或多個金鑰復原代理憑證。

- **outputscriptfile**會使用批次腳本輸出檔案，以抓取和復原私密金鑰。

- **outputfilebasename**會輸出檔案基底名稱。

```
[-f] [-unicodetext] [-silent] [-config Machine\CAName] [-p password] [-protectto SAMnameandSIDlist] [-csp provider]
```

### <a name="-recoverkey"></a>-recoverkey

復原已封存的私密金鑰。

```
certutil [options] -recoverkey recoveryblobinfile [PFXoutfile [recipientindex]]
```

```
[-f] [-user] [-silent] [-split] [-p password] [-protectto SAMnameandSIDlist] [-csp provider] [-t timeout]
```

### <a name="-mergepfx"></a>-mergePFX

合併 PFX 檔案。

```
certutil [options] -mergePFX PFXinfilelist PFXoutfile [extendedproperties]
```

其中：

- **PFXinfilelist**是 PFX 輸入檔的逗號分隔清單。

- **PFXoutfile**是 PFX 輸出檔案的名稱。

- **extendedproperties**包含任何擴充屬性。

```
[-f] [-user] [-split] [-p password] [-protectto SAMnameAndSIDlist] [-csp provider]
```

#### <a name="remarks"></a>備註

- 在命令列上指定的密碼必須是以逗號分隔的密碼清單。

- 如果指定了一個以上的密碼，則會將最後一個密碼用於輸出檔案。 如果只提供一個密碼，或最後一個密碼為 `*` ，則會提示使用者輸入輸出檔案密碼。

### <a name="-convertepf"></a>-convertEPF

將 PFX 檔案轉換為 EPF 檔案。

```
certutil [options] -convertEPF PFXinfilelist PFXoutfile [cast | cast-] [V3CAcertID][,salt]
```

其中：


- **PFXinfilelist**是 PFX 輸入檔的逗號分隔清單。

- **PFXoutfile**是 PFX 輸出檔案的名稱。

- **EPF**是 EPF 輸出檔的名稱。

- **cast**會使用 cast 64 加密。

- **cast-** 使用 cast 64 加密 (匯出) 

- **V3CAcertID**是 V3 CA 憑證對應 token。 如需詳細資訊，請參閱本文 `-store` 中的參數。

- **salt**是 EPF 輸出檔 salt 字串。

```
[-f] [-silent] [-split] [-dc DCName] [-p password] [-csp provider]
```

#### <a name="remarks"></a>備註

- 在命令列上指定的密碼必須是以逗號分隔的密碼清單。

- 如果指定了一個以上的密碼，則會將最後一個密碼用於輸出檔案。 如果只提供一個密碼，或最後一個密碼為 `*` ，則會提示使用者輸入輸出檔案密碼。

### <a name="-"></a>-?

顯示參數的清單。

```
certutil -?
certutil <name_of_parameter> -?
certutil -? -v
```

其中：

- **-?** 顯示完整的參數清單

- **-`<name_of_parameter>` -?** 顯示指定之參數的說明內容。

- **-?-v**會顯示完整的參數和選項清單。

## <a name="options"></a>選項。

此區段會根據命令定義您可以指定的所有選項。 每個參數都包含哪些選項可供使用的相關資訊。

| 選項。 | 描述 |
| ------- | ----------- |
| -nullsign | 使用資料的雜湊做為簽章。 |
| -f | 強制覆寫。 |
| -enterprise | 使用本機電腦的 enterprise registry 憑證存放區。 |
| -使用者 | 使用 HKEY_CURRENT_USER 金鑰或憑證存放區。 |
| -GroupPolicy | 使用 [群組原則] 憑證存放區。 |
| -ut | 顯示使用者範本。 |
| -mt | 顯示電腦範本。 |
| -Unicode | 以 Unicode 撰寫重新導向的輸出。 |
| -UnicodeText | 以 Unicode 寫入輸出檔。 |
| -gmt | 使用 GMT 顯示時間。 |
| -秒 | 以秒和毫秒為單位顯示時間。 |
| -silent | 使用 `silent` 旗標來取得 crypt 內容。 |
| -split | 分割內嵌的 asn.1 元素，並儲存至檔案。 |
| -v | 提供更詳細的 (詳細資訊) 資訊。 |
| -privatekey | 顯示密碼和私密金鑰資料。 |
| -pin 釘選 | 智慧卡 PIN。 |
| -urlfetch verify | 取得並驗證 AIA 憑證和 CDP Crl。 |
| -config Machine\CAName | 憑證授權單位單位和電腦名稱稱字串。 |
| -policyserver URLorID | 原則伺服器 URL 或識別碼。 針對 [選取] U/I，請使用 `-policyserver` 。 針對所有原則伺服器，使用`-policyserver *`|
| -匿名 | 使用匿名 SSL 認證。 |
| -kerberos | 使用 Kerberos SSL 認證。 |
| -clientcertificate clientcertID | 使用 x.509 憑證 SSL 認證。 針對 [選取] U/I，請使用 `-clientcertificate` 。 |
| -username 使用者名稱 | 使用命名帳戶作為 SSL 認證。 針對 [選取] U/I，請使用 `-username` 。 |
| -cert certID | 簽署憑證。 |
| -dc DCName | 以特定網域控制站為目標。 |
| -限制 restrictionlist | 以逗號分隔的限制清單。 每個限制都包含一個資料行名稱、一個關聯式運算子和一個常數整數、字串或日期。 一個資料行名稱前面可能會加上加號或減號，以指出排序次序。 例如：`requestID = 47`、`+requestername >= a, requestername` 或 `-requestername > DOMAIN, Disposition = 21` |
| -out columnlist | 以逗號分隔的資料行清單。 |
| -p 密碼 | 密碼 |
| -protectto SAMnameandSIDlist | 以逗號分隔的 SAM 名稱/SID 清單。 |
| -csp 提供者 | 提供者 |
| -t timeout | URL 提取超時（以毫秒為單位）。 |
| -symkeyalg symmetrickeyalgorithm [，keylength] | 具有選擇性金鑰長度之對稱金鑰演算法的名稱。 例如：`AES,128` 或 `3DES` |

### <a name="additional-references"></a>其他參考資料

如需有關如何使用此命令的其他範例，請參閱

- [Active Directory 憑證服務 (AD CS)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))

- [管理憑證的 Certutil 工作](/previous-versions/orphan-topics/ws.10/cc772898(v=ws.10))

- [certutil 命令](certutil.md)