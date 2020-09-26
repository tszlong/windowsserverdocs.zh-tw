---
title: scwcmd analyze
description: Scwcmd 分析命令的參考文章，可判斷電腦是否符合原則。
ms.topic: reference
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0d891262ebf04b1b8e604bc4756a3ca05888f8fa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388604"
---
# <a name="scwcmd-analyze"></a>scwcmd analyze

> 適用于： Windows Server 2012 R2 和 Windows Server 2012

判斷電腦是否符合原則。 結果會以 .xml 檔案傳回。

此命令也會接受電腦名稱稱清單做為輸入。 若要在瀏覽器中查看結果，請使用 **scwcmd view** 並指定 `%windir%\security\msscw\TransformFiles\scwanalysis.xsl` 為 .xsl 轉換。

## <a name="syntax"></a>語法

```
scwcmd analyze [[[/m:<computername> | /ou:<OuName>] /p:<policy>] | /i:<computerlist>] [/o:<resultdir>] [/u:<username>] [/pw:<password>] [/t:<threads>] [/l] [/e]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 一樣`<computername>` | 指定要分析之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 如果指定了 **/m** 參數，則也必須指定 **/p** 參數。 |
| /ou`<OuName>` | 指定組織 (單位的完整功能變數名稱 (FQDN) Active Directory Domain Services 中) 的 OU。 如果指定 **/ou** 參數，則也必須指定 **/p** 參數。 OU 中的所有電腦都會針對指定的原則進行分析。 |
| /p`<policy>` | 指定要用來執行分析之 .xml 原則檔案的路徑和檔案名。 |
| /i`<computerlist>` | 指定 .xml 檔案的路徑和檔案名，其中包含電腦清單及其預期的原則檔。 .Xml 檔案中的所有電腦都會針對其對應的原則檔案進行分析。 範例 .xml 檔案為 `%windir%\security\SampleMachineList.xml` 。 |
| /o`<resultdir>` | 指定應儲存分析結果檔案的路徑和目錄。 預設值是目前的目錄。 |
| u`<username>` | 指定在遠端電腦上執行分析時所要使用的替代使用者認證。 預設值是登入的使用者。 |
| /pw`<password>` | 指定在遠端電腦上執行分析時所要使用的替代使用者認證。 預設值為登入使用者的密碼。 |
| /t:`<threads>` | 指定在分析期間應維持的同時未處理分析作業數目。 值範圍是1-1000，預設值是40。 |
| /l | 導致記錄分析進程。 系統會為每個要分析的電腦產生一個記錄檔。 記錄檔會儲存在與結果檔案相同的目錄中。 使用 **/o** 選項來指定結果檔的目錄。 |
| /e | 如果找不到相符專案，請將事件記錄到應用程式事件記錄檔。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要針對檔案 *webpolicy.xml*分析安全性原則，請輸入：

```
scwcmd analyze /p:webpolicy.xml
```

若要使用*webadmin*帳戶的認證，在名為*webpolicy.xml* *web*伺服器的電腦上分析安全性原則，請輸入：

```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin
```

若要對檔案 *webpolicy.xml*分析安全性原則（ *最多100個執行緒*），並將結果輸出至 *resultserver* 共用中名為「結果」的檔案，請輸入：

```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results
```

若要針對使用*DomainAdmin*認證*webpolicy.xml*的檔案來分析*WebServers OU*的安全性原則，請輸入：

```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [scwcmd configure 命令](scwcmd-configure.md)

- [scwcmd register 命令](scwcmd-register.md)

- [scwcmd rollback 命令](scwcmd-rollback.md)

- [scwcmd 轉換命令](scwcmd-transform.md)

- [scwcmd view 命令](scwcmd-view.md)