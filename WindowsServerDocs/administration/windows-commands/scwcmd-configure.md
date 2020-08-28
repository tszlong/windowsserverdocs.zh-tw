---
title: Scwcmd 設定
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54faae6fd24aac91a94ec9ab1f373737569dda78
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036186"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> 適用于： Windows Server 2012 R2、Windows Server 2012

將 (SCW) 產生的安全性原則套用至電腦的安全性設定 Wizard。 此命令列工具也可接受電腦名稱稱清單做為輸入。

## <a name="syntax"></a>語法

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|一樣\<ComputerName>|指定要設定之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 如果指定了 **/m** 參數，則也必須指定 **/p** 參數。|
|/ou\<OuName>|指定組織 (單位的完整功能變數名稱 (FQDN) Active Directory Domain Services 中) 的 OU。 如果指定 **/ou** 參數，則也必須指定 **/p** 參數。 OU 中的所有電腦都會根據指定的原則進行分析。|
|/p\<Policy>|指定要用來執行設定之 .xml 原則檔案的路徑和檔案名。|
|/i\<ComputerList>|指定 .xml 檔案的路徑和檔案名，其中包含電腦清單及其預期的原則檔。 .Xml 檔案中的所有電腦都會根據其對應的原則檔案進行設定。 範例 .xml 檔案為% windir% \security\SampleMachineList.xml。|
|u\<UserName>|指定設定遠端電腦時要使用的替代使用者認證。 預設值是登入的使用者。|
|/pw\<Password>|指定設定遠端電腦時要使用的替代使用者認證。 預設值為登入使用者的密碼。|
|/t:\<Threads>|指定在設定程式期間應維持的同時未處理設定作業數目 (DefaultValue = 40，MinValue = 1，Timespan.maxvalue = 1000) 。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要針對檔案 webpolicy.xml 設定安全性原則，請輸入：
```
scwcmd configure /p:webpolicy.xml
```
若要針對使用 webadmin 帳號憑證 webpolicy.xml 的檔案設定電腦的安全性原則，請輸入：
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
若要在清單 campusmachines.xml 的所有電腦上設定安全性原則，最大值為100個執行緒，請輸入：
```
scwcmd configure /i:campusmachines.xml /t:100
```
若要使用 DomainAdmin 帳戶的認證，在 WebServers webpolicy.xml OU 中的所有電腦上設定安全性原則，請輸入：
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)