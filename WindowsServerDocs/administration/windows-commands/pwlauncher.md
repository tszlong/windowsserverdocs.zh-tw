---
title: pwlauncher
description: Pwlauncher 命令的參考主題，可啟用或停用 Windows To Go 啟動選項（pwlauncher）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b6793dead3a41abb82bc3940d0314bcd7610418
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472113"
---
# <a name="pwlauncher"></a>pwlauncher

啟用或停用 Windows To Go 啟動選項（pwlauncher）。 **Pwlauncher**命令列工具可讓您將電腦設定為自動開機進入 Windows to Go 工作區（假設有一個），而不需要輸入您的固件或變更您的啟動選項。

Windows To Go 啟動選項可讓使用者將其電腦設定為從 Windows 內的 USB 開機，而不需要輸入其固件，只要其固件支援從 USB 開機即可。 讓系統一律從 USB 開機，會有您應該考慮的含意。 例如，可能會不小心地開機包含惡意程式碼的 USB 裝置來危害系統，或可能會插入多個 USB 磁片磁碟機來造成開機衝突。 基於這個理由，預設設定預設會停用 Windows To Go 啟動選項。 此外，也需要有系統管理員許可權，才能設定 Windows To Go 啟動選項。 如果您使用 pwlauncher 命令列工具或 [**變更 Windows To Go 啟動選項**] 應用程式來啟用 Windows To go 啟動選項，電腦將會嘗試從插入電腦的任何 USB 裝置開機，然後再啟動。

## <a name="syntax"></a>語法

```
pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /enable | 啟用 Windows To Go 啟動選項，因此電腦會在出現時從 USB 裝置自動開機。 |
| /disable | 停用 Windows To Go 啟動選項，因此除非在固件中手動設定，否則無法從 USB 裝置啟動電腦。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要啟用從 USB 開機：

```
pwlauncher /enable
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
