---
title: Scwcmd 視圖
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 985b8edb2ef6a99abeb9545dfe62fa5255080b2c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636904"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> 適用于： Windows Server 2012 R2、Windows Server 2012

使用指定的 xsl 轉換轉譯 .xml 檔案。 此命令適用于使用不同的視圖顯示安全性設定 Wizard (SCW) .xml 檔案。

## <a name="syntax"></a>語法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/x\<Xmlfile.xml>|指定要查看的 .xml 檔案。 必須指定這個參數。|
|/s\<Xslfile.xsl>|指定要在轉譯過程中套用至 .xml 檔案的 .xsl 轉換。 這個參數對於 SCW .xml 檔案而言是選擇性的。 當 **view** 命令用來轉譯 SCW .xml 檔案時，它會自動嘗試載入指定之 .xml 檔案的正確預設轉換。 如果指定了 .xsl 轉換，則必須撰寫轉換，因為 .xml 檔案位於與 .xsl 轉換相同的目錄中。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要使用 Policyview xsl 轉換來查看 Policyfile.xml，請輸入：
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)