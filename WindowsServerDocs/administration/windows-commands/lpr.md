---
title: lpr
description: Lpr 命令的參考文章，它會將檔案傳送到執行線上印表機 Daemon 的電腦或印表機共用裝置， (LPD) 服務以準備進行列印。
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a35c71dfed14e48464292f43d74928f437f4be8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887057"
---
# <a name="lpr"></a>lpr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳送到執行線上印表機背景程式的電腦或印表機共用裝置 (LPD) 服務，以準備進行列印。

## <a name="syntax"></a>語法

```
lpr [-S <servername>] -P <printername> [-C <bannercontent>] [-J <jobname>] [-o | -o l] [-x] [-d] <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -S`<servername>` | 依名稱或 IP 位址指定 (，) 裝載 LPD 列印佇列的電腦或印表機共用裝置，以及您要顯示的狀態。  此為必要參數，且必須為大寫。 |
| -P`<printername> `| 依名稱指定 (，) 列印佇列的印表機，以及您要顯示的狀態。 若要尋找印表機的名稱，請開啟 [**印表機**] 資料夾。 此為必要參數，且必須為大寫。 |
| -C`<bannercontent>` | 在列印工作的 [橫幅] 頁面上指定要列印的內容。 如果您未包含此參數，則會在 [橫幅] 頁面上顯示已傳送列印工作的電腦名稱稱。 這個參數必須是大寫。 |
| -J`<jobname>` | 指定將在橫幅頁面上列印的列印工作名稱。 如果您未包含此參數，則會在 [橫幅] 頁面上顯示要列印的檔案名。 這個參數必須是大寫。 |
| `[-o | -o l]` | 指定您要列印的檔案類型。 參數 **-o**指定您要列印文字檔。 參數 **-o l**會指定您要列印二進位檔案 (例如，) 的 PostScript 檔案。 |
| -d | 指定必須在控制檔案之前傳送資料檔案。 如果您的印表機需要先傳送資料檔案，請使用此參數。 如需詳細資訊，請參閱您的印表機檔。 |
| -X | 指定**lpr**命令必須與 Sun Microsystems 作業系統相容， (稱為 SunOS) ，適用于最多和包含 4.1. 4_u1。 |
| `<filename>` | 依名稱指定要列印之檔案)  (。 此為必要參數。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要在*10.0.0.45*上將*Document.txt*文字檔列印至 LPD 主機上的*Laserprinter1*印表機佇列，請輸入：

```
lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt
```

若要在*10.0.0.45*將*PostScript_file ps* Adobe POSTSCRIPT 檔案列印到 LPD 主機上的*Laserprinter1*印表機佇列，請輸入：

```
lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
