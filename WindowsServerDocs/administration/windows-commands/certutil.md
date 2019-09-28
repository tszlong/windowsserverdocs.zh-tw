---
title: certutil
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c9946cc53fe3a901c3f6ee53f082a5b3d086c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379647"
---
# <a name="certutil"></a>certutil

Certutil 是安裝為憑證服務一部分的命令列程式。 您可以使用 Certutil 來傾印和顯示憑證授權單位單位 (CA) 設定資訊、設定憑證服務、備份和還原 CA 元件, 以及驗證憑證、金鑰組和憑證鏈。

當 certutil 在沒有其他參數的憑證授權單位單位上執行時, 它會顯示目前的憑證授權單位單位設定。 在非憑證授權單位單位上執行 cerutil 時, 此命令預設為執行 certutil [-](#-dump)傾印動詞。

> [!WARNING]
> 較早版本的 certutil 可能無法提供本檔中所述的所有選項。 您可以藉由執行[語法標記法](#syntax-notations)一節中所示的命令, 查看特定版本的 certutil 所提供的所有選項。

## <a name="menu"></a>功能表

本檔的主要章節包括:

- [之外](#verbs)
- [語法標記法](#syntax-notations)
- [選項](#options)
- [其他 certutil 範例](#additional-certutil-examples)

## <a name="verbs"></a>動詞

下表描述可搭配 certutil 命令使用的動詞。

|動詞|描述|
|-----|-----------|
|[-傾印](#-dump)|傾印設定資訊或檔案|
|[-asn](#-asn)|剖析 ASN 1 檔案|
|[-decodehex](#-decodehex)|解碼十六進位編碼的檔案|
|[-解碼](#-decode)|將 Base64 編碼的檔案解碼|
|[-編碼](#-encode)|將檔案編碼為 Base64|
|[-拒絕](#-deny)|拒絕擱置中的憑證要求|
|[-重新提交](#-resubmit)|重新提交擱置中的憑證要求|
|[-setattributes](#-setattributes)|設定擱置中憑證要求的屬性|
|[-setextension](#-setextension)|設定擱置中憑證要求的延伸模組|
|[-revoke](#-revoke)|撤銷憑證|
|[-isvalid](#-isvalid)|顯示目前憑證的處置|
|[-getconfig](#-getconfig)|取得預設設定字串|
|[-ping](#-ping)|嘗試聯絡 Active Directory 憑證服務要求介面|
|-pingadmin|嘗試聯絡 Active Directory 憑證服務管理介面|
|[-CAInfo](#-cainfo)|顯示憑證授權單位單位的相關資訊|
|[-ca cert](#-cacert)|取得憑證授權單位單位的憑證|
|[-ca. 鏈](#-cachain)|取得憑證授權單位單位的憑證鏈|
|[-GetCRL](#-getcrl)|取得憑證撤銷清單 (CRL)|
|[-CRL](#-crl)|發佈新的憑證撤銷清單（Crl） [或僅限 delta Crl]|
|[-關機](#-shutdown)|關閉 Active Directory 憑證服務|
|[-installCert](#-installcert)|安裝憑證授權單位單位憑證|
|[-renewCert](#-renewcert)|更新憑證授權單位單位憑證|
|[-架構](#-schema)|傾印憑證的架構|
|[-view](#-view)|傾印憑證視圖|
|[-db](#-db)|傾印原始資料庫|
|[-deleterow](#-deleterow)|從伺服器資料庫刪除資料列|
|[-備份](#-backup)|備份 Active Directory 憑證服務|
|[-backupDB](#-backupdb)|備份 Active Directory 憑證服務資料庫|
|[-backupKey](#-backupkey)|備份憑證服務憑證和私密金鑰 Active Directory|
|[-restore](#-restore)|還原 Active Directory 憑證服務|
|[-restoreDB](#-restoredb)|還原 Active Directory 憑證服務資料庫|
|[-restoreKey](#-restorekey)|還原 Active Directory 憑證服務憑證和私密金鑰|
|[-importPFX](#-importpfx)|匯入憑證和私密金鑰|
|[-dynamicfilelist](#-dynamicfilelist)|顯示動態檔案清單|
|[-databaselocations](#-databaselocations)|顯示資料庫位置|
|[-hashfile](#-hashfile)|產生並顯示檔案上的密碼編譯雜湊|
|[-store](#-store)|傾印證書存儲|
|[-addstore](#-addstore)|將憑證新增至存放區|
|[-delstore](#-delstore)|從存放區刪除憑證|
|[-verifystore](#-verifystore)|驗證存放區中的憑證|
|[-repairstore](#-repairstore)|修復金鑰關聯或更新憑證屬性或金鑰安全描述項|
|[-viewstore](#-viewstore)|傾印證書存儲|
|[-viewdelstore](#-viewdelstore)|從存放區刪除憑證|
|[-dsPublish](#-dspublish)|將憑證或憑證撤銷清單 (CRL) 發佈至 Active Directory|
|[-ADTemplate](#-adtemplate)|顯示 AD 範本|
|[-範本](#-template)|顯示憑證範本|
|[-TemplateCAs](#-templatecas)|顯示憑證範本的憑證授權單位單位 (Ca)|
|[-之 catemplates.txt 檔](#-catemplates)|顯示 CA 的範本|
|[-SetCASites](#-setcasites)|管理 Ca 的網站名稱|
|[-enrollmentServerURL](#-enrollmentserverurl)|顯示、新增或刪除與 CA 相關聯的註冊伺服器 Url|
|[-ADCA](#-adca)|顯示 AD Ca|
|[-CA](#-ca)|顯示註冊原則 Ca|
|[-原則](#-policy)|顯示註冊原則|
|[-PolicyCache](#-policycache)|顯示或刪除註冊原則快取專案|
|[-CredStore](#-credstore)|顯示、新增或刪除認證存放區專案|
|[-Installdefaulttemplates 動作](#-installdefaulttemplates)|安裝預設憑證範本|
|[-URLCache](#-urlcache)|顯示或刪除 URL 快取專案|
|[-脈衝](#-pulse)|脈衝自動註冊事件|
|[-MachineInfo](#-machineinfo)|顯示 Active Directory 機物件的相關資訊|
|[-DCInfo](#-dcinfo)|顯示網域控制站的相關資訊|
|[-EntInfo](#-entinfo)|顯示企業 CA 的相關資訊|
|[-TCAInfo](#-tcainfo)|顯示 CA 的相關資訊|
|[-SCInfo](#-scinfo)|顯示智慧卡的相關資訊|
|[-SCRoots](#-scroots)|管理智慧卡的根憑證|
|[-verifykeys](#-verifykeys)|驗證公用或私用金鑰集|
|[-驗證](#-verify)|驗證憑證、憑證撤銷清單 (CRL) 或憑證連結|
|[-verifyCTL](#-verifyctl)|確認 AuthRoot 或不允許的憑證 CTL|
|[-sign](#-sign)|重新簽署憑證撤銷清單 (CRL) 或憑證|
|[-vroot](#-vroot)|建立或刪除 web 虛擬根目錄和檔案共用|
|[-vocsproot](#-vocsproot)|建立或刪除 OCSP Web Proxy 的 web 虛擬根目錄|
|[-addEnrollmentServer](#-addenrollmentserver)|新增註冊伺服器應用程式|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|刪除註冊伺服器應用程式|
|[-addPolicyServer](#-addpolicyserver)|新增原則伺服器應用程式|
|[-deletePolicyServer](#-deletepolicyserver)|刪除原則伺服器應用程式|
|[-oid](#-oid)|顯示物件識別碼或設定顯示名稱|
|[-錯誤](#-error)|顯示與錯誤代碼相關聯的郵件內文|
|[-getreg](#-getreg)|顯示登錄值|
|[-setreg](#-setreg)|設定登錄值|
|[-delreg](#-delreg)|刪除登錄值|
|[-ImportKMS](#-importkms)|將使用者金鑰和憑證匯入至伺服器資料庫以進行金鑰保存|
|[-ImportCert](#-importcert)|將憑證檔案匯入資料庫|
|[-GetKey](#-getkey)|取得封存的私密金鑰復原 blob|
|[-RecoverKey](#-recoverkey)|復原封存的私密金鑰|
|[-MergePFX](#-mergepfx)|合併 PFX 檔案|
|[-ConvertEPF](#-convertepf)|將 PFX 檔案轉換為 EPF 檔案|
|-?|顯示指令動詞的清單|
|-動詞 >-？ *\<*|顯示指定之動詞命令的說明。|
|-? -v|顯示動詞和的完整清單|

返回[功能表](#menu)

## <a name="syntax-notations"></a>語法標記法

- 如需基本命令列語法, 請執行`certutil -?`
- 如需搭配特定動詞使用 certutil 的語法, 請執行**certutil**  *\<動詞 >* **-？**
- 若要將所有 certutil 語法傳送到文字檔, 請執行下列命令:  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

下表描述用來表示命令列語法的標記法。


|            表示法             |                  描述                  |
|---------------------------------|-----------------------------------------------|
| 不含括弧或大括弧的文字 |         您必須輸入的專案, 如下所示          |
|  \<角括弧內的文字 >  | 您必須為其提供值的預留位置 |
|  [括弧內的文字]  |                選擇性的項目                 |
|      {大括弧內的文字}       |       必要專案集;選擇一個       |
|         分隔號 (          |                       )                       |
|          省略號 (...)           |          可以重複的專案           |

返回[功能表](#menu)

## <a name="-dump"></a>-傾印

CertUtil [Options] [-dump]

CertUtil [Options] [-dump] File

傾印設定資訊或檔案

[-f][-無訊息][-split][-p 密碼][-t Timeout]

返回[功能表](#menu)

## <a name="-asn"></a>-asn

CertUtil [Options]-asn 檔案 [類型]

剖析 ASN 1 檔案

類型: 數值 CRYPT\_字串\_ \*解碼類型

返回[功能表](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [Options]-decodehex InFile OutFile [type]

類型: 數值 CRYPT\_字串\_ \*編碼類型

[-f]

返回[功能表](#menu)

## <a name="-decode"></a>-解碼

CertUtil [Options]-解碼 InFile OutFile

解碼 Base64 編碼的檔案

[-f]

返回[功能表](#menu)

## <a name="-encode"></a>-編碼

CertUtil [Options]-編碼 InFile OutFile

將檔案編碼為 Base64

[-f][-UnicodeText]

返回[功能表](#menu)

## <a name="-deny"></a>-拒絕

CertUtil [Options]-拒絕 RequestId

拒絕擱置要求

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-resubmit"></a>-重新提交

CertUtil [Options]-重新提交 RequestId

重新提交擱置中的要求

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-setattributes"></a>-setattributes

CertUtil [Options]-setattributes RequestId AttributeString

設定待決要求的屬性

RequestId--待決要求的數值要求識別碼

AttributeString--要求屬性名稱和值配對

- 名稱和值是以冒號分隔。
- 多個名稱, 值組會以分行符號分隔。
- 範例: "CertificateTemplate:User\nEMail:User@Domain.com"
- 每個 "\n" 序列都會轉換成分行符號分隔符號。

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [Options]-setextension RequestId ExtensionName 旗標 {Long |日期 |字串 |\@InFile}

設定待決要求的延伸模組

RequestId--待決要求的數值要求識別碼

ExtensionName--延伸模組的 ObjectId 字串

旗標--建議使用0。  1使延伸模組變成關鍵性, 2 會停用它, 3 則會執行這兩項工作。

如果最後一個參數是數值, 則會將其視為 Long。

如果可以剖析為日期, 則會將它視為日期。

如果開頭為 '\@', 則 token 的其餘部分會是包含二進位資料或 ascii 文字十六進位傾印的檔案名。

任何其他專案都會以字串的形式取得。

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-revoke"></a>-revoke

CertUtil [Options]-revoke SerialNumber [Reason]

撤銷憑證

SerialNumber要撤銷的憑證序號清單 (以逗號分隔)

原因: 數位或符號撤銷原因

- 0CRL_REASON_UNSPECIFIED:未指定 (預設值)
- 1：CRL_REASON_KEY_COMPROMISE:金鑰洩露
- 2：CRL_REASON_CA_COMPROMISE:CA 洩露
- 3：CRL_REASON_AFFILIATION_CHANGED:關係已變更
- 4:30CRL_REASON_SUPERSEDED:已取代
- 5:40CRL_REASON_CESSATION_OF_OPERATION:作業哈
- 7CRL_REASON_CERTIFICATE_HOLD:憑證保存
- 8CRL_REASON_REMOVE_FROM_CRL:從 CRL 移除
- -1：解除撤銷解除撤銷

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-isvalid"></a>-isvalid

CertUtil [Options]-isvalid SerialNumber |CertHash

顯示目前的憑證配置

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-getconfig"></a>-getconfig

CertUtil [選項]-getconfig

取得預設設定字串

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-ping"></a>-ping

CertUtil [Options]-ping [MaxSecondsToWait |CAMachineList]

Ping Active Directory 憑證服務要求介面

CAMachineList--逗號分隔的 CA 電腦名稱稱清單

1. 若為單一電腦, 請使用結尾的逗號
2. 顯示每部 CA 電腦的網站成本

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [選項]-CAInfo [InfoName [索引 |ErrorCode]]

顯示 CA 資訊

InfoName--指出要顯示的 CA 屬性 (請參閱下文)。 針對所有\*屬性使用 ""。

索引--選擇性的以零為基底的屬性索引

ErrorCode--數值錯誤碼

[-f][-split][-config Machine\CAName]

InfoName 引數語法:

- 文字檔檔案版本
- 基礎產品版本
- exitcount:結束模組計數
- exit [Index]：結束模組描述
- 策略原則模組描述
- 檔案名CA 名稱
- sanitizedname:已淨化的 CA 名稱
- dsname已淨化的 CA 簡短名稱 (DS 名稱)
- sharedfolder:共用資料夾
- error1 錯誤碼:錯誤訊息正文
- error2 相關說明錯誤碼:錯誤訊息正文和錯誤代碼
- 型CA 類型
- 資訊CA 資訊
- 父父 CA
- certcount:CA 憑證計數
- xchgcount:CA 交換憑證計數
- kracount:KRA 憑證計數
- kraused:KRA 憑證已使用計數
- propidmax:CA PropId 上限
- certstate [Index]：CA 憑證
- certversion [Index]：CA 憑證版本
- certstatuscode [Index]：CA 憑證驗證狀態
- crlstate [Index]：.CRL
- krastate [Index]：KRA 憑證
- crossstate + [Index]：正向交叉憑證
- crossstate-[Index]：反向交叉憑證
- cert [Index]：CA 憑證
- certchain [Index]：CA 憑證鏈
- certcrlchain [Index]：具有 Crl 的 CA 憑證鏈
- xchg [Index]：CA 交換憑證
- xchgchain [Index]：CA exchange 憑證鏈
- xchgcrlchain [Index]：CA exchange 憑證鏈與 Crl
- kra [Index]：KRA 憑證
- cross + [Index]：正向交叉憑證
- 交叉-[索引]：反向交叉憑證
- CRL [索引]：基底 CRL
- deltacrl [Index]：Delta CRL
- crlstatus [Index]：CRL 發行狀態
- deltacrlstatus [Index]：Delta CRL 發佈狀態
- dnsDNS 名稱
- 角色角色分離
- 公佈Advanced Server
- 範本範本
- csp [索引]：OCSP Url
- aia [索引]：AIA Url
- cdp [Index]：CDP Url
- localenameCA 地區設定名稱
- subjecttemplateoids:主旨範本 Oid

返回[功能表](#menu)

## <a name="-cacert"></a>-ca cert

CertUtil [選項]-ca. cert OutCACertFile [索引]

取得 CA 的憑證

OutCACertFile: 輸出檔

指數CA 憑證更新索引 (預設為最新)

[-f][-split][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-cachain"></a>-ca. 鏈

CertUtil [Options]-ca. 鏈 OutCACertChainFile [Index]

取出 CA 的憑證鏈

OutCACertChainFile: 輸出檔

指數CA 憑證更新索引 (預設為最新)

[-f][-split][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [Options]-GetCRL OutFile [Index] [delta]

取得 CRL

指數CRL 索引或金鑰索引 (預設為最新金鑰的 CRL)

差異: delta CRL (預設值為基底 CRL)

[-f][-split][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-crl"></a>-CRL

CertUtil [Options]-CRL [dd： hh | 重新發佈] [delta]

發行新的 Crl [或僅限 delta Crl]

dd: hh--新的 CRL 有效期間 (天和小時)

重新發佈--重新發佈最新的 Crl

差異--僅限 delta Crl (預設值為基底和 delta Crl)

[-split][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-shutdown"></a>-關機

CertUtil [選項]-關閉

關閉 Active Directory 憑證服務

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [Options]-installCert [CACertFile]

安裝憑證授權單位單位憑證

[-f][-無訊息][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [Options]-renewCert [ReuseKeys] [Machine\ParentCAName]

更新憑證授權單位單位憑證

使用-f 可忽略未處理的更新要求, 並產生新的要求。

[-f][-無訊息][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-schema"></a>-架構

CertUtil [選項]-架構 [Ext |Attrib |.CRL

傾印憑證架構

預設為要求和憑證資料表

分機延伸模組資料表

Attrib屬性資料表

.CRLCRL 資料表

[-split][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-view"></a>-view

CertUtil [選項]-view [佇列 |記錄 |LogFail |已撤銷 |Ext |Attrib |CRL] [csv]

傾印憑證視圖

佇列:要求佇列

記錄檔：已發行或撤銷的憑證, 加上失敗的要求

LogFail:失敗的要求

#A1撤銷的憑證

分機延伸模組資料表

Attrib屬性資料表

.CRLCRL 資料表

csv以逗號分隔值輸出

若要顯示所有專案的 StatusCode 資料行:-out StatusCode

若要顯示最後一個專案的所有資料行:-restrict "RequestId = = $"

若要顯示三個要求的 requestid 和配置:-限制 "RequestId > = 37\<, RequestId 40"-out "RequestId, 處置"

顯示所有基底 Crl 的資料列識別碼和 CRL 號碼:-限制 "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

顯示基底 CRL 號碼 3:-v-限制 "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

若要顯示整個 CRL 資料表:.CRL

針對日期限制使用 "Date [+ |-dd： hh]"

針對相對於目前時間的日期使用 "now + dd: hh"

[-無訊息][-split][-config Machine\CAName][-限制 RestrictionList][-out ColumnList]

返回[功能表](#menu)

## <a name="-db"></a>-db

CertUtil [Options]-db

傾印原始資料庫

[-config Machine\CAName][-限制 RestrictionList][-out ColumnList]

返回[功能表](#menu)

## <a name="-deleterow"></a>-deleterow

CertUtil [選項]-deleterow RowId |日期 [要求 |Cert |Ext |Attrib |.CRL

刪除伺服器資料庫資料列

邀請失敗和待決的要求 (提交日期)

Cert:過期和撤銷的憑證 (到期日)

分機延伸模組資料表

Attrib屬性資料表

.CRLCRL 資料表 (到期日)

若要刪除2001年1月22日提交的失敗和擱置要求:1/22/2001 要求

若要刪除2001年1月22日過期的所有憑證:1/22/2001 憑證

刪除 RequestId 37 的憑證資料列、屬性和延伸模組:37

若要刪除2001年1月22日到期的 Crl:1/22/2001 CRL

[-f][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-backup"></a>-備份

CertUtil [選項]-備份 BackupDirectory [增量] [KeepLog]

備份 Active Directory 憑證服務

BackupDirectory: 用來儲存備份資料的目錄

增量: 僅執行增量備份 (預設為完整備份)

KeepLog: 保留資料庫記錄檔 (預設為截斷記錄檔)

[-f][-config Machine\CAName][-p 密碼]

返回[功能表](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [Options]-backupDB BackupDirectory [增量] [KeepLog]

備份 Active Directory 憑證服務資料庫

BackupDirectory: 用來儲存備份資料庫檔案的目錄

增量: 僅執行增量備份 (預設為完整備份)

KeepLog: 保留資料庫記錄檔 (預設為截斷記錄檔)

[-f][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [Options]-backupKey BackupDirectory

備份 Active Directory 憑證服務憑證和私密金鑰

BackupDirectory: 用來儲存備份 PFX 檔案的目錄

[-f][-config Machine\CAName][-p 密碼][-t Timeout]

返回[功能表](#menu)

## <a name="-restore"></a>-restore

CertUtil [Options]-restore BackupDirectory

還原 Active Directory 憑證服務

BackupDirectory: 包含要還原之資料的目錄

[-f][-config Machine\CAName][-p 密碼]

返回[功能表](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [Options]-restoreDB BackupDirectory

還原 Active Directory 憑證服務資料庫

BackupDirectory: 包含要還原之資料庫檔案的目錄

[-f][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [Options]-restoreKey BackupDirectory |PFXFile

還原 Active Directory 憑證服務憑證和私密金鑰

BackupDirectory: 包含要還原之 PFX 檔案的目錄

PFXFile:要還原的 PFX 檔案

[-f][-config Machine\CAName][-p 密碼]

返回[功能表](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [Options]-importPFX [CertificateStoreName] PFXFile [修飾詞]

匯入憑證和私密金鑰

CertificateStoreName憑證存放區名稱。  請參閱[-store](#-store)。

PFXFile:要匯入的 PFX 檔案

修改下列一或多個以逗號分隔的清單:

1. AT_SIGNATURE:將 KeySpec 變更為 Signature
2. AT_KEYEXCHANGE:將 KeySpec 變更為金鑰交換
3. NoExport:使私密金鑰無法匯出
4. NoCert:不要匯入憑證
5. NoChain:不要匯入憑證鏈
6. NoRoot:不要匯入根憑證
7. 禁止使用密碼保護金鑰
8. NoProtect:不要密碼保護金鑰

預設為個人電腦存放區。

[-f][-使用者][-p 密碼][-csp 提供者]

返回[功能表](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [選項]-dynamicfilelist

顯示動態檔案清單

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [選項]-databaselocations

顯示資料庫位置

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [Options]-hashfile InFile [HashAlgorithm]

產生並顯示檔案上的密碼編譯雜湊

返回[功能表](#menu)

## <a name="-store"></a>-store

CertUtil [Options]-store [CertificateStoreName [CertId [OutputFile]]]

傾印證書存儲

CertificateStoreName憑證存放區名稱。 例如：

- 「我的」、「CA」 (預設值)、「根」、
- "ldap:///CN=Certification 機關, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ one？ objectClass = Microsoft-windows-certificationauthority" (查看根憑證)
- "ldap:///CN=CAName,CN=Certification 機關, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = Microsoft-windows-certificationauthority" (修改根憑證)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ t？ base？ objectClass = cRLDistributionPoint" (View Crl)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = Microsoft-windows-certificationauthority" (企業 CA 憑證)
- ldap(AD 電腦物件憑證)
- -使用者 ldap:(AD 使用者物件憑證)

CertId憑證或 CRL 符合 token。  這可以是序號、SHA-1 憑證、CRL、CTL 或公開金鑰雜湊、數值憑證索引 (0、1等等)、數值 CRL 索引 (.0、.1 等等)、數值 CTL 索引 (.)。0、。1等等)、公用金鑰、簽章或延伸 ObjectId、憑證主體一般名稱、電子郵件地址、UPN 或 DNS 名稱、金鑰容器名稱或 CSP 名稱、範本名稱或 ObjectId、EKU 或應用程式原則 ObjectId, 或 CRL 簽發者一般名稱。 其中有許多可能會導致多個相符專案。

OutputFile: 用來儲存相符憑證的檔案

使用-user 存取使用者存放區, 而不是電腦存放區。

使用-enterprise 來存取機器企業商店。

使用-服務來存取機器服務存放區。

使用-grouppolicy 存取電腦群組策略存放區。

例如：

- -enterprise NTAuth
- -企業根37
- -user My 26e0aaaf000000000004
- CA 11

[-f][-enterprise][-使用者][-GroupPolicy][-無訊息][-split][-dc DCName]

返回[功能表](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [Options]-addstore CertificateStoreName InFile

將憑證新增至存放區

CertificateStoreName憑證存放區名稱。  請參閱[-store](#-store)。

InFile要新增至存放區的憑證或 CRL 檔案。

[-f][-enterprise][-使用者][-GroupPolicy][-dc DCName]

返回[功能表](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [Options]-delstore CertificateStoreName CertId

從存放區刪除憑證

CertificateStoreName憑證存放區名稱。  請參閱[-store](#-store)。

CertId憑證或 CRL 符合 token。  請參閱[-store](#-store)。

[-enterprise][-使用者][-GroupPolicy][-dc DCName]

返回[功能表](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [Options]-verifystore CertificateStoreName [CertId]

驗證存放區中的憑證

CertificateStoreName憑證存放區名稱。  請參閱[-store](#-store)。

CertId憑證或 CRL 符合 token。  請參閱[-store](#-store)。

[-enterprise][-使用者][-GroupPolicy][-無訊息][-split][-dc DCName][-t Timeout]

返回[功能表](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [Options]-repairstore CertificateStoreName CertIdList [PropertyInfFile |SDDLSecurityDescriptor]

修復金鑰關聯或更新憑證屬性或金鑰安全描述項

CertificateStoreName憑證存放區名稱。  請參閱[-store](#-store)。

CertIdList: 以逗號分隔的憑證或 CRL 符合權杖清單。 請參閱[-store](#-store) CertId description。

PropertyInfFile--包含外部屬性的 INF 檔案:

```
[Properties]
     19 = Empty ; Add archived property, OR:
     19 =       ; Remove archived property

     11 = "{text}Friendly Name" ; Add friendly name property

     127 = "{hex}" ; Add custom hexadecimal property
         _continue_ = "00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f"
         _continue_ = "10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f"

     2 = "{text}" ; Add Key Provider Information property
       _continue_ = "Container=Container Name&"
       _continue_ = "Provider=Microsoft Strong Cryptographic Provider&"
       _continue_ = "ProviderType=1&"
       _continue_ = "Flags=0&"
       _continue_ = "KeySpec=2"

     9 = "{text}" ; Add Enhanced Key Usage property
       _continue_ = "1.3.6.1.5.5.7.3.2,"
       _continue_ = "1.3.6.1.5.5.7.3.1,"
```

[-f][-enterprise][-使用者][-GroupPolicy][-無訊息][-split][-csp 提供者]

返回[功能表](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [Options]-viewstore [CertificateStoreName [CertId [OutputFile]]]

傾印證書存儲

CertificateStoreName憑證存放區名稱。 例如：

- 「我的」、「CA」 (預設值)、「根」、
- "ldap:///CN=Certification 機關, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ one？ objectClass = Microsoft-windows-certificationauthority" (查看根憑證)
- "ldap:///CN=CAName,CN=Certification 機關, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = Microsoft-windows-certificationauthority" (修改根憑證)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ t？ base？ objectClass = cRLDistributionPoint" (View Crl)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = Microsoft-windows-certificationauthority" (企業 CA 憑證)
- ldap(AD 電腦物件憑證)
- -使用者 ldap:(AD 使用者物件憑證)

CertId憑證或 CRL 符合 token。 這可以是序號、SHA-1 憑證、CRL、CTL 或公開金鑰雜湊、數值憑證索引 (0、1等等)、數值 CRL 索引 (.0、.1 等等)、數值 CTL 索引 (.)。0、。1等等)、公用金鑰、簽章或延伸 ObjectId、憑證主體一般名稱、電子郵件地址、UPN 或 DNS 名稱、金鑰容器名稱或 CSP 名稱、範本名稱或 ObjectId、EKU 或應用程式原則 ObjectId, 或 CRL 簽發者一般名稱。 其中有許多可能會導致多個相符專案。

OutputFile: 用來儲存相符憑證的檔案

使用-user 存取使用者存放區, 而不是電腦存放區。

使用-enterprise 來存取機器企業商店。

使用-服務來存取機器服務存放區。

使用-grouppolicy 存取電腦群組策略存放區。

例如：

1. -enterprise NTAuth
2. -企業根37
3. -user My 26e0aaaf000000000004
4. CA 11

[-f][-enterprise][-使用者][-GroupPolicy][-dc DCName]

返回[功能表](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [Options]-viewdelstore [CertificateStoreName [CertId [OutputFile]]]

從存放區刪除憑證

CertificateStoreName憑證存放區名稱。 例如：

- 「我的」、「CA」 (預設值)、「根」、
- "ldap:///CN=Certification 機關, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ one？ objectClass = Microsoft-windows-certificationauthority" (查看根憑證)
- "ldap:///CN=CAName,CN=Certification 機關, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = Microsoft-windows-certificationauthority" (修改根憑證)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ t？ base？ objectClass = cRLDistributionPoint" (View Crl)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = Microsoft-windows-certificationauthority" (企業 CA 憑證)
- ldap(AD 電腦物件憑證)
- -使用者 ldap:(AD 使用者物件憑證)

CertId憑證或 CRL 符合 token。 這可以是序號、SHA-1 憑證、CRL、CTL 或公開金鑰雜湊、數值憑證索引 (0、1等等)、數值 CRL 索引 (.0、.1 等等)、數值 CTL 索引 (.)。0、。1等等)、公用金鑰、簽章或延伸 ObjectId、憑證主體一般名稱、電子郵件地址、UPN 或 DNS 名稱、金鑰容器名稱或 CSP 名稱、範本名稱或 ObjectId、EKU 或應用程式原則 ObjectId, 或 CRL 簽發者一般名稱。 其中有許多可能會導致多個相符專案。

OutputFile: 用來儲存相符憑證的檔案

使用-user 存取使用者存放區, 而不是電腦存放區。

使用-enterprise 來存取機器企業商店。

使用-服務來存取機器服務存放區。

使用-grouppolicy 存取電腦群組策略存放區。

例如：

1. -enterprise NTAuth
2. -企業根37
3. -user My 26e0aaaf000000000004
4. CA 11

[-f][-enterprise][-使用者][-GroupPolicy][-dc DCName]

返回[功能表](#menu)

## <a name="-dspublish"></a>-dsPublish

CertUtil [Options]-dsPublish CertFile [NTAuthCA |Rootca.cer |SubCA |CrossCA |KRA |使用者 |機器碼

CertUtil [Options]-dsPublish Crlfile.crl [DSCDPContainer [DSCDPCN]]

將憑證或 CRL 發佈至 Active Directory

CertFile: 要發行的憑證檔案

NTAuthCA:將憑證發佈至 DS Enterprise store

Rootca.cer將憑證發佈至 DS 受信任的根存放區

SubCA將 CA 憑證發佈至 DS CA 物件

CrossCA:將跨憑證發佈至 DS CA 物件

KRA將憑證發行至 DS 金鑰復原代理物件

使用者：將憑證發佈至使用者 DS 物件

機器碼將憑證發佈至電腦 DS 物件

Crlfile.crl要發佈的 CRL 檔案

DSCDPContainer:DS CDP 容器 CN, 通常是 CA 電腦名稱稱

DSCDPCN:DS CDP 物件 CN, 通常以已清理的 CA 簡短名稱和金鑰索引為基礎

使用-f 來建立 DS 物件。

[-f][-使用者][-dc DCName]

返回[功能表](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [Options]-ADTemplate [Template]

顯示 AD 範本

[-f][-使用者][-未通過][-mt][-dc DCName]

## <a name="-template"></a>-範本

CertUtil [Options]-Template [Template]

顯示註冊原則範本

[-f][-使用者][-無訊息][-PolicyServer URLOrId][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p 密碼]

返回[功能表](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [Options]-TemplateCAs 範本

顯示範本的 CAs

[-f][-使用者][-dc DCName]

返回[功能表](#menu)

## <a name="-catemplates"></a>-之 catemplates.txt 檔

CertUtil [Options]-之 catemplates.txt 檔 [Template]

顯示 CA 的範本

[-f][-使用者][-未通過][-mt][-config Machine\CAName][-dc DCName]

返回[功能表](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [選項]-SetCASites [set] [SiteName]

CertUtil [選項]-SetCASites 驗證 [SiteName]

CertUtil [選項]-SetCASites 刪除

設定、驗證或刪除 CA 網站名稱

- 使用-config 選項以單一 CA 為目標 (預設為所有 Ca)
- 只有在以單一 CA 為目標時, 才允許*SiteName*
- 使用-f 覆寫指定*SiteName*的驗證錯誤
- 使用-f 刪除所有 CA 網站名稱

[-f][-config Machine\CAName][-dc DCName]

> [!NOTE]
> 如需有關為 Active Directory Domain Services (AD DS) 網站感知設定 Ca 的詳細資訊, 請參閱[AD DS AD CS 和 PKI 用戶端的網站感知](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx)。

返回[功能表](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [Options]-enrollmentServerURL [URL AuthenticationType [Priority] [修飾詞]]

CertUtil [選項]-enrollmentServerURL URL 刪除

顯示、新增或刪除與 CA 相關聯的註冊伺服器 Url

AuthenticationType新增 URL 時, 請指定下列其中一個用戶端驗證方法

1. V5使用 Kerberos SSL 認證
2. UserName針對 SSL 認證使用命名帳戶
3. ClientCertificate使用 x.509 憑證 SSL 認證
4. Anonymous：使用匿名 SSL 認證

delete: 刪除與 CA 相關聯的指定 URL

優先順序: 如果在新增 URL 時未指定, 則預設為 ' 1 '

修飾詞--下列一或多個以逗號分隔的清單:

1. AllowRenewalsOnly:只有續訂要求可以透過此 URL 提交給此 CA
2. AllowKeyBasedRenewal:允許使用 AD 中沒有相關聯帳戶的憑證。 這僅適用于 ClientCertificate 和 AllowRenewalsOnly 模式

[-config Machine\CAName][-dc DCName]

返回[功能表](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [Options]-ADCA [CAName]

顯示 AD Ca

[-f][-split][-dc DCName]

返回[功能表](#menu)

## <a name="-ca"></a>-CA

CertUtil [Options]-CA [CAName |TemplateName

顯示註冊原則 Ca

[-f][-使用者][-無訊息][-split][-PolicyServer URLOrId][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p 密碼]

返回[功能表](#menu)

## <a name="-policy"></a>-原則

顯示註冊原則

[-f][-使用者][-無訊息][-split][-PolicyServer URLOrId][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p 密碼]

返回[功能表](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [選項]-PolicyCache [刪除]

顯示或刪除註冊原則快取專案

刪除: 刪除原則伺服器快取專案

-f: 使用-f 刪除所有快取專案

[-f][-使用者][-PolicyServer URLOrId]

返回[功能表](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [Options]-CredStore [URL]

CertUtil [選項]-CredStore URL 新增

CertUtil [選項]-CredStore URL 刪除

顯示、新增或刪除認證存放區專案

URL: 目標 URL。  使用\*來比對所有專案。 使用 https://machine\ * 來比對 URL 前置詞。

新增: 新增認證存放區專案。 您也必須指定 SSL 認證。

刪除: 刪除認證存放區專案

-f: 使用-f 來覆寫專案或刪除多個專案。

[-f][-使用者][-無訊息][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p 密碼]

返回[功能表](#menu)

## <a name="-installdefaulttemplates"></a>-Installdefaulttemplates 動作

CertUtil [選項]-Installdefaulttemplates 動作

安裝預設憑證範本

[-dc DCName]

返回[功能表](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [Options]-URLCache [URL |CRL |\* [delete]]

顯示或刪除 URL 快取專案

URL: 快取的 URL

CRL: 僅在所有快取的 CRL Url 上操作

\*: 在所有快取的 Url 上操作

刪除: 從目前使用者的本機快取中刪除相關的 Url

請使用-f 來強制提取特定的 URL, 並更新快取。

[-f][-split]

返回[功能表](#menu)

## <a name="-pulse"></a>-脈衝

CertUtil [Options]-脈衝

脈衝自動註冊事件

[-使用者]

返回[功能表](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [Options]-MachineInfo DomainName\MachineName $

顯示 Active Directory 電腦物件資訊

返回[功能表](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [選項]-DCInfo [網域] [驗證 |DeleteBad |DeleteAll

顯示網域控制站資訊

預設為顯示 DC 憑證而不進行驗證

[-f][-使用者][-urlfetch verify][-dc DCName][-t Timeout]

> [!TIP]
> 指定 Active Directory Domain Services （AD DS）網域 **[domain]** 以及指定網域控制站（ **-dc**）的功能已在 Windows Server 2012 中新增。 若要成功執行命令, 您必須使用屬於**Domain admins**或**Enterprise admins**成員的帳戶。 此命令的行為修改如下所示:</br>> 1。  如果未指定網域, 而且未指定特定的網域控制站, 則此選項會傳回要從預設網域控制站處理的網域控制站清單。</br>> 2。  如果未指定網域, 但指定網域控制站, 則會產生指定之網域控制站上的憑證報告。</br>> 3。  如果指定網域, 但未指定網域控制站, 則會在清單中為每個網域控制站的憑證上產生一份網域控制站清單, 以及報告。</br>> 4。  如果指定了網域和網域控制站, 則會從目標網域控制站產生網域控制站清單。 也會產生清單中每個網域控制站的憑證報告。

例如, 假設有一個名為 CPANDL 的網域, 且名為 CPANDL 的網域控制站。 您可以執行下列命令來抓取 CPANDL 的網域控制站及其憑證的清單-DC1: certutil-dc CPANDL-dc1-dcinfo CPANDL

返回[功能表](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [Options]-EntInfo DomainName\MachineName $

[-f][-使用者]

返回[功能表](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [Options]-TCAInfo [DomainDN |-]

顯示 CA 資訊

[-f][-enterprise][-使用者][-urlfetch verify][-dc DCName][-t Timeout]

返回[功能表](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [Options]-SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

顯示智慧卡資訊

CRYPT_DELETEKEYSET:刪除智慧卡上的所有金鑰

[-無訊息][-split][-urlfetch verify][-t Timeout]

返回[功能表](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [選項]-SCRoots update [+] [InputRootFile] [ReaderName]

CertUtil [選項]-SCRoots 儲存 \@OutputRootFile [ReaderName]

CertUtil [選項]-SCRoots view [InputRootFile |ReaderName]

CertUtil [選項]-SCRoots delete [ReaderName]

管理智慧卡的根憑證

[-f][-split][-p 密碼]

返回[功能表](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [Options]-verifykeys [Cspparameters.keycontainername CACertFile]

驗證公用/私密金鑰組

Cspparameters.keycontainername: 要驗證之金鑰的金鑰容器名稱。 預設為電腦金鑰。  使用者金鑰使用-user。

CACertFile: 簽署或加密憑證檔案

如果未指定任何引數, 則會根據其私密金鑰來驗證每個簽署 CA 憑證。

這項作業只能針對本機 CA 或本機密鑰來執行。

[-f][-使用者][-無訊息][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-verify"></a>-驗證

CertUtil [Options]-verify CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [Options]-verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Options]-驗證 Crlfile.crl CACertFile [IssuedCertFile]

CertUtil [Options]-驗證 Crlfile.crl CACertFile [DeltaCRLFile]

驗證憑證、CRL 或鏈

CertFile要驗證的憑證

ApplicationPolicyList: 選擇性的必要應用程式原則 Objectid 清單 (以逗號分隔)

IssuancePolicyList: 必要發佈原則 Objectid 的選擇性逗號分隔清單

CACertFile: 選擇性發行 CA 憑證以進行驗證

CrossedCACertFile: CertFile 的選擇性憑證交叉認證

Crlfile.crl要驗證的 CRL

IssuedCertFile: Crlfile.crl 所涵蓋的選擇性發行憑證

DeltaCRLFile: 選擇性的 delta CRL

如果指定 ApplicationPolicyList, 則鏈建築物僅限於指定之應用程式原則的有效鏈。

如果指定 IssuancePolicyList, 則鏈建築物僅限於指定之發佈原則的有效鏈。

如果指定 CACertFile, CACertFile 中的欄位會針對 CertFile 或 Crlfile.crl 進行驗證。

如果未指定 CACertFile, 則會使用 CertFile 來建立及驗證完整鏈。

如果同時指定 CACertFile 和 CrossedCACertFile, 則 CACertFile 和 CrossedCACertFile 中的欄位會針對 CertFile 進行驗證。

如果指定 IssuedCertFile, IssuedCertFile 中的欄位會針對 Crlfile.crl 進行驗證。

如果指定 DeltaCRLFile, DeltaCRLFile 中的欄位會針對 Crlfile.crl 進行驗證。

[-f][-enterprise][-使用者][-無訊息][-split][-urlfetch verify][-t Timeout]

返回[功能表](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [Options]-verifyCTL CTLObject [CertDir] [CertFile]

確認 AuthRoot 或不允許的憑證 CTL

CTLObject:識別要驗證的 CTL:

- AuthRootWU: 從 URL 快取讀取 AuthRoot CAB 和相符的憑證。 請改用-f, 改為從 Windows Update 下載。
- DisallowedWU: 從 URL 快取讀取不允許的憑證 CAB 和不允許的憑證存放區檔案。  請改用-f, 改為從 Windows Update 下載。
- AuthRoot: 讀取登錄快取的 AuthRoot CTL。  使用 with-f 和尚未受信任的 CertFile, 強制更新登錄快取的 AuthRoot 和不允許的憑證 Ctl。
- 不允許: 讀取登錄快取不允許的憑證 CTL。 -f 的行為與 AuthRoot 相同。
- CTLFileName: file 或 HTTP: CTL 或 CAB 的路徑

CertDir: 包含符合 CTL 專案之憑證的資料夾。 Http: 資料夾路徑的結尾必須是路徑分隔符號。 如果未使用 AuthRoot 指定資料夾或不允許, 則會搜尋多個位置以尋找相符的憑證: 本機憑證存放區、crypt32.dll、dll 資源和本機 URL 快取。 必要時, 請使用-f 從 Windows Update 下載。 否則會預設為與 CTLObject 相同的資料夾或網站。

CertFile: 包含要驗證之憑證的檔案。 憑證會針對 CTL 專案進行比對, 並顯示相符的結果。 隱藏大部分的預設輸出。

[-f][-使用者][-split]

返回[功能表](#menu)

## <a name="-sign"></a>-sign

CertUtil [選項]-sign InFileList |SerialNumber |CRL OutFileList [開始日期 + dd： hh] [+ SerialNumberList |-SerialNumberList |-ObjectIdList | \@ExtensionFile]

CertUtil [選項]-sign InFileList |SerialNumber |CRL OutFileList [#HashAlgorithm] [+ AlternateSignatureAlgorithm |-AlternateSignatureAlgorithm]

重新簽署 CRL 或憑證

InFileList: 要修改並重新簽署的憑證或 CRL 檔案清單 (以逗號分隔)

SerialNumber要建立的憑證序號。 有效期間和其他選項不得存在。

.CRL建立空的 CRL。 有效期間和其他選項不得存在。

OutFileList: 已修改的憑證或 CRL 輸出檔案清單 (以逗號分隔)。 檔案數目必須符合 InFileList。

開始日期 + dd: hh: 新的有效期間: 選擇性的日期加上;選擇性的天數和時數有效期間;如果同時指定這兩者, 請使用加號 (+) 分隔字元。 使用 [now [+ dd： hh]]，在目前的時間開始。 使用「永不」無到期日 (僅適用于 Crl)。

SerialNumberList: 要新增或移除的逗號分隔序號清單

ObjectIdList: 要移除的逗號分隔擴充功能 ObjectId 清單

\@ExtensionFile:包含要更新或移除之延伸模組的 INF 檔案:

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm雜湊演算法的名稱, 前面加上 # 符號

AlternateSignatureAlgorithm: 替代簽章演算法規範

減號會導致移除序號和延伸模組。 加號會導致序號新增至 CRL。 從 CRL 移除專案時, 此清單可能包含序號和 Objectid。 AlternateSignatureAlgorithm 前的減號會導致使用舊版簽章格式。 AlternateSignatureAlgorithm 前的加號會導致使用 alternature 簽章格式。 如果未指定 AlternateSignatureAlgorithm, 則會使用憑證或 CRL 中的簽章格式。

[-nullsign][-f][-無訊息][-Cert CertId]

返回[功能表](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [選項]-vroot [刪除]

建立/刪除 web 虛擬根目錄和檔案共用

返回[功能表](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [選項]-vocsproot [刪除]

建立/刪除 OCSP Web Proxy 的 web 虛擬根目錄

返回[功能表](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [Options]-addEnrollmentServer Kerberos |使用者名稱 |ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

新增註冊伺服器應用程式

如有必要, 請為指定的 CA 新增註冊伺服器應用程式和應用程式集區。 此命令不會安裝二進位檔或封裝。 下列其中一種驗證方法, 其中用戶端會連接到憑證註冊伺服器。

- V5使用 Kerberos SSL 認證
- UserName針對 SSL 認證使用命名帳戶
- ClientCertificate使用 x.509 憑證 SSL 認證
- AllowRenewalsOnly:只有續訂要求可以透過此 URL 提交給此 CA
- AllowKeyBasedRenewal--允許使用在 AD 中沒有相關聯帳戶的憑證。 這只適用于 ClientCertificate 和 AllowRenewalsOnly 模式。

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [Options]-deleteEnrollmentServer Kerberos |使用者名稱 |ClientCertificate

刪除註冊伺服器應用程式

如有必要, 請為指定的 CA 刪除註冊伺服器應用程式和應用程式集區。 此命令不會移除二進位檔或封裝。 下列其中一種驗證方法, 其中用戶端會連接到憑證註冊伺服器。

1. V5使用 Kerberos SSL 認證
2. UserName針對 SSL 認證使用命名帳戶
3. ClientCertificate使用 x.509 憑證 SSL 認證

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [Options]-addPolicyServer Kerberos |使用者名稱 |ClientCertificate [KeyBasedRenewal]

新增原則伺服器應用程式

視需要新增原則伺服器應用程式和應用程式集區。 此命令不會安裝二進位檔或封裝。 用戶端連線到憑證原則伺服器時所使用的下列其中一種驗證方法:

- V5使用 Kerberos SSL 認證
- UserName針對 SSL 認證使用命名帳戶
- ClientCertificate使用 x.509 憑證 SSL 認證
- KeyBasedRenewal:只有包含 KeyBasedRenewal 範本的原則才會傳回給用戶端。 此旗標僅適用于使用者名稱和 ClientCertificate 驗證。

返回[功能表](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [Options]-deletePolicyServer Kerberos |使用者名稱 |ClientCertificate [KeyBasedRenewal]

刪除原則伺服器應用程式

視需要刪除原則伺服器應用程式和應用程式集區。 此命令不會移除二進位檔或封裝。 用戶端連線到憑證原則伺服器時所使用的下列其中一種驗證方法:

1. V5使用 Kerberos SSL 認證
2. UserName針對 SSL 認證使用命名帳戶
3. ClientCertificate使用 x.509 憑證 SSL 認證
4. KeyBasedRenewal:KeyBasedRenewal 原則伺服器

返回[功能表](#menu)

## <a name="-oid"></a>-oid

CertUtil [Options]-oid ObjectId [DisplayName | delete [LanguageId [Type]]]

CertUtil [Options]-oid GroupId

CertUtil [Options]-oid AlgId |AlgorithmName [GroupId]

顯示 ObjectId 或設定顯示名稱

- ObjectId--要顯示或新增顯示名稱的 ObjectId
- GroupId--要列舉的 Objectid 的十進位 GroupId 數位
- AlgId--要查閱之 ObjectId 的十六進位 AlgId
- AlgorithmName--要查閱之 ObjectId 的演算法名稱
- DisplayName--要儲存在 DS 中的顯示名稱
- 刪除--刪除顯示名稱
- LanguageId--語言識別項 (預設為目前的:1033)
- 輸入--要建立的 DS 物件類型:1代表範本 (預設值), 2 代表發佈原則, 3 代表應用程式原則
- 使用-f 來建立 DS 物件。

[-f]

返回[功能表](#menu)

## <a name="-error"></a>-錯誤

CertUtil [Options]-錯誤 ErrorCode

顯示錯誤碼郵件內文

返回[功能表](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [選項]-getreg [{ca | 還原 | 原則 | 結束 | 範本 | 註冊 | 連鎖店 |PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

顯示登錄值

ca使用 CA 的登錄機碼

還原使用 CA 的 restore 登錄機碼

策略使用原則模組的登錄機碼

退出使用第一個結束模組的登錄機碼

template使用範本登錄機碼 (使用者範本使用-user)

享受使用註冊登錄機碼 (使用者內容使用-user)

系列使用連鎖設定登錄機碼

PolicyServers:使用原則伺服器登錄機碼

進程使用原則或結束模組的 ProgId (登錄子機碼名稱)

RegistryValueName: 登錄值名稱 (使用 "name\*" 表示前置詞相符)

值: 新的數值、字串或日期登錄值或檔案名。 如果數值的開頭為 "+" 或 "-", 則會在現有的登錄值中設定或清除在新值中指定的位。

如果字串值的開頭為 "+" 或 "-"，而現有的值為 REG_MULTI_SZ 值，則會在現有的登錄值中加入或移除字串。 若要強制建立 REG_MULTI_SZ 值，請在字串值的結尾加上 "\n"。

如果值的開頭是 "\@", 則值的其餘部分會是包含二進位值之十六進位文字表示的檔案名。 如果未參考有效的檔案，則會改為將它剖析為 [Date] [+ |-] [dd： hh]--選擇性日期加或減去選擇性的日和小時。 如果同時指定這兩者, 請使用加號 (+) 或減號 (-) 分隔字元。 針對相對於目前時間的日期使用 "now + dd: hh"。

使用 "chain\ChainCacheResyncFiletime \@now" 可有效地清除快取的 crl。

[-f][-使用者][-GroupPolicy][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [選項]-setreg [{ca | 還原 | 原則 | 結束 | 範本 | 註冊 | 連鎖店 |PolicyServers} \[ProgId @ no__t-1] RegistryValueName 值

設定登錄值

ca使用 CA 的登錄機碼

還原使用 CA 的 restore 登錄機碼

策略使用原則模組的登錄機碼

退出使用第一個結束模組的登錄機碼

template使用範本登錄機碼 (使用者範本使用-user)

享受使用註冊登錄機碼 (使用者內容使用-user)

系列使用連鎖設定登錄機碼

PolicyServers:使用原則伺服器登錄機碼

進程使用原則或結束模組的 ProgId (登錄子機碼名稱)

RegistryValueName: 登錄值名稱 (使用 "name\*" 表示前置詞相符)

值: 新的數值、字串或日期登錄值或檔案名。 如果數值的開頭為 "+" 或 "-", 則會在現有的登錄值中設定或清除在新值中指定的位。

如果字串值的開頭為 "+" 或 "-"，而現有的值為 REG_MULTI_SZ 值，則會在現有的登錄值中加入或移除字串。 若要強制建立 REG_MULTI_SZ 值，請在字串值的結尾加上 "\n"。

如果值的開頭是 "\@", 則值的其餘部分會是包含二進位值之十六進位文字表示的檔案名。 如果未參考有效的檔案，則會改為將它剖析為 [Date] [+ |-] [dd： hh]--選擇性日期加或減去選擇性的日和小時。 如果同時指定這兩者, 請使用加號 (+) 或減號 (-) 分隔字元。 針對相對於目前時間的日期使用 "now + dd: hh"。

使用 "chain\ChainCacheResyncFiletime \@now" 可有效地清除快取的 crl。

[-f][-使用者][-GroupPolicy][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [選項]-delreg [{ca | 還原 | 原則 | 結束 | 範本 | 註冊 | 連鎖店 |PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

刪除登錄值

ca使用 CA 的登錄機碼

還原使用 CA 的 restore 登錄機碼

策略使用原則模組的登錄機碼

退出使用第一個結束模組的登錄機碼

template使用範本登錄機碼 (使用者範本使用-user)

享受使用註冊登錄機碼 (使用者內容使用-user)

系列使用連鎖設定登錄機碼

PolicyServers:使用原則伺服器登錄機碼

進程使用原則或結束模組的 ProgId (登錄子機碼名稱)

RegistryValueName: 登錄值名稱 (使用 "name\*" 表示前置詞相符)

值: 新的數值、字串或日期登錄值或檔案名。 如果數值的開頭為 "+" 或 "-", 則會在現有的登錄值中設定或清除在新值中指定的位。

如果字串值的開頭為 "+" 或 "-"，而現有的值為 REG_MULTI_SZ 值，則會在現有的登錄值中加入或移除字串。 若要強制建立 REG_MULTI_SZ 值，請在字串值的結尾加上 "\n"。

如果值的開頭是 "\@", 則值的其餘部分會是包含二進位值之十六進位文字表示的檔案名。 如果未參考有效的檔案，則會改為將它剖析為 [Date] [+ |-] [dd： hh]--選擇性日期加或減去選擇性的日和小時。 如果同時指定這兩者, 請使用加號 (+) 或減號 (-) 分隔字元。 針對相對於目前時間的日期使用 "now + dd: hh"。

使用 "chain\ChainCacheResyncFiletime \@now" 可有效地清除快取的 crl。

[-f][-使用者][-GroupPolicy][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [Options]-ImportKMS UserKeyAndCertFile [CertId]

將使用者金鑰和憑證匯入伺服器資料庫以進行金鑰保存

UserKeyAndCertFile--包含要封存之使用者私密金鑰和憑證的資料檔案。  這可以是下列任何一項:

- Exchange 金鑰管理伺服器 (KMS) 匯出檔案
- PFX 檔案

CertIdKMS 匯出檔案解密憑證符合 token。  請參閱[-store](#-store)。

使用-f 匯入不是由 CA 發行的憑證。

[-f][-無訊息][-split][-config Machine\CAName][-p 密碼][-symkeyalg SymmetricKeyAlgorithm [，KeyLength]]

返回[功能表](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [Options]-ImportCert Certfile [ExistingRow]

將憑證檔案匯入資料庫

使用 ExistingRow 來匯入憑證, 以取代相同金鑰的暫止要求。

使用-f 匯入不是由 CA 發行的憑證。

CA 也可能需要設定為支援外部憑證匯入： certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [Options]-GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [Options]-GetKey SearchToken 腳本 OutputScriptFile

CertUtil [Options]-GetKey SearchToken 取出 |復原 OutputFileBaseName

取出已封存的私密金鑰修復 blob、產生復原腳本, 或復原封存金鑰

腳本: 產生用來抓取和復原金鑰的腳本 (如果找到多個相符的復原候選項目, 或者如果未指定輸出檔, 則為預設行為)。

抓取: 取出一或多個金鑰修復 Blob (如果只找到一個符合的復原候選, 而且已指定輸出檔, 則為預設行為)

復原: 在一個步驟中取出及復原私密金鑰 (需要金鑰復原代理憑證和私密金鑰)

SearchToken用來選取要復原的金鑰和憑證。

可以是下列任何一項:

1. 憑證一般名稱
2. 憑證序號
3. 憑證 SHA-1 雜湊 (指紋)
4. 憑證 KeyId SHA-1 雜湊 (主體金鑰識別碼)
5. 要求者名稱 (網域 \ 使用者)
6. UPN (使用者\@網域)

RecoveryBlobOutFile: 包含憑證鏈和相關聯私密金鑰的輸出檔, 仍然會加密為一或多個金鑰復原代理憑證。

OutputScriptFile: 輸出檔案, 其中包含用來抓取和復原私密金鑰的批次腳本。

OutputFileBaseName: 輸出檔案的基底名稱。 針對抓取, 會截斷任何延伸模組, 並針對每個金鑰修復 blob 附加一個憑證特定的字串和 rec 副檔名。  每個檔案都包含憑證鏈和相關聯的私密金鑰, 但仍加密為一或多個金鑰復原代理憑證。 針對復原, 會截斷任何延伸模組並附加 p12 副檔名。  包含已復原的憑證鏈和相關聯的私密金鑰, 儲存為 PFX 檔案。

[-f][-UnicodeText][-無訊息][-config Machine\CAName][-p 密碼][-ProtectTo SAMNameAndSIDList][-csp 提供者]

返回[功能表](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [Options]-RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

復原封存的私密金鑰

[-f][-使用者][-無訊息][-split][-p 密碼][-ProtectTo SAMNameAndSIDList][-csp 提供者][-t Timeout]

返回[功能表](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [Options]-MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList:以逗號分隔的 PFX 輸入檔案清單

PFXOutFile:PFX 輸出檔案

ExtendedProperties包含擴充屬性

在命令列上指定的密碼是以逗號分隔的密碼清單。  如果指定了一個以上的密碼, 則會將最後一個密碼用於輸出檔案。  如果只提供一個密碼, 或最後一個密碼是 "\*", 則會提示使用者輸入輸出檔案密碼。

[-f][-使用者][-split][-p 密碼][-ProtectTo SAMNameAndSIDList][-csp 提供者]

返回[功能表](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [Options]-ConvertEPF PFXInFileList EPFOutFile [cast | cast-] [V3CACertId] [，Salt]

將 PFX 檔案轉換為 EPF 檔案

PFXInFileList:以逗號分隔的 PFX 輸入檔案清單

EPF:EPF 輸出檔

鑄造使用 CAST 64 加密

cast-:使用 CAST 64 加密 (匯出)

V3CACertId:V3 CA 憑證相符 token。  請參閱[-store](#-store) CertId description。

SaltEPF 輸出檔 salt 字串

在命令列上指定的密碼是以逗號分隔的密碼清單。 如果指定了一個以上的密碼, 則會將最後一個密碼用於輸出檔案。  如果只提供一個密碼, 或最後一個密碼是 "\*", 則會提示使用者輸入輸出檔案密碼。

[-f][-無訊息][-split][-dc DCName][-p 密碼][-csp 提供者]

返回[功能表](#menu)

## <a name="options"></a>選項。

這個區段會定義您可以使用命令指定的選項。

|選項。|描述|
|-------|-----------|
|-nullsign|使用資料雜湊做為簽章|
|-f|強制覆寫|
|-enterprise|使用本機電腦的 Enterprise registry 憑證存放區|
|-使用者|使用 HKEY_CURRENT_USER 機碼或憑證存放區|
|-GroupPolicy|使用群組原則憑證存放區|
|-未通過|顯示使用者範本|
|-mt|顯示電腦範本|
|-Unicode|以 Unicode 寫入重新導向的輸出|
|-UnicodeText|以 Unicode 寫入輸出檔|
|-gmt|以 GMT 顯示時間|
|-秒|以秒和毫秒為單位顯示時間|
|-無訊息|使用無訊息旗標來取得 crypt 內容|
|-split|分割內嵌的 asn.1 元素, 並儲存至檔案|
|-v|詳細資訊作業|
|-privatekey|顯示密碼和私密金鑰資料|
|-pin 釘選|智慧卡 PIN|
|-urlfetch verify|取得並驗證 AIA 憑證和 CDP Crl|
|-config Machine\CAName|CA 和電腦名稱稱字串|
|-PolicyServer URLOrId|原則伺服器 URL 或識別碼。針對 [選取] U/I, 請使用-PolicyServer。 針對所有原則伺服器, 請使用-PolicyServer\*|
|-匿名|使用匿名 SSL 認證|
|-Kerberos|使用 Kerberos SSL 認證|
|-ClientCertificate ClientCertId|使用 x.509 憑證 SSL 認證。 針對 [選取] U/I, 請使用-clientCertificate。|
|-UserName 使用者名稱|使用命名帳戶作為 SSL 認證。 針對 [選取] U/I, 請使用-UserName。|
|-Cert CertId|簽署憑證|
|-dc DCName|以特定網域控制站為目標|
|-限制 RestrictionList|以逗號分隔的限制清單。 每個限制都包含一個資料行名稱、一個關聯式運算子和一個常數整數、字串或日期。 一個資料行名稱前面可能會加上加號或減號, 以指出排序次序。 例如：</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>「-RequesterName > 網域, 配置 = 21」|
|-out ColumnList|逗號分隔的資料行清單|
|-p 密碼|密碼|
|-ProtectTo SAMNameAndSIDList|以逗號分隔的 SAM 名稱/SID 清單|
|-csp 提供者|提供者|
|-t Timeout|URL 提取超時 (以毫秒為單位)|
|-symkeyalg SymmetricKeyAlgorithm [，KeyLength]|具有選擇性金鑰長度的對稱金鑰演算法名稱, 範例:AES、128或3DES|

返回[功能表](#menu)

## <a name="additional-certutil-examples"></a>其他 certutil 範例

如需如何使用此命令的一些範例, 請參閱

1. [從命令列管理 Active Directory 憑證服務 (AD CS) 的 Certutil 範例](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [管理憑證的 Certutil 工作](https://technet.microsoft.com/library/cc772898.aspx)
3. [使用 CertUtil 命令列工具的二進位要求匯出逐步解說](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [根 CA 憑證更新](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

返回[功能表](#menu)
