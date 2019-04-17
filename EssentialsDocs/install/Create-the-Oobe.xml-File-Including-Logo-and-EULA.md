---
title: "建立 Oobe.xml 檔案包括商標和使用者授權合約"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f99a2051e114b3c890f1cdac23aebf58689980
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>建立 Oobe.xml 檔案包括商標和使用者授權合約

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以新增自己的終端使用者授權合約 (EULA) 初始設定，利用 Oobe.xml 檔案。 Oobe.xml 是用來提供文字與影像的初始設定，Windows 歡迎和其他網頁向使用者的檔案。 您可以新增多個要自訂根據終端使用者的語言及國家或地區選擇 content Oobe.xml 檔案。 如需詳細資訊，請查看[Windows 評定及 Windows 8 部署套件適用於](https://go.microsoft.com/fwlink/?LinkId=248694)的文件。  
  
 除了 Microsoft 使用者授權合約，會顯示在您的公司使用者授權合約。 Microsoft 使用者授權合約將會顯示在初始設定一般使用者體驗，期間的第一的使用者授權合約，然後您使用者授權合約將會顯示。 您的使用者授權合約可以隨時隨地放在伺服器上，而且您 Oobe.xml 檔案中指定的位置。  
  
#### <a name="to-add-your-company-eula-and-logo"></a>新增您的公司使用者授權合約和商標  
  
1.  在文字編輯器中，例如記事本開放 Oobe.xml 檔案。  
  
2.  在 < logopath\ >< 日 logopath\ > 標籤，請輸入您商標檔案絕對的路徑。 此檔案應包含 32 位元可移植網路圖形 (.png) 檔案 240 x 100 像素的。  
  
3.  在 < eulafilename\ >< 日 eulafilename\ > 標籤中，輸入絕對路徑使用者授權合約檔案。 使用者授權合約檔案必須是 rtf 格式 (.rtf) 檔案。  
  
4.  在 < name\ >< 日 name\ > 標籤，請輸入您的公司名稱。  
  
     下列範例顯示標記 Oobe.xml 檔案中：  
  
    ```  
  
    <FirstExperience>  
       <oobe>  
          <oem>  
             <name>Fabrikam</name>  
             <logopath>c:\fabrikam\fabrikam.png</logopath>  
             <eulafilename>c:\fabrikam\fabrikam_eula.rtf</eulafilename>  
          </oem>  
       </oobe>  
    </FirstExperience>  
  
    ```  
  
5.  儲存檔案。  
  
6.  將檔案 Oobe.xml 放在下列位置的其中之一：  
  
    |Oobe.xml 位置|條件來判定位置|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\|伺服器出貨單一國家/地區單一語言系統中。|  
    |%windir%\system32\oobe\info\default\\ < language\ >|伺服器出貨單一國家/地區多語言系統中。|  
    |< 國家/地區 > %windir%\system32\oobe\info\\ \ 和 %windir%\system32\oobe\info\\ < 國家/地區 > \\ < language\ > \|伺服器會以多個國家/地區出貨，並設定需要在每個國家/地區為基礎，每一個都有單一語言的自訂項目。 < 國家/地區 > 所在的國家或地區位置部署伺服器之後，而且 < language\ > 小數點的地區設定識別碼 (LCID) 版本的地理位置識別碼 (GeoID) 的小數點版本。|  
  
 如果您有白色文字與其他公司商標時，它可能會顯示因為藍色的背景設定流程中更好。  您也可以設定登錄金鑰和值指定此商標。  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>若要指定公司商標設定 OEM 登錄鍵  
  
1.  在伺服器上，將滑鼠移到畫面的右上角，然後按一下**搜尋**。  
  
2.  在搜尋方塊中，輸入**regedit**，然後按一下 [Regedit 應用程式。  
  
3.  在瀏覽窗格中，瀏覽至**跳**，展開 [**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**。 如果 OEM 金鑰不存在，建立按鍵，如下所示：  
  
    1.  以滑鼠右鍵按一下**Windows Server**，按一下 [**新**，然後按一下 [**鍵**。  
  
    2.  按鍵名稱，輸入**OEM**。  
  
4.  （選擇性）如果您要建立商標的項目，您可以建立不同的鍵來區分商標的語言版本。 例如，如果您擁有的商標英文與德文的版本，您可以建立 en-us-我們金鑰和 de-de 金鑰。 因為所有商標檔案儲存在相同的資料夾，您必須提供商標映像檔執行個體的每一種語言的唯一名稱。 例如，您可以建立稱為 LogoWithWhiteText_en.png 和 LogoWithWhiteText_de.png 檔案。  
  
5.  任一以滑鼠右鍵按一下**OEM**或以滑鼠右鍵按一下適當的語言金鑰，請按 [**新**，然後按一下 [**字串值**。  
  
6.  輸入 LogoWithWhiteText 字串，並再按下 ENTER。  
  
7.  新的字串，以滑鼠右鍵按一下，然後按一下**修改]**。  
  
8.  輸入含有商標映像的路徑，然後按一下 [確定]。  
  
## <a name="see-also"></a>也了  
 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)