---
title: 使用 My Server 應用程式來連線到 Windows Server Essentials
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: 4e40b57f-6917-43ef-92e0-030baa9d2b99
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: 2b4346a3fddd8b1b8f79890ff8924e94bededfbc
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947365"
---
# <a name="use-the-my-server-app-to-connect-to-windows-server-essentials"></a>使用 My Server 應用程式來連線到 Windows Server Essentials

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

適用于 Windows Server Essentials 的 My Server 應用程式可讓您從膝上型電腦、個人電腦或 Surface 裝置連線到資源，並在您的 Windows Server Essentials 伺服器上執行輕系統管理工作。 在 [我的伺服器] 中，您可以管理使用者、裝置和警示，以及使用伺服器上的共用檔案。 離線時，您可以繼續使用最近在 [我的伺服器] 中存取的檔案，下次連線時，您的離線變更會與伺服器自動同步處理。  
  
 如果您的伺服器正在執行 Windows Server Essentials 作業系統，請使用原始的 My Server 應用程式。 如果您的伺服器執行的是 Windows Server Essentials 作業系統，則必須使用更新的 My Server 2012 R2 應用程式。 這兩個應用程式皆可從 [Windows 適用的應用程式](https://windows.microsoft.com/windows-8/apps)下載。  
  
> [!TIP]
> 
>  若要從您的 Windows Phone 存取 Windows Server Essentials 中的資源，請使用 My Server Phone 應用程式（適用于 Windows Server Essentials）或 My Server 2012 R2 Phone 應用程式（適用于 Windows Server Essentials）。 如需 My Server phone 應用程式的相關資訊，請參閱[使用 My server 應用程式進行 Windows Phone](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)。 如需 My Server 2012 R2 Phone 應用程式的相關資訊，請參閱部落格文章： [My Server 2012 R2 Windows 和 Windows Phone 應用程式](https://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx)。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [My Server 2012 R2 的新功能](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_WhatsNew)  
  
-   [作業系統需求](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_OS)  
  
-   [安裝我的伺服器](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Install)  
  
-   [使用我的伺服器](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_UseServer)  
  
-   [My Server 的功能](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Features)  
  
-   [如何從局域網路連接到您的伺服器](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_ConnectServer)  

>  若要從您的 Windows Phone 存取 Windows Server Essentials 中的資源，請使用 My Server Phone 應用程式（適用于 Windows Server Essentials）或 My Server 2012 R2 Phone 應用程式（適用于 Windows Server Essentials）。 如需 My Server phone 應用程式的相關資訊，請參閱[使用 My server 應用程式進行 Windows Phone](../use/Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)。 如需 My Server 2012 R2 Phone 應用程式的相關資訊，請參閱部落格文章： [My Server 2012 R2 Windows 和 Windows Phone 應用程式](https://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx)。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [My Server 2012 R2 的新功能](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_WhatsNew)  
  
-   [作業系統需求](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_OS)  
  
-   [安裝我的伺服器](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Install)  
  
-   [使用我的伺服器](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_UseServer)  
  
-   [My Server 的功能](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Features)  
  
-   [如何從局域網路連接到您的伺服器](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_ConnectServer)  

  
##  <a name="BKMK_WhatsNew"></a>My Server 2012 R2 的新功能  
 除了「我的伺服器」中可用的功能之外，「我的伺服器 2012 R2」也為使用 Windows Server Essentials 的客戶提供下列新功能：  
  
-   **存取 Sharepoint online 小組網站和文件庫**-利用與 Windows server Essentials Microsoft Office 365 整合來存取 sharepoint online 小組網站，並在我的伺服器 2012 R2 中開啟 sharepoint online 文件庫中的檔。  
  
-   **與伺服器和用戶端電腦的遠端桌面**連線-使用 My Server 2012 R2 中的**遠端**連線，透過遠端桌面連線到您的 Windows server Essentials 伺服器和用戶端電腦。  
  
##  <a name="BKMK_OS"></a>作業系統需求  
 若要在您的膝上型電腦、個人電腦或 Surface 裝置上使用「我的伺服器」或「我的伺服器 2012 R2」，電腦必須具有下列其中一種作業系統： Windows 8.1、Windows RT 8.1、Windows 8 或 Windows RT。  
  
##  <a name="BKMK_Install"></a>安裝我的伺服器  
 您可以從 [Windows 適用的應用程式](https://windows.microsoft.com/windows-8/apps) 市集下載合適的 My Server 應用程式。 如果您的電腦具有 Windows 8 或 Windows RT 作業系統，而且您想要使用伺服器名稱連接到內部網路上的伺服器，則也需要從伺服器安裝憑證。  
  
#### <a name="to-install-my-server-or-my-server-2012-r2-on-your-windows-based-laptop-pc-or-surface-device"></a>在您的 Windows 膝上型電腦、個人電腦或 Surface 裝置上安裝 My Server 或 My Server 2012 R2  
  
1.  從 [Windows 適用的應用程式](https://windows.microsoft.com/windows-8/apps) 安裝合適的 My Server 應用程式到您的 Windows 膝上型電腦、個人電腦或 Surface 裝置。  
  
    |伺服器作業系統：|您剛才購買的產品旁的 [下載]|  
    |-----------------------------|--------------|  
    | Windows Server Essentials|[我的伺服器](https://apps.microsoft.com/webpdp/app/19d94f81-db21-4234-8b49-806694dbbaea)|  
    | Windows Server Essentials|[我的伺服器 2012 R2](https://apps.microsoft.com/webpdp/app/67e86695-bda3-4f32-96c4-2e20e56f1cf3)|  
  
2.  如果您想要使用伺服器名稱透過內部網路連線到您的 Windows Server Essentials 伺服器，且您的裝置具有 Windows 8 或 Windows RT 作業系統，請執行下列 p，在您的電腦上安裝預設的 CAROOT.CER .cer 憑證rocedure. Windows 8.1 和 Windows RT 8.1 中不需要手動安裝憑證。  
  
###  <a name="BKMK_InstallCert"></a>若要在 Windows 8、Windows 8.1 或 Windows RT 裝置上安裝「我的伺服器」的憑證  
  
1.  請開啟您電腦上的網頁瀏覽器，並從該伺服器下載預設憑證 CAROOT.cer。 若要這樣做，請輸入下列命令，將伺服器的名稱 (例如 **marketing.contoso.com**) 以 <*伺服器名稱*> 取代：  
  
     **HTTP：//< servername\>/connect/default.aspx？ get = caroot.cer .cer**  
  
2.  安裝憑證：  
  
    1.  在 [開始] 頁面中，開啟 [檔案總管]。  
  
    2.  請確定會顯示隱藏的項目和檔案名稱副檔名：在 [檢視] 索引標籤的 **[顯示/隱藏]** 群組，選取 [隱藏的項目] 和 [副檔名] 核取方塊。  
  
    3.  巡覽至包含您剛才下載之 CAROOT.cer 檔案的資料夾。  
  
    4.  以滑鼠右鍵按一下 CAROOT.cer 檔案，然後按一下 [開啟]。  
  
    5.  在 [憑證] 對話方塊中按一下 [安裝憑證]。  
  
    6.  選取 [本機電腦] 做為安裝位置，然後按 [下一步]。  
  
    7.  在 **[憑證存放區精靈]** 頁面上，選取 [將所有憑證放入以下的存放區]，並使用 [瀏覽] 來選擇 [受信任的根憑證授權單位] 存放區。 然後按一下 [完成]。  
  
##  <a name="BKMK_UseServer"></a>使用我的伺服器  
 若要開始使用 My Server 或 My Server 2012 R2 應用程式，請開啟應用程式和其功能的快速導覽。  
  
#### <a name="to-open-my-server-or-my-server-2012-r2"></a>開啟 My Server 或 My Server 2012 R2  
  
1.  從 Windows 裝置的 [**開始**] 頁面開啟 [我的伺服器] （適用于 Windows server essentials）或 [我的伺服器 2012 R2] （適用于 Windows server essentials）。  
  
2.  使用您在伺服器上的帳戶登入 Windows Server Essentials 伺服器。 若要識別伺服器：  
  
    -   在外部部署環境中，請使用伺服器的功能變數名稱，例如**marketing.contoso.com**。  
  
    -   如果您要透過內部網路連線到內部部署，請使用伺服器名稱-在上述範例中，伺服器名稱為**marketing**。 （如果您使用 Windows 8 或 Windows RT 電腦連線內部部署，而您想要使用這個方法，則必須從伺服器安裝 CAROOT.CER 憑證。 如需詳細資訊，請參閱[若要在 windows 8、Windows 8.1 或 WINDOWS RT 裝置上安裝「我的伺服器](#BKMK_InstallCert)」的憑證。）  
  
###  <a name="BKMK_Features"></a>My Server 的功能  
 下表描述 My Server 和 My Server 2012 R2 應用程式的功能。 您在伺服器上的帳戶類型會決定您可以檢視及執行的項目。 所有的使用者都可以使用共用資源、自訂其 **[最近]** 顯示，決定離線存取快取最近使用之檔案的時間長度，和開啟或關閉透過付費連線的下載。 Windows Server Essentials 伺服器上的系統管理員也可以管理警示、裝置和使用者。 標準使用者帳戶具有的檢視警示和連線至裝置的功能更加有限，取決於使用者帳戶的屬性；個別功能的需求會在下表中註明。  
  
### <a name="features-of-the-my-server-and-my-server-2012-r2-apps-for-windows-server-essentials"></a>Windows Server Essentials 的 My Server 和 My Server 2012 R2 應用程式  
  
|功能集|說明|  
|-----------------|-----------------|  
|管理警示|-（僅限系統管理員）解決伺服器上的警示，或忽略不需要採取動作的警示。 開啟或關閉通知 ([權限] 設定、[通知] 選項)<br />-（標準使用者帳戶）查看網路健康狀態警示。<br />     **注意：** 使用者若要在 [我的伺服器] 中查看警示，必須在使用者帳戶的 **[一般**] 設定中選取 [**使用者可以查看網路健康情況警示**] 設定。 如需詳細資訊，請參閱 [Manage user accounts using the Dashboard](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)。|  
|管理裝置|(僅限系統管理員)<br /><br /> -當您連線到 Windows Server Essentials 伺服器時，請在 [**裝置**] 視圖中查看每個已連線電腦的詳細資料。 離線的裝置會呈現灰色。<br />-啟動和停止已連線電腦的備份。<br />-在「我的伺服器」中開啟或關閉通知。 ([權限] 設定、[通知] 選項)<br /><br /> 所有使用者：<br /><br /> -查看您的使用者帳戶可存取的用戶端電腦。 ([裝置] 顯示)<br />-監視這些電腦的警示。 ([警示] 顯示)<br />-（僅限 My Server 2012 R2）使用遠端 Web 存取連線到這些電腦。 ([裝置] 顯示、[遠端連線] 按鈕)|  
|使用遠端桌面連線到電腦。|（僅限我的伺服器 2012 R2）開啟與您的 Windows Server Essentials 伺服器或用戶端電腦的遠端桌面會話。 ([裝置] 顯示、[遠端連線] 按鈕)<br /><br /> **注意：** 若要啟用這項功能，請從您電腦上的適用于 Windows 的應用程式下載並安裝[遠端桌面應用程式](https://apps.microsoft.com/webpdp/app/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)。 標準使用者帳戶可以連接至其有權登入的裝置。 若要讓使用者可登入電腦，請將該電腦加入此使用者帳戶的 [電腦存取] 索引標籤。 如需詳細資訊，請參閱 [Assign user accounts permission to log on to specific network computers](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)。|  
|管理使用者|(僅限系統管理員) 變更使用者帳戶的密碼。 在伺服器上結束使用者的會話。 ([使用者] 設定)|  
|使用共用的檔案|<ul><li>從共用檔案（您可以在伺服器上存取的共用資料夾）、私人共用或 Windows 8.1 裝置、SkyDrive 或網路存放裝置上傳和下載檔案。 建立資料夾。 加入 (上傳)、編輯和刪除伺服器上的檔案。</li><li>在上傳或下載檔案時檢視傳輸狀態。 取消傳輸。 解決檔案衝突。</li><li>順利使用在本機電腦、伺服器、SkyDrive 或網路存放裝置的檔案和資料夾。 檔案清單會顯示您最近在電腦、SkyDrive 或網路儲存裝置中已經使用的資料夾，以及顯示該伺服器的共用資料夾，並且可讓您瀏覽這些位置中任何一處的資料夾。</li><li>搜尋該伺服器的資料夾和檔案；請按一下要下載的檔案，並在預設應用程式中開啟。 在離線模式中，僅能搜尋離線檔案。</li><li>共用圖片、音樂及視訊。 按一下檔案，在 Windows 8 圖片、音樂或影片播放機中開啟檔案。 或者使用 [開啟] 或 [開啟檔案] ，在另一個應用程式中開啟此檔案。 一如往常，可以將您選擇的應用程式設為該媒體類型的預設應用程式。<br /><br />     **注意：** 根據預設，媒體串流功能無法在已安裝 Windows Server Essentials 體驗角色的 Windows Server Essentials 和 Windows Server 2012 R2 中使用。 如需詳細資訊，請參閱[管理數位媒體](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)。<br /><br /> <ul><li>**圖片** - 在 [圖片] **圖片** 檢視中，點選圖片加以開啟。 再次點選該圖片，以返回 My Server 中的縮圖檢視。</li><li>**音樂**-在 [**音樂**] 視圖中，查看在伺服器上共用的專輯或歌曲。 在此音樂播放程式中點選項目加以開啟。</li><li>影片-按一下 [影片]**視圖中**的縮圖，以開啟影片**播放方式。**</li></ul></li></ul>|  
|使用 SharePoint Online 文件庫|（僅限我的伺服器 2012 R2）使用小組 SharePoint Online 文件庫中的檔案。 開啟小組網站。 (SharePoint Online 區段：開啟此小組網站，並向下鑽研至您想要開啟的文件庫或檔案。 當您開啟檔案時，您必須使用與您的網路帳戶相關聯的 Microsoft 線上帳戶登入 Office 365。）<br /><br /> **注意：** 若要使用這項功能，伺服器必須與 Office 365 整合，Office 365 訂閱必須包含 SharePoint Online，而且您在伺服器上的使用者帳戶必須具有與其相關聯的 Microsoft Online Services 帳戶。 如需將 Office 365 和 SharePoint Online 與 Windows Server Essentials 整合的相關資訊，請參閱[Windows Server essentials 的服務整合總覽-第1部分](https://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)和[windows Server Essentials 的服務整合總覽-第2部分](https://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)。|  
|自訂 [最近] 顯示|[最近] 清單可讓您快速存取您現在使用的檔案。 您可以進行下列變更：<br /><br /> -設定要顯示最近的檔案歷程記錄的天數。 ([最近] 設定：[最近使用過的檔案保留天數]；預設 = 7 天)<br />-如果您不再需要使用已使用的檔案，請清除 [**最近**] 顯示。 這並不會影響快取；檔案仍然可離線使用。 （**最近**的設定：**清除**按鈕）<br />     **秘訣：** 如果您不需要離線存取檔案，請使用 [**離線**設定] 中的 [**全部清除**]，從快取中移除檔案。|  
|離線工作|根據預設，您已在過去 7 天期間存取的檔案可離線瀏覽。 每當您連線到伺服器時，就會與伺服器同步處理離線變更。<br /><br /> 您可以對離線設定進行下列變更：<br /><br /> -變更要快取的工作天數。 (在 My Server 中為 [離線] 設定，在 My Server 2012 R2 中為 [檔案] 設定：[快取期間]；預設 = 7 天)<br />-允許透過付費網路進行檔案傳輸，或關閉該功能。 這項功能預設為關閉，以避免透過付費網路傳輸檔案的昂貴費用。 ([離線] 或 [檔案] 設定：[開啟或關閉透過付費網路的檔案自動傳輸]；預設 = [關閉])<br />-您可能偶爾會想要清除快取。 在 [最近] 或 [檔案] 設定檢查快取大小；然後使用 [清除] 或 [全部清除] 來清除此快取。<br /><br /> **秘訣：** 若要找出哪些檔案可離線使用，請在任何共用資料夾中選取**離線**快取篩選，而不是 [**全部**]。<br /><br /> **注意：** 根據預設，快取會儲存**最近**清單所顯示的相同檔案歷程記錄。 請注意，如果您清除此快取，或此快取的設定和 [最近] 顯示不相符，則 [最近] 清單中的某些檔案可能無法離線使用。|  
|背景同步處理|每次當您登入 My Server，以及當您連線到伺服器同時進行變更時，您的檔案就會與伺服器同步處理。 為了保留效能，不會在大多數的資料夾自動執行需要大量資源的背景同步處理。 為了避免昂貴的傳輸費用，不會在 3G 網路上執行背景同步處理。<br /><br /> -解決檔案衝突，顯示在 [**傳輸狀態**] 頁面的 [**衝突**] 區域中。 當執行檔案傳輸時，主畫面會顯示 [傳輸狀態] 和 [警示] 常用鍵。|  
  
##  <a name="BKMK_ConnectServer"></a>如何從局域網路連接到您的伺服器  
 若要在 Windows Server Essentials for Windows Phone、Windows 8 和 Windows 8.1 中順利使用 My Server 應用程式，您必須先在裝置上安裝伺服器憑證。 這會讓裝置連線到您在本機網路中執行 Windows Server Essentials 的伺服器。 使用下列程序連接到您在區域網路中的伺服器，並在 Windows Phone 或正在執行 Windows 8 或 Windows 8.1 的電腦上安裝伺服器憑證。  
  
#### <a name="to-connect-to-your-server-from-your-windows-phone"></a>從 Windows Phone 連線到伺服器  
  
1.  在您執行 Windows 8 或 Windows 8.1 的 Windows Phone，開啟 Internet Explorer。  
  
2.  在網址列中，輸入**HTTP：//< 您伺服器名稱\>/connect/default.aspx？ get = caroot.cer .cer**，然後按 enter。  
  
3.  當系統提示您安裝 caroot.cer 憑證時，請按一下 [安裝]。  
  
4.  請等待憑證安裝完成。  
  
    > [!NOTE]
    >  完成安裝憑證之後，可以使用您的伺服器名稱和網路認證登入 Windows Phone 的 My Server 應用程式。  
  
#### <a name="to-connect-to-your-server-in-your-local-network-from-a-computer-running-windows-8-or-windows-81"></a>從執行 Windows 8 或 Windows 8.1 的電腦連線到區域網路中的伺服器  
  
1.  在執行 Windows 8 或 Windows 8.1 的電腦上，開啟 Internet Explorer。  
  
2.  在網址列中，輸入**HTTP：//< 您伺服器名稱\>/connect/default.aspx？ get = caroot.cer .cer**，然後按 enter。  
  
3.  當系統提示您安裝 caroot.cer 憑證時，按一下 [開啟]。  
  
4.  在 [憑證] 對話方塊中，按一下 [安裝憑證]。  
  
5.  在 [憑證匯入精靈] 中，選取 [本機電腦] 做為存放區位置。  
  
6.  選取 [將所有憑證放入以下的存放區]，然後瀏覽至 [信任的根憑證授權單位] 位置。  
  
7.  遵循指示完成精靈。  
  
    > [!NOTE]
    >  完成安裝憑證之後，可以使用您的伺服器名稱和網路認證登入 Windows 8 或 Windows 8.1 的 My Server 應用程式。  
  
## <a name="see-also"></a>請參閱  
  
-   [Windows Server Essentials 的服務整合總覽-第1部分](https://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Windows Server Essentials 的服務整合總覽-第2部分](https://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [管理隨處存取](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [遠端工作](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [遠端工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

