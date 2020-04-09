---
title: diskcomp
description: 適用于 diskcomp 的 Windows 命令主題，它會比較兩個磁片的內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e36e644616e25659c1a2a5ca684e975fd06fc19f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845551"
---
# <a name="diskcomp"></a>diskcomp

比較兩個磁片磁碟機的內容。 如果在沒有參數的情況下使用， **diskcomp**會使用目前的磁片磁碟機來比較這兩個磁片。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
diskcomp [<Drive1>: [<Drive2>:]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive1 >|指定包含其中一個軟碟的磁片磁碟機。|
|\<Drive2 >|指定包含其他軟碟的磁片磁碟機。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 使用磁片

  **Diskcomp**命令只適用于磁片磁碟機。 您不能將**diskcomp**與硬碟搭配使用。 如果您為*Drive1*或*Drive2*指定硬碟， **diskcomp**會顯示下列錯誤訊息：  
  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```  
- 比較磁片

  如果要比較的兩個磁片上的所有追蹤都相同， **diskcomp**會顯示下列訊息：  
  ```
  Compare OK
  ```  
  如果追蹤不同， **diskcomp**會顯示類似下列的訊息：  
  ```
  Compare error on
  side 1, track 2
  ```  
  當**diskcomp**完成比較時，會顯示下列訊息：  
  ```
  Compare another diskette (Y/N)?
  ```  
  如果您按下 [Y]， **diskcomp**會提示您插入磁片以進行下一個比較。 如果您按 N， **diskcomp**會停止比較。

  當**diskcomp**進行比較時，它會忽略磁片的磁片區編號。
- 省略磁片磁碟機參數

  如果您省略*Drive2*參數， **diskcomp**會使用目前的磁片磁碟機來進行*Drive2*。 如果您省略這兩個磁片磁碟機參數， **diskcomp**會同時使用目前的磁片磁碟機。 如果目前的磁片磁碟機與*Drive1*相同， **diskcomp**會提示您視需要交換磁片。
- 使用一部磁片磁碟機

  如果您為*Drive1*和*Drive2*指定相同的軟碟磁碟機， **diskcomp**會使用一個磁片磁碟機來比較它們，並視需要提示您插入磁片。 視磁片的容量和可用的記憶體數量而定，您可能必須多次交換磁片。
- 比較不同類型的磁片

  **Diskcomp**無法比較單側磁片與雙側磁片，也不能與高密度磁片搭配雙密度磁片。 如果*Drive1*中的磁片與*Drive2*中的磁片類型不同， **diskcomp**就會顯示下列訊息：  
  ```
  Drive types or diskette types not compatible
  ```  
- 將**diskcomp**與網路和重新導向的磁片磁碟機搭配使用

  **Diskcomp**無法在網路磁碟機機或由**subst**命令所建立的磁片磁碟機上運作。 如果您嘗試使用**diskcomp**搭配任何一種類型的磁片磁碟機， **diskcomp**會顯示下列錯誤訊息：  
  ```
  Invalid drive specification
  ```  
- 比較原始磁片與複本

  當您使用**diskcomp**搭配使用**複製**所建立的磁片時， **diskcomp**可能會顯示類似下列的訊息：  
  ```
  Compare error on 
  side 0, track 0
  ```  
  即使磁片上的檔案完全相同，也可能會發生這種錯誤。 雖然**複製**重複資訊，但不一定要將它放在目的地磁片上的相同位置。
- 瞭解**diskcomp**結束代碼

  下表說明每個結束代碼。  

  |結束代碼|描述|
  |---------|-----------|
  |0|磁片相同|
  |1|發現差異|
  |3|發生硬錯誤|
  |4|發生初始化錯誤|

  若要處理**diskcomp**傳回的結束代碼，您可以在 batch 程式的**if**命令列上使用 ERRORLEVEL 環境變數。

## <a name="examples"></a><a name=BKMK_examples></a>典型

如果您的電腦只有一個磁片磁碟機（例如磁片磁碟機 A），而您想要比較兩個磁片，請輸入：
```
diskcomp a: a:
```
**Diskcomp**會提示您視需要插入每個磁片。

下列範例說明如何在使用**if**命令列上 ERRORLEVEL 環境變數的 batch 程式中處理**diskcomp**結束代碼：
```
rem Checkout.bat compares the disks in drive A and B 
echo off 
diskcomp a: b: 
if errorlevel 4 goto ini_error 
if errorlevel 3 goto hard_error 
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok 
:ini_error 
echo ERROR: Insufficient memory or command invalid 
goto exit 
:hard_error 
echo ERROR: An irrecoverable error occurred 
goto exit 
:break 
echo You just pressed CTRL+C to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
