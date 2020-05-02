---
title: bdehdcfg
description: Bdehdcfg 命令的參考主題，它會準備硬碟，其中包含 BitLocker 磁碟機加密所需的分割區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bcc901847bb8d687d59bc3270dab39de0af8d60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718562"
---
# <a name="bdehdcfg"></a>bdehdcfg

準備硬碟，其中包含 BitLocker 磁碟機加密所需的分割區。 大部分的 Windows 7 安裝都不需要使用此工具，因為 BitLocker 安裝套裝程式含在必要時準備和重新分割磁片磁碟機的功能。

> [!WARNING]
> 「**電腦設定 \ 系統管理範本 \Bitlocker 磁片磁碟機 Encryption\Fixed 資料磁片**磁碟機」中，有與 [**拒絕受群組原則 BitLocker 保護之固定磁片磁碟機的寫入存取權**] 的已知衝突。
>
>如果在啟用此原則設定的電腦上執行 bdehdcfg，您可能會遇到下列問題：
>
>- 如果您嘗試壓縮磁片磁碟機並建立系統磁片磁碟機，磁片磁碟機大小將會成功減少，並會建立原始磁碟分割。 不過，原始資料分割不會格式化。 隨即顯示下列錯誤訊息：無法格式化新的 active Drive。 您可能需要為 BitLocker 手動準備您的磁片磁碟機。
>
>- 如果您嘗試使用未配置的空間來建立系統磁片磁碟機，則會建立原始磁碟分割。 不過，原始資料分割不會格式化。 隨即顯示下列錯誤訊息：無法格式化新的 active Drive。 您可能需要為 BitLocker 手動準備您的磁片磁碟機。
>
>- 如果您嘗試將現有的磁片磁碟機合併到系統磁片磁碟機，此工具將無法將所需的開機檔案複製到目標磁片磁碟機，以建立系統磁片磁碟機。 隨即顯示下列錯誤訊息： BitLocker 安裝程式無法複製開機檔案。 您可能需要為 BitLocker 手動準備您的磁片磁碟機。
>
>- 如果強制執行此原則設定，則無法重新分割硬碟，因為磁片磁碟機已受到保護。 如果您要從舊版 Windows 升級組織中的電腦，而且這些電腦已設定單一磁碟分割，則在將原則設定套用至電腦之前，您應該先建立所需的 BitLocker 系統磁碟分割。

## <a name="syntax"></a>語法

```
bdehdcfg [–driveinfo <drive_letter>] [-target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}] [–newdriveletter] [–size <size_in_mb>] [-quiet]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |----------- |
| [bdehdcfg： system.io.driveinfo](bdehdcfg-driveinfo.md) | 顯示指定磁片磁碟機上磁碟分割的磁碟機號、總大小、最大可用空間和磁碟分割特性。 只會列出有效的磁碟分割。 如果已有四個主要或擴充分割區，則不會列出未配置的空間。 |
| [bdehdcfg：目標](bdehdcfg-target.md) | 定義要使用哪一部分的磁片磁碟機做為系統磁片磁碟機，並使部分變成作用中。 |
| [bdehdcfg： newdriveletter](bdehdcfg-newdriveletter.md) | 將新的磁碟機號指派給用來做為系統磁片磁碟機的磁片磁碟機部分。 |
| [bdehdcfg：大小](bdehdcfg-size.md) | 決定建立新的系統磁片磁碟機時，系統磁碟分割的大小。 |
| [bdehdcfg： quiet](bdehdcfg-quiet.md) | 防止在命令列介面中顯示所有動作和錯誤，並將 bdehdcfg 導向在後續磁片磁碟機準備期間可能發生的任何「是/否」提示。 |
| [bdehdcfg：重新開機](bdehdcfg-restart.md) | 指示電腦在磁片磁碟機準備完成後重新開機。 |
| /? | 在命令提示字元顯示 [說明]。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
