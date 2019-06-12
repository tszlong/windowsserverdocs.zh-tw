---
title: cd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 53340612d26eaa7c4ae6fd977a0eac573f91881d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434607"
---
# <a name="cd"></a>cd



顯示的名稱，或變更目前的目錄。 如果搭配磁碟機代號 (例如`cd C:`)， **cd**在指定的磁碟機中會顯示目前目錄的名稱。 如果未指定參數，使用**cd**會顯示目前的磁碟機和目錄。

> [!NOTE]
> 此命令等同於**chdir**命令。

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
|/d|變更目前的磁碟機，以及磁碟機目前的目錄。|
|\<磁碟機 >:|指定要顯示或變更 （如果不同於目前的磁碟機） 的磁碟機。|
|\<Path>|指定您想要顯示或變更目錄的路徑。|
|[..]|指定您想要變更的父資料夾。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

如果已啟用命令延伸模組，下列條件適用於**cd**命令：
- 目前的目錄字串會轉換成相同大小寫磁碟上的名稱。 比方說，`cd C:\TEMP`在磁碟上的情形下，會設定目前的目錄至 C:\Temp。
- 空間被視為不做為分隔符號，因此*路徑*可以包含空格，而不需要封閉式引號。 例如:  
  ```
  cd username\programs\start menu
  ```  
  等同於：  
  ```
  cd "username\programs\start menu"
  ```  
  如果停用延伸模組時，才不過，必要的引號。

若要停用命令延伸模組，請輸入：
```
cmd /e:off
```

## <a name="BKMK_examples"></a>範例

根目錄是磁碟機的目錄階層的頂端。 若要返回根目錄，請輸入：
```
cd\
```
若要變更您位於不同的磁碟機上的預設目錄，請輸入：
```
cd [<Drive>:\[<Directory>]]
```
若要確認變更至的目錄，請輸入：
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)