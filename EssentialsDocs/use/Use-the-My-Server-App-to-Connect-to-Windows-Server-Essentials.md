---
title: 使用 My Server 應用程式來連線到 Windows Server Essentials
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e40b57f-6917-43ef-92e0-030baa9d2b99
9author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c7c5114bbd3aa479e2b6afe807fee2ec8d3d5bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435881"
---
# <a name="use-the-my-server-app-to-connect-to-windows-server-essentials"></a>使用 My Server 應用程式來連線到 Windows Server Essentials

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

適用於 Windows Server Essentials 的 My Server 應用程式可讓您連接到資源，以及在 Windows Server Essentials 伺服器從您的膝上型電腦、 個人電腦或 Surface 裝置上執行簡單的系統管理工作。 在 [我的伺服器] 中，您可以管理使用者、裝置和警示，以及使用伺服器上的共用檔案。 離線時，您可以繼續使用最近在 [我的伺服器] 中存取的檔案，下次連線時，您的離線變更會與伺服器自動同步處理。  
  
 如果您的伺服器執行 Windows Server Essentials 的作業系統，請使用原始的 My Server 應用程式。 如果您的伺服器執行 Windows Server Essentials 的作業系統，您必須使用更新的 My Server 2012 R2 應用程式。 這兩個應用程式皆可從 [Windows 適用的應用程式](https://windows.microsoft.com/windows-8/apps)下載。  
  
> [!TIP]
> 
>  若要從 Windows Phone 存取 Windows Server Essentials 中的資源，使用 My Server phone 應用程式 （適用於 Windows Server Essentials) 或 My Server 2012 R2 phone 應用程式 （適用於 Windows Server Essentials)。 如需 My Server phone 應用程式的詳細資訊，請參閱[適用於 Windows Phone 使用 My Server 應用程式](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)。 如需 My Server 2012 R2 Phone 應用程式的相關資訊，請參閱部落格文章： [My Server 2012 R2 Windows 和 Windows Phone 應用程式](http://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx)。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [何謂 My Server 2012 R2 的新功能](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_WhatsNew)  
  
-   [作業系統需求](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_OS)  
  
-   [安裝 My Server](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Install)  
  
-   [使用 My Server](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_UseServer)  
  
-   [My Server 的功能](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Features)  
  
-   [如何從您的區域網路連線到您的伺服器](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_ConnectServer)  

>  若要從 Windows Phone 存取 Windows Server Essentials 中的資源，使用 My Server phone 應用程式 （適用於 Windows Server Essentials) 或 My Server 2012 R2 phone 應用程式 （適用於 Windows Server Essentials)。 如需 My Server phone 應用程式的詳細資訊，請參閱[適用於 Windows Phone 使用 My Server 應用程式](../use/Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)。 如需 My Server 2012 R2 Phone 應用程式的相關資訊，請參閱部落格文章： [My Server 2012 R2 Windows 和 Windows Phone 應用程式](http://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx)。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [何謂 My Server 2012 R2 的新功能](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_WhatsNew)  
  
-   [作業系統需求](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_OS)  
  
-   [安裝 My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Install)  
  
-   [使用 My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_UseServer)  
  
-   [My Server 的功能](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Features)  
  
-   [如何從您的區域網路連線到您的伺服器](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_ConnectServer)  

  
##  <a name="BKMK_WhatsNew"></a> 何謂 My Server 2012 R2 的新功能  
 除了 My Server 中可用的功能，My Server 2012 R2 會提供下列新功能與 Windows Server Essentials 的客戶：  
  
-   **存取 SharePoint Online 小組網站和文件庫**-使用 Microsoft Office 365 整合，與 Windows Server Essentials 以存取 SharePoint Online 小組網站，並開啟您在 My Server 2012 R2 中的 SharePoint Online 程式庫中的文件。  
  
-   **在伺服器和用戶端電腦的遠端桌面連線**-使用**Remote Connect**在 My Server 2012 R2，若要使用遠端桌面連接到您的 Windows Server Essentials 的伺服器和用戶端電腦。  
  
##  <a name="BKMK_OS"></a> 作業系統需求  
 若要在您的膝上型電腦、 個人電腦或 Surface 裝置上使用 My Server 或 My Server 2012 R2，電腦必須具有下列作業系統的其中一個：Windows 8.1、 Windows RT 8.1、 8 或 Windows RT  
  
##  <a name="BKMK_Install"></a> 安裝 My Server  
 您可以從 [Windows 適用的應用程式](https://windows.microsoft.com/windows-8/apps) 市集下載合適的 My Server 應用程式。 如果您的電腦有 Windows 8 或 Windows RT 作業系統，而且您想要使用的伺服器名稱連接到您的內部網路上的伺服器，您也需要從伺服器中安裝憑證。  
  
#### <a name="to-install-my-server-or-my-server-2012-r2-on-your-windows-based-laptop-pc-or-surface-device"></a>在您的 Windows 膝上型電腦、個人電腦或 Surface 裝置上安裝 My Server 或 My Server 2012 R2  
  
1.  從 [Windows 適用的應用程式](https://windows.microsoft.com/windows-8/apps) 安裝合適的 My Server 應用程式到您的 Windows 膝上型電腦、個人電腦或 Surface 裝置。  
  
    |伺服器作業系統：|下載|  
    |-----------------------------|--------------|  
    | Windows Server Essentials|[My Server](https://apps.microsoft.com/webpdp/app/19d94f81-db21-4234-8b49-806694dbbaea)|  
    | Windows Server Essentials|[My Server 2012 R2](https://apps.microsoft.com/webpdp/app/67e86695-bda3-4f32-96c4-2e20e56f1cf3)|  
  
2.  如果您想要能夠使用 透過內部網路，連線到您的 Windows Server Essentials 伺服器 伺服器名稱，而您的裝置已在 Windows 8 或 Windows RT 作業系統，在電腦上安裝預設的 CAROOT.cer 憑證執行下列 procedure。 Windows 8.1 和 Windows RT 8.1 中不需要手動安裝憑證。  
  
###  <a name="BKMK_InstallCert"></a> 若要在 Windows 8、 Windows 8.1 或 Windows RT 裝置上安裝 My Server 憑證  
  
1.  請開啟您電腦上的網頁瀏覽器，並從該伺服器下載預設憑證 CAROOT.cer。 若要這樣做，請輸入下列命令，將伺服器的名稱 (例如 **marketing.contoso.com**) 以 <*伺服器名稱*> 取代：  
  
     **http://<servername\>/connect/default.aspx?get=caroot.cer**  
  
2.  安裝憑證：  
  
    1.  在 [開始]  頁面中，開啟 [檔案總管]  。  
  
    2.  請確定會顯示隱藏的項目和檔案名稱副檔名：在 [檢視]  索引標籤的 **[顯示/隱藏]** 群組，選取 [隱藏的項目]  和 [副檔名]  核取方塊。  
  
    3.  巡覽至包含您剛才下載之 CAROOT.cer 檔案的資料夾。  
  
    4.  以滑鼠右鍵按一下 CAROOT.cer 檔案，然後按一下 [開啟]  。  
  
    5.  在 [憑證]  對話方塊中按一下 [安裝憑證]  。  
  
    6.  選取 [本機電腦]  做為安裝位置，然後按 [下一步]  。  
  
    7.  在 **[憑證存放區精靈]** 頁面上，選取 [將所有憑證放入以下的存放區]  ，並使用 [瀏覽]  來選擇 [受信任的根憑證授權單位]  存放區。 然後按一下 [完成]  。  
  
##  <a name="BKMK_UseServer"></a> 使用 My Server  
 若要開始使用 My Server 或 My Server 2012 R2 應用程式，請開啟應用程式和其功能的快速導覽。  
  
#### <a name="to-open-my-server-or-my-server-2012-r2"></a>開啟 My Server 或 My Server 2012 R2  
  
1.  開啟 My Server （適用於 Windows Server Essentials) 或 My Server 2012 R2 （適用於 Windows Server Essentials)，從**啟動**Windows 裝置的頁面。  
  
2.  Windows Server Essentials 伺服器的伺服器上使用您的帳戶登入。 若要識別伺服器：  
  
    -   在關閉內部部署環境中，使用伺服器的網域名稱-例如， **marketing.contoso.com**。  
  
    -   如果您要透過內部網路連接內部部署，使用伺服器名稱，在上述範例中，伺服器名稱是**行銷**。 （如果您用來連接內部，Windows 8 或 Windows RT 的電腦，而且您想要使用這個方法，您必須安裝 CAROOT.cer 憑證從伺服器。 如需詳細資訊，請參閱 <<c0> [ 若要在 Windows 8、 Windows 8.1 或 Windows RT 裝置上安裝 My Server 憑證](#BKMK_InstallCert)。)  
  
###  <a name="BKMK_Features"></a> My Server 的功能  
 下表描述 My Server 和 My Server 2012 R2 應用程式的功能。 您在伺服器上的帳戶類型會決定您可以檢視及執行的項目。 所有的使用者都可以使用共用資源、自訂其 **[最近]** 顯示，決定離線存取快取最近使用之檔案的時間長度，和開啟或關閉透過付費連線的下載。 Windows Server Essentials 伺服器上的系統管理員也可以管理警示、 裝置和使用者。 標準使用者帳戶具有的檢視警示和連線至裝置的功能更加有限，取決於使用者帳戶的屬性；個別功能的需求會在下表中註明。  
  
### <a name="features-of-the-my-server-and-my-server-2012-r2-apps-for-windows-server-essentials"></a>Windows Server Essentials 的 My Server 和 My Server 2012 R2 應用程式  
  
|功能集|描述|  
|-----------------|-----------------|  
|管理警示|-（僅限系統管理員） 解決警示的伺服器上，或略過不須採取行動的警示。 開啟或關閉通知 ([權限]  設定、[通知]  選項)<br />-（標準使用者帳戶） 檢視網路健康狀態警示。<br />     **注意：** 若要檢視警示，在 My Server 中，使用者**使用者可以檢視網路健康狀態警示**必須在 選取設定**一般**的使用者帳戶的設定。 如需詳細資訊，請參閱 [Manage user accounts using the Dashboard](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)。|  
|管理裝置|(僅限系統管理員)<br /><br /> -當您連接到 Windows Server Essentials 伺服器時，檢視每個連線的電腦中的詳細**裝置**檢視。 離線的裝置會呈現灰色。<br />-啟動和停止已連線電腦的備份。<br />-在 我的伺服器上開啟 開啟或關閉通知。 ([權限]  設定、[通知]  選項)<br /><br /> 所有使用者：<br /><br /> -檢視您的使用者帳戶具有存取權的用戶端電腦。 ([裝置]  顯示)<br />-監視這些電腦的警示。 ([警示]  顯示)<br />-(My Server 2012 R2 中只有） 連線到這些電腦使用遠端 Web 存取。 ([裝置]  顯示、[遠端連線]  按鈕)|  
|使用遠端桌面連線到電腦。|(我僅 Server 2012 R2)使用 Windows Server Essentials 伺服器或用戶端電腦開啟遠端桌面工作階段。 ([裝置]  顯示、[遠端連線]  按鈕)<br /><br /> **注意：** 若要啟用這項功能，請從您電腦上的 [Windows 適用的應用程式] 下載並安裝[遠端桌面應用程式](https://apps.microsoft.com/webpdp/app/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)。 標準使用者帳戶可以連接至其有權登入的裝置。 若要讓使用者可登入電腦，請將該電腦加入此使用者帳戶的 [電腦存取]  索引標籤。 如需詳細資訊，請參閱 [ 指派使用者帳戶權限給登入特定的網路電腦](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)。|  
|管理使用者|(僅限系統管理員) 變更使用者帳戶的密碼。 結束使用者工作階段，在伺服器上。 ([使用者]  設定)|  
|使用共用的檔案|<ul><li>上傳和下載檔案的共用檔案 （共用資料夾的伺服器有存取權），私用的共用，或從 Windows 8.1 的裝置、 SkyDrive 或網路儲存體。 建立資料夾。 加入 (上傳)、編輯和刪除伺服器上的檔案。</li><li>在上傳或下載檔案時檢視傳輸狀態。 取消傳輸。 解決檔案衝突。</li><li>順利使用在本機電腦、伺服器、SkyDrive 或網路存放裝置的檔案和資料夾。 檔案清單會顯示您最近在電腦、SkyDrive 或網路儲存裝置中已經使用的資料夾，以及顯示該伺服器的共用資料夾，並且可讓您瀏覽這些位置中任何一處的資料夾。</li><li>搜尋該伺服器的資料夾和檔案；請按一下要下載的檔案，並在預設應用程式中開啟。 在離線模式中，僅能搜尋離線檔案。</li><li>共用圖片、音樂及視訊。 按一下要在 Windows 8 的圖片、 音樂或視訊播放程式中開啟的檔案。 或者使用 [開啟]  或 [開啟檔案]  ，在另一個應用程式中開啟此檔案。 一如往常，可以將您選擇的應用程式設為該媒體類型的預設應用程式。<br /><br />     **注意：** 根據預設，媒體串流處理功能是在 Windows Server Essentials 和 Windows Server 2012 R2 中無法使用已安裝的 Windows Server Essentials 體驗角色。 如需詳細資訊，請參閱 < [Manage Digital Media](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)。<br /><br /> <ul><li>**圖片** - 在 [圖片]  檢視中，點選圖片加以開啟。 再次點選該圖片，以返回 My Server 中的縮圖檢視。</li><li>**音樂**-在**音樂**檢視中，檢視的專輯或歌曲在伺服器上共用。 在此音樂播放程式中點選項目加以開啟。</li><li>**影片**-按一下縮圖中的**影片**檢視，以開啟此視訊播放程式。</li></ul></li></ul>|  
|使用 SharePoint Online 文件庫|(我僅 Server 2012 R2)使用您的小組 SharePoint Online 文件庫中的檔案。 開啟小組網站。 (SharePoint Online 區段：開啟此小組網站，並向下鑽研至您想要開啟的文件庫或檔案。 當您開啟檔案時，您必須登入 Office 365 與您的網路帳戶相關聯的 Microsoft 線上帳戶。）<br /><br /> **注意：** 若要使用這項功能，必須與 Office 365 整合伺服器、 Office 365 訂用帳戶必須包含 SharePoint Online 和您在伺服器上的使用者帳戶必須具有與其相關聯的 Microsoft Online Services 帳戶。 如需整合 Windows Server Essentials 中的 Office 365 和 SharePoint Online 的資訊，請參閱[服務整合概觀 Windows Server Essentials-第 1 部分](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)和[服務整合概觀Windows Server Essentials-第 2 部分](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)。|  
|自訂 [最近]  顯示|[最近]  清單可讓您快速存取您現在使用的檔案。 您可以進行下列變更：<br /><br /> -設定要顯示最近使用的檔案歷程記錄的天數。 ([最近]  設定：[最近使用過的檔案保留天數]  ；預設 = 7 天)<br />-清除**最近**顯示，如果您不再需要處理您已使用的檔案。 這並不會影響快取；檔案仍然可離線使用。 ([最近]  設定：[清除]  按鈕)<br />     **變更複寫用快取資料夾路徑**如果您不需要離線存取的檔案，使用**全部清除**中**Offline**設定從快取中移除的檔案。|  
|離線工作|根據預設，您已在過去 7 天期間存取的檔案可離線瀏覽。 每當您連線到伺服器時，就會與伺服器同步處理離線變更。<br /><br /> 您可以對離線設定進行下列變更：<br /><br /> -變更至快取的工作天數。 (**Offline**在 My Server 中，設定**檔案**My Server 2012 R2 中的設定：[快取期間]  ；預設 = 7 天)<br />-透過付費網路，允許的檔案傳輸，或關閉該功能。 這項功能預設為關閉，以避免透過付費網路傳輸檔案的昂貴費用。 ([離線]  或 [檔案]  設定：[開啟或關閉透過付費網路的檔案自動傳輸]  ；預設 = [關閉]  )<br />-您偶爾可能會想要清除快取。 在 [最近]  或 [檔案]  設定檢查快取大小；然後使用 [清除]  或 [全部清除]  來清除此快取。<br /><br /> **變更複寫用快取資料夾路徑**若要找出哪些檔案可離線使用，請在任何共用資料夾中，選取 [已離線快取]  篩選器，而非 [全部]  。<br /><br /> **注意：** 根據預設，此快取會儲存與 [最近]  清單顯示相同的檔案記錄。 請注意，如果您清除此快取，或此快取的設定和 [最近]  顯示不相符，則 [最近]  清單中的某些檔案可能無法離線使用。|  
|背景同步處理|每次當您登入 My Server，以及當您連線到伺服器同時進行變更時，您的檔案就會與伺服器同步處理。 為了保留效能，不會在大多數的資料夾自動執行需要大量資源的背景同步處理。 為了避免昂貴的傳輸費用，不會在 3G 網路上執行背景同步處理。<br /><br /> -解決檔案衝突，顯示在**衝突**區域**傳輸狀態**頁面。 當執行檔案傳輸時，主畫面會顯示 [傳輸狀態]  和 [警示]  常用鍵。|  
  
##  <a name="BKMK_ConnectServer"></a> 如何從您的區域網路連線到您的伺服器  
 若要在 Windows Server Essentials for Windows Phone、Windows 8 和 Windows 8.1 中順利使用 My Server 應用程式，您必須先在裝置上安裝伺服器憑證。 這會讓裝置連線到您在本機網路中執行 Windows Server Essentials 的伺服器。 使用下列程序連接到您在區域網路中的伺服器，並在 Windows Phone 或正在執行 Windows 8 或 Windows 8.1 的電腦上安裝伺服器憑證。  
  
#### <a name="to-connect-to-your-server-from-your-windows-phone"></a>從 Windows Phone 連線到伺服器  
  
1.  在您執行 Windows 8 或 Windows 8.1 的 Windows Phone，開啟 Internet Explorer。  
  
2.  在 [網址] 列中，輸入**http://<yourservername\>/connect/default.aspx?get=caroot.cer**，然後按 Enter 鍵。  
  
3.  當系統提示您安裝 caroot.cer 憑證時，請按一下 [安裝]  。  
  
4.  請等待憑證安裝完成。  
  
    > [!NOTE]
    >  完成安裝憑證之後，可以使用您的伺服器名稱和網路認證登入 Windows Phone 的 My Server 應用程式。  
  
#### <a name="to-connect-to-your-server-in-your-local-network-from-a-computer-running-windows-8-or-windows-81"></a>從執行 Windows 8 或 Windows 8.1 的電腦連線到區域網路中的伺服器  
  
1.  在執行 Windows 8 或 Windows 8.1 的電腦上，開啟 Internet Explorer。  
  
2.  在 [網址] 列中，輸入**http://<yourservername\>/connect/default.aspx?get=caroot.cer**，然後按 Enter 鍵。  
  
3.  當系統提示您安裝 caroot.cer 憑證時，按一下 [開啟]  。  
  
4.  在 [憑證]  對話方塊中，按一下 [安裝憑證]  。  
  
5.  在 [憑證匯入精靈] 中，選取 [本機電腦]  做為存放區位置。  
  
6.  選取 [將所有憑證放入以下的存放區]  ，然後瀏覽至 [信任的根憑證授權單位]  位置。  
  
7.  遵循指示完成精靈。  
  
    > [!NOTE]
    >  完成安裝憑證之後，可以使用您的伺服器名稱和網路認證登入 Windows 8 或 Windows 8.1 的 My Server 應用程式。  
  
## <a name="see-also"></a>另請參閱  
  
-   [Windows Server Essentials-第 1 部分的服務整合概觀](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Windows Server Essentials-第 2 部分的服務整合概觀](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [管理隨處存取](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [在遠端工作](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [在遠端工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

