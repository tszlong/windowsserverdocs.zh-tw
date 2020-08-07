---
title: Scwcmd 轉換
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5af1f9d1f2ee5386da8b02f4142c156c3711852f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883091"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> 適用于： Windows Server 2012 R2、Windows Server 2012

使用 [安全性設定] 嚮導將產生的安全性原則檔案轉換 (SCW) 至) 中 (GPO Active Directory Domain Services 的新群組原則物件。 轉換作業不會變更執行此作業之伺服器上的任何設定。 轉換作業完成後，系統管理員必須將 GPO 連結到所需的 Ou，才能將原則部署至伺服器。

需要網域系統管理員認證才能完成轉換作業。

> [!IMPORTANT]
> Internet Information Services 無法使用群組原則來部署 (IIS) 安全性原則設定。</br>除非在伺服器上一次啟動時自動啟動 Windows 防火牆服務，否則列出已核准應用程式的 > 防火牆原則不應部署到伺服器。



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

若要從名為 FileServerPolicy.xml 的檔案中建立名為 FileServerSecurity 的 GPO，請輸入：
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)