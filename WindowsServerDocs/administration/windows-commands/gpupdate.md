---
title: gpupdate
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d358c47bd278cf11c4bab6887302bf6d053529ec
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192663"
---
# <a name="gpupdate"></a>gpupdate



更新群組原則設定。 如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/target:{Computer | 使用者}|只有使用者或電腦原則設定，就會更新。|
|/force|重新套用所有原則設定。 根據預設，會套用已變更的原則設定。|
|/wait:\<值 >|設定要等待原則完成後，回到命令提示字元中處理秒的數。 在超過時間限制時，出現命令提示字元，但原則處理會繼續。 預設值為 600 秒。 該值**0**表示不會等候。 該值 **-1**表示無限期等候。</br>在指令碼，此命令使用時間限制指定，您可以執行**gpupdate** ，並繼續進行，而不必完成時命令**gpupdate**。 或者，您可以使用此命令，以便指定沒有時間限制**gpupdate**完成執行之前執行與其相依的其他命令。|
|/logoff|已更新的群組原則設定之後，會使登出。 這是為了使用這些群組原則用戶端延伸不會在背景更新循環處理原則，但不要處理原則，當使用者登入。 範例包括使用者為目標的軟體安裝和資料夾重新導向。 如果沒有所呼叫的任何延伸模組需要登出，此選項將任何作用。|
|/boot|套用群組原則設定之後，會導致重新啟動電腦。 這是為了使用這些群組原則用戶端延伸不會在背景更新循環處理原則，但不要處理程序在電腦啟動時的原則。 範例包括電腦為目標的軟體安裝。 如果沒有所呼叫的任何延伸模組需要重新啟動，此選項將任何作用。|
|/sync|會導致下一個前景原則應用程式，以同步方式完成。 在電腦開機及使用者登入時，會套用幕前原則。 您可以針對使用者、 電腦或使用的同時指定這 **/目標**參數。 **/Force**並 **/wait**參數都會被忽略，如果您指定它們。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   **Gpupdate**命令適用於 Windows Server 2008 R2、 Windows Server 2008、 Windows 7 旗艦版、 Windows 7 專業版、 Windows Vista Ultimate、 Windows Vista Enterprise 和 Windows Vista Business。

## <a name="examples"></a>範例

強制所有的群組原則設定，無論是否有變更的背景更新。
```
gpupdate /force
```

#### <a name="additional-references"></a>其他參考資料

-   [群組原則 TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令列語法關鍵](command-line-syntax-key.md)