---
title: Scwcmd 設定
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac4333628c33b60daabbb6cff55575d6ec8cd5f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835181"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> 適用目標︰Windows Server 2012 R2、Windows Server 2012

將安全性設定向導（SCW）產生的安全性原則套用至電腦。 此命令列工具也會接受電腦名稱稱清單做為輸入。

## <a name="syntax"></a>語法

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/m：\<ComputerName >|指定要設定之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 如果指定 **/m**參數，則也必須指定 **/p**參數。|
|/ou：\<OuName >|在 Active Directory Domain Services 中指定組織單位（OU）的完整功能變數名稱（FQDN）。 如果指定 **/ou**參數，則也必須指定 **/p**參數。 OU 中的所有電腦都會根據指定的原則進行分析。|
|/p：\<原則 >|指定要用來執行設定之 .xml 原則檔案的路徑和檔案名。|
|/i：\<ComputerList >|指定 .xml 檔案的路徑和檔案名，其中包含一份電腦清單及其預期的原則檔案。 .Xml 檔案中的所有電腦都將根據其對應的原則檔案進行設定。 範例 .xml 檔案為%windir%\security\SampleMachineList.xml。|
|/u：\<UserName >|指定設定遠端電腦時要使用的替代使用者認證。 預設為登入的使用者。|
|/pw：\<密碼 >|指定設定遠端電腦時要使用的替代使用者認證。 預設值為登入使用者的密碼。|
|/t：\<執行緒 >|指定在設定程式期間應維持的同時未處理設定作業數目（DefaultValue = 40，MinValue = 1，int32.maxvalue = 1000）。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

若要針對 webpolicy 檔案設定安全性原則，請輸入：
```
scwcmd configure /p:webpolicy.xml
```
若要使用 webadmin 帳號憑證，針對檔案 webpolicy 設定電腦的安全性原則，請輸入：
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
若要在清單 campusmachines 上的所有電腦上設定安全性原則，最大值為100個執行緒，請輸入：
```
scwcmd configure /i:campusmachines.xml /t:100
```
若要使用 DomainAdmin 帳戶的認證，針對檔案 webpolicy 在 WebServers OU 中的所有電腦上設定安全性原則，請輸入：
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)