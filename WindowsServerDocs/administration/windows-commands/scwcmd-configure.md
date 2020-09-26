---
title: scwcmd configure
description: Scwcmd configure 命令的參考文章，此命令會將安全性設定 Wizard (SCW) 產生的安全性原則套用至電腦。
ms.topic: reference
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a57d2142f8fc7fd788a5669c5318ff6444c34734
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388585"
---
# <a name="scwcmd-configure"></a>scwcmd configure

> 適用于： Windows Server 2012 R2 和 Windows Server 2012

將 (SCW) 產生的安全性原則套用至電腦的安全性設定 Wizard。 此命令列工具也可接受電腦名稱稱清單做為輸入。

## <a name="syntax"></a>語法

```
scwcmd configure [[[/m:<computername> | /ou:<OuName>] /p:<policy>] | /i:<computerlist>] [/u:<username>] [/pw:<password>] [/t:<threads>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 一樣`<computername>` | 指定要設定之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 如果指定了 **/m** 參數，則也必須指定 **/p** 參數。 |
| /ou`<OuName>` | 指定組織 (單位的完整功能變數名稱 (FQDN) Active Directory Domain Services 中) 的 OU。 如果指定 **/ou** 參數，則也必須指定 **/p** 參數。 OU 中的所有電腦都會針對指定的原則進行設定。 |
| /p`<policy>` | 指定要用來執行設定之 .xml 原則檔案的路徑和檔案名。 |
| /i`<computerlist>` | 指定 .xml 檔案的路徑和檔案名，其中包含電腦清單及其預期的原則檔。 .Xml 檔案中的所有電腦都會針對其對應的原則檔案進行分析。 範例 .xml 檔案為 `%windir%\security\SampleMachineList.xml` 。 |
| u`<username>` | 指定在遠端電腦上執行設定時所要使用的替代使用者認證。 預設值是登入的使用者。 |
| /pw`<password>` | 指定在遠端電腦上執行設定時所要使用的替代使用者認證。 預設值為登入使用者的密碼。 |
| /t:`<threads>` | 指定在分析期間應維持的同時未處理設定作業數目。 值範圍是1-1000，預設值是40。 |
| /l | 導致記錄分析進程。 系統會為每個要分析的電腦產生一個記錄檔。 記錄檔會儲存在與結果檔案相同的目錄中。 使用 **/o** 選項來指定結果檔的目錄。 |
| /e | 如果找不到相符專案，請將事件記錄到應用程式事件記錄檔。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要針對檔案 *webpolicy.xml*設定安全性原則，請輸入：

```
scwcmd configure /p:webpolicy.xml
```

若要使用*webadmin*帳戶的認證，為*webpolicy.xml* *172.16.0.0*設定電腦的安全性原則，請輸入：

```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```

若要在清單 *campusmachines.xml* 的所有電腦上設定安全性原則， *最大值為100個執行緒*，請輸入：

```
scwcmd configure /i:campusmachines.xml /t:100
```

若要針對使用*DomainAdmin*認證*webpolicy.xml*的檔案設定*WebServers OU*的安全性原則，請輸入：

```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [scwcmd 分析命令](scwcmd-analyze.md)

- [scwcmd register 命令](scwcmd-register.md)

- [scwcmd rollback 命令](scwcmd-rollback.md)

- [scwcmd 轉換命令](scwcmd-transform.md)

- [scwcmd view 命令](scwcmd-view.md)