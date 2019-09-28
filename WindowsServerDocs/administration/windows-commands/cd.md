---
title: cd
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed0942232eb205a8198d4b3d366ca9482af1f4b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379715"
---
# <a name="cd"></a>cd



顯示或變更目前目錄的名稱。 如果只搭配磁碟機號使用（例如，`cd C:`）， **cd**會顯示指定磁片磁碟機中目前目錄的名稱。 如果在沒有參數的情況下使用， **cd**會顯示目前的磁片磁碟機和目錄。

> [!NOTE]
> 此命令與**chdir**命令相同。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/d|變更目前的磁片磁碟機以及磁片磁碟機的目前目錄。|
|@no__t 0Drive >：|指定要顯示或變更的磁片磁碟機（如果不同于目前的磁片磁碟機）。|
|\<Path >|指定您想要顯示或變更之目錄的路徑。|
|[..]|指定您想要變更為父資料夾。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

如果已啟用命令延伸模組，則下列條件適用于**cd**命令：
- 目前的目錄字串會轉換成使用與磁片上的名稱相同的大小寫。 例如，如果磁片上發生這種情況，`cd C:\TEMP` 會將目前的目錄設為 C：\Temp。
- 空格不會被視為分隔符號，因此*Path*可以包含空格，而不需要括住引號。 例如:  
  ```
  cd username\programs\start menu
  ```  
  與相同：  
  ```
  cd "username\programs\start menu"
  ```  
  不過，如果已停用延伸模組，則需要引號。

若要停用命令延伸模組，請輸入：
```
cmd /e:off
```

## <a name="BKMK_examples"></a>典型

根目錄是磁片磁碟機目錄階層的頂端。 若要返回根目錄，請輸入：
```
cd\
```
若要在不同于您所在的磁片磁碟機上變更預設目錄，請輸入：
```
cd [<Drive>:\[<Directory>]]
```
若要驗證目錄的變更，請輸入：
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)