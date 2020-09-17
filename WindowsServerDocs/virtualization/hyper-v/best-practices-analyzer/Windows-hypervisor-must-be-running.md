---
title: Windows 虛擬程式必須正在執行
description: 提供指示以解決這個最佳做法分析程式規則所報告的問題。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
ms.date: 10/03/2016
ms.openlocfilehash: 75437680370672a1eef9fad2957f398ce4f267d7
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90744814"
---
# <a name="windows-hypervisor-must-be-running"></a>Windows 虛擬程式必須正在執行

>適用於：Windows Server 2016

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|Prerequisites|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*Windows 虛擬程式未執行。*

## <a name="impact"></a>影響

*在 Windows 虛擬程式執行之前，無法啟動虛擬機器。*

## <a name="resolution"></a>解決方案

*檢查 Windows Server 類別目錄，以查看此伺服器是否有資格執行 Hyper-v。接下來，請確定已針對硬體輔助虛擬化和硬體強制執行的資料執行防止啟用 BIOS。然後，檢查 Hyper-v-管理程式事件記錄檔。*

若要檢查目錄，請參閱 [Windows Server catalog](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228) 。

> [!CAUTION]
> 變更電腦系統 BIOS 中的特定參數可能會導致該電腦停止載入作業系統，或者可以讓硬體裝置（例如硬碟）無法使用。 請一律查閱電腦的使用者手冊，以判斷設定系統 BIOS 的適當方式。 此外，最好是追蹤您所修改的參數及其原始值，以便稍後視需要還原它們。 如果您在變更系統 BIOS 中的參數之後遇到問題，請嘗試載入預設設定 (選項通常可在 BIOS 設定公用程式) 中取得，或請洽詢電腦製造商以取得協助。

#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>確認 BIOS 或 UEFI 中的虛擬化支援

1.  重新開機電腦，並透過設定工具存取 BIOS 或 UEFI。 當電腦經過開機程式時，通常可以存取這項工具。 當您開啟大部分的電腦之後，會出現幾秒鐘的訊息，其中列出用來開啟設定工具的按鍵或按鍵組合。

2.  找出虛擬化和硬體強制執行的資料執行防止 (DEP) 的設定，並確認它們是否已開啟。 以下是設定工具中這些設定的常用功能表位置，以及它們可能命名的範例：

    -   虛擬化支援：

        -   通常會在主要處理器或效能的設定底下使用。 有時候會在安全性設定下。

        -   尋找包含「虛擬化」或「虛擬化技術」的參數名稱。

    -   硬體強制的 DEP：

        -   通常可在 [安全性] 或 [記憶體] 設定底下使用。

        -   尋找包含「執行」、「執行」或「預防」的參數名稱。

3.  如有必要，請遵循設定工具中的指示來開啟設定。 儲存變更並結束。

4.  如果您進行了任何變更，請關閉電源，然後重新開啟以完成。

    > [!IMPORTANT]
    > 我們建議您關閉電源，然後重新開啟 (有時也稱為電源週期) ，因為變更不會套用到某些電腦上，直到發生這種情況。

接下來，檢查 Hyper-v-管理程式事件記錄檔。 如果發生問題，您也會檢查系統記錄檔。

#### <a name="to-check-the-event-logs"></a>檢查事件記錄檔

1.  開啟 [事件檢視器]。 按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按一下 [ **事件檢視器**]。

2.  開啟 Hyper-v-管理程式事件記錄檔。 在流覽窗格中，展開 [**應用程式及服務記錄**檔  >>  **Microsoft**  >>  **Windows**  >>  **hyper-v-管理**元件]，然後按一下 [**操作**]。

3.  如果 Windows 執行程式正在執行，則不需要採取任何進一步的動作。 如果 Windows 虛擬程式未執行，請執行下列動作：

4.  開啟系統記錄檔。  (在流覽窗格中，展開 [ **Windows 記錄** 檔]，然後選取 [ **System**] ) 

5.  使用篩選器來尋找 Hyper-v-管理程式事件：
    1. 在 [ **動作** ] 窗格中，按一下 [ **篩選目前的記錄**]。 若為 **事件來源**，請指定「Hyper-v-管理程式」。
    2. 尋找報告問題的事件。 例如，事件識別碼41表示 BIOS 設定發生問題：「Hyper-v 啟動失敗;VMX 不存在或未在 BIOS 中啟用。」

### <a name="see-also"></a>另請參閱
如需在 Windows 10 上使用 Hyper-v 的詳細資訊，包括如何檢查電腦是否可以執行 Hyper-v，請參閱 [Windows 10 Hyper-v 系統需求](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。