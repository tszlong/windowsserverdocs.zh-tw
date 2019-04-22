---
title: 將商標新增至儀表板、遠端 Web 存取和啟動列
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823499"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>將商標新增至儀表板、遠端 Web 存取和啟動列

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a> 將商標新增至儀表板、 遠端 Web 存取和啟動列  
 您可以透過新增登錄項目來新增許多商標。 作業系統登錄中的所有商標項目皆位於 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM 之下。  
  
 所有品牌聯盟都必須符合以下標誌要求：  
  
-   Windows Server Essentials 標誌必須具有的最小寬度**170 像素**，正確的外觀比例，跟**96 DPI**。  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>若要變更登錄來新增商標  
  
1.  在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]**。  
  
2.  在 [搜尋] 方塊中，輸入 **regedit**，然後按一下 [Regedit]  應用程式。  
  
3.  在瀏覽窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft** 及 **Windows Server**。 如果 **OEM** 機碼不存在，請完成下列步驟來建立機碼：  
  
    1.  以滑鼠右鍵按一下 **[Windows Server]**，按一下 **[新增]**，然後按一下 **[機碼]**。  
  
    2.  輸入 **OEM** 作為機碼名稱。  
  
4.  (選用) 如果您要為標誌建立項目，可以建立不同的機碼以區分不同語言版本的標誌。 例如，如果您同時有英文版和德文版的標誌，可以建立 en-us 機碼和 de-de 機碼。 因為所有的標誌檔都會儲存在同一個資料夾，所以您必須提供標誌影像檔的多個執行個體，每個語言的檔案具備唯一的名稱。 例如，您可以建立名為 DashboardLogo_en.png 和 DashboardLogo_de.png 的檔案。  
  
5.  以滑鼠右鍵按一下 **OEM** ，或以滑鼠右鍵按一下適當的語言機碼，按一下 **[新增]**，然後按一下 **[字串值]**。  
  

6.  輸入字串的名稱，然後按 ENTER 鍵。 請參考[登錄字串和值](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)表格，取得字串名稱與資料值。  

6.  輸入字串的名稱，然後按 ENTER 鍵。 請參考[登錄字串和值](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)表格，取得字串名稱與資料值。  

  
7.  在滑鼠右鍵上按一下新字串，然後按一下 **[修改]**。  
  
8.  根據該表格輸入與字串名稱相關聯的值，然後按一下 **[確定]**。  
  
9. 如果您要為標誌影像或附加連結建立項目，請將檔案複製到 %programFiles%\Windows Server\Bin\OEM。 如果 OEM 目錄不存在，請建立該目錄。  
  
10. 如果所做的變更會影響遠端 Web 存取，則當客戶接手伺服器後，客戶必須在伺服器上開啟遠端 Web 存取。 建議客戶執行下列操作：  
  
    1.  在儀表板上，按一下 **[設定]**，然後按一下 **[隨處存取]** 索引標籤。  
  
    2.  當開啟「隨處存取」時，按一下 **[設定]**，然後在「設定隨處存取」精靈的 **[選擇要啟用的隨處存取功能]** 頁面中清除 [遠端 Web 存取] 核取方塊 。  
  
    3.  按一下**設定**。  
  
###  <a name="BKMK_RegStrings"></a> 下表列出在登錄變更會影響商標的位置、 字串名稱，以及資料值。  
  
### <a name="registry-strings-and-values"></a>登錄字串和值  
  
