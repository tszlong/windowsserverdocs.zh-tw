---
title: Scwcmd 視圖
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c969931301afaab6cdf00e5a0238715fc655a79e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883079"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> 適用于： Windows Server 2012 R2、Windows Server 2012

使用指定的 .xsl 轉換來呈現 .xml 檔案。 此命令可讓您使用不同的視圖，將安全性設定向導顯示 (SCW) .xml 檔案中。

## <a name="syntax"></a>語法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/x\<Xmlfile.xml>|指定要查看的 .xml 檔案。 必須指定這個參數。|
|/s\<Xslfile.xsl>|指定要套用至 .xml 檔案的 .xsl 轉換，做為轉譯進程的一部分。 針對 SCW .xml 檔案，這個參數是選擇性的。 當**view**命令用來轉譯 SCW .xml 檔案時，它會自動嘗試為指定的 .xml 檔案載入正確的預設轉換。 如果指定了 .xsl 轉換，則必須撰寫轉換，因為 .xml 檔案與 .xsl 轉換位於相同的目錄中。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要使用 Policyview 轉換來查看 Policyfile.xml，請輸入：
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)