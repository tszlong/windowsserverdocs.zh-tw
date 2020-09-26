---
title: scwcmd view
description: Scwcmd view 命令的參考文章，此命令會使用指定的 .xsl 轉換轉譯 .xml 檔案。
ms.topic: reference
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e411bf06015684e7dedb94d109f5e4294f46af84
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388928"
---
# <a name="scwcmd-view"></a>scwcmd view

> 適用于： Windows Server 2012 R2 和 Windows Server 2012

使用指定的 xsl 轉換轉譯 .xml 檔案。 此命令適用于使用不同的視圖顯示安全性設定 Wizard (SCW) .xml 檔案。

## <a name="syntax"></a>語法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /x`<Xmlfile.xml>` | 指定要查看的 .xml 檔案。 必須指定這個參數。 |
| /s`<Xslfile.xsl>` | 指定要在轉譯過程中套用至 .xml 檔案的 .xsl 轉換。 這個參數對於 SCW .xml 檔案而言是選擇性的。 當 **view** 命令用來轉譯 SCW .xml 檔案時，它會自動嘗試載入指定之 .xml 檔案的正確預設轉換。 如果指定了 .xsl 轉換，則必須撰寫轉換，因為 .xml 檔案位於與 .xsl 轉換相同的目錄中。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="example"></a>範例

若要使用*Policyview xsl*轉換來查看*Policyfile.xml* ，請輸入：

```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [scwcmd 分析命令](scwcmd-analyze.md)

- [scwcmd configure 命令](scwcmd-configure.md)

- [scwcmd register 命令](scwcmd-register.md)

- [scwcmd rollback 命令](scwcmd-rollback.md)

- [scwcmd 轉換命令](scwcmd-transform.md)