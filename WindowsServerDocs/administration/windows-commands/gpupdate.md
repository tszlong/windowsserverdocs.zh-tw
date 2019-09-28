---
title: gpupdate
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 994e37ebd972d881e06bdb99d5256e75096ccd81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375639"
---
# <a name="gpupdate"></a>gpupdate

更新群組原則設定。 如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>參數

|     參數     |                                                                                                                                                                                                                                                                                                                             描述                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /target： {Computer |                                                                                                                                                                                                                                                                                                                                使用者                                                                                                                                                                                                                                                                                                                                |
|      /force       |                                                                                                                                                                                                                                                                                   重新套用所有原則設定。 根據預設，只會套用已變更的原則設定。                                                                                                                                                                                                                                                                                    |
|  /wait： \<VALUE >   | 設定在返回命令提示字元之前，等候原則處理完成的秒數。 超過時間限制時，會出現命令提示字元，但原則處理會繼續進行。 預設值為600秒。 值**0**表示不等待。 值 **-1**表示無限期等候。</br>在腳本中，藉由指定時間限制來使用此命令，您可以執行**gpupdate** ，並繼續執行不依賴**gpupdate**完成的命令。 或者，您可以使用此命令，而不指定時間限制，讓**gpupdate**完成執行，然後再執行相依于它的其他命令。 |
|      /logoff      |                                                                                                                                   在更新群組原則設定之後，會造成登出。 這些群組原則不會在背景更新週期處理原則，而是在使用者登入時執行處理原則的用戶端延伸，這是必要的。 範例包括以使用者為目標的軟體安裝和資料夾重新導向。 如果沒有呼叫需要登出的延伸模組，這個選項就不會有任何作用。                                                                                                                                    |
|       /boot       |                                                                                                                                       會在套用群組原則設定後重新開機電腦。 這群組原則對於不會在背景更新週期處理原則，而是在電腦啟動時處理原則的用戶端延伸而言，這是必要的。 範例包括以電腦為目標的軟體安裝。 如果沒有呼叫需要重新開機的擴充功能，這個選項就不會有任何作用。                                                                                                                                        |
|       /sync       |                                                                                                                                                                              導致下一個前景原則應用程式以同步方式執行。 前景原則會在電腦開機和使用者登入時套用。 您可以使用 **/target**參數，為使用者、電腦或兩者指定此項。 如果您指定 **/force**和 **/wait**參數，則會予以忽略。                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                在命令提示字元顯示 [說明]。                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>備註

-   **Gpupdate**命令適用于 windows Server 2008 R2、windows server 2008、Windows 7 旗艦版、Windows 7 專業版、Windows Vista 旗艦版、Windows vista Enterprise 和 Windows vista Business。

## <a name="examples"></a>範例

強制所有群組原則設定的背景更新，不論它們是否已變更。

```
gpupdate /force
```

#### <a name="additional-references"></a>其他參考資料

-   [群組原則技術中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令列語法關鍵](command-line-syntax-key.md)