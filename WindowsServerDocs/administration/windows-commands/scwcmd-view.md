---
title: Scwcmd 檢視
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef1dd72903108edd6c5fb450c536a9325fcf546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889549"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> 適用於：Windows Server 2012 R2, Windows Server 2012

使用指定的.xsl 轉換，以呈現的.xml 檔案。 此命令可用於藉由使用不同的檢視中顯示的安全性設定精靈 (SCW) 的.xml 檔案。

## <a name="syntax"></a>語法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/x:\<Xmlfile.xml>|指定要檢視的.xml 檔案。 必須指定這個參數。|
|/s:\<Xslfile.xsl>|指定.xsl 来套用至轉換的.xml 檔案做為轉譯程序的一部分。 這個參數是選擇性的 SCW.xml 檔案。 當**檢視**命令用來呈現 SCW.xml 檔案，它會自動將會嘗試載入指定的.xml 檔案的正確預設轉換。 如果指定.xsl 轉換，則必須撰寫轉換假設為.xsl 轉換相同的目錄中的.xml 檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

只有在執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的電腦上使用 Scwcmd.exe。

## <a name="BKMK_Examples"></a>範例

若要檢視 Policyfile.xml 使用 Policyview.xsl 轉換，請輸入：
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)