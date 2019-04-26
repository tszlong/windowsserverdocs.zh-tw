---
title: pwlauncher
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1aa2e22f74323bb6cabfc644ca67e17a7fcbd3fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851019"
---
# <a name="pwlauncher"></a>pwlauncher



啟用或停用 Windows To Go 啟動選項 (pwlauncher)。 **Pwlauncher**命令列工具可讓您設定電腦自動開機進入 Windows To Go 工作區 （假設其為存在），而不需要您輸入您的韌體或變更您的啟動選項。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/enable|啟用 Windows To Go 啟動選項，讓從 USB 裝置，當其存在時，會自動開機電腦|
|/disable|停用 Windows To Go 啟動選項，使電腦無法開機的 USB 裝置，除非在韌體中手動設定。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

最大的障礙，使用者想要使用 Windows To Go 取得他們的電腦，從 USB 開機。 這是傳統上由輸入韌體，並嘗試不同的設定選項，直到電腦已正確設定。 這對大多數使用者的簡單工作並不是非常危險，因為韌體中包含可以使系統變成無法使用，如果使用不正確的選項。 若要協助減輕這個問題，Windows 8and 更新版本的作業系統包含一個功能稱為 「 Windows To Go 啟動選項？ 可讓使用者能夠設定他們的電腦從 Windows 內從 USB 開機，而未曾輸入其韌體，只要其韌體支援從 USB 開機。 啟用系統一律從 USB 開機時，第一次會具有您應該考慮的影響。 例如，包含惡意程式碼的 USB 裝置可能會不小心開機至危害系統，或多個 USB 磁碟機無法插入會導致開機衝突。 基於這個理由，預設組態會有 Windows To Go 啟動選項依預設停用。 此外，系統管理員權限，才能設定 Windows To Go 啟動選項。 如果您啟用 Windows To Go 啟動選項使用 pwlauncher 命令列工具或**變更 Windows To Go 啟動選項**電腦會嘗試從任何之前插入電腦的 USB 裝置開機應用程式已啟動。

## <a name="BKMK_examples"></a>範例

下列範例示範如何使用**pwlauncher**命令以啟用從 USB 開機：
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)