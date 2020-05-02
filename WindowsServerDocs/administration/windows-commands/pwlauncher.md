---
title: pwlauncher
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ddda3ab7831643c3c2c096ae87893d97f90155c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722746"
---
# <a name="pwlauncher"></a>pwlauncher



啟用或停用 Windows To Go 啟動選項（pwlauncher）。 **Pwlauncher**命令列工具可讓您將電腦設定為自動開機進入 Windows to Go 工作區（假設有一個），而不需要輸入您的固件或變更您的啟動選項。



## <a name="syntax"></a>語法

```
Pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/enable|啟用 Windows To Go 啟動選項，讓電腦能夠在出現時從 USB 裝置自動開機|
|/disable|停用 Windows To Go 啟動選項，讓電腦無法從 USB 裝置開機，除非在固件中手動設定。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

使用者想要使用 Windows To Go 的最大障礙，是讓他們的電腦從 USB 開機。 這通常是藉由進入固件並嘗試不同的設定選項來完成，直到電腦設定正確為止。 這不是大部分使用者的簡單工作，而且非常有風險，因為如果使用錯誤，則該固件包含的選項可能會導致系統無法使用。 為了協助解決這個問題，Windows 8and 較新的作業系統包含一個名為 Windows To Go 啟動選項的功能，可讓使用者將其電腦設定為從 Windows 內的 USB 開機，而不需要輸入其固件，只要其固件支援從 USB 啟動即可。 讓系統一律從 USB 開機，會有您應該考慮的含意。 例如，可能會不小心地開機包含惡意程式碼的 USB 裝置來危害系統，或可能會插入多個 USB 磁片磁碟機來造成開機衝突。 基於這個理由，預設設定預設會停用 Windows To Go 啟動選項。 此外，也需要有系統管理員許可權，才能設定 Windows To Go 啟動選項。 如果您使用 pwlauncher 命令列工具或 [**變更 Windows To Go 啟動選項**] 應用程式來啟用 Windows To go 啟動選項，電腦將會嘗試從插入電腦的任何 USB 裝置開機，然後再啟動。

## <a name="examples"></a>範例

若要示範如何使用**pwlauncher**命令來啟用 USB 開機：
```
Pwlauncher /enable
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)