|商標的位置|描述|名稱字串|資料值|  
|--------------------------|-----------------|-----------------|----------------|  
|儀表板標誌|將標誌影像新增到儀表板。 儀表板標誌必須是 .png 格式，而且寬高比不得大於 350 像素 x 38 像素。<br /><br /> **重要事項：** 若要在儀表板上同時加上您的標誌，則必須編輯 OPK DVD 上提供的圖案，並將您公司的標誌附加至影像上，同時必須遵守適當的空間留白需求。 如需詳細資訊，請參閱提供的圖案範例。|DashboardLogo|標誌影像檔的名稱|  
|DashboardClientLogo|將標誌影像新增到儀表板的用戶端登入畫面。|DashboardClientLogo|標誌影像檔的名稱|  
|網站背景圖片|變更遠端 Web 存取登入頁面上顯示的背景影像。 一般的解析度會如下列方式顯示：<br /><br /> -1024 x 768 像素的解析度會精確地填滿登入頁面<br /><br /> -800 x 600 像素的解析度會置於頁面，並且有黑色框線<br /><br /> ---1280 x 720 像素的解析度會置中並不會超過 1024 x 768 的像素|LogonBackground|背景影像檔的名稱|  
|網站標題|標題取代遠端 Web 存取站台從 Windows Server Essentials 以您選擇的標題。|WebsiteName|新的遠端 Web 存取網站標題|  
|網站標誌|變更遠端 Web 存取網站上的預設標誌。 預期的標誌大小是 32 x 32 像素。 如果您的標誌小於或大於這個大小，標誌將會延展或壓縮成符合這個大小|WebsiteLogo|標誌影像檔的名稱|  
|附加的網站標誌|您的合作夥伴標誌將會顯示在遠端 Web 存取網站上 Microsoft 標誌的正下方。 預期的標誌大小寬高比是 200 像素 x 50 像素。 如果您的標誌大於這個大小，標誌將會依照原始外觀比例縮小成符合這個大小。 如果您的標誌小於這個大小，標誌將會置於這個 200 x 50 像素空間的中央，且不會變更大小或外觀比例。|OEMLogo|標誌影像檔的名稱|  

|在網站首頁與登入頁面上的連結 |將連結附加至遠端 Web 存取網站首頁與登入頁面。 包含連結資訊的 .xml 必須位於 %programFiles%\Windows Server\Bin\OEM。 下列範例顯示 .xml 檔案的格式：<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> < 名稱連結\=LogonLinkName ><br /> <Text\>LogonLinkDescription</Text\><br /> <Url\>LogonLinkURL</Url\><br /> < 圖示\>LinkIcon < / 圖示\><br /> < / link>\><br /> < / LogonLinks\><br /> <HomepageLinks\><br /> < 名稱連結\=HomepageLinkName ><br /> <Text\>HomepageLinkDescription</Text\><br /> <Url\>HomepageLinkURL</Url\><br /> < / link>\><br /> </HomepageLinks\><br /> < / OemLinks\>|LinksXML |請參閱[LinksXML 元素](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)資料表項目和描述的清單。 |  

|在網站首頁與登入頁面上的連結 |將連結附加至遠端 Web 存取網站首頁與登入頁面。 包含連結資訊的 .xml 必須位於 %programFiles%\Windows Server\Bin\OEM。 下列範例顯示 .xml 檔案的格式：<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> < 名稱連結\=LogonLinkName ><br /> <Text\>LogonLinkDescription</Text\><br /> <Url\>LogonLinkURL</Url\><br /> < 圖示\>LinkIcon < / 圖示\><br /> < / link>\><br /> < / LogonLinks\><br /> <HomepageLinks\><br /> < 名稱連結\=HomepageLinkName ><br /> <Text\>HomepageLinkDescription</Text\><br /> <Url\>HomepageLinkURL</Url\><br /> < / link>\><br /> </HomepageLinks\><br /> < / OemLinks\>|LinksXML |請參閱[LinksXML 元素](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)資料表項目和描述的清單。 |  

|啟動列標誌 |將標誌影像新增至啟動列。 啟動列標誌必須是.png 格式，您必須不能高於 64 像素。 |LaunchpadLogo |標誌影像檔的名稱 |  
  
###  <a name="BKMK_Links"></a> 下表列出並描述了 LinksXML 字串名稱元素。  
  
### <a name="linksxml-elements"></a>LinksXML 元素  
  
|LinksXML 元素|描述|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|登入連結名稱。|  
|LogonLinkDescription|顯示為登入頁面連結的文字。|  
|LogonLinkURL|解析為登入頁面連結的 URL。|  
|LinkIcon|用於登入連結的圖示檔名稱。 這個檔案應與 .xml 檔案放在相同的資料夾位置中。 連結圖示影像應該是 16 x 16 像素，而且應該是 .png 格式。 如果未提供 LinkIcon，就會使用預設的連結圖示影像。|  
|**HomepageLinks**|  
|HomepageLinkName|首頁連結名稱。|  
|HomepageLinkDescription|會顯示為首頁連結的文字。|  
|HomepageLinkURL|解析為首頁連結的 URL。|  
|HomepageLinkIcon|用於首頁連結的圖示檔名稱。 這個檔案應與 .xml 檔案放在相同的資料夾位置中。 HomepageLinkIcon 影像應該是 16 x 16 像素，而且應該是 .png 格式。 如果未提供 HomepageLinkIcon，就會使用預設的首頁連結圖示影像。|  
  
## <a name="see-also"></a>另請參閱  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

