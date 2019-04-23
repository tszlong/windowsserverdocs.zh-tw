---
title: print
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d85fc5b2cd5f5ba09ebdf4756a5adb60c1759f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831549"
---
# <a name="print"></a>print



將文字檔案傳送到印表機。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/d:\<PrinterName >|指定您想要列印工作的印表機。 若要列印到本機連線的印表機，請印表機連線的位置在電腦上指定的連接埠。</br>-有效的值，平行連接埠為 LPT1、 LPT2、 和 LPT3。</br>-有效的值，針對序列連接埠為 COM1、 COM2、 COM3、 和 COM4。</br>您也可以使用其佇列名稱，指定網路印表機 (\\\\*ServerName*\*PrinterName *)。 如果您未指定印表機，列印工作傳送到 LPT1 預設。|
|\<磁碟機 >:|指定您想要列印的檔案所在的邏輯或實體磁碟機。 此參數不是必要的如果您想要列印的檔案位於目前的磁碟機上。|
|\<Path>|指定您想要列印的檔案的位置。 此參數不是必要的如果您想要列印的檔案位於目前目錄中。|
|\<FileName>[ ...]|必要。 指定您想要列印的檔案。 您可以在單一命令中包含多個檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果您將它傳送至印表機，連接到本機電腦上的序列或平行連接埠，可以在背景中列印檔案。
-   您也可以使用命令提示字元中執行許多組態工作**模式**命令。

    請參閱[模式](mode.md)如需詳細資訊：  
    -   設定印表機連線到平行連接埠
    -   設定印表機連線至序列埠
    -   顯示印表機的狀態
    -   準備用於程式碼頁面切換的印表機

## <a name="BKMK_examples"></a>範例

若要傳送在本機電腦，所連線到 LPT2 Report.txt 印表機目前的目錄中的檔案，請輸入：
```
print /d:lpt2 report.txt
```
若要傳送 c:\Accounting 目錄中的檔案 Report.txt Printer1 列印佇列\\ \\CopyRoom 伺服器類型：
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[列印命令參考資料](print-command-reference.md)

[Mode](mode.md)