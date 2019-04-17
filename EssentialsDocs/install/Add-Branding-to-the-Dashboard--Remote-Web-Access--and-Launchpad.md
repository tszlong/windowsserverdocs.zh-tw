---
title: "新增至儀表板，遠端網路存取權，Launchpad 品牌"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>新增至儀表板，遠端網路存取權，Launchpad 品牌

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_Branding"></a>新增至儀表板，遠端網路存取權，Launchpad 品牌  
 您可以讓許多商標新增登錄中新增項目。 在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM 位於所有商標登錄作業系統的項目。  
  
 所有聯名必須符合下列商標需求：  
  
-   Windows Server Essentials 商標必須具備最小的寬度**170 像素**，將正確的外觀比例，以**96 DPI**。  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>若要新增藉由變更登錄商標  
  
1.  在伺服器上，將滑鼠移到畫面的右上角，然後按一下**搜尋**。  
  
2.  在搜尋方塊中，輸入**regedit**，然後按**Regedit**應用程式。  
  
3.  在瀏覽窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**。 如果**OEM**鍵不存在，請完成下列步驟來建立：  
  
    1.  以滑鼠右鍵按一下**Windows Server**，按一下 [**新**，然後按一下 [**鍵**。  
  
    2.  輸入**OEM**鍵的名稱。  
  
4.  （選擇性）如果您要建立商標的項目，您可以建立不同的鍵來區分商標的語言版本。 例如，如果您擁有的商標英文與德文的版本，您可以建立 en-us-我們金鑰和 de-de 金鑰。 因為所有商標檔案儲存在相同的資料夾，您必須提供商標映像檔執行個體的每一種語言的唯一名稱。 例如，您會建立稱為 DashboardLogo_en.png 和 DashboardLogo_de.png 檔案。  
  
5.  任一以滑鼠右鍵按一下**OEM**或以滑鼠右鍵按一下適當的語言金鑰，請按 [**新**，然後按一下 [**字串值**。  
  

6.  輸入字串的名稱，然後按 ENTER 鍵。 請參考[字串登錄及值](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)字串的姓名和資料值表格。  

6.  輸入字串的名稱，然後按 ENTER 鍵。 請參考[字串登錄及值](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings)字串的姓名和資料值表格。  

  
7.  新的字串，以滑鼠右鍵按一下，然後按一下**修改]**。  
  
8.  值輸入名稱，相關聯的表格，然後按一下**[確定]**。  
  
9. 如果您要建立的商標影像或附加的連結的項目，請將檔案複製到 %programFiles%\Windows Server\Bin\OEM。 如果 OEM directory 不存在，建立它。  
  
10. 如果進行變更之後客戶拍擁有伺服器的影響遠端網路存取，他們必須打開遠端 Web 存取。 建議客戶執行下列動作：  
  
    1.  儀表板，請按一下**設定**，然後按一下 [**隨處存取**索引標籤。  
  
    2.  如果任何地方存取您電腦，按一下 [**設定**，然後清除 [在遠端 Web 存取核取方塊**選擇隨處存取功能，可讓**的任何地方存取精靈設定頁面。  
  
    3.  按一下**設定**。  
  
###  <a name="BKMK_RegStrings"></a>下表列出登錄變更影響商標的位置、字串名稱，以及資料值。  
  
### <a name="registry-strings-and-values"></a>登錄字串及值  
  
|商標位置|描述|名稱|資料值。|  
|--------------------------|-----------------|-----------------|----------------|  
|商標儀表板|儀表板中新增商標映像。 儀表板商標必須.png 格式，且不會超過 350 像素 38 像素高。<br /><br /> **重要事項：**以聯名您商標儀表板，您必須編輯作品磚，提供 OPK dvd 影像公司商標附加時下列適當空白的空間需求。 如需詳細資訊查看所提供的範例磚。|DashboardLogo|商標映像檔的名稱|  
|DashboardClientLogo|新增至儀表板 client 登入畫面的商標映像。|DashboardClientLogo|商標映像檔的名稱|  
|網站背景圖片|變更背景影像，會顯示在網路上的存取登入頁面。 會如下所示出現的一般解析度︰<br /><br /> 為 1024 x 768 像素解析度精確會填滿登入頁面<br /><br /> -800 x 600 像素的解析度置中頁面上，顯示黑色的邊框以<br /><br /> -將置中 1280 x 720 像素解析度並不會超過 1024 x 768 像素|LogonBackground|背景影像檔案的名稱|  
|網站的標題|取代標題遠端 Web 存取網站的 Windows Server Essentials 的以您選擇的標題。|WebsiteName|新的網路存取網站標題|  
|商標網站|變更預設的商標遠端 Web 存取網站上。 預期的大小的是商標的 32 乘 32 像素。 它商標為較小或更長的時間，如果將延伸或減少符合下列尺寸做|WebsiteLogo|商標映像檔的名稱|  
|新增的網站商標|您的合作夥伴商標將會顯示下方遠端 Web 存取網站上顯示的 Microsoft 商標。 商標預期的大小為 200 像素高 50 像素。 如果您的商標更長的時間，它將會成為小成維持原始的外觀比例。 如果您商標小於這將會在 200 50 像素空間置中，大小或外觀比例都不會變更。|OEMLogo|商標映像檔的名稱|  

|在 [網站首頁及登入頁面的連結 |加上遠端 Web 存取網站的首頁與登入頁面的連結。 包含的資訊連結.xml 必須位於 %programFiles%\Windows Server\Bin\OEM。 下列範例顯示.xml 檔案的格式：<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < 連結 Name\ = LogonLinkName ><br /> < Text\ > LogonLinkDescription < 日 Text\ ><br /> < Url\ > LogonLinkURL < 日 Url\ ><br /> < Icon\ > LinkIcon < 日 Icon\ ><br /> < 日 Link\ ><br /> < 日 LogonLinks\ ><br /> < HomepageLinks\ ><br /> < 連結 Name\ = HomepageLinkName ><br /> < Text\ > HomepageLinkDescription < 日 Text\ ><br /> < Url\ > HomepageLinkURL < 日 Url\ ><br /> < 日 Link\ ><br /> < 日 HomepageLinks\ ><br /> < 日 OemLinks\ > |LinksXML |請參考[LinksXML 項目](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)清單中的項目和描述表格。|  

|在 [網站首頁及登入頁面的連結 |加上遠端 Web 存取網站的首頁與登入頁面的連結。 包含的資訊連結.xml 必須位於 %programFiles%\Windows Server\Bin\OEM。 下列範例顯示.xml 檔案的格式：<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < 連結 Name\ = LogonLinkName ><br /> < Text\ > LogonLinkDescription < 日 Text\ ><br /> < Url\ > LogonLinkURL < 日 Url\ ><br /> < Icon\ > LinkIcon < 日 Icon\ ><br /> < 日 Link\ ><br /> < 日 LogonLinks\ ><br /> < HomepageLinks\ ><br /> < 連結 Name\ = HomepageLinkName ><br /> < Text\ > HomepageLinkDescription < 日 Text\ ><br /> < Url\ > HomepageLinkURL < 日 Url\ ><br /> < 日 Link\ ><br /> < 日 HomepageLinks\ ><br /> < 日 OemLinks\ > |LinksXML |請參考[LinksXML 項目](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links)清單中的項目和描述表格。|  

|Launchpad 商標 |新增 Launchpad 商標映像。 在.png 格式，您必須不高於 64 像素必須 Launchpad 商標。|LaunchpadLogo |商標映像檔的名稱 |  
  
###  <a name="BKMK_Links"></a>下表列出，並描述 LinksXML 字串名稱的項目。  
  
### <a name="linksxml-elements"></a>LinksXML 項目  
  
|LinksXML 項目|描述|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|登入連結名稱。|  
|LogonLinkDescription|會顯示為登入頁面的連結文字。|  
|LogonLinkURL|登入頁面連結到解析 url。|  
|LinkIcon|[登入 \] 連結] 圖示檔案的名稱。 此檔案應該在相同資料夾.xml 檔案的位置。 連結] 圖示的影像應該 16 x 16 像素，並且應該是.png 格式。 如果您不提供 LinkIcon，會使用預設連結] 圖示的影像。|  
|**HomepageLinks**|  
|HomepageLinkName|首頁連結名稱。|  
|HomepageLinkDescription|顯示網頁的連結文字。|  
|HomepageLinkURL|解析為連結首頁的 URL。|  
|HomepageLinkIcon|圖示檔案首頁連結的名稱。 此檔案應該在相同資料夾.xml 檔案的位置。 HomepageLinkIcon 影像應該 16 x 16 像素，並且應該是.png 格式。 如果您不提供 HomepageLinkIcon，會使用預設首頁連結] 圖示的影像。|  
  
## <a name="see-also"></a>也了  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

