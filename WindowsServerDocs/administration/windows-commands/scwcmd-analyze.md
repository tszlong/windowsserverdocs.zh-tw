---
title: Scwcmd 分析
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9ae0e162ca97c61bcbbc4026d3355302ef264ae4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637023"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> 適用于： Windows Server 2012 R2、Windows Server 2012

判斷電腦是否符合原則。 結果會以 .xml 檔案傳回。 也接受電腦名稱稱清單做為輸入。 若要在瀏覽器中查看結果，請使用 **scwcmd view** 並將 **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** 指定為 .xsl 轉換。

## <a name="syntax"></a>語法

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|一樣\<ComputerName>|指定要分析之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 如果指定了 **/m** 參數，則也必須指定 **/p** 參數。|
|/ou\<OuName>|指定組織 (單位的完整功能變數名稱 (FQDN) Active Directory Domain Services 中) 的 OU。 如果指定 **/ou** 參數，則也必須指定 **/p** 參數。 OU 中的所有電腦都會針對指定的原則進行分析。|
|/p\<Policy>|指定要用來執行分析之 .xml 原則檔案的路徑和檔案名。|
|/i\<ComputerList>|指定 .xml 檔案的路徑和檔案名，其中包含電腦清單及其預期的原則檔。 .Xml 檔案中的所有電腦都會針對其對應的原則檔案進行分析。 範例 .xml 檔案為% windir% \security\SampleMachineList.xml。|
|/o\<ResultDir>|指定應儲存分析結果檔案的路徑和目錄。 預設值是目前的目錄。|
|u\<UserName>|指定在遠端電腦上執行分析時所要使用的替代使用者認證。 預設值是登入的使用者。|
|/pw\<Password>|指定在遠端電腦上執行分析時所要使用的替代使用者認證。 預設值為登入使用者的密碼。|
|/t:\<Threads>|指定在分析 (DefaultValue = 40、MinValue = 1、int32.maxvalue = 1000) 期間應維持的同時未處理分析作業數目。|
|/l|導致記錄分析進程。 系統會為每個要分析的電腦產生一個記錄檔。 記錄檔會儲存在與結果檔案相同的目錄中。 使用 **/o** 選項來指定結果檔的目錄。|
|/e|如果找不到相符專案，請將事件記錄到應用程式事件記錄檔。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要針對檔案 webpolicy.xml 分析安全性原則，請輸入：
```
scwcmd analyze /p:webpolicy.xml

```
若要使用 webadmin 帳戶的認證，在名為 webpolicy.xml web 伺服器的電腦上分析安全性原則，請輸入：
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
若要對檔案 webpolicy.xml 分析安全性原則（最多100個執行緒），並將結果輸出至 resultserver 共用中名為「結果」的檔案，請輸入：
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
若要針對使用 DomainAdmin 認證 webpolicy.xml 的檔案來分析 WebServers OU 的安全性原則，請輸入：
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)