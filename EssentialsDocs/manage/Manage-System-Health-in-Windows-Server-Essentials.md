---
title: "管理 Windows Server Essentials 的系統健康"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91635a58c64fbf74d3b0139be7c9c36365487319
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-system-health-in-windows-server-essentials"></a>管理 Windows Server Essentials 的系統健康

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
 
 本主題討論如何檢視和回應使用儀表板您網路中的所有通知。  
  
> [!NOTE]
>  在 Windows Server Essentials 和 Windows Server 2012 R2 已安裝 Windows Server Essentials 體驗的角色，健康警示伺服器及 client 電腦在網路不會再顯示警示檢視器，但改為可以檢視在**健康報告**索引標籤的**Home**頁面。  
  
 Windows Server Essentials 主動監視每一部電腦已連接到伺服器與警示相關系統 s 健康狀態，包括重要的更新問題系統管理員遺失惡意程式碼防護、 過期病毒定義 client 在電腦上，以及其他重要需要動作的問題。 這些問題會顯示為通知在 [提醒檢視器可以發自伺服器 s 儀表板或 client 電腦的 Launchpad Windows Server Essentials，或在**健康報告**索引標籤，在 Windows Server Essentials。 根據預設，重新整理警示每隔 30 分鐘，但您也可以隨時評估您的網路警示，即可**重新整理**中檢視器警示，或在**健康報告**索引標籤。  
  
 下列主題會協助您了解、 檢視與回應警示警示檢視器中及也提供指示來設定您的電子郵件中收到警示通知，伺服器：  
  
-   [健康相關報告增益集](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [檢視警示使用警示檢視器](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [整理警示警示檢視器](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [回應警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [設定電子郵件通知的警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [潛在的電腦警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>健康相關報告增益集  
 健康報告的增益集 Windows Server Essentials 提供整合 Windows Server Essentials 網路相關資訊，可讓您將這項資訊給其他人。 此資訊可在**報告**索引標籤的儀表板。 使用**報告**索引標籤上，您可以執行下列動作：  
  
-   [產生視或排程報告](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [自訂報告的 content](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [電子郵件報告](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:**您可以下載 「 健康報告增益集從 Windows Server Essentials 的[Microsoft 下載中心 」](https://go.microsoft.com/fwlink/p/?LinkId=266342)。  
>   
>  **Windows Server Essentials:**預設健康報告增益集整合與 Windows Server Essentials 或 Windows Server 2012 R2 與 Windows Server Essentials 體驗角色安裝，以及健康報告也會顯示在**健康報告**] 索引標籤的儀表板 s **Home**頁面。  
  
###  <a name="BKMK_Generate"></a>產生視或排程報告  
 健康報告增益集安裝並重新儀表板，新的索引標籤後,**報告**會新增至儀表板。 您可以按一下產生視隨時健康報告**產生健康報告**工作在**報告**索引標籤。  
  
 健康報告也之後，清單窗格中，由的日期和時間產生報告中建立新的項目。 若要打開項目，您可以在 [清單] 窗格中，按兩下或您可以選取它，然後按一下**開放健康報告**窗格中。 報告是 HTML 格式在新視窗中顯示。  
  
 除了以手動方式產生報告，您也可以會每日或每小時為排程自動轉換報告。 若要這樣做，請在 [工作] 窗格中，按一下 [**自訂健康報告設定**，然後按一下 [**排程與電子郵件**索引標籤。**排程**功能預設會關閉，並關閉，選取 [**產生健康報告排定的時間在**核取方塊。  
  
###  <a name="BKMK_Customize"></a>自訂報告的 content  
 健康報告包含下列項目：  
  
-   **重要警示和警告**的重要提醒和警示儀表板上檢視器中看到的警告與一致，這是。 資訊警示 」 不包含健康報告。  
  
-   **嚴重錯誤事件登入**的應用程式與服務登，以及登入過去 24 小時錯誤會出現在**的詳細資料**報告的區段。  
  
-   **伺服器備份**上次伺服器備份的資訊，會出現在**的詳細資料**區段的報告。  
  
-   **自動開始服務無法執行**同時也報告，如果不執行自動 [開始] 畫面的服務，此服務有關的資訊會列在**的詳細資料**區段的報告。  
  
-   **更新**您可以看到伺服器中的所有 client 電腦的更新狀態**的詳細資料**區段。  
  
-   **儲存**在中顯示的清單中的磁碟機的容量**的詳細資料**區段。  
  
 健康報告，在第一次檢視**摘要**，然後紅色錯誤圖示或黃色警告圖示這些項目，按**的詳細資料**檢視詳細資料項目的相關相同的資料列上的連結。  
  
 如果您不感興趣的資料點預設包含在報告的一些，您可以自訂報告的 content 按一下**自訂健康報告設定**中工作窗格中，然後按一下 [ **Content**] 索引標籤清除您不要 t 想要查看報告中 content 核取方塊。 例如，如果您有自己的伺服器備份計劃 t 不想要查看的伺服器備份的相關警告，您可能會排除伺服器備份報告清除**伺服器備份**核取方塊。  
  
###  <a name="BKMK_emailreport"></a>電子郵件報告  
 登入儀表板朗讀報告是仍然方便的一些系統管理員，尤其是有一部以上的伺服器管理。 「 郵件 」 功能的亮起，也報告之後，電子郵件將被傳送至指定的電子郵件地址與 content 報告的清單。 系統管理員可以輕鬆地檢視的任何裝置或任何 client 的應用程式，這份報告，並確保伺服器在其最佳狀態執行。  
  
 在**健康自訂報告設定**對話方塊中，您可以電子郵件、 變更 SMTP 設定，以及指定的電子郵件收件者的清單之後您會注意到新的任務會出現在 [工作] 窗格：**以電子郵件傳送健康報告**。 如需 SMTP 設定的詳細資訊，請查看[設定電子郵件通知的警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)。  
  
 您可以選取現有的報告，然後按一下**以電子郵件傳送健康報告**。 您也可以產生新的報告，並讓它會自動傳送給您的收件匣。 如果您已設定自動被轉換報告的排程，報告將會自動傳遞至您的收件匣之後產生排程每天 （或每個小時的時間）。  
  
##  <a name="BKMK_View"></a>檢視警示使用警示檢視器  
 本章節將告訴您如何使用儀表板或 Launchpad 開放警示檢視器檢視伺服器網路上的所有電腦的健康狀態。  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>若要使用儀表板開放警示檢視器  
  
1.  打開儀表板。  
  
2.  在瀏覽窗格中，按一下任何顯示的警示圖示 （重大、 警告或資訊）。 這樣警示檢視器。  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>若要從 Launchpad 開放警示檢視器  
  
1.  從電腦連接到伺服器，開放 Launchpad。 如果系統要求，登入 Launchpad 與您的使用者名稱和密碼。  
  
2.  按一下任一項打開警示檢視器 Launchpad 底部顯示警示圖示 （重大、 警告，而且資訊），然後依照指示詳細資料窗格中的 [提醒] 檢視器解析警示。  
  
##  <a name="BKMK_Organize"></a>整理警示警示檢視器  
 您組織警示檢視器中的通知，顯示他們可以依據他們嚴重性 （重大、 警告或資訊），或根據電腦名稱。  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>組織警示警示檢視器  
  
1.  打開儀表板。  
  
2.  在瀏覽窗格中，按一下 [顯示的通知圖示 （重大、 警告或資訊） 的任何。 這樣警示檢視器。  
  
3.  展開**組織**下拉式清單中，然後執行下列其中一個動作：  
  
    1.  選取**電腦依篩選**，然後按一下您想要檢視通知的電腦名稱。 這會顯示提醒，僅限用於所選取的電腦警示檢視器中。  
  
    2.  選取 [**篩選警示類型，**，然後按一下您想要檢視警示的通知類型 （重大、 警告或資訊）。 會顯示此選項僅選取警示類型警示檢視器中。  
  
##  <a name="BKMK_Respond"></a>回應警示  
 當您遇到提醒時，您可以選擇加入執行下列其中一個動作：  
  
-   [解除警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [忽略警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [讓警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Delete 警示](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>解除警示  
 依照解析警示解析度指示警示檢視器中。 警示解析之後，它仍會顯示，警示檢視器中重新整理之前。  
  
###  <a name="BKMK_3"></a>忽略警示  
 您可以選擇略過警示，如果您希望稍後回應。 當您略過警示時，它仍會列在警示檢視器和，但它是停用，灰色。 忽略的警示不在電腦的整體健康評量。 若要處理略過的警示，您需要以便警示。  
  
##### <a name="to-ignore-an-alert"></a>忽略警示  
  
1.  Windows Server Essentials 伺服器連接的電腦上，從左 Launchpad。  
  
2.  Launchpad，按一下 [顯示通知圖示 （重大、 警告，而且資訊） 的任何。 這樣警示檢視器。  
  
3.  警示檢視器中，選取您想要略過、 提醒，然後在**工作**區段中，按**忽略警示**。  
  
 若要回應停用的警示，您將需要第一次以便警示。  
  
###  <a name="BKMK_5"></a>讓警示  
 您可以讓您選擇略過稍早警示。 警示會支援之後，您可以解析它或視 delete 它。 在標示為忽略時，停用，就會出現通知。 您可讓您先前已停用警示，它變成作用中，並再試一次包含在的電腦整體健康評量。  
  
##### <a name="to-enable-an-alert"></a>若要讓警示  
  
1.  伺服器連接的電腦上，從左 Launchpad。  
  
2.  在 Launchpad，按一下任何打開檢視器通知的顯示警示圖示 （重大、 警告，而且資訊）。  
  
3.  在警示檢視器，以滑鼠右鍵按一下您想要讓，然後按一下 [提醒**讓警示**。  
  
###  <a name="BKMK_4"></a>Delete 警示  
 您可以 delete 警示，如果您不想解析或請忽略它。 您可以使用在 Launchpad 警示檢視器 delete 警示因您的電腦。 如果您 delete 警示，並再試一次在下一個網路健康評估循環伺服器偵測到的問題，它會產生新警示。  
  
##### <a name="to-delete-an-alert"></a>若要 delete 警示  
  
1.  伺服器連接的電腦上，從左 Launchpad。  
  
2.  在 Launchpad，按一下任何打開檢視器通知的顯示警示圖示 （重大、 警告，而且資訊）。  
  
3.  在警示檢視器，以滑鼠右鍵按一下您想要 delete，然後按一下 [提醒**Delete 警示**。  
  
##  <a name="BKMK_Email"></a>設定電子郵件通知的警示  
 您可以設定您的伺服器來透過相關警示的項目電子郵件通知您。 這些警示的電子郵件通知包含及相關資訊的網路問題它解析度的步驟，這是相同的資訊，會顯示提醒檢視器中。 部分的網路健康評估會以程式設計方式。  
  
 當您設定您的伺服器來傳送電子郵件中的警示通知時，通知期間網路健康評估找到傳送的電子郵件通知。 不過，並非所有警示檢視器中報告的警示的電子郵件中回報。  
  
 每 30 分鐘，伺服器，評估警示網路上執行工作警示的電子郵件評估。 設定電子郵件通知，就會發生的任何警示傳送電子郵件通知。 如果警示是以避免流信箱下次評估循環仍在使用中不會傳送具有第二個電子郵件。 不過，如果在未來的警示評估循環偵測到新警示時，電子郵件通知就會傳送，包括新與先前警示。  
  
###  <a name="BKMK_list"></a>警示的電子郵件通知會導致  
 當您設定您的伺服器來傳送電子郵件通知的警示下列警示警示檢視器中的導致電子郵件通知：  
  
-   Client 電腦備份中有錯誤。  
  
-   請洽詢。  
  
-   一或多個服務無法執行。  
  
-   不執行 Windows Server 儲存服務。  
  
-   您在評估期間是透過。  
  
-   立即啟動。  
  
-   來源伺服器已關機。  
  
-   BPA 掃描結果可能包含錯誤。  
  
-   BPA 掃描結果可能包含警告。  
  
-   無法使用的任何地方存取憑證。  
  
-   已經過期的憑證隨處存取。  
  
-   在路由器設定不正確。  
  
-   未正確設定網頁伺服器。  
  
-   遠端桌面服務設定不正確。  
  
-   防火牆設定不正確。  
  
-   網際網路網域名稱已過期。  
  
-   無法更新網際網路網域名稱。  
  
-   授權錯誤： 森林信任檢查。  
  
-   授權錯誤： 網域控制站檢查。  
  
-   授權錯誤： FSMO 角色檢查。  
  
-   授權錯誤： 執法 FSMO 原則。  
  
-   授權錯誤： 執法載入原則。  
  
-   授權錯誤： Active Directory Domain Services。  
  
-   您的 Office 365 裝機費已過期。  
  
-   Office 365 驗證失敗。  
  
-   不正確的密碼的原則。  
  
-   密碼同步服務無法使用 Office 365 同步使用者密碼。  
  
-   變更 Windows 密碼。  
  
-   Office 365 密碼不是您的 Windows 密碼相同。  
  
-   無法連接到 Exchange Server。  
  
-   Microsoft Exchange Server 發生問題。  
  
-   未連接一或多個硬碟伺服器的備份。  
  
-   備份的硬碟機不會有可用空間的伺服器備份。  
  
-   因為可能會不會取得磁碟機快照伺服器備份未成功。  
  
-   未成功完成排定的備份。  
  
-   一或多個伺服器預先定義的資料夾遺失。  
  
-   可用空間不足一或多個伺服器硬碟機。  
  
-   VSS 寫入儲存服務無法執行。  
  
-   在硬碟上的儲存空間不足容量。  
  
-   一或多個磁碟機，無法正常運作且離線。  
  
###  <a name="BKMK_SMTP"></a>在您的伺服器來傳送電子郵件中的 Windows Server Essentials 警示的通知設定 SMTP  
 本節如何設定您的伺服器來傳送電子郵件通知的警示。  
  
> [!NOTE]
>  您可以下載 「 健康報告增益集從 Windows Server Essentials 的[Microsoft 下載中心](https://go.microsoft.com/fwlink/p/?LinkId=266342)。  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>設定電子郵件通知的警示  
  
1.  從**儀表板**，開放**警示檢視器**。  
  
2.  在**警示檢視器**，按一下 [**設定警示的電子郵件通知**。  
  
3.  在**設定電子郵件通知的警示**視窗中，按**支援**。  
  
4.  在**SMTP 設定**視窗中，執行下列動作：  
  
    1.  適用於**的電子郵件地址**，輸入您想要使用的電子郵件給我們的電子郵件地址，系統會從通知。 此電子郵件地址會顯示為通知寄件者的位址。  
  
    2.  適用於**SMTP 伺服器名稱**，請在**的電子郵件地址**文字方塊中，輸入您在步驟匣 4a 指定 SMTP 伺服器的名稱。 （請參考下表 1 的幾個部分 SMTP 伺服器名稱）。  
  
    3.  適用於**SMTP 連接埠**，輸入用來傳送和接收的電子郵件 SMTP 伺服器連接埠號碼。 （請參考下表 1 的所使用的部分 SMTP 伺服器連接埠號碼）。  
  
    4.  選取 [**這個伺服器需要安全連接 (SSL)**如果 SMTP 伺服器使用 SSL （查看表 1）。  
  
    5.  選取 [**這個伺服器需要驗證**如果 SMTP 伺服器需要一的使用者名稱和密碼的資訊 （查看表 1）。 如果您選取此核取方塊，輸入的使用者名稱和密碼的電子郵件地址，您輸入**的電子郵件地址**欄位中執行 「 步驟匣 4a、，然後按一下 [ **[確定]**。  
  
    > [!NOTE]
    >  您可以從您的網際網路服務提供者取得 SMTP 伺服器名稱、 連接埠號碼，並使用 SSL 有關的資訊。  
  
     **下表 1** SMTP 的範例伺服器名稱、 驗證和 SSL 加密需求，以及連接埠號碼  
  
    |SMTP 伺服器|所需的 SSL|需要驗證|連接埠號碼|帳號名稱日登入|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|[是]|[是]|587|驗證的網域名稱和密碼提供完整的電子郵件地址。|  
    |smtp.live.com|[是]|[是]|587|驗證的網域名稱和密碼提供完整的電子郵件地址。|  
    |smtp.comcast.net|[是]|否]|587|驗證的網域名稱和密碼提供完整的電子郵件地址。|  
    |smtp.mail.yahoo.com|否]|[是]|25|提供只不的使用者名稱的網域名稱的電子郵件地址。|  
  
5.  在**設定警示通知**，為**電子郵件收件者**，輸入您想要透過電子郵件中收到警示通知人員的電子郵件地址。 請確定您的分號 （;） 將每個電子郵件地址。  
  
6.  若要確認您已設定 SMTP 伺服器設定正確傳送電子郵件通知的警示，按一下 [**套用並傳送電子郵件**。  
  
    > [!NOTE]
    >  當您按下**套用並傳送電子郵件**，通常是您會收到範例電子郵件通知未列出的健康警示。 不過，如果已設定為在傳送電子郵件通知的健康警示找出這項測試程序期間，此警示包含測試電子郵件中。  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>在您的伺服器來傳送健康報告 Windows Server Essentials 設定 SMTP  
 本節如何設定您的伺服器 SMTP 設定，以便您可以收到健康報告透過電子郵件。  
  
> [!NOTE]
>  預設健康報告增益集整合與 Windows Server Essentials 或 Windows Server 2012 R2 與 Windows Server Essentials 體驗角色安裝，以及健康報告也會顯示在**健康報告**索引標籤的儀表板 s **Home**頁面。  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>設定電子郵件通知的健康報告  
  
1.  從**儀表板**，按一下 [**報告**索引標籤。  
  
2.  在**健康報告工作**工作窗格中，按一下 [**自訂健康報告設定**。  
  
3.  在**健康自訂報告設定**視窗中，按**排程與電子郵件**索引標籤。  
  
4.  在**排程與電子郵件**索引標籤的**的電子郵件**區段中，執行下列動作：  
  
    1.  按一下**讓**，然後輸入您想要傳送健康使用的電子郵件地址報告。 此電子郵件地址會顯示為透過電子郵件傳送健康報告寄件者的位址。  
  
        1.  適用於**SMTP 伺服器名稱**，輸入 SMTP 伺服器的名稱。 （請參考下表 1 的幾個部分 SMTP 伺服器名稱）。  
  
        2.  適用於**SMTP 連接埠**，輸入用來傳送和接收的電子郵件 SMTP 伺服器連接埠號碼。 （請參考下表 1 的所使用的部分 SMTP 伺服器連接埠號碼）。  
  
        3.  選取 [**這個伺服器需要安全連接 (SSL)**如果 SMTP 伺服器使用 SSL （查看表 1）。  
  
        4.  選取 [**這個伺服器需要驗證**如果 SMTP 伺服器需要一的使用者名稱和密碼的資訊 （查看表 1）。 如果您選取此核取方塊，輸入的使用者名稱和密碼的電子郵件地址，您輸入**的電子郵件地址**欄位中執行 「 步驟匣 4a、，然後按一下 [ **[確定]**。  
  
            > [!NOTE]
            >  您可以從您的網際網路服務提供者取得 SMTP 伺服器名稱、 連接埠號碼，並使用 SSL 有關的資訊。  
  
            > [!NOTE]
            >  Microsoft 非常建議使用 SSL 因為報告可能會包含伺服器可能會由惡意派對偵測到的安全漏洞的狀態 (例如： 遺失的 windows 更新)。 讓 SSL 將加密的資料傳輸與降低公開伺服器弱點的風險。  
  
5.  之後您可以電子郵件，**設定變更 SMTP**顯示的連結。 也新的工作，**以電子郵件傳送健康報告**，就會顯示在**健康報告工作**。  
  
     **下表 1** SMTP 的範例伺服器名稱、 驗證和 SSL 加密需求，以及連接埠號碼  
  
    |SMTP 伺服器|所需的 SSL|需要驗證|連接埠號碼|帳號名稱日登入|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|[是]|[是]|587|驗證的網域名稱和密碼提供完整的電子郵件地址。|  
    |smtp.live.com|[是]|[是]|587|驗證的網域名稱和密碼提供完整的電子郵件地址。|  
    |smtp.comcast.net|[是]|否]|587|驗證的網域名稱和密碼提供完整的電子郵件地址。|  
    |smtp.mail.yahoo.com|否]|[是]|25|提供只不的使用者名稱的網域名稱的電子郵件地址。|  
  
6.  在**健康自訂報告設定**，為**自動將健康報告傳送給下列電子郵件收件者：**，輸入您想要透過電子郵件接收健康報告人員的電子郵件地址。 請確定您的分號 （;） 將每個電子郵件地址。  
  
7.  若要確認您已設定 SMTP 伺服器設定正確要透過電子郵件、 健康報告傳送 Helath 報告] 索引標籤上儀表板，選取報告，然後按一下**以電子郵件傳送健康報告**窗格中。  
  
##  <a name="BKMK_Potential"></a>潛在的電腦警示  
 本章節討論了解及管理您的電腦，已連接到伺服器以及，會顯示在您的電腦 Launchpad 特定的通知。  
  
 下表列出一些產生，如果是適用於您電腦上警示檢視器顯示電腦提醒。  
  
|警示標題|警示影響和解析度|  
|-----------------|---------------------------------|  
|目前的網路防火牆狀態提供精簡的保護，這台電腦。|未經授權的人員或軟體可能無法存取 Windows 防火牆不已這台電腦。|  
|防毒保護已關閉，無法安裝，或不是最新。|您的電腦上的資料是風險如果**病毒防護**關閉或不更新安全性設定。 [為保護您的電腦](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect)，請依照下列步驟的指示。|  
|間諜軟體及垃圾的軟體保護已關閉，無法安裝，或不最新狀態。|您的電腦上的資料是風險如果**間諜軟體和不想要的軟體保護**關閉或更新。 [為保護您的電腦](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect)，請依照下列步驟的指示。|  
|Windows Update 已關閉。|您將無法再的新功能和修正功能的更新受益，除非在 Windows Update 會亮起來。 按一下 [將 Windows Update，請在提醒檢視器]**開放的 Windows Update**。<br /><br /> 如果**開放的 Windows Update**工作不會顯示，您無法登入的電腦位置的提醒。 您必須登入的電腦警示執行這項工作警示檢視器中。|  
|應該安裝重要的更新。|您將無法再的新功能和修正功能的更新受益，除非在 Windows Update 會亮起來。 按一下 [將 Windows Update，請在提醒檢視器]**開放的 Windows Update**。<br /><br /> 如果**開放的 Windows Update**工作不會顯示，您無法登入的電腦位置的提醒。 您必須登入的電腦警示執行這項工作警示檢視器中。|  
|套用更新電腦重新開機。|您將無法再的新功能和修正功能的更新受益，直到套用。 儲存您的資料，並重新開機，套用更新。|  
|在硬碟上的可用空間不足。|如果您不做可用空間，您無法儲存的其他資訊。 若要增加電腦上的可用空間，請考慮將下列選項：<br /><br /> 新增新的硬碟機。<br /><br /> -執行**[磁碟清理]**若要移除舊和暫存檔案。<br /><br /> -移動檔案的共用資料夾另一部電腦上。<br /><br /> 保存在抽取式媒體，例如 CD、 DVD 或外接式硬碟上的檔案。|  
|**檔案歷史**在此電腦上執行未正確設定伺服器上的代理程式。|無法建立檔案歷史備份。|  
|一或多個服務無法執行。||  
|變更 Windows 密碼。||  
|Microsoft Office 365 密碼不是您的 Windows 密碼相同。||  
  
###  <a name="BKMK_Protect"></a>為保護您的電腦  
  
1.  開放資訊安全中心。  
  
2.  判斷安裝的防毒保護的狀態。  
  
3.  執行下列工作而定保護狀態的其中一個動作：  
  
    -   如果這不支援，讓它。  
  
    -   如果不是最新狀態，完成更新的特徵標記程序。  
  
    -   如果尚未安裝防毒保護，請考慮將安裝它。  
  
## <a name="see-also"></a>也了  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
