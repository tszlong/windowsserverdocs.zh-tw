---
title: Scwcmd 設定
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31838ac7299cc30a7b7dde3beb47669df772487c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857499"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> 適用於：Windows Server 2012 R2, Windows Server 2012

將安全性設定精靈 SCW 產生的安全性原則套用到電腦。 這個命令列工具也會接受一份電腦名稱做為輸入。

## <a name="syntax"></a>語法

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/m:\<電腦名稱 >|請指定 NetBIOS 名稱、 DNS 名稱或若要設定電腦的 IP 位址。 如果 **/m**指定參數，則 **/p**也必須指定參數。|
|/ou:\<OuName >|指定的 Active Directory 網域服務中的組織單位 (OU) 的完整的網域名稱 (FQDN)。 如果 **/ou**指定參數，則 **/p**也必須指定參數。 根據指定的原則，就會分析在 OU 中的所有電腦。|
|/p:\<原則 >|指定用來執行設定的.xml 原則檔案路徑和檔案名稱。|
|/i:\<ComputerList >|指定.xml 檔案，其中包含一份電腦以及其預期的原則檔案路徑和檔案的名稱。 會根據其對應的原則檔中設定的.xml 檔案中的所有電腦。 範例.xml 檔案是 %windir%\security\samplemachinelist.xml。|
|您\<使用者名稱 >|指定替代的使用者認證，以便在設定遠端電腦時使用。 預設為已登入的使用者。|
|/pw:\<密碼 >|指定替代的使用者認證，以便在設定遠端電腦時使用。 預設為已登入使用者的密碼。|
|/t:\<執行緒 >|指定應該在設定處理期間維護的同時未完成的組態作業數目 (預設值 = 40，MinValue = 1，MaxValue = 1000年)。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

只有在執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的電腦上使用 Scwcmd.exe。

## <a name="BKMK_Examples"></a>範例

若要設定安全性原則檔 webpolicy.xml 針對，輸入：
```
scwcmd configure /p:webpolicy.xml
```
若要使用 webadmin 帳戶認證，在對檔案 webpolicy.xml 172.16.0.0 設定電腦的安全性原則，請輸入：
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
若要在清單 campusmachines.xml，最多 100 個執行緒上的所有電腦上設定安全性原則，請輸入：
```
scwcmd configure /i:campusmachines.xml /t:100
```
若要使用 DomainAdmin 帳戶的認證，對檔案 webpolicy.xml WebServers OU 中所有電腦上設定安全性原則，請輸入：
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)