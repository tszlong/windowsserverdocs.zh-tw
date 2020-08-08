---
title: Windows 虛擬程式必須正在執行
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: aa89f3735151b2dee795c1a325e22446e3770fa0
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994533"
---
# <a name="windows-hypervisor-must-be-running"></a>Windows 虛擬程式必須正在執行

>適用於：Windows Server 2016

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|必要條件|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*Windows 虛擬程式未執行。*

## <a name="impact"></a>影響

*在執行 Windows 執行程式之前，無法啟動虛擬機器。*

## <a name="resolution"></a>解決方法

*檢查 Windows 伺服器目錄，以查看此伺服器是否已限定執行 Hyper-v。接下來，請確定已啟用 BIOS 來進行硬體輔助虛擬化和硬體強制的資料執行防止。然後，檢查 Hyper-v-虛擬機器的事件記錄檔。*

若要檢查目錄，請參閱[Windows Server catalog](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228) 。

> [!CAUTION]
> 變更電腦的系統 BIOS 中的某些參數，可能會導致該電腦停止載入作業系統，或是讓硬體裝置（例如硬碟機）無法使用。 請務必洽詢電腦的使用者手冊，以判斷設定系統 BIOS 的正確方式。 此外，追蹤您修改的參數及其原始值，是個不錯的主意，以便您可以在稍後視需要進行還原。 如果您在變更系統 BIOS 中的參數之後遇到問題，請嘗試載入預設設定 (選項通常會在 BIOS 設定公用程式) 中使用，或洽詢電腦製造商尋求協助。

#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>確認 BIOS 或 UEFI 中的虛擬化支援

1.  重新開機電腦，並透過設定工具存取 BIOS 或 UEFI。 此工具的存取權通常可在電腦進行開機程式時使用。 當您開啟大部分的電腦之後，就會出現幾秒鐘的訊息，列出按鍵或按鍵組合來開啟設定工具。

2.  尋找虛擬化和硬體強制執行的資料執行防止 (DEP) 的設定，並確認它們是否已開啟。 以下是設定工具中這些設定的常見功能表位置，以及它們可能命名的範例：

    -   虛擬化支援：

        -   通常會在主要處理器或效能的設定底下提供。 有時候它是在安全性設定之下。

        -   尋找包含「虛擬化」或「虛擬化技術」的參數名稱。

    -   硬體強制 DEP：

        -   通常會在 [安全性] 或 [記憶體] 設定下提供。

        -   尋找包含「執行」、「執行」或「防護」的參數名稱。

3.  如有需要，請遵循設定工具中的指示來開啟設定。 儲存變更並結束。

4.  如果您進行了任何變更，請關閉電源，然後回到 [完成]。

    > [!IMPORTANT]
    > 我們建議您關閉電源，然後再重新開啟 (有時稱為電源週期) 因為不會在某些電腦上套用變更，直到發生這種情況。

接下來，檢查 Hyper-v-虛擬機器的事件記錄檔。 如果發生問題，您也會檢查系統記錄檔。

#### <a name="to-check-the-event-logs"></a>若要檢查事件記錄檔

1.  開啟 [事件檢視器]。 依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [**事件檢視器**]。

2.  開啟 [Hyper-v-虛擬機器] 事件記錄檔。 在流覽窗格中，展開 [**應用程式及服務**] [記錄] [  >>  **Microsoft**  >>  **Windows**  >>  **hyper-v-虛擬機器**]，然後按一下 [**操作**]。

3.  如果 Windows 虛擬機器正在執行，則不需要採取進一步的動作。 如果 Windows 程式管理程式未執行，請執行下列動作：

4.  開啟 [系統] 記錄檔。  ([流覽] 窗格中，展開 [ **Windows 記錄**]，然後選取 [ **System**. ) 

5.  使用篩選器來尋找 Hyper-v-虛擬機器的事件：
    1. 在 [**動作**] 窗格中，按一下 [**篩選目前的記錄**]。 針對 [**事件來源**]，指定 [Hyper-v-虛擬機器]。
    2. 尋找報告問題的事件。 例如，事件識別碼41表示 BIOS 設定的問題：「Hyper-v 啟動失敗;VMX 不存在或未在 BIOS 中啟用。」

### <a name="see-also"></a>另請參閱
如需在 Windows 10 上使用 Hyper-v 的詳細資訊，包括如何檢查您的電腦是否可以執行 Hyper-v，請參閱[Windows 10 Hyper-v 系統需求](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。