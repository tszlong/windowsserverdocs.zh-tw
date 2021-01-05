---
title: bcdedit
description: Bcdedit 命令的參考文章，此命令會建立新的存放區、修改現有的存放區，以及新增開機功能表參數。
ms.topic: reference
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/27/2018
ms.openlocfilehash: 5278e05e04dc4960c4a298aeaeaf0616bbaa0d0b
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696915"
---
# <a name="bcdedit"></a>bcdedit

開機設定資料 (BCD) 檔案提供一個存放區，可用於說明開機應用程式和開機應用程式設定。 存放區中的物件和元素足以取代 Boot.ini。

BCDEdit 為命令列工具，可用於管理 BCD 存放區。 它可用於各種用途，包括建立新的存放區、修改現有的存放區、新增開機功能表參數等等。 BCDEdit 本質上可提供與舊版 Windows 上之 Bootcfg.exe 相同的目的，但有兩項重大改進：

- 會公開比 Bootcfg.exe 更廣泛的開機參數範圍。

- 具有改良的腳本支援。

> [!NOTE]
> 需要系統管理權限，才可使用 BCDEdit 來修改 BCD。

BCDEdit 是用於編輯 Windows Vista 和更新版 Windows 之開機設定的主要工具。 其隨附於 Windows Vista 的發佈版本的 %WINDIR%\System32 資料夾中。

BCDEdit 受限於標準的資料型態，主要是設計來執行對 BCD 之單一且常見的變更。 如為更複雜的操作或非標準的資料型態，請考慮使用 BCD Windows Management Instrumentation (WMI) 應用程式發展介面 (API)，來建立更強大且更有彈性的自訂工具。

## <a name="syntax"></a>語法

```
bcdedit /command [<argument1>] [<argument2>] ...
```

### <a name="parameters"></a>參數

### <a name="general-bcdedit-command-line-options"></a>一般 BCDEdit Command-Line 選項

| 選項 | 描述 |
| ------ | ----------- |
| /? | 顯示 BCDEdit 命令清單。 若執行此命令時未加上引數，則會顯示可用命令的摘要。 若要顯示特定命令的詳細說明，請執行 **bcdedit/？** `<command>`，其中 `<command>` 是您要搜尋的命令名稱，以取得詳細資訊。 例如， **bcdedit/？ createstore** 會顯示 createstore 命令的詳細說明。 |

#### <a name="parameters-that-operate-on-a-store"></a>在存放區上運作的參數

| 選項 | 描述 |
| ------ | ----------- |
| /createstore | 建立新的且空的開機設定資料存放區。 建立的存放區並非系統存放區。 |
| /export | 將系統存放區的內容匯出至檔案。 這個檔案稍後可用於還原系統存放區的狀態。 這個命令僅對系統存放區有效。 |
| /import | 使用先前使用 **/export** 選項所產生的備份資料檔案，還原系統存放區的狀態。 這個命令會在進行匯入之前，先刪除任何存在於系統存放區中的項目。 這個命令僅對系統存放區有效。 |
| /store | 這個選項可用於大部分的 BCDedit 命令，以指定要使用的存放區。 如果未指定這個選項，則 BCDEdit 會在系統存放區上操作。 執行 **bcdedit/store** 命令本身相當於執行 **bcdedit/enum active** 命令。 |

#### <a name="parameters-that-operate-on-entries-in-a-store"></a>在存放區中的專案上運作的參數

| 參數 | 描述 |
| ------ | ----------- |
| /copy | 在相同的系統存放區中，製作指定之開機項目的複本。 |
| /create | 在開機設定資料存放區中建立新項目。 如果指定了知名的識別碼，則無法指定 **/application**、 **/inherit** 和 **/device** 參數。 如果未指定或不知道識別碼，則必須指定 **/application**、 **/inherit** 或 **/device** 選項。 |
| /delete | 從指定的項目刪除元素。 |

#### <a name="parameters-that-operate-on-entry-options"></a>在輸入選項上操作的參數

| 參數 | 描述 |
| ------ | ----------- |
| /deletevalue | 從開機項目刪除指定的元素。 |
| /set | 設定項目選項值。 |

#### <a name="parameters-that-control-output"></a>控制輸出的參數

| 參數 | 描述 |
| ------ | ----------- |
| /enum | 列出存放區中的項目。 **/Enum** 選項是 BCEdit 的預設值，因此執行不含參數的 **bcdedit** 命令，相當於執行 **bcdedit/enum active** 命令。 |
| /v | 詳細資訊模式。 通常任何通用的項目識別元都會以好記的簡短形式呈現。 將 **/v** 指定為命令列選項，會將所有識別碼全部顯示為 full。 執行 **bcdedit/v** 命令本身相當於執行 **bcdedit/enum active/v** 命令。 |

#### <a name="parameters-that-control-the-boot-manager"></a>控制開機管理程式的參數

| 參數 | 描述 |
| ------ | ----------- |
| /bootsequence | 指定一次性顯示順序，以用於下一次開機。 此命令類似于 **/displayorder** 選項，但只有在下次電腦啟動時才會使用。 之後，電腦會還原為原本的顯示順序。 |
| /default | 指定在逾時過期時，開機管理程式要選取的預設項目。 |
| /displayorder | 指定將開機參數顯示給使用者時，開機管理程式所使用的顯示順序。 |
| /timeout | 指定在開機管理程式選取預設項目之前，要等候的時間 (以秒為單位)。 |
| /toolsdisplayorder | 指定顯示 [ **工具** ] 功能表時，開機管理程式要使用的顯示順序。 |

#### <a name="parameters-that-control-emergency-management-services"></a>控制緊急管理服務的參數

| 參數 | 描述 |
| ------ | ----------- |
| /bootems | 啟用或停用指定項目的緊急管理服務 (EMS)。 |
| /ems | 啟用或停用指定之作業系統開機項目的 EMS。 |
| /emssettings | 設定電腦的全域 EMS 設定。 **/emssettings** 不會啟用或停用任何特定開機專案的 EMS。 |

#### <a name="parameters-that-control-debugging"></a>控制偵錯工具的參數

| 參數 | 描述 |
| ------ | ----------- |
| /bootdebug | 啟用或停用指定之開機項目的開機偵錯工具。 雖然這個命令可用於任何的開機項目，但它僅對開機應用程式有效。 |
| /dbgsettings | 指定或顯示系統的全域偵錯工具設定。 此命令不會 enablepose。 若要設定個別的全域偵錯工具設定，請使用 **bcdedit/set** `<dbgsettings> <type> <value>` 命令。 |
| /debug | 啟用或停用指定之開機項目的核心偵錯工具。 |

## <a name="additional-references"></a>其他參考資料

如需如何使用 BCDEdit 的範例，請參閱 [Bcdedit Options 參考](/windows-hardware/drivers/devtest/bcd-boot-options-reference) 文章。

若要查看用來表示命令列語法的標記法，請參閱  [命令列語法索引鍵](command-line-syntax-key.md)。
