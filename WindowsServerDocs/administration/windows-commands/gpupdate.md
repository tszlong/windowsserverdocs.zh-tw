---
title: gpupdate
description: Gpupdate 命令的參考文章，此命令會更新群組原則設定。
ms.topic: reference
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 76bf70a617f2b87c042946faea27235f16af1533
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634689"
---
# <a name="gpupdate"></a>gpupdate

更新群組原則設定。

## <a name="syntax"></a>語法

```
gpupdate [/target:{computer | user}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| /target:`{computer|user}` | 指定只更新使用者或僅更新電腦原則設定。 依預設，會更新使用者和電腦原則設定。 |
| /force | 重新套用所有原則設定。 依預設，只會套用已變更的原則設定。 |
| /wait`<VALUE>` | 設定在返回命令提示字元之前，等候原則處理完成的秒數。 當超過時間限制時，就會出現命令提示字元，但原則處理會繼續。 預設值為 600 秒。 **0**值表示不等候。 值 **-1** 表示無限期等候。<p>在腳本中，藉由在指定時間限制的情況下使用此命令，您可以執行 **gpupdate** ，然後繼續執行不相依于 **gpupdate**完成的命令。 或者，您可以使用此命令且未指定時間限制，讓 **gpupdate** 完成執行，然後才執行相依于該命令的其他命令。 |
| /logoff | 更新群組原則設定之後，會導致登出。 這是群組原則用戶端延伸模組的必要項，這些擴充功能不會在背景更新週期處理原則，而是在使用者登入時進行處理原則。 範例包括以使用者為目標的軟體安裝和資料夾重新導向。 如果沒有任何需要登出的擴充功能，則此選項不會有任何作用。 |
| /boot | 套用群組原則設定之後，電腦會重新開機。 這是群組原則用戶端延伸模組的必要項，這些延伸模組不會在背景更新週期處理原則，而是在電腦啟動時進行處理原則。 範例包括以電腦為目標的軟體安裝。 如果沒有任何需要重新開機的擴充功能，則此選項不會有任何作用。 |
| /sync | 導致下一個前景原則應用程式以同步方式完成。 [電腦開機] 和 [使用者登入] 會套用前景原則。 您可以使用 **/target** 參數，為使用者、電腦或兩者指定這個參數。 如果您指定 **/force** 和 **/wait** 參數，則會予以忽略。 |
| /? | 在命令提示字元顯示 [說明]。 |

### <a name="examples"></a>範例

若要強制所有群組原則設定的背景更新，不論它們是否已變更，請輸入：

```
gpupdate /force
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)