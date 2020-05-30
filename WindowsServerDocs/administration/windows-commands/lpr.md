---
title: lpr
description: Lpr 命令的參考主題，它會將檔案傳送到執行線上印表機 Daemon （LPD）服務的電腦或印表機共用裝置，以準備進行列印。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d11b72ee807869613505052cfb51e80a89a80d70
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2020
ms.locfileid: "84223028"
---
# <a name="lpr"></a>lpr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳送到執行線上印表機 Daemon （LPD）服務的電腦或印表機共用裝置，以準備進行列印。

## <a name="syntax"></a>語法

```
lpr [-S <servername>] -P <printername> [-C <bannercontent>] [-J <jobname>] [-o | -o l] [-x] [-d] <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -S`<servername>` | 指定以您要顯示的狀態裝載 LPD 列印佇列的電腦或印表機共用裝置（依名稱或 IP 位址）。  此為必要參數，且必須為大寫。 |
| -P`<printername> `| 以您想要顯示之狀態的列印佇列指定（依名稱）印表機。 若要尋找印表機的名稱，請開啟 [**印表機**] 資料夾。 此為必要參數，且必須為大寫。 |
| -C`<bannercontent>` | 在列印工作的 [橫幅] 頁面上指定要列印的內容。 如果您未包含此參數，則會在 [橫幅] 頁面上顯示已傳送列印工作的電腦名稱稱。 這個參數必須是大寫。 |
| -J`<jobname>` | 指定將在橫幅頁面上列印的列印工作名稱。 如果您未包含此參數，則會在 [橫幅] 頁面上顯示要列印的檔案名。 這個參數必須是大寫。 |
| `[-o | -o l]` | 指定您要列印的檔案類型。 參數 **-o**指定您要列印文字檔。 參數 **-o l**指定您要列印二進位檔案（例如，PostScript 檔案）。 |
| -d | 指定必須在控制檔案之前傳送資料檔案。 如果您的印表機需要先傳送資料檔案，請使用此參數。 如需詳細資訊，請參閱您的印表機檔。 |
| -X | 指定**lpr**命令必須與 Sun Microsystems 作業系統（稱為 SunOS）相容，才能釋放到 4.1. 4_u1。 |
| `<filename>` | 指定要列印的檔案（依名稱）。 此為必要參數。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將檔 *.txt*文字檔列印至 LPD 主機上的*Laserprinter1*印表機佇列（位於*10.0.0.45*），請輸入：

```
lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt
```

若要在*10.0.0.45*將*PostScript_file ps* Adobe POSTSCRIPT 檔案列印到 LPD 主機上的*Laserprinter1*印表機佇列，請輸入：

```
lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
