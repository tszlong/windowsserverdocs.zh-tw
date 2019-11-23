---
title: chkntfs
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f940fe81f0e7e01495e071931059b2375b78bb22
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379349"
---
# <a name="chkntfs"></a>chkntfs



當電腦啟動時，顯示或修改自動磁片檢查。 如果在沒有選項的情況下使用， **chkntfs**會顯示指定磁片區的檔案系統。 如果已排程執行自動檔案檢查， **chkntfs**會顯示指定的磁片區是否已變更，或是否已排程在下次電腦啟動時進行檢查。

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

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|\<磁片區 > [...]|指定電腦啟動時要檢查的一或多個磁片區。 有效的磁片區包含磁碟機號（後面接著冒號）、掛接點或磁片區名稱。|
|/d|還原所有的**chkntfs**預設設定，但自動檢查檔案的倒數計時時間除外。 根據預設，系統會在電腦啟動時檢查所有磁片區，而**chkdsk**會在那些已變更的磁片區上執行。|
|/t [：\<時間 >]|將 [Autochk 起始倒數計時時間] 變更為指定的時間量（以秒為單位）。 如果您沒有輸入時間， **/t**會顯示目前的倒數計時時間。|
|/x \<磁片區 > [...]|指定當電腦啟動時要排除的一或多個磁片區，即使磁片區標示為需要**chkdsk**也一樣。|
|/c \<磁片區 > [...]|排程一或多個要在電腦啟動時檢查的磁片區，並在那些已變更的磁片區上執行**chkdsk** 。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>典型

若要顯示磁片磁碟機 C 的檔案系統類型，請輸入：
```
chkntfs c:
```
下列輸出表示 NTFS 檔案系統：
```
The type of the file system is NTFS.
```

> [!NOTE]
> 如果已排程執行自動檔案檢查，則會顯示其他輸出，指出磁片磁碟機是否已變更，或者是否已手動排程在下次啟動電腦時進行檢查。

若要顯示 [Autochk 起始倒數計時時間]，請輸入：
```
chkntfs /t
```
例如，如果倒數時間設定為10秒，則會顯示下列輸出：
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
若要將 [Autochk 起始倒數計時時間] 變更為30秒，請輸入：
```
chkntfs /t:30
```

> [!NOTE]
> 雖然您可以將 [Autochk 起始倒數計時時間] 設定為零，但這樣做將會導致您無法取消可能耗費時間的自動檔案檢查。

**/X**命令列選項不是累加。 如果您多次輸入，最新的專案就會覆寫先前的專案。 若要排除多個要檢查的磁片區，您必須在單一命令中將它們列出。 例如，若要排除 D 和 E 磁片區，請輸入：
```
chkntfs /x d: e:
```
**/C**命令列選項為累加。 如果您多次輸入 **/c** ，則每個專案都會保留。 若要確保只檢查特定磁片區，請重設預設值以清除所有先前的命令，排除所有磁片區不進行檢查，然後在所需的磁片區上排程自動檔案檢查。

例如，若要排程 D 磁片區上的自動檔案檢查，而不是 C 或 E 磁片區，請依序輸入下列命令：
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)