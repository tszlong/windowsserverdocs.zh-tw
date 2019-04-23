---
title: chkntfs
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876c0e0c254216ac217aea7d165d5f4e3a7da9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861989"
---
# <a name="chkntfs"></a>chkntfs



顯示或修改自動檢查電腦上啟動時的磁碟。 如果不設定選項，使用**chkntfs**會顯示指定的磁碟區的檔案系統。 如果執行時，自動檔案檢查排程**chkntfs**顯示指定的磁碟區是否已變更，或排定要檢查在下次電腦啟動。

> [!NOTE]
> 若要執行**chkntfs**，您必須是 Administrators 群組的成員。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁碟區 > [...]|指定要檢查在電腦啟動時的一或多個磁碟區。 有效的磁碟區包括磁碟機代號 （後面接著冒號），掛接點或磁碟區名稱。|
|/d|會將所有**chkntfs**預設設定，除了檔案自動檢查的倒數時間。 根據預設，電腦啟動時時，會檢查所有磁碟區並**chkdsk**執行上，有問題。|
|/t [:\<Time>]|變更 Autochk.exe 倒數計時的時間，以秒為單位指定的時間量。 如果您未輸入一次 **/t**會顯示目前的倒數時間。|
|/x\<磁碟區 > [...]|指定要排除檢查當電腦啟動時，即使磁碟區標示為需要的一或多個磁碟區**chkdsk**。|
|/c\<磁碟區 > [...]|排程進行檢查時電腦會啟動，並執行的一或多個磁碟區**chkdsk**上，有問題。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要顯示的磁碟機 C 的檔案系統類型，請輸入：
```
chkntfs c:
```
下列的輸出會指出 NTFS 檔案系統：
```
The type of the file system is NTFS.
```

> [!NOTE]
> 如果檔案自動檢查排程執行，其他的輸出會顯示，表示是否已變更磁碟機，或已手動排定要核取下一次電腦啟動。

若要顯示 Autochk.exe 倒數計時的時間，請輸入：
```
chkntfs /t
```
比方說，如果倒數計時時間設定為 10 秒時，會顯示下列輸出：
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
若要變更 Autochk.exe 倒數計時的時間為 30 秒，請輸入：
```
chkntfs /t:30
```

> [!NOTE]
> 雖然您可以設定 Autochk.exe 倒數計時的時間為零，這樣做會讓您從取消可能會很耗時的檔案自動核取。

**/X**命令列選項不是累加。 如果您輸入了一次以上，最新的項目會覆寫先前的項目。 若要檢查排除多個磁碟區，您必須列出每個在單一命令。 例如，若要排除在 D 和 E 的磁碟區，請輸入：
```
chkntfs /x d: e:
```
**/C**命令列選項是累積。 如果您鍵入 **/c**一次以上，會保留每個項目。 若要確保已核取 特定磁碟區，重設的預設值，若要清除所有先前的命令，將所有的磁碟區排除檢查，並再排程自動檢查所需的磁碟區上的檔案。

比方說，若要排程自動檢查在 D 磁碟區，但不是 C 或 E 磁碟區上的檔案，請在順序中輸入下列命令：
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)