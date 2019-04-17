---
title: 疑難排解使用者設定檔的事件
description: 如何疑難排解載入和卸載使用者設定檔使用事件及追蹤記錄檔。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082013"
---
# <a name="troubleshoot-user-profiles-with-events"></a>疑難排解使用者設定檔的事件

>適用於： Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2 及 Windows Server 2016。

本主題說明如何疑難排解載入和卸載使用者設定檔使用事件及追蹤記錄檔。 下列各節說明如何使用三個使用者設定檔資訊記錄的事件記錄檔。

## <a name="step-1-checking-events-in-the-application-log"></a>步驟 1： 檢查應用程式記錄檔中的事件

疑難排解問題載入和卸載的使用者 （包括漫遊使用者設定檔） 的設定檔是使用事件檢視器來檢查使用者設定檔服務應用程式中的記錄記錄任何警告和錯誤事件中第一個步驟。

以下是如何檢視應用程式記錄檔中的使用者設定檔服務事件：

1. 啟動事件檢視器。 若要這樣做，開啟 **[控制台]**、 選取 [**系統及安全性**]，然後、 [**系統管理工具**] 區段中選取**檢視事件記錄檔**。 [事件檢視器] 視窗隨即開啟。
2. 在主控台樹狀目錄中第一次瀏覽至**Windows 記錄檔**，然後**應用程式**。
3. 在 [動作] 窗格中，選取 [**篩選目前的記錄**。 [篩選目前的記錄] 對話方塊隨即開啟。
4. [**事件來源**] 方塊中選取**使用者設定檔服務**] 核取方塊，然後再選取 **[確定]**。
5. 檢閱清單的清單應特別注意錯誤事件的事件。
6. 當您尋找值得注意的事件時，請選取事件記錄檔線上說明連結以顯示其他資訊及疑難排解程序。
7. 執行進一步的疑難排解，記下的日期和時間的重大事件，然後檢查操作的記錄檔 （如步驟 2 中所述） 來檢視關於使用者設定檔服務已執行周圍的錯誤或警告事件的時間的詳細資料。

>[!NOTE]
>您可以放心地忽略 User Profile Service 事件 1530年"Windows 偵測到登錄檔案時仍在使用其他應用程式或服務中。 」

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>步驟 2： 使用者設定檔服務的檢視作業記錄檔

如果您不能解決使用單獨的應用程式記錄檔的問題，請使用下列程序可檢視使用者設定檔服務事件操作的記錄檔中。 此記錄檔顯示服務內部運作的一些並可協助找出在設定檔負載的位置或卸載發生問題的處理程序。

Windows 應用程式記錄檔和使用者設定檔服務運作的記錄檔會啟用所有 Windows 安裝中的預設值。

以下是如何操作的記錄檔檢視使用者設定檔服務：

1. 在事件檢視器主控台樹狀目錄，瀏覽至 [**應用程式及服務記錄檔**，則**Microsoft**，則**Windows**、 則**User Profile Service**] 及 [然後**操作**。
2. 檢查所發生事件的周圍所述的應用程式記錄檔錯誤或警告事件的時間。

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>步驟 3： 啟用及檢視分析與偵錯記錄檔

如果您需要比操作的記錄檔提供的詳細資訊，您可以啟用分析與偵錯記錄在受影響的電腦上。 記錄的此層級更詳細的而且應停用以外時嘗試疑難排解問題。

以下是如何啟用及檢視分析與偵錯記錄檔：

1. 在事件檢視器的 [**動作**] 窗格中，選取 [**檢視**]，然後選取 [**顯示分析與偵錯記錄檔**。
2. 瀏覽至 [**應用程式及服務記錄檔**，則**Microsoft**，則**Windows**則**與使用者設定檔服務**，然後**診斷**。
3. 選取 [**啟用記錄]** ，然後選取 [**是]**。 這可讓診斷記錄檔，將會開始記錄。
4. 如果您需要更多詳細資訊，請參閱[步驟 4： 建立及解碼追蹤](#step-4:-creating-and-decoding-a-trace)如需如何建立追蹤記錄檔的詳細資訊。
5. 完成時疑難排解問題、 瀏覽至 [**診斷**記錄檔、 選取 [**停用記錄]**、 選取 [**檢視**，然後清除 [**顯示分析與偵錯記錄檔**] 核取方塊來隱藏分析與偵錯記錄。

## <a name="step-4-creating-and-decoding-a-trace"></a>步驟 4： 建立及解碼追蹤

如果您無法解決問題使用事件，您可以建立追蹤記錄 （ETL 檔案） 時重現問題，並再將它使用從 Microsoft 符號伺服器的公用符號解碼。 追蹤記錄檔提供使用者設定檔服務執行，以及可幫助查明發生失敗的非常特定資訊。

使用 ETL 追蹤時的最佳策略是先擷取可能的最小記錄檔。 一旦解碼記錄檔，搜尋失敗的記錄。

以下是如何建立及解碼追蹤使用者設定檔服務：

1. 電腦其中使用者會遇到問題，使用屬於本機 Administrators 群組的成員帳戶登入。
2. 從提升權限的命令提示字元輸入下列命令，其中*\ < 路徑 >* 是至本機資料夾您先前已建立，例如 C:\\logs 路徑：
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. 從 [開始] 畫面中，選取的使用者名稱，然後再選取**交換器帳戶**正在小心不要將登出系統管理員。 如果您使用遠端桌面，關閉 [管理員工作階段來建立使用者工作階段。
4. 重現問題。 重現問題的程序通常是來登入為面臨議題、 關閉、 登入使用者的使用者或兩者。
5. 之後重現問題，登入本機系統管理員身分一次。
6. 從提升權限的命令提示字元執行下列命令將 ETL 檔案儲存在記錄檔：
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. 輸入下列命令，將 ETL 檔案匯出至目前目錄 （可能是您隸屬的資料夾或 %WINDIR%\\System32 資料夾） 中的人力資源可讀取檔案：
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. 開啟**Summary.txt**檔案和**Dumpfile.xml**檔案 （您可以開啟其在 Microsoft Excel 中更輕鬆地檢視記錄檔的完整詳細資料）。 尋找包含**失敗**或**失敗**； 的事件您可以放心地忽略的**未知**的事件名稱包含的行。

## <a name="more-information"></a>更多資訊

* [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)