---
title: Scwcmd 分析
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce3428b281ede582ed781afbdee9dea495b52ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873909"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> 適用於：Windows Server 2012 R2, Windows Server 2012

判斷電腦是否符合原則。 .Xml 檔案中，會傳回結果。 也接受一份電腦名稱做為輸入。 若要在瀏覽器中檢視結果，請使用**scwcmd 檢視**並指定 **%windir%\security\msscw\TransformFiles\scwanalysis.xsl**為.xsl 轉換。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/m:\<電腦名稱 >|請指定 NetBIOS 名稱、 DNS 名稱或要分析之電腦的 IP 位址。 如果 **/m**指定參數，則 **/p**也必須指定參數。|
|/ou:\<OuName >|指定的 Active Directory 網域服務中的組織單位 (OU) 的完整的網域名稱 (FQDN)。 如果 **/ou**指定參數，則 **/p**也必須指定參數。 針對指定的原則，就會分析在 OU 中的所有電腦。|
|/p:\<原則 >|指定用來執行分析的.xml 原則檔案路徑和檔案名稱。|
|/i:\<ComputerList >|指定.xml 檔案，其中包含一份電腦以及其預期的原則檔案路徑和檔案的名稱。 針對其對應的原則檔，就會分析的.xml 檔案中的所有電腦。 範例.xml 檔案是 %windir%\security\samplemachinelist.xml。|
|/o\<ResultDir >|指定用來儲存分析的結果檔案的目錄與路徑。 預設值是目前的目錄。|
|您\<使用者名稱 >|指定遠端電腦上執行分析時要使用替代的使用者認證。 預設為已登入的使用者。|
|/pw:\<密碼 >|指定遠端電腦上執行分析時要使用替代的使用者認證。 預設為已登入使用者的密碼。|
|/t:\<執行緒 >|指定應該在分析期間維護的同時未完成的分析作業的數目 (預設值 = 40，MinValue = 1，MaxValue = 1000年)。|
|/l|會導致會記錄在分析程序。 正在分析每一部電腦時，將產生一個記錄檔。 記錄檔會儲存為結果檔案相同的目錄中。 使用 **/o**選項來指定結果檔案的目錄。|
|/e|如果找到不相符，請在應用程式事件記錄檔記錄事件。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

只有在執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的電腦上使用 Scwcmd.exe。

## <a name="BKMK_Examples"></a>範例

若要分析檔案 webpolicy.xml 針對安全性原則，請輸入：
```
scwcmd analyze /p:webpolicy.xml

```
若要分析使用 webadmin 帳戶的認證來命名檔案 webpolicy.xml 對 web 伺服器的電腦上的安全性原則，請輸入：
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
若要分析的安全性原則，針對檔案 webpolicy.xml，最多 100 個執行緒，並將結果輸出至名為 results resultserver 共用中的檔案，請輸入：
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
若要分析對檔案 webpolicy.xml WebServers ou 的安全性原則，使用 DomainAdmin 認證，請輸入：
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)