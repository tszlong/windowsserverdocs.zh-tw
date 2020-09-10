---
title: 建立包含標誌和 EULA 的 Oobe.xml 檔案
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: d6c1a721107d96f8a2a5de89f95c97ab87bf740a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623762"
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>建立包含標誌和 EULA 的 Oobe.xml 檔案

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以使用 Oobe.xml 檔案，將自己的使用者授權合約 (EULA) 新增至「初始設定」。 Oobe.xml 是一個檔案，針對要呈現給使用者的「初始設定」、Windows 歡迎畫面和其他頁面，提供文字和影像。 您可以新增多個 Oobe.xml 檔案，以便根據使用者的語言和國家或地區選項自訂內容。 如需詳細資訊，請參閱 [Windows 8 的 Windows 評定及部署套件](https://go.microsoft.com/fwlink/?LinkId=248694) 文件。

 除了 Microsoft EULA 以外，也會顯示您公司的 EULA。 在使用者的初始設定經驗中，Microsoft EULA 會是第一個顯示的 EULA，接著會顯示您的 EULA。 您的 EULA 可放置於伺服器上的任意位置，而這是透過在 Oobe.xml 檔案中指定位置。

#### <a name="to-add-your-company-eula-and-logo"></a>新增您公司的 EULA 和標誌

1. 在文字編輯器 (例如「記事本」) 中開啟 Oobe.xml 檔案。

2. 在 <l h \></logopath \> 標記內，輸入您標誌檔案的絕對路徑。 此檔案應包含 32 位元可攜式網路圖形 (.png) 檔案 (大小為 240x 100 像素)。

3. 在 <eulafilename \></eulafilename \> 標記內，輸入 EULA 檔案的絕對路徑。 EULA 檔案必須是 RTF 格式 (.rtf) 的檔案。

4. 在 <名稱 \></name \> 標記內，輸入您的公司名稱。

    下列範例顯示 Oobe.xml 檔案中的標記：

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

5. 儲存檔案。

6. 將 Oobe.xml 檔案放在下列其中一個位置：

   |Oobe.xml 位置|用於決定位置的條件|
   |-----------------------|----------------------------------------|
   |%windir%\system32\oobe\info \| 伺服器是在單一國家/地區和單一語言系統中運送。|
   |%windir%\system32\oobe\info\default \\<語言\>|伺服器會以單一國家/地區和多語言系統出貨。|
   |%windir%\system32\oobe\info \\<國家/地區> \ 和%windir%\system32\oobe\info \\<國家/地區>\\<語言， \> \| 而伺服器會傳送給多個國家/地區，而設定則需要以每個國家/地區為基礎的自訂，每個都有單一語言。 其中 <國家/地區> 是要部署伺服器所在國家或地區之地理位置識別碼 (GeoID) 的十進位版本，而 <language \> 是地區設定識別碼 (LCID) 的十進位版本。|

   如果您另有白色文字的公司標誌，則此標誌可以在設定流程中的藍色背景顯示得更好。  您可以透過選擇設定登錄機碼和值來指定此標誌。

#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>若要透過設定 OEM 登錄機碼來指定公司標誌

1.  在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]**。

2.  在 [搜尋] 方塊中，輸入 **regedit**，然後按一下 [Regedit] 應用程式。

3.  在導覽窗格中，瀏覽至 **HKEY_LOCAL_MACHINE**，展開 **SOFTWARE**，展開 **Microsoft**，展開 **Windows Server**。 如果沒有 OEM 機碼，則按照下面方式建立：

    1.  以滑鼠右鍵按一下 **[Windows Server]**，按一下 **[新增]**，然後按一下 **[機碼]**。

    2.  機碼名稱請輸入 **OEM**。

4.  (選用) 如果您要為標誌建立項目，可以建立不同的機碼以區分不同語言版本的標誌。 例如，如果您同時有英文版和德文版的標誌，可以建立 en-us 機碼和 de-de 機碼。 因為所有的標誌檔都會儲存在同一個資料夾，所以您必須提供標誌影像檔的多個執行個體，每個語言的檔案具備唯一的名稱。 例如，您可以建立名為 LogoWithWhiteText_en.png 和 LogoWithWhiteText_de.png 的檔案。

5.  以滑鼠右鍵按一下 **OEM**，或以滑鼠右鍵按一下適當的語言機碼，按一下 **[新增]**，然後按一下 **[字串值]**。

6.  請輸入「LogoWithWhiteText」作為字串，再按 ENTER。

7.  在滑鼠右鍵上按一下新字串，然後按一下 **[修改]**。

8.  輸入含有標誌影像的路徑，然後按一下 [確定]。

## <a name="see-also"></a>另請參閱
 [使用 Windows Server ESSENTIALS ADK 消費者入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)[建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)