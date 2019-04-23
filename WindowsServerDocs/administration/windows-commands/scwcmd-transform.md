---
title: Scwcmd 轉換
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8a6a6e37c2c2a362f3aa0aeadef615ff5065713f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843809"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> 適用於：Windows Server 2012 R2, Windows Server 2012

轉換到新群組原則物件 (GPO) 在 Active Directory 網域服務中使用的安全性設定精靈 (SCW) 所產生的安全性原則檔。 轉換作業不會變更執行所在的伺服器上的任何設定。 轉換作業完成之後，系統管理員必須 GPO 連結到所需的 Ou，以將原則部署至伺服器。

網域系統管理員認證才能完成轉換作業。

> [!IMPORTANT]
> 無法使用群組原則部署 Internet Information Services (IIS) 安全性原則設定。</br>> 防火牆原則，列出已核准的應用程式應該不會部署到伺服器的 Windows 防火牆服務自動啟動伺服器上次啟動。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/p:\<Policyfile.xml >|指定應套用.xml 原則檔案的路徑和檔案名稱。 必須指定這個參數。|
|/g:\<GPODisplayName>|指定 GPO 的顯示名稱。 必須指定這個參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

只有在執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的電腦上使用 Scwcmd.exe。

## <a name="BKMK_Examples"></a>範例

若要建立一個名為從名為 FileServerPolicy.xml FileServerSecurity 的 GPO，請輸入：
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)