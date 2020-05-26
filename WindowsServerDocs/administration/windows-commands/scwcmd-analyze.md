---
title: Scwcmd 分析
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13e33329c399a77c1dd9b2e6ff63de6196c30420
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820988"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> 適用于： Windows Server 2012 R2、Windows Server 2012

判斷電腦是否符合原則。 結果會以 .xml 檔案傳回。 也接受電腦名稱稱清單做為輸入。 若要在瀏覽器中查看結果，請使用**scwcmd view** ，並將 **%windir%\security\msscw\TransformFiles\scwanalysis.xsl**指定為 .xsl 轉換。

## <a name="syntax"></a>語法

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|/m： \< ComputerName>|指定要分析之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 如果指定 **/m**參數，則也必須指定 **/p**參數。|
|/ou： \< OuName>|在 Active Directory Domain Services 中指定組織單位（OU）的完整功能變數名稱（FQDN）。 如果指定 **/ou**參數，則也必須指定 **/p**參數。 OU 中的所有電腦都會針對指定的原則進行分析。|
|/p： \< 原則>|指定要用來執行分析之 .xml 原則檔案的路徑和檔案名。|
|/i： \< ComputerList>|指定 .xml 檔案的路徑和檔案名，其中包含一份電腦清單及其預期的原則檔案。 .Xml 檔案中的所有電腦都會根據其對應的原則檔案進行分析。 範例 .xml 檔案為%windir%\security\SampleMachineList.xml。|
|/o： \< ResultDir>|指定應儲存分析結果檔案的路徑和目錄。 預設值是目前的目錄。|
|/u： \< UserName>|指定在遠端電腦上執行分析時要使用的替代使用者認證。 預設為登入的使用者。|
|/pw： \< 密碼>|指定在遠端電腦上執行分析時要使用的替代使用者認證。 預設值為登入使用者的密碼。|
|/t： \< 執行緒>|指定在分析期間應維持的同時未完成分析作業數目（DefaultValue = 40，MinValue = 1，int32.maxvalue = 1000）。|
|/l|會記錄分析進程。 系統會為每一部要分析的電腦產生一個記錄檔。 記錄檔將會儲存在與結果檔案相同的目錄中。 使用 **/o**選項來指定結果檔案的目錄。|
|/e|如果找到不相符的專案，請將事件記錄至應用程式事件記錄檔。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要針對 webpolicy 檔案分析安全性原則，請輸入：
```
scwcmd analyze /p:webpolicy.xml

```
若要使用 webadmin 帳戶的認證，在名為 web 伺服器的電腦上分析安全性原則，請輸入：
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
若要針對 webpolicy 檔案分析安全性原則（最多100個執行緒），並將結果輸出至 resultserver 共用中名為的檔案，請輸入：
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
若要使用 DomainAdmin 認證針對檔案 webpolicy 分析 WebServers OU 的安全性原則，請輸入：
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)