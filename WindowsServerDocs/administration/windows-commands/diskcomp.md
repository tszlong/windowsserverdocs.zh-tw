---
title: diskcomp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccd1a347f9ac51fc98c963dedb1c0ab3fcd27d41
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439595"
---
# <a name="diskcomp"></a>diskcomp



比較兩個磁碟的內容。 如果未指定參數，使用**diskcomp**來比較兩個磁碟會使用目前的磁碟機。如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive1>|指定包含其中一個磁碟的磁碟機。|
|\<Drive2>|指定包含其他磁碟的磁碟機。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 使用磁碟

  **Diskcomp**命令只適用於磁碟。 您無法使用**diskcomp**與硬碟。 如果您指定的硬碟*Drive1*或是*Drive2*， **diskcomp**會顯示下列錯誤訊息：  
  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```  
- 比較磁碟

  如果要比較的兩個磁碟上的所有追蹤都都一樣， **diskcomp**顯示下列訊息：  
  ```
  Compare OK
  ```  
  曲目不相同，如果**diskcomp**會顯示類似下面的訊息：  
  ```
  Compare error on
  side 1, track 2
  ```  
  當**diskcomp**完成相較之下，它會顯示下列訊息：  
  ```
  Compare another diskette (Y/N)?
  ```  
  如果您按下 Y **diskcomp**會提示您插入磁碟，以供下一步 的比較。 如果您按下 N **diskcomp**停止比較。

  當**diskcomp**進行比較，它會忽略磁碟的磁碟區數目。
- 省略磁碟機參數

  如果您省略*Drive2*參數**diskcomp**使用的目前磁碟機*Drive2*。 如果省略這兩個磁碟機的參數， **diskcomp**同時使用目前的磁碟機。 如果目前的磁碟機是相同*Drive1*， **diskcomp**會提示您視需要交換磁碟。
- 使用一個磁碟機

  如果您指定的相同軟碟機*Drive1*並*Drive2*， **diskcomp**比較它們使用一個磁碟機，並提示您視需要插入磁片。 您可能要一次以上，將磁碟交換容量的磁碟以及可用的記憶體數量而定。
- 比較不同類型的磁碟

  **Diskcomp**無法比較的單面磁碟雙面磁碟時，也不適用之高密度的雙精度浮點數密度磁碟。 如果使用中的磁碟*Drive1*不是在磁碟相同的型別*Drive2*， **diskcomp**顯示下列訊息：  
  ```
  Drive types or diskette types not compatible
  ```  
- 使用**diskcomp**的網路和重新導向的磁碟機

  **Diskcomp**或建立的磁碟機上的網路磁碟機無法運作**subst**命令。 如果您嘗試使用**diskcomp**與這些類型的任何磁碟機**diskcomp**會顯示下列錯誤訊息：  
  ```
  Invalid drive specification
  ```  
- 比較原始磁碟的複本

  當您使用**diskcomp**使用您所使用的磁碟**複製**， **diskcomp**可能會顯示類似下面的訊息：  
  ```
  Compare error on 
  side 0, track 0
  ```  
  即使在磁碟上的檔案都相同，就會發生這種類型的錯誤。 雖然**複製**複製的詳細資訊，它不會不一定是將它放在目的地磁碟上的相同位置。
- 了解**diskcomp**結束代碼

  下表說明每個結束代碼。  

  |結束代碼|描述|
  |---------|-----------|
  |0|是相同的磁碟|
  |1|找不到的差異|
  |3|發生硬體錯誤|
  |4|發生初始化錯誤|

  要傳回的處理序結束代碼**diskcomp**，您可以使用 ERRORLEVEL 環境變數上**如果**批次程式中的命令列。

## <a name="BKMK_examples"></a>範例

如果您的電腦有只有一個軟碟機 （例如，磁碟機的），而且您想要比較兩個磁碟，輸入：
```
diskcomp a: a:
```
**Diskcomp**會提示您插入每個磁碟，視需要。

下列範例說明如何處理**diskcomp**結束批次程式上使用 ERRORLEVEL 環境變數中的程式碼**如果**命令列：
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
echo "You just pressed CTRL+C" to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
