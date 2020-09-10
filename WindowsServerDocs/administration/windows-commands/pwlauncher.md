---
title: pwlauncher
description: Pwlauncher 命令的參考文章，可啟用或停用 Windows To Go 啟動選項 (pwlauncher) 。
ms.topic: reference
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8bdc4f7b3b5c91cb804f6760a60ec421652d1642
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639929"
---
# <a name="pwlauncher"></a>pwlauncher

啟用或停用 Windows To Go 啟動選項 (pwlauncher) 。 **Pwlauncher**命令列工具可讓您將電腦設定為自動開機進入 Windows To Go 工作區 (假設有一個) ，而不需要輸入您的固件或變更啟動選項。

Windows To Go 啟動選項可讓使用者將他們的電腦設定為從 Windows 內部開機，而不需要輸入其固件，只要其固件支援從 USB 開機即可。 讓系統一律從 USB 先開機，會影響您應該考慮的事項。 例如，包含惡意程式碼的 USB 裝置可能會不慎開機來危害系統，或可能插上多個 USB 磁片磁碟機來造成開機衝突。 基於這個理由，預設設定會預設為停用 Windows To Go 啟動選項。 此外，也需要系統管理員許可權才能設定 Windows To Go 啟動選項。 如果您使用 pwlauncher 命令列工具或 **變更 Windows To Go 啟動選項** 應用程式啟用 Windows To Go 啟動選項，電腦會嘗試從任何插入電腦的 USB 裝置開機後再啟動。

## <a name="syntax"></a>語法

```
pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /enable | 啟用 Windows To Go 啟動選項，如此一來，電腦就會自動從 USB 裝置開機（如果有的話）。 |
| /disable | 停用 Windows To Go 啟動選項，因此除非在固件中手動設定，否則無法從 USB 裝置啟動電腦。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要啟用從 USB 開機：

```
pwlauncher /enable
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
