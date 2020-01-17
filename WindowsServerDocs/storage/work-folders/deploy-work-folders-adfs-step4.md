---
title: 搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾 - 步驟 4 設定 Web 應用程式 Proxy
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 06/24/2017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: f0222226191719fd11e68def8970c0e83529f801
ms.sourcegitcommit: 8771a9f5b37b685e49e2dd03c107a975bf174683
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76145904"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾：步驟 4 設定 Web 應用程式 Proxy

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明使用 Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy 部署工作資料夾的第四個步驟。 您可以在這些主題中找到這個程序的其他步驟︰  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：總覽](deploy-work-folders-adfs-overview.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟1，設定 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟2，AD FS 設定後的工作](deploy-work-folders-adfs-step2.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟3、設定工作資料夾](deploy-work-folders-adfs-step3.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟5，設定用戶端](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   本節涵蓋的指示適用于 Windows Server 2019 或 Windows Server 2016 環境。 如果您使用 Windows Server 2012 R2，請依照 [Windows Server 2012 R2 指示](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

若要設定 Web 應用程式 Proxy 以搭配工作資料夾使用，請使用以下程序。  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>安裝 AD FS 和工作資料夾憑證  
您必須在將安裝 Web 應用程式 Proxy 角色的電腦上，把先前建立的 AD FS 和工作資料夾憑證 (步驟 1 設定 AD FS 和步驟 3 設定工作資料夾) 安裝到本機電腦憑證存放區。  
  
因為您將安裝的是無法在「受信任的根憑證授權單位」憑證存放區中追溯發行者的自我簽署憑證，您還必須將憑證複製到該存放區。  
  
若要安裝憑證，請依照下列步驟執行：  
  
1.  按一下 **\[開始\]** ，然後按一下 **\[執行\]** 。  
  
2.  輸入 **MMC**。  
  
3.  按一下 **\[檔案\]** 功能表上的 **\[新增/移除嵌入式管理單元\]** 。  
  
4.  在 **\[可用的嵌入式管理單元\]** 清單中，選取 **\[憑證\]** ，然後按一下 **\[新增\]** 。 \[憑證嵌入式管理單元精靈\] 就會啟動。  
  
5.  選取 **\[電腦帳戶\]** ，然後按 **\[下一步\]** 。  
  
6.  選取 **\[本機電腦 (執行這個主控台的電腦)\]** ，然後按一下 **\[完成\]** 。  
  
7.  按一下 **\[確定\]** 。  
  
8.  展開資料夾 **Console Root\Certificates\(Local Computer)\Personal\Certificates**。  
  
9. 以滑鼠右鍵按一下 **\[憑證\]** ，按一下 **\[所有工作\]** ，然後按一下 **\[匯入\]** 。  
  
10. 瀏覽至含有 AD FS 憑證的資料夾，然後依照精靈中的指示匯入檔案，並將它放在憑證存放區。  
  
11. 重複執行步驟 9 和 10，這次瀏覽到工作資料夾憑證並將其匯入。  
  
12. 展開資料夾 **Console Root\Certificates\(Local Computer)\Trusted Root Certification Authorities\Certificates**。  
  
13. 以滑鼠右鍵按一下 **\[憑證\]** ，按一下 **\[所有工作\]** ，然後按一下 **\[匯入\]** 。  
  
14. 瀏覽至含有 AD FS 憑證的資料夾，然後依照精靈中的指示匯入檔案，並將它放在「受信任的根憑證授權單位」存放區。  
  
15. 重複執行步驟 13 和 14，這次瀏覽到工作資料夾憑證並將其匯入。  
  
## <a name="install-web-application-proxy"></a>安裝 Web 應用程式 Proxy  
若要安裝 Web 應用程式 Proxy，請依照下列步驟執行︰  
  
1.  在您打算安裝的 Web 應用程式 Proxy 的伺服器上，開啟 **\[伺服器管理員\]** 並啟動 **\[新增角色及功能\]** 精靈。  
  
2.  按一下精靈第一頁和第二頁上的 **\[下一步\]** 。  
  
3.  在 **\[選取伺服器\]** 頁面上，選取您的伺服器，然後按 **\[下一步\]** 。  
  
4.  在 **\[伺服器角色\]** 頁面上，選取 **\[遠端存取\]** 角色，然後按 **\[下一步\]** 。  
  
5.  在 \[功能\] 頁面和 \[遠端存取\] 頁面上，按 **\[下一步\]** 。  
  
6.  在 **\[角色服務\]** 頁面上，選取 **\[Web 應用程式 Proxy\]** ，按一下 **\[新增功能\]** ，然後按 **\[下一步\]** 。

7.  在 [**確認安裝選項**] 頁面上，按一下 [**安裝**]。  
  
## <a name="configure-web-application-proxy"></a>設定 Web 應用程式 Proxy  
若要設定 Web 應用程式 Proxy，請依照下列步驟執行︰  
  
1.  按一下 \[伺服器管理員\] 頂端的警告旗標，然後按一下連結以開啟 \[Web 應用程式的 Proxy 設定精靈\]。  
  
2.  在 \[歡迎使用\] 頁面上，按 **\[下一步\]** 。  
  
3.  在 **\[同盟伺服器\]** 頁面上，輸入同盟服務名稱。 在測驗範例中，這是 **blueadfs.contoso.com**。  
  
4.  輸入同盟伺服器上的本機系統管理員帳戶的認證。 不要輸入網域認證 (例如，contoso\administrator)，而輸入本機認證 (例如系統管理員)。  
  
5.  在 **\[AD FS Proxy 憑證\]** 頁面上，選取之前匯入的 AD FS 憑證。 在測試案例中，這是 **blueadfs.contoso.com**。 按一下 **\[下一步\]** 。  
  
6.  確認頁面會顯示將執行的 Windows PowerShell 命令以設定服務。 按一下 **\[設定\]** 。  
  
## <a name="publish-the-work-folders-web-application"></a>發佈工作資料夾 web 應用程式  
下一個步驟是發佈將可讓用戶端使用工作資料夾的 Web 應用程式。 若要發佈工作資料夾 Web 應用程式，請依照下列步驟執行︰  
  
1. 開啟 **\[伺服器管理員\]** ，並在 **\[工具\]** 功能表中，按一下 **\[遠端存取管理\]** 以開啟 \[遠端存取管理\] 主控台。  
  
2. 在 **\[設定\]** 下，按一下 **\[Web 應用程式 Proxy\]** 。  
  
3. 在 **\[工作\]** 下，按一下 **\[發佈\]** 。 \[發行新應用程式精靈\] 隨即開啟。  
  
4. 在 \[歡迎使用\] 頁面上，按 **\[下一步\]** 。  
  
5. 在 **\[預先驗證\]** 頁面上，選取 **\[Active Directory 同盟服務 (AD FS)\]** ，然後按 **\[下一步\]** 。  
  
6. 在 **\[支援戶端\]** 頁面上，選取 **\[OAuth2\]** ，然後按 **\[下一步\]** 。

7. 在 **\[信賴憑證者\]** 頁面上，選取 **\[工作資料夾\]** ，然後按 **\[下一步\]** 。 本清單是從 AD FS 發佈到 Web 應用程式 Proxy。  
  
8. 在 **\[發行設定\]** 頁面上，輸入下列，然後按 **\[下一步\]** ：  
  
   -   您想要用於 Web 應用程式的名稱  
  
   -   工作資料夾的外部 URL  
  
   -   工作資料夾憑證的名稱  
  
   -   工作資料夾的後端 URL  
  
   根據預設，精靈可讓後端 URL 與外部 URL 一樣。  
  
   如需測驗範例，請使用這些值︰  
  
   名稱︰**WorkFolders**  
  
   外部 URL： **https://workfolders.contoso.com**  
  
   外部憑證︰**您先前安裝的工作資料夾憑證**  
  
   後端伺服器 URL： **https://workfolders.contoso.com**  
  
9. 確認頁面會顯示將執行的 \[Windows PowerShell\] 命令以發佈應用程式。 按一下 **\[發佈\]** 。  
  
10. 在 **\[結果\]** 頁面上，您應該會看見已應用程式成功發佈。
    >[!NOTE]
    > 如果您有多個工作資料夾伺服器，就必須針對每個工作資料夾伺服器發佈工作資料夾 Web 應用程式 (重複步驟 1-10)。  
  
下一步：[搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 5 設定用戶端](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>請參閱  
[工作資料夾總覽](Work-Folders-Overview.md)  
  

