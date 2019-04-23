---
title: 必須執行 Windows hypervisor
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: f34e10918c60fb602c3a88ef3434cda619b11d8d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884139"
---
# <a name="windows-hypervisor-must-be-running"></a>必須執行 Windows hypervisor

>適用於：Windows Server 2016
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|必要條件|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*Windows hypervisor 未執行。*  
  
## <a name="impact"></a>影響  
  
*無法啟動虛擬機器，直到 Windows hypervisor 執行。*  
  
## <a name="resolution"></a>解析度  
  
*請檢查以查看此伺服器，是否要限定執行 HYPER-V 的 Windows Server catalog。接下來，請確定 BIOS 已啟用硬體輔助虛擬化和硬體強制執行資料執行防止。然後，檢查 Hyper V Hypervisor 事件記錄檔。*  
  
若要檢查目錄，請參閱[Windows Server catalog](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228)。  
  
> [!CAUTION]  
> 變更電腦的系統 BIOS 中的某些參數可能會造成電腦停止載入作業系統，或它可以讓硬體裝置，例如硬碟機，無法使用。 一律先查閱手冊，以判斷適當的方式來設定系統 BIOS 的電腦。 此外，它一律是個不錯的主意，來追蹤您修改，而且它們的原始值，以便您可以稍後再還原它們所需的參數。 如果您遇到問題，在變更系統 BIOS 中的參數之後，嘗試載入預設值 （會提供的選項通常在 BIOS 設定公用程式），或連絡電腦製造商尋求協助。  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>若要確認在 BIOS 或 UEFI 的虛擬化支援  
  
1.  重新啟動電腦，並透過組態工具中存取的 BIOS 或 UEFI。 這項工具的存取通常會是可用，當電腦完成開機程序時。 立即開啟大部分的電腦之後，訊息會出現幾秒鐘的時間，列出的按鍵或按鍵以開啟組態工具的組合。  
  
2.  尋找虛擬化和硬體強制執行資料執行防止 (DEP) 設定，並確認它們位於。 以下是常見的功能表位置的組態工具和他們可能的名稱是什麼的範例中的這些設定：  
  
    -   虛擬化支援：  
  
        -   通常可在主要的處理器或效能的設定。 有時會在 安全性設定。  
  
        -   尋找包含 「 虛擬化 」 或 「 虛擬化技術 」 的參數名稱。  
  
    -   硬體強制執行 DEP:  
  
        -   通常位於安全性] 或 [記憶體設定。  
  
        -   尋找包含 「 執行 」、 「 執行 」 或 「 防止 」 的參數名稱。  
  
3.  如有必要，開啟設定組態工具中的指示。 儲存變更並結束。  
  
4.  如果您進行任何變更，關閉電源，然後再回到上完成。  
  
    > [!IMPORTANT]  
    > 我們建議您關閉電源，然後重新開機 （有時稱為電源循環） 因為才發生這種情況，將不套用某些電腦上的變更。  
  
接下來，檢查 Hyper V Hypervisor 事件記錄檔。 如果有問題，您也會檢查系統記錄檔。  
  
#### <a name="to-check-the-event-logs"></a>若要檢查事件記錄檔  
  
1.  開啟 [事件檢視器]。 按一下 **開始**，按一下**系統管理工具**，然後按一下**事件檢視器**。  
  
2.  開啟 Hyper V Hypervisor 事件記錄檔。 在 [導覽] 窗格中，依序展開**Applications and Services Logs** >> **Microsoft** >> **Windows**  >>  **Hyper-V-Hypervisor**，然後按一下**操作**。  
  
3.  如果執行 Windows hypervisor，不需要任何進一步的動作。 如果 Windows hypervisor 未執行，這樣做：  
  
4.  開啟系統記錄檔。 (在 [導覽] 窗格中，依序展開**Windows 記錄檔**，然後選取**系統**。)  
  
5.  您可以使用篩選來尋找 Hyper V Hypervisor 的事件：   
    1. 在 **動作**窗格中，按一下**篩選目前的記錄**。 針對**事件來源**，指定 「 Hyper-V-Hypervisor 」。   
    2. 尋找報表問題的事件。 比方說，事件識別碼 41 表示 BIOS 設定發生問題：「 HYPER-V 啟動失敗;任一 VMX 不存在或未在 BIOS 中啟用。 」  
  
### <a name="see-also"></a>另請參閱  
如需詳細資訊包括如何檢查您的電腦可以執行 HYPER-V 的 Windows 10 上使用 HYPER-V，請參閱[Windows 10 HYPER-V 系統需求](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)。 


