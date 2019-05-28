---
title: certutil
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: faaf936e4c23579e908e12543c07d0764a2cdcc1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192619"
---
# <a name="certutil"></a>certutil

Certutil.exe 是命令列程式所安裝憑證服務的一部分。 您可以使用 Certutil.exe 來傾印和顯示憑證授權單位 (CA) 設定資訊，請設定憑證服務備份和還原 CA 元件，請確認憑證、 金鑰組和憑證鏈結。

未包含其他參數的憑證授權單位上執行 certutil 時，它會顯示目前的憑證授權單位設定。 當 cerutil 非憑證授權單位上執行時，此命令會預設為執行 certutil [-傾印](#-dump)動詞命令。

> [!WARNING]
> 舊版的 certutil 可能不會提供所有這份文件中所述的選項。 您可以看到所有選項提供執行命令所示的特定版本的 certutil[語法標記法](#syntax-notations)一節。

## <a name="menu"></a>功能表

本文件中的主要區段是：

- [Verbs](#verbs)
- [語法標記法](#syntax-notations)
- [選項](#options)
- [其他的 certutil 範例](#additional-certutil-examples)

## <a name="verbs"></a>動詞

下表說明可以使用 certutil 命令中使用的動詞命令。

|動詞|描述|
|-----|-----------|
|[-dump](#-dump)|傾印組態資訊或檔案|
|[-asn](#-asn)|剖析 ASN.1 檔案|
|[-decodehex](#-decodehex)|解碼十六進位編碼的檔案|
|[-decode](#-decode)|解碼 Base64 編碼的檔案|
|[-encode](#-encode)|將檔案編碼為 Base64|
|[-deny](#-deny)|拒絕擱置的憑證要求|
|[-resubmit](#-resubmit)|重新提交擱置的憑證要求|
|[-setattributes](#-setattributes)|設定擱置的憑證要求的屬性|
|[-setextension](#-setextension)|設定擱置的憑證要求的擴充功能|
|[-revoke](#-revoke)|撤銷憑證|
|[-isvalid](#-isvalid)|顯示的處置目前的憑證。|
|[-getconfig](#-getconfig)|取得預設的組態字串|
|[-ping](#-ping)|嘗試連絡 Active Directory 憑證服務要求的介面|
|-pingadmin|嘗試連絡 Active Directory 憑證服務系統管理員介面|
|[-CAInfo](#-cainfo)|顯示的憑證授權單位的相關資訊|
|[-ca.cert](#-cacert)|擷取憑證授權單位的憑證|
|[-ca.chain](#-cachain)|擷取憑證授權單位的憑證鏈結|
|[-GetCRL](#-getcrl)|取得憑證撤銷清單 (CRL)|
|[-CRL](#-crl)|發佈新的憑證撤銷清單 (Crl) [或 delta Crl]|
|[-shutdown](#-shutdown)|關閉 Active Directory 憑證服務|
|[-installCert](#-installcert)|安裝憑證授權單位憑證|
|[-renewCert](#-renewcert)|更新憑證授權單位憑證|
|[-schema](#-schema)|傾印憑證的結構描述|
|[-view](#-view)|傾印憑證檢視|
|[-db](#-db)|將未經處理的資料庫傾印|
|[-deleterow](#-deleterow)|從伺服器資料庫刪除資料列|
|[-backup](#-backup)|備份 Active Directory 憑證服務|
|[-backupDB](#-backupdb)|備份 Active Directory 憑證服務資料庫|
|[-backupKey](#-backupkey)|備份 Active Directory 憑證服務的憑證和私密金鑰|
|[-restore](#-restore)|還原 Active Directory 憑證服務|
|[-restoreDB](#-restoredb)|還原 Active Directory 憑證服務資料庫|
|[-restoreKey](#-restorekey)|還原 Active Directory 憑證服務的憑證和私密金鑰|
|[-importPFX](#-importpfx)|匯入憑證和私密金鑰|
|[-dynamicfilelist](#-dynamicfilelist)|顯示動態的檔案清單|
|[-databaselocations](#-databaselocations)|顯示資料庫位置|
|[-hashfile](#-hashfile)|產生並顯示透過檔案的密碼編譯雜湊|
|[-store](#-store)|傾印的憑證存放區|
|[-addstore](#-addstore)|將憑證新增至存放區|
|[-delstore](#-delstore)|從存放區刪除憑證|
|[-verifystore](#-verifystore)|確認憑證存放區中|
|[-repairstore](#-repairstore)|修復金鑰的關聯，或更新憑證內容] 或 [金鑰的安全性描述元|
|[-viewstore](#-viewstore)|傾印的憑證存放區|
|[-viewdelstore](#-viewdelstore)|從存放區刪除憑證|
|[-dsPublish](#-dspublish)|將憑證或憑證撤銷清單 (CRL) 發佈至 Active Directory|
|[-ADTemplate](#-adtemplate)|顯示 AD 範本|
|[-Template](#-template)|顯示憑證範本|
|[-TemplateCAs](#-templatecas)|顯示 憑證授權單位 (Ca) 憑證範本|
|[-CATemplates](#-catemplates)|CA 的顯示範本|
|[-SetCASites](#-setcasites)|管理 Ca 的網站名稱|
|[-enrollmentServerURL](#-enrollmentserverurl)|顯示、 新增或刪除與 CA 相關聯的註冊伺服器 Url|
|[-ADCA](#-adca)|顯示 AD Ca|
|[-CA](#-ca)|顯示註冊原則 Ca|
|[-Policy](#-policy)|顯示註冊原則|
|[-PolicyCache](#-policycache)|顯示或刪除註冊原則快取項目|
|[-CredStore](#-credstore)|顯示、 新增或刪除認證存放區項目|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|安裝預設憑證範本|
|[-URLCache](#-urlcache)|顯示或刪除 URL 快取項目|
|[-pulse](#-pulse)|Pulse 自動註冊事件|
|[-MachineInfo](#-machineinfo)|顯示 Active Directory 電腦物件的相關資訊|
|[-DCInfo](#-dcinfo)|顯示網域控制站的相關資訊|
|[-EntInfo](#-entinfo)|顯示企業 CA 的相關資訊|
|[-TCAInfo](#-tcainfo)|顯示 CA 的相關資訊|
|[-SCInfo](#-scinfo)|顯示智慧卡的相關資訊|
|[-SCRoots](#-scroots)|管理智慧卡的根憑證|
|[-verifykeys](#-verifykeys)|確認公用或私用金鑰組|
|[-verify](#-verify)|確認憑證、 憑證撤銷清單 (CRL) 或憑證鏈結|
|[-verifyCTL](#-verifyctl)|請確認 AuthRoot 或不允許的憑證的 CTL|
|[-sign](#-sign)|重新簽署的憑證撤銷清單 (CRL) 或憑證|
|[-vroot](#-vroot)|建立或刪除 web 虛擬根目錄和檔案共用|
|[-vocsproot](#-vocsproot)|建立或刪除 web 虛擬根目錄的 OCSP web proxy|
|[-addEnrollmentServer](#-addenrollmentserver)|新增註冊伺服器應用程式|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|刪除註冊伺服器應用程式|
|[-addPolicyServer](#-addpolicyserver)|新增原則伺服器的應用程式|
|[-deletePolicyServer](#-deletepolicyserver)|刪除原則伺服器應用程式|
|[-oid](#-oid)|顯示的物件識別碼，或設定顯示名稱|
|[-error](#-error)|顯示並出現錯誤代碼相關聯的訊息文字|
|[-getreg](#-getreg)|顯示的登錄值|
|[-setreg](#-setreg)|設定登錄值|
|[-delreg](#-delreg)|刪除登錄值|
|[-ImportKMS](#-importkms)|使用者金鑰和憑證匯入金鑰保存的伺服器資料庫|
|[-ImportCert](#-importcert)|憑證檔案匯入資料庫|
|[-GetKey](#-getkey)|擷取已封存的私密金鑰復原的 blob|
|[-RecoverKey](#-recoverkey)|復原封存的私密金鑰|
|[-MergePFX](#-mergepfx)|合併 PFX 檔案|
|[-ConvertEPF](#-convertepf)|PFX 檔案轉換成 EPF 檔案|
|-?|顯示指令動詞的清單|
|- *\<verb>* -?|顯示說明所指定的動詞。|
|-? -v|顯示動詞命令的完整清單和|

返回[功能表](#menu)

## <a name="syntax-notations"></a>語法標記法

- 如需執行的基本命令列語法 `certutil -?`
- 如需有關使用使用特定的動詞命令的 certutil 語法，請執行**certutil** *\<動詞 >* **-嗎？**
- 若要傳送的 certutil 語法的所有文字檔案，請執行下列命令：  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

下表描述用來表示命令列語法標記法。

|標記法|描述|
|--------|-----------|
|文字，而不需要方括號或大括號|您必須依照顯示輸入的項目|
|\<角括弧內的文字 >|您必須提供值的預留位置|
|[方括號內的文字]|選擇性的項目|
|{大括號內的文字}|一組必要的項目;選擇其中一個|
|分隔號 （|)|分隔符號是互斥的項目;選擇其中一個|
|省略符號 （...）|可重複的項目|

返回[功能表](#menu)

## <a name="-dump"></a>-dump

CertUtil [Options] [-dump]

CertUtil [Options] [-傾印] 檔案

傾印組態資訊或檔案

[-f] [-silent] [-split] [-p Password] [-t Timeout]

返回[功能表](#menu)

## <a name="-asn"></a>-asn

CertUtil [選項]-asn 檔案 [類型]

剖析 ASN.1 檔案

類型： 數值的 CRYPT\_字串\_\*解碼類型

返回[功能表](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [選項]-decodehex InFile OutFile [類型]

類型： 數值的 CRYPT\_字串\_\*編碼類型

[-f]

返回[功能表](#menu)

## <a name="-decode"></a>-decode

CertUtil [Options] -decode InFile OutFile

解碼 Base64 編碼的檔案

[-f]

返回[功能表](#menu)

## <a name="-encode"></a>-encode

CertUtil [Options] -encode InFile OutFile

將檔案編碼為 Base64

[-f] [-UnicodeText]

返回[功能表](#menu)

## <a name="-deny"></a>-拒絕

CertUtil [選項]-deny RequestId

拒絕擱置要求

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-resubmit"></a>-resubmit

CertUtil [選項]-重新提交 RequestId

重新提交暫止要求

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-setattributes"></a>-setattributes

CertUtil [Options] -setattributes RequestId AttributeString

設定屬性的暫止要求

RequestId-數值要求識別碼的暫止要求

AttributeString-要求的屬性名稱 / 值組

- 名稱和值都是分號分隔。
- 多個名稱及值組是新行字元分隔。
- 範例:"CertificateTemplate:User\nEMail:User@Domain.com」
- 每個"\n"序列會轉換成新行字元分隔符號。

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-setextension"></a>-setextension

RequestId ExtensionName 加上旗標的 [選項] CertUtil-setextension {Long |日期 |字串 |@InFile}

集延伸模組的暫止要求

RequestId-數字要求識別碼的暫止要求

延伸模組的 ExtensionName-ObjectId 字串

旗標--建議使用 0。  重要的 1 會延伸模組、 2 會停用它、 3 兩者都執行。

如果最後一個參數是數值，它會被視為長時間。

如果它可以剖析為日期，它會被視為一個日期。

如果它的開頭 ' @'，此語彙基元的其餘部分則包含二進位資料或 ascii 文字的十六進位傾印的檔名。

任何其他項目會被視為字串。

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-revoke"></a>-revoke

CertUtil [Options] -revoke SerialNumber [Reason]

撤銷憑證

序號：若要撤銷的憑證序號的逗號分隔清單

原因： 數值或符號的撤銷原因

- 0:CRL_REASON_UNSPECIFIED:未指定 （預設值）
- 1：CRL_REASON_KEY_COMPROMISE:金鑰洩露
- 2：CRL_REASON_CA_COMPROMISE:CA 洩露
- 3：CRL_REASON_AFFILIATION_CHANGED:聯盟已變更
- 4:CRL_REASON_SUPERSEDED:已取代
- 5:CRL_REASON_CESSATION_OF_OPERATION:作業停止
- 6:CRL_REASON_CERTIFICATE_HOLD:憑證保留
- 8:CRL_REASON_REMOVE_FROM_CRL:CRL 中移除
- -1：解除撤銷：解除撤銷

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-isvalid"></a>-isvalid

CertUtil [Options] -isvalid SerialNumber | CertHash

顯示目前的憑證配置

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-getconfig"></a>-getconfig

CertUtil [Options] -getconfig

取得預設的組態字串

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-ping"></a>-ping

CertUtil [Options] -ping [MaxSecondsToWait | CAMachineList]

Ping Active Directory 憑證服務要求的介面

CAMachineList-逗號分隔的 CA 電腦名稱清單

1. 針對單一機器，請使用終止逗號
2. 顯示每個 CA 電腦的站台成本

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [Options] -CAInfo [InfoName [Index | ErrorCode]]

顯示 CA 資訊

資訊的名稱，表示 CA 屬性以顯示 （如下所示）。 使用 「\*"之所有屬性。

索引--選擇性屬性，以零為起始的索引

ErrorCode-數字錯誤碼

[-f] [-split] [-config Machine\CAName]

資訊名稱引數的語法：

- 檔案：檔案版本
- 產品：產品版本
- exitcount:結束模組計數
- 結束 [Index]:結束模組描述
- 原則：原則模組描述
- 名稱：CA 名稱
- sanitizedname:處理過的 CA 名稱
- dsname:處理過的 CA 簡短名稱 （DS）
- sharedfolder:共用的資料夾
- error1 ErrorCode:錯誤訊息文字
- error2 ErrorCode:錯誤訊息文字和錯誤代碼
- 類型：CA 類型
- 資訊：CA 資訊
- 父代：父系 CA
- certcount:CA 憑證計數
- xchgcount:CA 交換憑證計數
- kracount:KRA 憑證計數
- kraused:KRA 憑證使用的計數
- propidmax:Maximum CA PropId
- certstate [Index]:CA 憑證
- certversion [Index]:CA 憑證版本
- certstatuscode [Index]:CA 憑證的驗證狀態
- crlstate [Index]:CRL
- krastate [Index]:KRA 憑證
- crossstate + [Index]:正向交互憑證
- crossstate-[Index]:向後交互憑證
- 憑證 [Index]:CA 憑證
- certchain [Index]:CA 憑證鏈結
- certcrlchain [Index]:利用 Crl 的 CA 憑證鏈結
- xchg [Index]:CA 交換憑證
- xchgchain [Index]:CA 交換憑證鏈結
- xchgcrlchain [Index]:利用 Crl 的 CA 交換憑證鏈結
- kra [Index]:KRA 憑證
- 跨 + [Index]:正向交互憑證
- 跨-[Index]:向後交互憑證
- CRL [Index]:基底 CRL
- deltacrl [Index]:Delta CRL
- crlstatus [Index]:CRL 發佈狀態
- deltacrlstatus [Index]:Delta CRL 發佈狀態
- dns:DNS 名稱
- 角色：角色分離
- 廣告：Advanced Server
- 範本：範本
- csp [Index]:OCSP Url
- aia [Index]:AIA Url
- cdp [Index]:CDP Url
- localename:CA 的地區設定名稱
- subjecttemplateoids:主旨範本 Oid

返回[功能表](#menu)

## <a name="-cacert"></a>-ca.cert

CertUtil [Options] -ca.cert OutCACertFile [Index]

擷取 CA 的憑證

OutCACertFile： 輸出檔

索引：CA 憑證更新索引 （預設為最新）

[-f] [-split] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-cachain"></a>-ca.chain

CertUtil [Options] -ca.chain OutCACertChainFile [Index]

擷取 CA 的憑證鏈結

OutCACertChainFile： 輸出檔

索引：CA 憑證更新索引 （預設為最新）

[-f] [-split] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [Options] -GetCRL OutFile [Index] [delta]

取得 CRL

索引：CRL 索引鍵索引 （最新的索引鍵有預設為 CRL）

差異： delta CRL （預設值為基底 CRL）

[-f] [-split] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-crl"></a>-CRL

CertUtil [Options] -CRL [dd:hh | republish] [delta]

發佈新 Crl [或只有 delta Crl]

dd:hh-新的 CRL 有效期間，在工作日和時數

重新發行-重新發行最新的 Crl

差異-只有 delta Crl （預設為基本與 delta Crl）

[-split] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-shutdown"></a>-shutdown

CertUtil [Options] -shutdown

關閉 Active Directory 憑證服務

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [Options] -installCert [CACertFile]

安裝憑證授權單位憑證

[-f] [-silent] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [Options] -renewCert [ReuseKeys] [Machine\ParentCAName]

更新憑證授權單位憑證

使用-f 來略過未處理更新要求，並產生新的要求。

[-f] [-silent] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-schema"></a>-schema

CertUtil [選項]-結構描述 [Ext |Attrib |CRL]

傾印憑證結構描述

預設為要求和憑證的資料表

Ext:延伸模組資料表

Attrib:屬性資料表

CRL:CRL 資料表

[-split] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-view"></a>-view

CertUtil [選項]-檢視 [佇列 |記錄檔 |LogFail |撤銷 |Ext |Attrib |CRL] [csv]

傾印憑證檢視

佇列:要求佇列

記錄檔：發行或撤銷的憑證，再加上失敗的要求

LogFail:失敗的要求

撤銷：已撤銷的憑證

Ext:延伸模組資料表

Attrib:屬性資料表

CRL:CRL 資料表

csv:輸出為以逗號分隔值

若要顯示 StatusCode 資料行的所有項目:-out StatusCode

若要顯示所有資料行的最後一個項目:-限制 「 RequestId = = $"

若要顯示的三個要求 RequestId 和配置:-限制"RequestId > = 37，RequestId\<40"-"RequestId，配置 」 出

若要可顯示所有基底 Crl 資料列 Id 和 CRL 編號:-限制 」 CRLMinBase = 0"-"CRLRowId CRLNumber"出 CRL

To display Base CRL Number 3: -v -restrict "CRLMinBase=0,CRLNumber=3" -out "CRLRawCRL" CRL

若要顯示整個 CRL 資料表：CRL

使用 「 日期 [+ |-dd:hh] 」 的日期限制

使用 [now + dd:hh] 相對於目前日期

[-無訊息][-分割][-config Machine\CAName][-限制 RestrictionList][-columnlist 就會出]

返回[功能表](#menu)

## <a name="-db"></a>-db

CertUtil [Options] -db

傾印未經處理的資料庫

[-config Machine\CAName][-限制 RestrictionList][-columnlist 就會出]

返回[功能表](#menu)

## <a name="-deleterow"></a>-deleterow

CertUtil [選項]-deleterow RowId |日期 [要求 |憑證 |Ext |Attrib |CRL]

刪除伺服器資料庫的資料列

要求：失敗和擱置的要求 （提交日期）

Cert:到期及撤銷的憑證 （到期日）

Ext:延伸模組資料表

Attrib:屬性資料表

CRL:CRL 資料表 （到期日）

若要刪除失敗和擱置 2001 年 1 月 22 日所提交的要求：1/22/2001 要求

若要刪除所有過期的憑證由 2001 年 1 月 22 日：1/22/2001 憑證

若要刪除的 RequestId 37 的憑證資料行、 屬性和擴充功能：37

若要刪除過期的 Crl 的 2001 年 1 月 22 日：2001 年 1 月 22 日 CRL

[-f] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-backup"></a>-backup

CertUtil [選項]-備份 BackupDirectory [增量] [KeepLog]

備份 Active Directory 憑證服務

BackupDirectory： 目錄來儲存備份資料

累加： 執行增量備份只 （預設值為完整備份）

KeepLog： 保留的資料庫記錄檔 （預設值為截斷記錄檔）

[-f] [-config Machine\CAName] [-p Password]

返回[功能表](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [Options] -backupDB BackupDirectory [Incremental] [KeepLog]

備份 Active Directory 憑證服務資料庫

BackupDirectory： 目錄來儲存備份的資料庫檔案

累加： 執行增量備份只 （預設值為完整備份）

KeepLog： 保留的資料庫記錄檔 （預設值為截斷記錄檔）

[-f] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [Options] -backupKey BackupDirectory

備份 Active Directory 憑證服務憑證和私密金鑰

BackupDirectory： 目錄來儲存備份的 PFX 檔案

[-f] [-config Machine\CAName] [-p Password] [-t Timeout]

返回[功能表](#menu)

## <a name="-restore"></a>-restore

CertUtil [Options] -restore BackupDirectory

還原 Active Directory 憑證服務

包含要還原資料的備份目錄： 目錄

[-f] [-config Machine\CAName] [-p Password]

返回[功能表](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [Options] -restoreDB BackupDirectory

還原 Active Directory 憑證服務資料庫

包含要還原的資料庫檔案的備份目錄： 目錄

[-f] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [Options] -restoreKey BackupDirectory | PFXFile

還原 Active Directory 憑證服務憑證和私密金鑰

包含要還原的 PFX 檔案的備份目錄： 目錄

PFXFile:若要還原的 PFX 檔案

[-f] [-config Machine\CAName] [-p Password]

返回[功能表](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [Options] -importPFX [CertificateStoreName] PFXFile [Modifiers]

匯入憑證和私密金鑰

CertificateStoreName:憑證存放區名稱。  請參閱[-儲存](#-store)。

PFXFile:要匯入的 PFX 檔案

修飾詞：一或多個項目以逗號分隔清單：

1. AT_SIGNATURE:變更簽章 KeySpec
2. AT_KEYEXCHANGE:變更金鑰交換 KeySpec
3. NoExport:讓非可匯出私密金鑰
4. NoCert:匯入憑證
5. NoChain:無法匯入的憑證鏈結
6. NoRoot:匯入的根憑證
7. 保護：保護金鑰與密碼
8. NoProtect:執行沒有密碼保護的金鑰

預設值是個人電腦存放區。

[-f] [-user] [-p Password] [-csp Provider]

返回[功能表](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [Options] -dynamicfilelist

顯示動態的檔案清單

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [Options] -databaselocations

顯示資料庫位置

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [Options] -hashfile InFile [HashAlgorithm]

產生並顯示透過檔案的密碼編譯雜湊

返回[功能表](#menu)

## <a name="-store"></a>-store

CertUtil [Options] -store [CertificateStoreName [CertId [OutputFile]]]

傾印憑證存放區

CertificateStoreName:憑證存放區名稱。 範例：

- "My"，"CA"（預設值）、 「 根 」，
- "ldap: / / CN = 憑證授權單位，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 一個？ objectClass = 憑證授權單位"（檢視的根憑證）
- "ldap: / / CN = CAName，CN = 憑證授權單位，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基底？ objectClass = 憑證授權單位 」 （修改根憑證）
- "ldap: / / CN = CAName，CN = MachineName，CN = CDP，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ certificateRevocationList？ 基底？ objectClass = cRLDistributionPoint"(檢視 Crl)
- "ldap: / / CN = NTAuthCertificates，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基底？ objectClass = 憑證授權單位"（企業 CA 憑證）
- ldap:（AD 電腦物件憑證）
- -使用者 ldap:（AD 使用者物件憑證）

CertId:憑證或 CRL 相符的語彙基元。  這可以是序號、 sha-1 憑證、 CRL、 CTL 或公開金鑰雜湊，數值 cert 索引 （0，1，依此類推），數字的 CRL 索引 (。 0、.1 等等)，數字的 CTL 索引 (...0...1，依此類推），公用金鑰、 簽章或延伸模組 ObjectId，憑證主體一般名稱，電子郵件地址的 UPN 或 DNS 名稱、 金鑰容器名稱或 CSP 名稱、 範本名稱或 ObjectId，EKU 或應用程式原則的 ObjectId 或 CRL 簽發者一般名稱。 其中許多可能會導致多個相符項目。

要儲存比對憑證的 OutputFile： 檔案

使用-使用者存取的使用者存放區，而不是電腦存放區。

使用-enterprise 能夠存取電腦的企業存放區。

使用-存取機器服務存放區的服務。

您可以使用-grouppolicy 來存取電腦群組原則存放區。

範例：

- -enterprise NTAuth
- -企業根 37
- -使用者我 26e0aaaf000000000004
- CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName]

返回[功能表](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [Options] -addstore CertificateStoreName InFile

新增憑證存放區

CertificateStoreName:憑證存放區名稱。  請參閱[-儲存](#-store)。

InFile:要加入至儲存的憑證或 CRL 檔案。

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回[功能表](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [Options] -delstore CertificateStoreName CertId

從存放區刪除憑證

CertificateStoreName:憑證存放區名稱。  請參閱[-儲存](#-store)。

CertId:憑證或 CRL 相符的語彙基元。  請參閱[-儲存](#-store)。

[-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回[功能表](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [Options] -verifystore CertificateStoreName [CertId]

確認憑證存放區中

CertificateStoreName:憑證存放區名稱。  請參閱[-儲存](#-store)。

CertId:憑證或 CRL 相符的語彙基元。  請參閱[-儲存](#-store)。

[-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName] [-t Timeout]

返回[功能表](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [Options] -repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

修復金鑰的關聯或更新憑證內容] 或 [金鑰安全性描述元

CertificateStoreName:憑證存放區名稱。  請參閱[-儲存](#-store)。

憑證或 CRL 的相符項目 token CertIdList： 以逗號分隔清單。 請參閱[-儲存](#-store)CertId 描述。

包含外部內容 PropertyInfFile-INF 檔案：

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

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-csp Provider]

返回[功能表](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [Options] -viewstore [CertificateStoreName [CertId [OutputFile]]]

傾印憑證存放區

CertificateStoreName:憑證存放區名稱。 範例：

- "My"，"CA"（預設值）、 「 根 」，
- "ldap: / / CN = 憑證授權單位，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 一個？ objectClass = 憑證授權單位"（檢視的根憑證）
- "ldap: / / CN = CAName，CN = 憑證授權單位，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基底？ objectClass = 憑證授權單位 」 （修改根憑證）
- "ldap: / / CN = CAName，CN = MachineName，CN = CDP，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ certificateRevocationList？ 基底？ objectClass = cRLDistributionPoint"(檢視 Crl)
- "ldap: / / CN = NTAuthCertificates，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基底？ objectClass = 憑證授權單位"（企業 CA 憑證）
- ldap:（AD 電腦物件憑證）
- -使用者 ldap:（AD 使用者物件憑證）

CertId:憑證或 CRL 相符的語彙基元。 這可以是序號、 sha-1 憑證、 CRL、 CTL 或公開金鑰雜湊，數值 cert 索引 （0，1，依此類推），數字的 CRL 索引 (。 0、.1 等等)，數字的 CTL 索引 (...0...1，依此類推），公用金鑰、 簽章或延伸模組 ObjectId，憑證主體一般名稱，電子郵件地址的 UPN 或 DNS 名稱、 金鑰容器名稱或 CSP 名稱、 範本名稱或 ObjectId，EKU 或應用程式原則的 ObjectId 或 CRL 簽發者一般名稱。 其中許多可能會導致多個相符項目。

要儲存比對憑證的 OutputFile： 檔案

使用-使用者存取的使用者存放區，而不是電腦存放區。

使用-enterprise 能夠存取電腦的企業存放區。

使用-存取機器服務存放區的服務。

您可以使用-grouppolicy 來存取電腦群組原則存放區。

範例：

1. -enterprise NTAuth
2. -企業根 37
3. -使用者我 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回[功能表](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [Options] -viewdelstore [CertificateStoreName [CertId [OutputFile]]]

從存放區刪除憑證

CertificateStoreName:憑證存放區名稱。 範例：

- "My"，"CA"（預設值）、 「 根 」，
- "ldap: / / CN = 憑證授權單位，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 一個？ objectClass = 憑證授權單位"（檢視的根憑證）
- "ldap: / / CN = CAName，CN = 憑證授權單位，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基底？ objectClass = 憑證授權單位 」 （修改根憑證）
- "ldap: / / CN = CAName，CN = MachineName，CN = CDP，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ certificateRevocationList？ 基底？ objectClass = cRLDistributionPoint"(檢視 Crl)
- "ldap: / / CN = NTAuthCertificates，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基底？ objectClass = 憑證授權單位"（企業 CA 憑證）
- ldap:（AD 電腦物件憑證）
- -使用者 ldap:（AD 使用者物件憑證）

CertId:憑證或 CRL 相符的語彙基元。 這可以是序號、 sha-1 憑證、 CRL、 CTL 或公開金鑰雜湊，數值 cert 索引 （0，1，依此類推），數字的 CRL 索引 (。 0、.1 等等)，數字的 CTL 索引 (...0...1，依此類推），公用金鑰、 簽章或延伸模組 ObjectId，憑證主體一般名稱，電子郵件地址的 UPN 或 DNS 名稱、 金鑰容器名稱或 CSP 名稱、 範本名稱或 ObjectId，EKU 或應用程式原則的 ObjectId 或 CRL 簽發者一般名稱。 其中許多可能會導致多個相符項目。

要儲存比對憑證的 OutputFile： 檔案

使用-使用者存取的使用者存放區，而不是電腦存放區。

使用-enterprise 能夠存取電腦的企業存放區。

使用-存取機器服務存放區的服務。

您可以使用-grouppolicy 來存取電腦群組原則存放區。

範例：

1. -enterprise NTAuth
2. -企業根 37
3. -使用者我 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回[功能表](#menu)

## <a name="-dspublish"></a>-dsPublish

[選項] CertUtil-dsPublish CertFile [NTAuthCA |RootCA |SubCA |CrossCA |KRA |使用者 |機器]

CertUtil [Options] -dsPublish CRLFile [DSCDPContainer [DSCDPCN]]

將憑證或 CRL 發佈到 Active Directory

要發行的憑證檔案： 憑證檔案

NTAuthCA:將憑證發佈到 DS 企業存放區

RootCA:將憑證發佈到 DS 受信任的根存放區

SubCA:將 CA 憑證發佈到 DS CA 物件

CrossCA:發行交叉憑證到 DS CA 物件

KRA:將憑證發佈到 DS Key Recovery Agent 物件

使用者：將憑證發佈到使用者 DS 物件

電腦：將憑證發佈到機器 DS 物件

CRLFile:若要發佈的 CRL 檔案

DSCDPContainer:DS CDP 容器 CN，通常是 CA 電腦名稱

DSCDPCN:DS CDP 物件 CN，通常取決於處理過的 CA 簡短名稱和索引鍵索引

您可以使用-f 建立 DS 物件。

[-f] [-user] [-dc DCName]

返回[功能表](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [Options] -ADTemplate [Template]

顯示 AD 範本

[-f] [-user] [-ut] [-mt] [-dc DCName]

## <a name="-template"></a>-Template

CertUtil [Options] -Template [Template]

顯示註冊原則範本

[-f] [-user] [-silent] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回[功能表](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [Options] -TemplateCAs Template

如需範本的顯示 Ca

[-f] [-user] [-dc DCName]

返回[功能表](#menu)

## <a name="-catemplates"></a>-CATemplates

CertUtil [Options] -CATemplates [Template]

CA 的顯示範本

[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]

返回[功能表](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [Options] -SetCASites [set] [SiteName]

CertUtil [Options] -SetCASites verify [SiteName]

CertUtil [Options] -SetCASites delete

設定、 驗證或刪除 CA 的網站名稱

- 針對單一 CA 使用-config 選項 （預設為所有 Ca）
- *SiteName*目標設為單一的 CA 時，才允許
- 使用-f 來覆寫指定的驗證錯誤*SiteName*
- 若要刪除所有 CA 網站名稱中使用-f

[-f] [-config Machine\CAName] [-dc DCName]

> [!NOTE]
> 如需有關如何設定 Active Directory 網域服務 (AD DS) 站台感知 Ca 的詳細資訊，請參閱[AD CS 和 PKI 用戶端的 AD DS 網站感知](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx)。

返回[功能表](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [Options] -enrollmentServerURL [URL AuthenticationType [Priority] [Modifiers]]

CertUtil [Options] -enrollmentServerURL URL delete

顯示、 新增或刪除與 CA 相關聯的註冊伺服器 Url

AuthenticationType:新增 URL 時指定其中一個下列的用戶端驗證方法

1. Kerberos:使用 Kerberos 的 SSL 憑證
2. 使用者名稱：使用名為 SSL 認證的帳戶
3. ClientCertificate:使用 X.509 憑證的 SSL 憑證
4. Anonymous：使用匿名的 SSL 憑證

刪除： 刪除指定的 URL 與 CA 相關聯

如果未指定時新增 URL 優先順序： 預設值為 '1'

修飾詞，以逗號分隔一或多個項目的清單：

1. AllowRenewalsOnly:只更新要求提交給此 CA，透過此 URL
2. AllowKeyBasedRenewal:允許使用的憑證，在 AD 中有任何相關聯的帳戶。 這適用於只使用 ClientCertificate 和 AllowRenewalsOnly 模式

[-config Machine\CAName][-dc DCName]

返回[功能表](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [Options] -ADCA [CAName]

顯示 AD Ca

[-f] [-split] [-dc DCName]

返回[功能表](#menu)

## <a name="-ca"></a>-CA

CertUtil [Options] -CA [CAName | TemplateName]

顯示註冊原則 Ca

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回[功能表](#menu)

## <a name="-policy"></a>-Policy

顯示註冊原則

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回[功能表](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [Options] -PolicyCache [delete]

顯示或刪除註冊原則快取項目

刪除： 刪除原則伺服器快取項目

-f： 刪除所有的快取項目使用-f

[-f] [-user] [-PolicyServer URLOrId]

返回[功能表](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [Options] -CredStore [URL]

CertUtil [Options] -CredStore URL add

CertUtil [Options] -CredStore URL delete

顯示、 新增或刪除認證存放區項目

URL： 目標 URL。  使用\*來比對所有項目。 使用 https://machine\* 來比對的 URL 前置詞。

新增： 加入認證存放區項目。 您也必須指定 SSL 認證。

刪除： 刪除認證存放區項目

-f： 覆寫項目，或刪除多個項目，請使用-f。

[-f] [-user] [-silent] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回[功能表](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [Options] -InstallDefaultTemplates

安裝預設憑證範本

[-dc DCName]

返回[功能表](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [Options] -URLCache [URL | CRL | \* [delete]]

顯示或刪除 URL 快取項目

URL： 快取的 URL

所有快取 CRL Url 只作用於 CRL:

\*： 對所有快取的 Url

刪除： 刪除相關的 Url，從目前使用者的本機快取

您可以使用-f，強制擷取特定的 URL，並更新快取。

[-f] [-split]

返回[功能表](#menu)

## <a name="-pulse"></a>-pulse

CertUtil [Options] -pulse

Pulse 自動註冊事件

[-使用者]

返回[功能表](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [Options] -MachineInfo DomainName\MachineName$

顯示 Active Directory 電腦物件資訊

返回[功能表](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [Options] -DCInfo [Domain] [Verify | DeleteBad | DeleteAll]

顯示網域控制站的資訊

預設值是顯示 DC 未經驗證的憑證

[-f] [-user] [-urlfetch] [-dc DCName] [-t Timeout]

> [!TIP]
> 指定 Active Directory 網域服務 (AD DS) 網域的能力 **[網域]** ，並指定網域控制站 (**dc**) 在 Windows Server 2012 中新增。 若要成功執行命令，您必須使用成員的帳戶**Domain Admins**或是**Enterprise Admins**。 此命令的行為修改如下所示：</br>> 1.  如果未指定網域，且未指定特定的網域控制站，這個選項就會傳回一份網域控制站來處理從預設網域控制站。</br>> 2.  如果未指定網域，但網域控制站，則會產生指定的網域控制站上的憑證報表。</br>> 3.  如果已指定網域，但未指定的網域控制站，網域控制站的清單會產生連同憑證清單中的每個網域控制站上的報告。</br>> 4.  如果指定了網域和網域控制站，網域控制站的清單會產生目標的網域控制站。 此外，也會產生報表的清單中的每個網域控制站的憑證。

例如，假設名為 CPANDL DC1 是網域控制站名為 CPANDL 網域。 您可以執行下列命令來擷取網域控制站和其憑證的清單，從 CPANDL DC1: certutil-dc cpandl dc1-dcinfo cpandl

返回[功能表](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [Options] -EntInfo DomainName\MachineName$

[-f] [-user]

返回[功能表](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [Options] -TCAInfo [DomainDN | -]

顯示 CA 資訊

[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t Timeout]

返回[功能表](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [Options] -SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

顯示智慧卡資訊

CRYPT_DELETEKEYSET:刪除智慧卡上的所有索引鍵

[-silent] [-split] [-urlfetch] [-t Timeout]

返回[功能表](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [Options] -SCRoots update [+][InputRootFile] [ReaderName]

CertUtil [Options] -SCRoots save @OutputRootFile [ReaderName]

CertUtil [Options] -SCRoots view [InputRootFile | ReaderName]

CertUtil [Options] -SCRoots delete [ReaderName]

管理智慧卡的根憑證

[-f] [-split] [-p Password]

返回[功能表](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [Options] -verifykeys [KeyContainerName CACertFile]

確認公用/私用金鑰組

要驗證的金鑰 KeyContainerName： 金鑰容器名稱。 電腦金鑰的預設值。  使用-對使用者的金鑰的使用者。

CACertFile： 簽章或加密憑證的檔案

如果未不指定任何引數，每個簽章的 CA 憑證會驗證對其私密金鑰。

只能針對本機 CA 或本機金鑰執行此作業。

[-f]。[-使用者][-無訊息][-config Machine\CAName]

返回[功能表](#menu)

## <a name="-verify"></a>-verify

CertUtil [Options] -verify CertFile [ApplicationPolicyList | - [IssuancePolicyList]]

CertUtil [Options] -verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Options] -verify CRLFile CACertFile [IssuedCertFile]

CertUtil [Options] -verify CRLFile CACertFile [DeltaCRLFile]

確認憑證、 CRL 或鏈結

憑證檔案：若要確認憑證

需要應用程式原則的 Objectid ApplicationPolicyList： 選擇性的逗號分隔清單

需要發行原則的 Objectid IssuancePolicyList： 選擇性的逗號分隔清單

CACertFile： 選擇性發行 CA 憑證來進行驗證

CrossedCACertFile： 選擇性憑證交互檢定的憑證檔案

CRLFile:若要確認 CRL

選擇性 IssuedCertFile： 涵蓋 CRLFile 核發的憑證

DeltaCRLFile： 選擇性的 delta CRL

如果指定 ApplicationPolicyList，鏈結建立僅限於適用於指定應用程式原則的鏈結。

如果指定 IssuancePolicyList，鏈結建立限於鏈結適用於指定的發佈原則。

如果指定 CACertFile，針對 CertFile 或 CRLFile 驗證 CACertFile 中的欄位。

如果未指定 CACertFile，CertFile 用來建置和驗證完整鏈結中。

如果同時指定 CACertFile 和 CrossedCACertFile，CACertFile 和 CrossedCACertFile 中的欄位會驗證對憑證檔案。

如果指定 IssuedCertFile，則是會針對 CRLFile 驗證 IssuedCertFile 中的欄位。

如果指定 DeltaCRLFile，則是會針對 CRLFile 驗證 DeltaCRLFile 中的欄位。

[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t Timeout]

返回[功能表](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [Options] -verifyCTL CTLObject [CertDir] [CertFile]

請確認 AuthRoot 或不允許的憑證的 CTL

CTLObject:識別驗證 CTL:

- AuthRootWU： 從 URL 快取中讀取 AuthRoot 封包和相符的憑證。 使用-f 改為從 Windows Update 下載。
- DisallowedWU： 讀取不允許憑證封包，並不允許從 URL 快取的憑證存放區檔案。  使用-f 改為從 Windows Update 下載。
- AuthRoot： 讀取的登錄快取 AuthRoot CTL。  快取 AuthRoot 和不允許憑證的 Ctl 時，請使用-f 與已經不是強制更新登錄信任的憑證檔案。
- 不允許： 讀取的登錄快取不允許憑證的 CTL。 -f 會具有相同的行為如同 AuthRoot。
- CTLFileName： 檔案或 http: CTL 或封包的路徑

包含憑證相符 CTL 項目的 CertDir： 資料夾。 Http： 資料夾路徑必須以路徑分隔符號結尾。 如果 AuthRoot 或不允許使用未指定資料夾時，會比對憑證搜尋多個位置： 本機憑證存放區、 crypt32.dll 資源及本機 URL 快取。 若要下載從 Windows Update，必要時使用-f。 否則預設為相同的資料夾或網站為 CTLObject。

包含驗證憑證的憑證檔案： 檔案。 憑證會符合 CTL 的項目，且符合所顯示的結果。 隱藏大部分的預設輸出。

[-f] [-user] [-split]

返回[功能表](#menu)

## <a name="-sign"></a>-標誌

CertUtil [Options] -sign InFileList|SerialNumber|CRL OutFileList [StartDate+dd:hh] [+SerialNumberList | -SerialNumberList | -ObjectIdList | @ExtensionFile]

CertUtil [Options] -sign InFileList|SerialNumber|CRL OutFileList [#HashAlgorithm] [+AlternateSignatureAlgorithm | -AlternateSignatureAlgorithm]

重新簽署的 CRL 或憑證

若要修改並重新簽署的憑證或 CRL 檔案 InFileList： 逗號分隔清單

序號：若要建立的憑證序號。 不能存在有效期間和其他選項。

CRL:建立空的 CRL。 不能存在有效期間和其他選項。

修改的憑證或 CRL 輸出檔案 OutFileList： 以逗號分隔清單。 檔案的數目必須符合 InFileList。

StartDate + dd:hh： 新的有效期間： 選擇性的日期加上;選擇性的天與小時的有效期間;如果同時指定這兩者，請使用加號 （+） 分隔符號。 使用 [目前 [+ dd:hh]] 以在目前的時間開始。 使用 「 從未 」 沒有到期日 （適用於僅 Crl)。

SerialNumberList： 以逗號分隔的數列數字清單新增或移除

若要移除 ObjectIdList： 以逗號分隔副檔名 ObjectId 清單

@ExtensionFile: 包含要更新或移除延伸模組的 INF 檔案：

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm:前面加上 # 符號的雜湊演算法名稱

AlternateSignatureAlgorithm： 替代簽章演算法規範

以負號會導致序號和延伸模組被移除。 加號會導致加入 CRL 的序號。 時從 CRL 中移除項目，此清單可能包含序號和 Objectid。 AlternateSignatureAlgorithm 前面的減號會造成使用舊的簽章格式。 加號才 AlternateSignatureAlgorithm 引發 alternature 簽章格式，才能使用。 如果未指定 AlternateSignatureAlgorithm 則會使用憑證或 CRL 的簽章格式。

[-nullsign] [-f] [-silent] [-Cert CertId]

返回[功能表](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [Options] -vroot [delete]

建立/刪除 web 虛擬根目錄和檔案共用

返回[功能表](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [Options] -vocsproot [delete]

建立/刪除 web 虛擬根目錄 OCSP web proxy

返回[功能表](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [Options] -addEnrollmentServer Kerberos | UserName | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

新增註冊伺服器應用程式

新增註冊的伺服器應用程式和應用程式集區中，使用如有需要，針對指定的 CA。 此命令不會安裝二進位檔或封裝。 其中一個用戶端連線到憑證註冊伺服器的下列驗證方法。

- Kerberos:使用 Kerberos 的 SSL 憑證
- 使用者名稱：使用名為 SSL 認證的帳戶
- ClientCertificate:使用 X.509 憑證的 SSL 憑證
- AllowRenewalsOnly:只更新要求提交給此 CA，透過此 URL
- AllowKeyBasedRenewal-允許使用的憑證，在 AD 中有任何相關聯的帳戶。 這適用於只使用 ClientCertificate 和 AllowRenewalsOnly 模式。

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [Options] -deleteEnrollmentServer Kerberos | UserName | ClientCertificate

刪除註冊伺服器應用程式

刪除註冊伺服器應用程式和應用程式集區中，使用如有需要，針對指定的 CA。 此命令不會移除二進位檔或封裝。 其中一個用戶端連線到憑證註冊伺服器的下列驗證方法。

1. Kerberos:使用 Kerberos 的 SSL 憑證
2. 使用者名稱：使用名為 SSL 認證的帳戶
3. ClientCertificate:使用 X.509 憑證的 SSL 憑證

[-config Machine\CAName]

返回[功能表](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [Options] -addPolicyServer Kerberos | UserName | ClientCertificate [KeyBasedRenewal]

新增原則伺服器的應用程式

如有必要，請加入原則的伺服器應用程式和應用程式集區。 此命令不會安裝二進位檔或封裝。 其中一種用戶端連線到憑證原則伺服器的下列驗證方法：

- Kerberos:使用 Kerberos 的 SSL 憑證
- 使用者名稱：使用名為 SSL 認證的帳戶
- ClientCertificate:使用 X.509 憑證的 SSL 憑證
- KeyBasedRenewal:只有包含 KeyBasedRenewal 範本的原則才會傳回給用戶端。 這個旗標僅適用於使用者名稱和 ClientCertificate 驗證。

返回[功能表](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [Options] -deletePolicyServer Kerberos | UserName | ClientCertificate [KeyBasedRenewal]

刪除原則伺服器應用程式

如有必要，請刪除原則的伺服器應用程式和應用程式集區。 此命令不會移除二進位檔或封裝。 其中一種用戶端連線到憑證原則伺服器的下列驗證方法：

1. Kerberos:使用 Kerberos 的 SSL 憑證
2. 使用者名稱：使用名為 SSL 認證的帳戶
3. ClientCertificate:使用 X.509 憑證的 SSL 憑證
4. KeyBasedRenewal:KeyBasedRenewal 原則伺服器

返回[功能表](#menu)

## <a name="-oid"></a>-oid

CertUtil [Options] -oid ObjectId [DisplayName | delete [LanguageId [Type]]]

CertUtil [Options] -oid GroupId

CertUtil [Options] -oid AlgId | AlgorithmName [GroupId]

顯示 ObjectId 或設定顯示名稱

- ObjectId-ObjectId 來顯示或新增顯示名稱
- GroupId-十進位 GroupId 數字要列舉的 Objectid
- AlgId-十六進位 AlgId 為查閱的 ObjectId 的
- 若要查閱的 ObjectId 的 AlgorithmName-演算法名稱
- 將存放於 DS DisplayName-顯示名稱
- 刪除-刪除顯示名稱
- LanguageId-語言識別碼 (目前的預設值：1033)
- 類型--DS 物件建立的類型：範本 （預設值），2 代表發佈原則，應用程式原則 3 1
- 您可以使用-f 建立 DS 物件。

[-f]

返回[功能表](#menu)

## <a name="-error"></a>-error

CertUtil [Options] -error ErrorCode

顯示錯誤訊息文字

返回[功能表](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [Options] -getreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]][RegistryValueName]

顯示登錄值

ca:使用 CA 的登錄機碼

還原：使用 CA 還原登錄機碼

原則：使用原則模組的登錄機碼

結束：使用先結束模組的登錄機碼

範本：使用範本的登錄機碼 （使用-使用者範本的使用者）

註冊：使用註冊登錄機碼 （使用-使用者內容的使用者）

鏈結：使用鏈結 configuration 登錄機碼

PolicyServers:使用原則伺服器登錄機碼

ProgId:使用原則或結束模組的 ProgId （登錄子機碼名稱）

RegistryValueName： 登錄值名稱 (使用"名稱\*"為前置詞比對)

值： 新的數值、 字串或日期的登錄值或檔案名稱。 如果數字值的開頭"+"或"-"，指定新值的位元會設定或清除現有的登錄值中。

如果字串值的開頭"+"或"-"，與現有的值為 REG_MULTI_SZ 值、 加入或移除現有的登錄值的字串。 若要強制建立 REG_MULTI_SZ 的值，請新增"\n"結尾的字串值。

如果值的開頭"@"，值的其餘部分是包含二進位值的十六進位文字表示的檔案名稱。 如果它未參考有效的檔案，它會改為剖析，為 [Date] [+ |-] [dd:hh]-選用的日期，加或減選擇性的天數和時數。 如果同時指定這兩者，請使用加號 （+） 或減號 （-） 分隔。 相對於目前時間的日期，請使用 [now + dd:hh]。

使用 「 chain\ChainCacheResyncFiletime @now」 有效地清除快取的 Crl。

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [Options] -setreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]]RegistryValueName Value

設定登錄值

ca:使用 CA 的登錄機碼

還原：使用 CA 還原登錄機碼

原則：使用原則模組的登錄機碼

結束：使用先結束模組的登錄機碼

範本：使用範本的登錄機碼 （使用-使用者範本的使用者）

註冊：使用註冊登錄機碼 （使用-使用者內容的使用者）

鏈結：使用鏈結 configuration 登錄機碼

PolicyServers:使用原則伺服器登錄機碼

ProgId:使用原則或結束模組的 ProgId （登錄子機碼名稱）

RegistryValueName： 登錄值名稱 (使用"名稱\*"為前置詞比對)

值： 新的數值、 字串或日期的登錄值或檔案名稱。 如果數字值的開頭"+"或"-"，指定新值的位元會設定或清除現有的登錄值中。

如果字串值的開頭"+"或"-"，與現有的值為 REG_MULTI_SZ 值、 加入或移除現有的登錄值的字串。 若要強制建立 REG_MULTI_SZ 的值，請新增"\n"結尾的字串值。

如果值的開頭"@"，值的其餘部分是包含二進位值的十六進位文字表示的檔案名稱。 如果它未參考有效的檔案，它會改為剖析，為 [Date] [+ |-] [dd:hh]-選用的日期，加或減選擇性的天數和時數。 如果同時指定這兩者，請使用加號 （+） 或減號 （-） 分隔。 相對於目前時間的日期，請使用 [now + dd:hh]。

使用 「 chain\ChainCacheResyncFiletime @now」 有效地清除快取的 Crl。

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [Options] -delreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]][RegistryValueName]

刪除登錄值

ca:使用 CA 的登錄機碼

還原：使用 CA 還原登錄機碼

原則：使用原則模組的登錄機碼

結束：使用先結束模組的登錄機碼

範本：使用範本的登錄機碼 （使用-使用者範本的使用者）

註冊：使用註冊登錄機碼 （使用-使用者內容的使用者）

鏈結：使用鏈結 configuration 登錄機碼

PolicyServers:使用原則伺服器登錄機碼

ProgId:使用原則或結束模組的 ProgId （登錄子機碼名稱）

RegistryValueName： 登錄值名稱 (使用"名稱\*"為前置詞比對)

值： 新的數值、 字串或日期的登錄值或檔案名稱。 如果數字值的開頭"+"或"-"，指定新值的位元會設定或清除現有的登錄值中。

如果字串值的開頭"+"或"-"，與現有的值為 REG_MULTI_SZ 值、 加入或移除現有的登錄值的字串。 若要強制建立 REG_MULTI_SZ 的值，請新增"\n"結尾的字串值。

如果值的開頭"@"，值的其餘部分是包含二進位值的十六進位文字表示的檔案名稱。 如果它未參考有效的檔案，它會改為剖析，為 [Date] [+ |-] [dd:hh]-選用的日期，加或減選擇性的天數和時數。 如果同時指定這兩者，請使用加號 （+） 或減號 （-） 分隔。 相對於目前時間的日期，請使用 [now + dd:hh]。

使用 「 chain\ChainCacheResyncFiletime @now」 有效地清除快取的 Crl。

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [Options] -ImportKMS UserKeyAndCertFile [CertId]

使用者金鑰和憑證匯入金鑰保存的伺服器資料庫

UserKeyAndCertFile-資料檔包含使用者的私密金鑰和要封存的憑證。  這可以是下列其中一項：

- Exchange 金鑰管理伺服器 (KMS) 匯出檔案
- PFX 檔案

CertId:KMS 匯出檔案解密的憑證相符的語彙基元。  請參閱[-儲存](#-store)。

您可以使用-f，匯入不是由 CA 簽發的憑證。

[-f] [-silent] [-split] [-config Machine\CAName] [-p Password] [-symkeyalg SymmetricKeyAlgorithm[,KeyLength]]

返回[功能表](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [Options] -ImportCert Certfile [ExistingRow]

憑證檔案匯入資料庫

您可以使用 ExistingRow 匯入的憑證來取代相同索引鍵的暫止要求。

您可以使用-f，匯入不是由 CA 簽發的憑證。

CA 可能也需要設定為支援外部憑證匯入： certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

返回[功能表](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [Options] -GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [選項]-GetKey SearchToken 指令碼 OutputScriptFile

CertUtil [選項]-GetKey SearchToken 擷取 |復原 OutputFileBaseName

擷取已封存的私密金鑰復原 blob、 產生復原指令碼，或復原備份的金鑰

指令碼： 產生指令碼來擷取和還原的金鑰 （預設行為或如果找不到多個相符的復原候選項目，如果輸出檔案不是指定）。

擷取： 擷取一或多個金鑰復原 Blob （如果找到完全相符復原候選項目，並指定輸出檔，預設行為）

復原： 擷取和還原 （需要 Key Recovery Agent 憑證和私密金鑰） 的一個步驟中的私用金鑰

SearchToken:用來選取要復原的憑證與金鑰。

可以是下列其中一項：

1. 憑證一般名稱
2. 憑證序號
3. 憑證的 sha-1 雜湊 （指紋）
4. 憑證 KeyId sha-1 雜湊 （主體金鑰識別項）
5. 要求者名稱 （網域 \ 使用者）
6. UPN (user@domain)

包含憑證鏈結和相關聯的私密金鑰，仍會加密到一個或多個 Key Recovery Agent 憑證 RecoveryBlobOutFile： 輸出檔。

包含批次指令碼來擷取並復原私密金鑰 OutputScriptFile： 輸出檔。

OutputFileBaseName： 輸出檔主檔名。 擷取，任何延伸模組會截斷並針對每個金鑰修復 blob 附加憑證特定字串和.rec 延伸模組。  每個檔案包含憑證鏈結和相關聯的私密金鑰，仍會加密到一個或多個 Key Recovery Agent 憑證。 復原的任何延伸模組會截斷並.p12 副檔名附加。  包含修復的憑證鏈結和相關聯的私密金鑰，儲存為 PFX 檔案。

[-f] [-UnicodeText] [-silent] [-config Machine\CAName] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

返回[功能表](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [Options] -RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

復原封存的私密金鑰

[-f] [-user] [-silent] [-split] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider] [-t Timeout]

返回[功能表](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [Options] -MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList:以逗號分隔的 PFX 輸入的檔清單

PFXOutFile:輸出的 PFX 檔案

ExtendedProperties:包含擴充的屬性

在命令列上指定的密碼是以逗號分隔的密碼清單。  如果指定一個以上的密碼，則上次的密碼用於輸出檔案中。  如果只有提供一個密碼，或上次的密碼是 「\*」，將會提示使用者輸入輸出檔案的密碼。

[-f] [-user] [-split] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

返回[功能表](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [Options] -ConvertEPF PFXInFileList EPFOutFile [cast | cast-] [V3CACertId][,Salt]

將 PFX 檔案轉換成 EPF 檔案

PFXInFileList:以逗號分隔的 PFX 輸入的檔清單

EPF:EPF 輸出檔案

轉換：使用 CAST 64 加密

轉型為：使用 CAST 64 加密 （匯出）

V3CACertId:V3 CA 的憑證相符的語彙基元。  請參閱[-儲存](#-store)CertId 描述。

Salt:EPF 輸出檔案 salt 字串

在命令列上指定的密碼是以逗號分隔的密碼清單。 如果指定一個以上的密碼，則上次的密碼用於輸出檔案中。  如果只有提供一個密碼，或上次的密碼是 「\*」，將會提示使用者輸入輸出檔案的密碼。

[-f] [-silent] [-split] [-dc DCName] [-p Password] [-csp Provider]

返回[功能表](#menu)

## <a name="options"></a>選項。

本章節會定義您可以使用此命令指定的選項。

|選項。|描述|
|-------|-----------|
|-nullsign|資料的雜湊做為簽章|
|-f|強制覆寫|
|-enterprise|使用本機企業登錄憑證存放區|
|-使用者|HKEY_CURRENT_USER 所使用的金鑰或憑證存放區|
|-GroupPolicy|使用群組原則的憑證存放區|
|-ut|顯示使用者範本|
|-mt|顯示機器的範本|
|-Unicode|以 Unicode 撰寫重新導向的輸出|
|-UnicodeText|以 Unicode 寫入輸出檔案|
|-gmt|顯示時間為 GMT|
|-seconds|以秒和毫秒為單位顯示時間|
|-無訊息|使用無訊息的旗標來取得密碼編譯內容|
|-split|分割內嵌的 ASN.1 項目，並儲存到檔案|
|-v|詳細資訊的作業|
|-privatekey|顯示密碼和私密金鑰的資料|
|-釘選 釘選|智慧卡 PIN|
|-urlfetch|擷取，並確認憑證 AIA 和 CDP Crl|
|-config Machine\CAName|CA 和電腦名稱的字串|
|-PolicyServer URLOrId|原則伺服器 URL 或 id。選取 U，使用-PolicyServer /。 針對所有的原則伺服器，請使用-PolicyServer \*|
|-Anonymous|使用匿名的 SSL 憑證|
|-Kerberos|使用 Kerberos 的 SSL 憑證|
|-ClientCertificate ClientCertId|使用 X.509 憑證的 SSL 認證。 選取 U / 我使用 clientCertificate。|
|-UserName 使用者名稱|您可以使用具名的帳戶取得 SSL 認證。 選取 U / 我使用-UserName。|
|憑證 CertId|簽署憑證|
|dc DCName|目標特定的網域控制站|
|-限制 RestrictionList|以逗號分隔的限制清單。 每個限制是由資料行名稱、 關係運算子和常數整數、 字串或日期所組成。 可能前面加入一個資料行名稱，正或負號來指示排序順序。 範例：</br>「 RequestId = 47"</br>"+ RequesterName > =，RequesterName < b"</br>"-RequesterName > 網域]、 [配置 = 21 」|
|-out columnlist 就會|以逗號分隔的資料行清單|
|-p 密碼|密碼|
|-ProtectTo SAMNameAndSIDList|以逗號分隔的 SAM 名稱/SID 清單|
|-csp 提供者|提供者|
|-t 逾時|以毫秒為單位的 URL 擷取逾時|
|-symkeyalg SymmetricKeyAlgorithm[,KeyLength]|選擇性的金鑰長度的對稱金鑰演算法名稱範例：AES 128 或 3DES|

返回[功能表](#menu)

## <a name="additional-certutil-examples"></a>其他的 certutil 範例

如需如何使用此命令的一些範例，請參閱

1. [從命令列管理 Active Directory 憑證服務 (AD CS) 的 Certutil 範例](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [管理憑證的 Certutil 工作](https://technet.microsoft.com/library/cc772898.aspx)
3. [使用 CertUtil.exe 命令列工具的逐步解說的二進位要求匯出](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [根 CA 憑證更新](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

返回[功能表](#menu)
