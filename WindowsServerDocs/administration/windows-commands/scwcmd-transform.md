---
title: Scwcmd 轉換
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36ee3a99828c7fdd9d4fc0ca14cbc0e203b01ea0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384314"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> 適用於：Windows Server 2012 R2、Windows Server 2012

將使用安全性設定 Wizard （SCW）所產生的安全性原則檔案，轉換為 Active Directory Domain Services 中新的群組原則物件（GPO）。 轉換作業不會變更執行此作業之伺服器上的任何設定。 轉換作業完成後，系統管理員必須將 GPO 連結到所需的 Ou，才能將原則部署至伺服器。

需要網域系統管理員認證才能完成轉換作業。

> [!IMPORTANT]
> 無法使用群組原則部署 Internet Information Services （IIS）安全性原則設定。</br>除非在伺服器上一次啟動時自動啟動 Windows 防火牆服務，否則列出已核准應用程式的 > 防火牆原則不應部署到伺服器。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/p： \<Policyfile >|指定應套用之 .xml 原則檔的路徑和檔案名。 必須指定這個參數。|
|/g： \<GPODisplayName >|指定 GPO 的顯示名稱。 必須指定這個參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="BKMK_Examples"></a>典型

若要從名為 FileServerPolicy 的檔案建立名為 FileServerSecurity 的 GPO，請輸入：
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)