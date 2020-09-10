---
title: Scwcmd 轉換
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0ac70c1d8f19c0824e1ea432fa719875a0d89fea
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636933"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> 適用于： Windows Server 2012 R2、Windows Server 2012

將使用 [安全性設定] (SCW) 產生的安全性原則檔案轉換成) Active Directory Domain Services 中的新群組原則物件 (GPO。 轉換作業不會變更執行的伺服器上的任何設定。 轉換作業完成之後，系統管理員必須將 GPO 連結到所需的 Ou，以將原則部署至伺服器。

需要有網域系統管理員認證，才能完成轉換操作。

> [!IMPORTANT]
> Internet Information Services 無法使用群組原則部署 (的 IIS) 安全性原則設定。</br>除非 Windows 防火牆服務在伺服器上次啟動時自動啟動，否則不應將列出已核准應用程式的 > 防火牆原則部署到伺服器。



## <a name="syntax"></a>語法

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/p\<Policyfile.xml>|指定應套用之 .xml 原則檔的路徑和檔案名。 必須指定這個參數。|
|/g\<GPODisplayName>|指定 GPO 的顯示名稱。 必須指定這個參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要從名為 FileServerPolicy.xml 的檔案建立名為 FileServerSecurity 的 GPO，請輸入：
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)