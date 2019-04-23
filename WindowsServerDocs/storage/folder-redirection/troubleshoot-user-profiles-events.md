---
title: 使用者設定檔與事件疑難排解
description: 如何疑難排解問題載入和卸載使用事件和追蹤記錄檔的使用者設定檔。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827949"
---
# <a name="troubleshoot-user-profiles-with-events"></a>使用者設定檔與事件疑難排解

>適用於：Windows 10，Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2 和 Windows Server 2016。

本主題討論如何疑難排解問題載入和卸載的使用者設定檔，使用事件和追蹤記錄檔。 下列各節說明如何使用三個事件記錄檔會記錄使用者設定檔資訊。

## <a name="step-1-checking-events-in-the-application-log"></a>步驟 1：檢查應用程式記錄檔中的事件

中載入和卸載的使用者設定檔 （包括漫遊使用者設定檔） 是使用事件檢視器檢查任何使用者設定檔服務應用程式中的記錄所記錄的警告和錯誤事件的問題疑難排解的第一個步驟。

若要檢視應用程式記錄檔中的使用者設定檔服務事件的方法如下：

1. 啟動事件檢視器。 若要這樣做，請開啟**控制台中**，選取**系統及安全性**，然後在**系統管理工具**區段中，選取**檢視事件記錄檔**. [事件檢視器] 視窗隨即開啟。
2. 在主控台樹狀目錄中，第一次瀏覽至**Windows 記錄檔**，然後**應用程式**。
3. 在 [動作] 窗格中，選取**篩選目前的記錄**。 [篩選目前的記錄檔] 對話方塊隨即開啟。
4. 在 **事件來源**方塊中，選取**使用者設定檔服務**核取方塊，然後選取**確定**。
5. 檢閱事件，並特別注意的錯誤事件的清單。
6. 當您發現值得注意的事件時，選取事件日誌線上說明連結以顯示其他資訊和疑難排解程序。
7. 若要執行進一步的疑難排解，附註的日期和時間的值得注意的事件，，然後檢查作業的記錄檔 （如步驟 2 中所述） 以檢視有關使用者設定檔服務所執行的錯誤或警告的事件發生時間前後的詳細資料。

>[!NOTE]
>您可以放心地忽略使用者設定檔服務事件 1530年 「 Windows 偵測到您的登錄檔是仍在使用其他應用程式或服務。 」

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>步驟 2：檢視作業記錄檔的使用者設定檔服務

如果您不能解決此問題，使用單獨的應用程式記錄檔，請使用下列程序來檢視作業記錄檔中的使用者設定檔服務事件。 此記錄檔顯示某些服務的內部運作方式，並可協助找出位置的設定檔載入或卸載處理程序發生問題。

在所有的 Windows 安裝中的預設會啟用 Windows 應用程式記錄檔和使用者設定檔服務作業記錄檔。

若要檢視作業記錄檔的使用者設定檔服務的方法如下：

1. 在 事件檢視器主控台樹狀目錄中，瀏覽至**Applications and Services Logs**，然後**Microsoft**，然後**Windows**，然後**User Profile Service**，然後**操作**。
2. 檢查您的應用程式記錄檔中所述的錯誤或警告事件發生時間前後所發生的事件。

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>步驟 3：啟用並檢視分析和偵錯記錄檔

如果您需要比作業記錄檔所提供的更多詳細資料，您可以啟用 分析和偵錯在受影響的電腦上的記錄檔。 此層級的記錄會更詳細說明，並應停用，除非是要針對問題進行疑難排解。

以下是如何啟用和檢視分析和偵錯記錄檔：

1. 在 **動作**窗格中的 事件檢視器中，選取**檢視**，然後選取**顯示分析與偵錯記錄檔**。
2. 瀏覽至**Applications and Services Logs**，然後**Microsoft**，然後**Windows**，然後**User Profile Service**，然後**診斷**。
3. 選取 **啟用記錄**，然後選取**是**。 這可讓診斷的記錄檔，將會啟動記錄。
4. 如果您需要更多詳細資訊，請參閱[步驟 4:建立和解碼追蹤](#step-4:-creating-and-decoding-a-trace)如需有關如何建立追蹤記錄檔。
5. 當您完成疑難排解問題時，瀏覽至**診斷**記錄檔中，選取**停用記錄**，選取**檢視**，然後清除 **顯示分析與偵錯記錄檔**核取方塊以隱藏分析與偵錯記錄。

## <a name="step-4-creating-and-decoding-a-trace"></a>步驟 4：建立和解碼追蹤

如果您不能解決此問題，使用事件，您就可以重現問題，建立追蹤記錄 （ETL 檔案），並再將它使用從 Microsoft 符號伺服器的公用符號解碼。 追蹤記錄檔提供什麼 User Profile Service 正在進行，並協助找出發生失敗的特定資訊。

使用 ETL 追蹤時的最佳策略是第一次擷取可能的最小記錄。 一旦記錄檔會解碼，搜尋 失敗的記錄。

以下是如何建立和使用者設定檔服務的解碼追蹤：

1. 登入，使用者所遭遇問題，使用屬於本機 Administrators 群組的成員帳戶的電腦。
2. 從提升權限的命令提示字元輸入下列命令，其中*\<路徑\>* 是本機資料夾路徑，您先前已建立，例如 c:\\記錄檔：
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. 從 開始 畫面中，選取使用者名稱，然後按**切換帳戶**，但請小心不要將關閉系統管理員。 如果您使用遠端桌面，關閉 系統管理員工作階段，以建立使用者工作階段。
4. 重現問題。 重現問題的程序通常是以使用者遇到問題，將使用者登入，或兩者的身分登入。
5. 重現問題之後, 身分登入本機系統管理員一次。
6. 從提升權限的命令提示字元執行下列命令，以將記錄儲存到 ETL 檔案：
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. 輸入下列命令，以將 ETL 檔案匯出到目前目錄中的人們可讀取檔案 (可能是主資料夾或 %WINDIR%\\System32 資料夾中):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. 開啟**Summary.txt**檔案並**Dumpfile.xml** （您可以在中開啟它們，更輕鬆地檢視記錄檔的完整詳細資訊的 Microsoft Excel） 的檔案。 尋找包含的事件**失敗**或**失敗**; 您可以放心地忽略包含的程式行**未知**事件名稱。

## <a name="more-information"></a>詳細資訊

* [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)