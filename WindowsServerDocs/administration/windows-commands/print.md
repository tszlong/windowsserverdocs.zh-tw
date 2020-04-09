---
title: print
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36966d8d3beb032ee0dcee50d9bd5bc0111bf4f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837371"
---
# <a name="print"></a>print



將文字檔傳送至印表機。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/d：\<PrinterName >|指定您要列印工作的印表機。 若要列印到本機連線的印表機，請指定印表機連線所在電腦上的埠。</br>-平行埠的有效值為 LPT1、LPT2 和 LPT3。</br>-有效的序列埠值為 COM1、COM2、COM3 和 COM4。</br>您也可以使用其佇列名稱（\\\\*ServerName*\*PrinterName *）來指定網路印表機。 如果您沒有指定印表機，列印工作預設會傳送至 LPT1。|
|\<磁片磁碟機 >：|指定您要列印之檔案所在的邏輯或實體磁片磁碟機。 如果您要列印的檔案位於目前磁片磁碟機上，則不需要此參數。|
|\<路徑 >|指定您要列印之檔案的位置。 如果您要列印的檔案位於目前目錄中，則不需要這個參數。|
|\<FileName > [...]|必要。 指定您要列印的檔案。 您可以在一個命令中包含多個檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果您將檔案傳送到連接至本機電腦上序列或平行埠的印表機，檔案就可以在背景列印。
-   您可以使用**Mode**命令，從命令提示字元執行許多設定工作。

    如需的詳細資訊，請參閱[模式](mode.md)：  
    -   設定連接到平行埠的印表機
    -   設定連接至序列埠的印表機
    -   顯示印表機的狀態
    -   準備印表機以進行字碼頁切換

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將目前目錄中的檔案報告 .txt 傳送至本機電腦上的 LPT2，請輸入：
```
print /d:lpt2 report.txt
```
若要將 c:\Accounting 目錄中的檔案報告 .txt 傳送至 \\\\CopyRoom 伺服器上的 Printer1 列印佇列，請輸入：
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

[列印命令參考資料](print-command-reference.md)

[Mode](mode.md)