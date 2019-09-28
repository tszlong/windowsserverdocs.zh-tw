---
title: bcdedit
description: '**Bcdedit**的 Windows 命令主題-建立新的存放區、修改現有的存放區，以及新增開機功能表參數。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: 9448f4461a089a93382ef8cd9e804b382fca27e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382253"
---
# <a name="bcdedit"></a>bcdedit



開機設定資料（BCD）檔案會提供用來描述開機應用程式和開機應用程式設定的存放區。 存放區中的物件和元素會有效地取代 Boot.ini。

BCDEdit 是用來管理 BCD 存放區的命令列工具。 它可用於各種用途，包括建立新的存放區、修改現有的存放區、新增開機功能表參數等等。 BCDEdit 在舊版 Windows 上的功能基本上與 mmc.exe 相同，但有兩項主要的改良：
-   會公開比 Bootcfg 更廣範圍的開機參數。
-   已改善腳本支援。

> [!NOTE]
> 必須有系統管理許可權，才能使用 BCDEdit 來修改 BCD。

BCDEdit 是編輯 Windows Vista 和更新版本之 Windows 開機設定的主要工具。 它隨附于 Windows Vista 散發套件的%WINDIR%\System32 資料夾中。

BCDEdit 僅限於標準資料類型，主要是用來對 BCD 執行單一一般變更。 針對更複雜的作業或非標準的資料類型，請考慮使用 BCD Windows Management Instrumentation （WMI）應用程式開發介面（API）來建立更強大且彈性的自訂工具。

## <a name="syntax"></a>語法

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>參數

### <a name="general-bcdedit-command-line-option"></a>一般 BCDEdit 命令列選項

|選項|描述|
|------|-----------|
|/?|顯示 BCDEdit 命令的清單。 不使用引數執行此命令會顯示可用命令的摘要。 若要顯示特定命令的詳細說明，請執行**bcdedit/？** @no__t 0command >，其中 <command> 是您要搜尋的命令名稱，以取得的詳細資訊。 例如， **bcdedit/？ createstore**會顯示 createstore 命令的詳細說明。|

### <a name="parameters-that-operate-on-a-store"></a>在存放區上操作的參數

|選項|描述|
|------|-----------|
|/createstore|建立新的空白開機設定資料存放區。 建立的存放區不是系統存放區。|
|/export|將系統存放區的內容匯出至檔案。 稍後可以使用這個檔案來還原系統存放區的狀態。 此命令只對系統存放區有效。|
|/import|使用先前使用 **/export**選項所產生的備份資料檔案，還原系統存放區的狀態。 此命令會先刪除系統存放區中的任何現有專案，然後再進行匯入。 此命令只對系統存放區有效。|
|/store|此選項可與大部分的 BCDedit 命令搭配使用，以指定要使用的存放區。 如果未指定此選項，則 BCDEdit 會在系統存放區上運作。 執行**bcdedit/store**命令本身相當於執行**bcdedit/enum active**命令。|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>在存放區中的專案上運作的參數

|參數|描述|
|---------|-----------|
|/copy|在相同的系統存放區中建立指定之開機專案的複本。|
|/create|在開機設定資料存放區中建立新的專案。 如果指定了已知的識別碼，則無法指定 **/application**、 **/inherit**和 **/device**參數。 如果識別碼未指定或不知名，則必須指定 **/application**、 **/inherit**或 **/device**選項。|
|/delete|從指定的專案中刪除元素。|

### <a name="parameters-that-operate-on-entry-options"></a>在輸入選項上操作的參數

|參數|描述|
|---------|-----------|
|/deletevalue|從開機專案中刪除指定的元素。|
|/set|設定專案選項值。|

### <a name="parameters-that-control-output"></a>控制輸出的參數

|參數|描述|
|---------|-----------|
|/enum|列出存放區中的專案。 **/Enum**選項是 BCEdit 的預設值，因此在沒有參數的情況下執行**bcdedit**命令相當於執行**bcdedit/enum active**命令。|
|/v|詳細資訊模式。 通常，任何已知的專案識別碼都會以易記的速記格式來表示。 指定 **/v**做為命令列選項會顯示完整的所有識別碼。 執行**bcdedit/v**命令本身相當於執行**bcdedit/enum active/v**命令。|

### <a name="parameters-that-control-the-boot-manager"></a>控制開機管理程式的參數

|參數|描述|
|---------|-----------|
|/bootsequence|指定要用於下一次開機的一次性顯示順序。 此命令類似于 **/displayorder**選項，不同之處在于它只會在下次電腦啟動時使用。 之後，電腦會還原成原始顯示順序。|
|/default|指定開機管理程式在超時時間到期時所選取的預設專案。|
|/displayorder|指定在向使用者顯示開機參數時，開機管理程式所使用的顯示順序。|
|/timeout|指定開機管理程式選取預設專案之前，要等候的時間（以秒為單位）。|
|/toolsdisplayorder|指定顯示 [**工具**] 功能表時，開機管理程式要使用的顯示順序。|

### <a name="parameters-that-control-emergency-management-services"></a>控制緊急管理服務的參數

|參數|描述|
|---------|-----------|
|/bootems|啟用或停用指定專案的緊急管理服務（EMS）。|
|/ems|啟用或停用指定之作業系統開機專案的 EMS。|
|/emssettings|設定電腦的全域 EMS 設定。 **/emssettings**不會啟用或停用任何特定開機專案的 EMS。|

### <a name="parameters-that-control-debugging"></a>控制偵錯工具的參數

|參數|描述|
|---------|-----------|
|/bootdebug|啟用或停用指定之開機專案的開機偵錯工具。 雖然此命令適用于任何開機專案，但只有開機應用程式才有效。|
|/dbgsettings|指定或顯示系統的全域偵錯工具設定。 此命令不會啟用或停用內核偵錯工具;針對該目的使用 **/debug**選項。 若要設定個別的全域偵錯工具設定，請使用**bcdedit/set** \<dbgsettings > <type> <value> 命令。|
|/debug|啟用或停用指定之開機專案的內核偵錯工具。|

## <a name="examples"></a>範例

如需 BCDEdit 的範例，請參閱[Bcdedit 選項參考](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference)。