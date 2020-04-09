---
title: Scwcmd 視圖
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36c6422a0118b0c6d6d70adbadfb401532121c3f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835061"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> 適用目標︰Windows Server 2012 R2、Windows Server 2012

使用指定的 .xsl 轉換來呈現 .xml 檔案。 此命令有助於使用不同的視圖顯示安全性設定 Wizard （SCW） .xml 檔案。

## <a name="syntax"></a>語法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/x：\<Xmlfile >|指定要查看的 .xml 檔案。 必須指定這個參數。|
|/s：\<Xslfile. xsl >|指定要套用至 .xml 檔案的 .xsl 轉換，做為轉譯進程的一部分。 針對 SCW .xml 檔案，這個參數是選擇性的。 當**view**命令用來轉譯 SCW .xml 檔案時，它會自動嘗試為指定的 .xml 檔案載入正確的預設轉換。 如果指定了 .xsl 轉換，則必須撰寫轉換，因為 .xml 檔案與 .xsl 轉換位於相同的目錄中。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

若要使用 Policyview 轉換來查看 Policyfile，請輸入：
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)