---
title: bcdedit
description: 適用於 Windows 命令主題**bcdedit** -建立新的存放區、 修改現有的存放區，並新增開機功能表參數。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c1ac016b299cbd72a406121c54762f4457b24286
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872439"
---
# <a name="bcdedit"></a>bcdedit



開機設定資料 (BCD) 檔案提供一個存放區，用於說明開機應用程式和開機應用程式設定。 物件和存放區中的項目取代 Boot.ini。

BCDEdit 是用於管理 BCD 存放區的命令列工具。 它可以用於各種用途，包括建立新的存放區、 修改現有的存放區、 新增開機功能表參數等等。 BCDEdit 有基本上相同的用途為 Bootcfg.exe 較舊版本 Windows，但有兩項重大改進：
-   會顯示比 Bootcfg.exe 更廣泛的開機參數。
-   已改善指令碼支援。

> [!NOTE]
> 系統管理員權限，才能使用 BCDEdit 修改 BCD。

BCDEdit 是用於編輯之開機設定的 Windows Vista 和更新版本的 Windows 的主要工具。 它是隨附於 Windows Vista 的發佈 %WINDIR%\System32 資料夾中。

BCDEdit 限於標準的資料類型，而且主要設計來執行單一的一般變更 bcd。 更複雜的作業或使用非標準的資料類型，請考慮使用 BCD Windows Management Instrumentation (WMI) 應用程式設計介面 (API) 來建立更強大且彈性的自訂工具。

## <a name="syntax"></a>語法

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>參數

### <a name="general-bcdedit-command-line-option"></a>一般 BCDEdit 命令列選項

|選項|描述|
|------|-----------|
|/?|顯示 BCDEdit 命令清單。 執行此命令未提供引數，就會顯示可用命令的摘要。 若要顯示特定命令的詳細的說明，請執行**bcdedit /？** \<命令 >，其中<command>是您要搜尋的詳細資訊的相關命令的名稱。 例如， **bcdedit /？ createstore**顯示 Createstore 命令的詳細的說明。|

### <a name="parameters-that-operate-on-a-store"></a>存放區運作的參數

|選項|描述|
|------|-----------|
|/createstore|建立新的空的開機設定資料存放區。 建立的存放區不是系統存放區。|
|/export|匯出到檔案儲存系統的內容。 這個檔案可以用來還原系統存放區狀態的更新版本。 此命令是僅適用於系統存放區。|
|/import|使用先前使用所產生的備份資料檔案來還原系統存放區的狀態 **/匯出**選項。 匯入之前，此命令會刪除系統存放區中任何現有的項目。 此命令是僅適用於系統存放區。|
|/store|此選項可以搭配大部分的 BCDedit 命令，以指定要使用的存放區。 如果未指定此選項，然後 BCDEdit 上運作的系統存放區。 執行**bcdedit /store**命令本身就相當於執行**bcdedit /enum active**命令。|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>存放區中的項目操作的參數

|參數|描述|
|---------|-----------|
|/copy|在相同的系統存放區中，會建立一份指定的開機項目。|
|/create|開機設定資料存放區中建立新的項目。 如果指定的已知的識別項，則 **/應用程式**， **/ 繼承**，並 **/device**不指定參數。 如果識別項不是指定或不大家都知道， **/應用程式**， **/ 繼承**，或 **/device**必須指定選項。|
|/delete|從指定的項目中刪除項目。|

### <a name="parameters-that-operate-on-entry-options"></a>在項目選項運作的參數

|參數|描述|
|---------|-----------|
|/deletevalue|從開機項目中刪除指定的項目。|
|/set|設定項目選項值。|

### <a name="parameters-that-control-output"></a>控制輸出的參數

|參數|描述|
|---------|-----------|
|/enum|列出存放區中的項目。 **/Enum**選項是 BCEdit，因此執行的預設值**bcdedit**不含參數的命令就相當於執行**bcdedit /enum active**命令。|
|/v|詳細資訊模式。 通常，由其好記的簡短形式表示的任何已知的項目識別項。 指定 **/v**做為命令列選項完整顯示所有的識別項。 執行**bcdedit /v**命令本身就相當於執行**bcdedit /enum active /v**命令。|

### <a name="parameters-that-control-the-boot-manager"></a>控制開機管理程式的參數

|參數|描述|
|---------|-----------|
|/bootsequence|指定一次性顯示順序，以用於下一次開機。 此命令是類似於 **/displayorder**選項，但它只能在下一次電腦啟動。 之後，電腦會還原為原始的顯示順序。|
|/default|指定在逾時到期時，請選取開機管理員的預設項目。|
|/displayorder|指定向使用者顯示開機參數時，會使用的開機管理員顯示順序。|
|/timeout|指定等待，以秒為單位，開機管理程式選取預設項目之前的時間。|
|/toolsdisplayorder|指定使用時顯示的開機管理員顯示順序**工具**功能表。|

### <a name="parameters-that-control-emergency-management-services"></a>控制緊急管理服務的參數

|參數|描述|
|---------|-----------|
|/bootems|啟用或停用指定的項目的緊急管理服務 (EMS)。|
|/ems|啟用或停用指定的作業系統開機項目的 EMS。|
|/emssettings|設定電腦的全域 EMS 設定。 **/emssettings**不會啟用或停用任何特定開機項目的 EMS。|

### <a name="parameters-that-control-debugging"></a>參數以控制偵錯

|參數|描述|
|---------|-----------|
|/bootdebug|啟用或停用指定的開機項目的開機偵錯工具。 雖然這個命令適用於任何開機項目，它是只對開機應用程式有效。|
|/dbgsettings|會指定或顯示系統的全域偵錯工具設定。 此命令不會啟用或停用核心偵錯工具中;使用  **/ 偵錯**針對該用途的選項。 若要設定個別全域偵錯工具設定，請使用**bcdedit /set** \<dbgsettings > <type> <value> 命令。|
|/debug|啟用或停用指定的開機項目的核心偵錯工具。|

## <a name="examples"></a>範例

如需 BCDEdit 的範例，請參閱 < [BCDEdit 選項參考](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference)。