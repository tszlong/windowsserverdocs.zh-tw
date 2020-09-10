---
title: bdehdcfg
description: Bdehdcfg 命令的參考文章，可針對 BitLocker 磁碟機加密所需的磁碟分割準備硬碟。
ms.topic: reference
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e14a361fc0cb163466519ef2ccdefed642ca5632
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632793"
---
# <a name="bdehdcfg"></a>bdehdcfg

使用 BitLocker 磁碟機加密所需的磁碟分割準備硬碟。 大部分的 Windows 7 安裝都不需要使用這項工具，因為 BitLocker 安裝套裝程式含視需要準備和重新分割磁片磁碟機的能力。

> [!WARNING]
> 在**電腦設定 \Bitlocker 磁片磁碟機 Encryption\Fixed 資料磁片磁碟機的電腦設定 \Windows 範本**中，有一個與 [**拒絕存取未受群組原則 BitLocker 保護之磁片磁碟機的寫入權限**] 的已知衝突。
>
>如果啟用此原則設定時，在電腦上執行 bdehdcfg，您可能會遇到下列問題：
>
>- 如果您嘗試壓縮磁片磁碟機並建立系統磁片磁碟機，磁片磁碟機大小將會順利縮減，而且會建立原始磁碟分割。 但是，不會將原始分割區格式化。 會顯示下列錯誤訊息：無法格式化新的使用中磁片磁碟機。 您可能需要手動準備 BitLocker 的磁片磁碟機。
>
>- 如果您嘗試使用未配置的空間來建立系統磁片磁碟機，則會建立原始磁碟分割。 但是，不會將原始分割區格式化。 會顯示下列錯誤訊息：無法格式化新的使用中磁片磁碟機。 您可能需要手動準備 BitLocker 的磁片磁碟機。
>
>- 如果您嘗試將現有的磁片磁碟機合併到系統磁片磁碟機，此工具將無法將必要的開機檔案複製到目標磁片磁碟機，以建立系統磁片磁碟機。 會顯示下列錯誤訊息： BitLocker 安裝程式無法複製開機檔案。 您可能需要手動準備 BitLocker 的磁片磁碟機。
>
>- 如果強制執行此原則設定，則無法重新配置硬碟機，因為磁片磁碟機受到保護。 如果您要從舊版 Windows 升級組織中的電腦，而且這些電腦已設定單一磁碟分割，您應該在將原則設定套用至電腦之前，先建立必要的 BitLocker 系統磁碟分割。

## <a name="syntax"></a>語法

```
bdehdcfg [–driveinfo <drive_letter>] [-target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}] [–newdriveletter] [–size <size_in_mb>] [-quiet]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |----------- |
| [bdehdcfg： getdrives](bdehdcfg-driveinfo.md) | 顯示指定磁片磁碟機上磁碟分割的磁碟機號、大小總計、最大可用空間和磁碟分割特性。 只會列出有效的資料分割。 如果有四個主要或延伸磁碟分割，則不會列出未配置的空間。 |
| [bdehdcfg：目標](bdehdcfg-target.md) | 定義磁片磁碟機中要做為系統磁片磁碟機的部分，並使部分成為使用中。 |
| [bdehdcfg： newdriveletter](bdehdcfg-newdriveletter.md) | 將新的磁碟機號指派給用來作為系統磁片磁碟機的磁片磁碟機部分。 |
| [bdehdcfg：大小](bdehdcfg-size.md) | 決定建立新的系統磁片磁碟機時，系統磁碟分割的大小。 |
| [bdehdcfg： quiet](bdehdcfg-quiet.md) | 防止在命令列介面中顯示所有動作和錯誤，並將 [是] 的答案導向至後續磁片磁碟機準備期間可能會發生的任何「是/否」提示。 |
| [bdehdcfg：重新開機](bdehdcfg-restart.md) | 指示電腦在磁片磁碟機準備完成後重新開機。 |
| /? | 在命令提示字元顯示 [說明]。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
