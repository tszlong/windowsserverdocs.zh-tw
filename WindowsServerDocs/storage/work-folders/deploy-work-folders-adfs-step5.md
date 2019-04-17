---
title: "搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾 - 步驟 5 設定用戶端"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: f168292b-0dbc-44b9-965f-d480e5134a0c
ms.openlocfilehash: fa8b2b15ff411a59b28308a329d7ca2341ef0886
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-5-set-up-clients"></a>搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 5 設定用戶端

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明使用 Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy 部署工作資料夾的第五個步驟。 您可以在這些主題中找到這個程序的其他步驟︰  
  
-   [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰概觀](deploy-work-folders-adfs-overview.md)  
  
-   [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 1 設定 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 2 AD FS 後續設定工作](deploy-work-folders-adfs-step2.md)  
  
-   [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 3 設定工作資料夾](deploy-work-folders-adfs-step3.md)  
  
-   [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾 - 步驟 4 設定 Web 應用程式 Proxy](deploy-work-folders-adfs-step4.md)  
  
使用以下程序設定已加入網域和未加入網域的 Windows 用戶端。 您可以使用這些用戶端測試檔案是否在用戶端的工作資料夾間正確同步。  
  
## <a name="set-up-a-domain-joined-client"></a>設定已加入網域的用戶端  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>安裝 AD FS 和工作資料夾憑證  
您必須在已加入網域的用戶端電腦上，將稍早建立的 AD FS 和工作資料夾憑證安裝到本機憑證存放區。  
  
因為您將安裝的是無法在「受信任的根憑證授權單位」憑證存放區中追溯發行者的自我簽署憑證，您還必須將憑證複製到該存放區。  
  
若要安裝憑證，請依照下列步驟執行：  
  
1.  按一下 **\[開始\]**，然後按一下 **\[執行\]**。  
  
2.  輸入 **MMC**。  
  
3.  按一下 **\[檔案\]** 功能表上的 **\[新增/移除嵌入式管理單元\]**。  
  
4.  在 **\[可用的嵌入式管理單元\]** 清單中，選取 **\[憑證\]**，然後按一下 **\[新增\]**。 \[憑證嵌入式管理單元精靈\] 隨即啟動。  
  
5.  選取 **\[電腦帳戶\]**，然後按 **\[下一步\]**。  
  
6.  選取 **\[本機電腦 (執行這個主控台的電腦)\]**，然後按一下 **\[完成\]**。  
  
7.  按一下 **\[確定\]**。  
  
8.  展開資料夾 Console Root\Certificates\(Local Computer)\Personal\Certificates。  
  
9. 以滑鼠右鍵按一下 **\[憑證\]**，按一下 **\[所有工作\]**，然後按一下 **\[匯入\]**。  
  
10. 瀏覽至含有 AD FS 憑證的資料夾，然後依照精靈中的指示匯入檔案，並將它放在憑證存放區。  
  
11. 重複執行步驟 9 和 10，這次瀏覽到工作資料夾憑證並將其匯入。  
  
12. 展開資料夾 Console Root\Certificates\(Local Computer)\Trusted Root Certification Authorities\Certificates。  
  
13. 以滑鼠右鍵按一下 **\[憑證\]**，按一下 **\[所有工作\]**，然後按一下 **\[匯入\]**。  
  
14. 瀏覽至含有 AD FS 憑證的資料夾，然後依照精靈中的指示匯入檔案，並將它放在「受信任的根憑證授權單位」存放區。  
  
15. 重複執行步驟 13 和 14，這次瀏覽到工作資料夾憑證並將其匯入。  
  
### <a name="configure-work-folders-on-the-client"></a>在用戶端上設定工作資料夾  
若要在用戶端電腦上設定工作資料夾，請依照下列步驟執行︰  
  
1.  在用戶端電腦上，開啟 **\[控制台\]**，然後按一下 **\[工作資料夾\]**。  
  
2.  按一下 **\[設定工作資料夾\]**。  
  
3.  在 **\[輸入您的公司電子郵件地址\]** 頁面上，輸入使用者的電子郵件地址 (例如，user@contoso.com) 或工作資料夾 URL (測試範例中為 https://workfolders.contoso.com)，然後按 **\[下一步\]**。  
  
4.  如果使用者連接至企業網路，驗證是由 Windows 整合式驗證執行。 如果使用者未連接企業網路，驗證會由 ADFS (OAuth) 執行並且將提示使用者輸入認證。 輸入您的認證，然後按一下 **\[確定\]**。  
  
5.  在您驗證後，會顯示 **\[導入「工作資料夾」\]**頁面，您可以選擇是否變更工作資料夾的目錄位置。 按 **\[下一步\]**。  
  
6.  **\[安全性原則\]** 頁面會列出您為工作資料夾設定的安全性原則。 按 **\[下一步\]**。  
  
7.  顯示訊息，指出工作資料夾已開始與電腦同步。 按一下 **\[關閉\]**。  
  
8.  **\[管理「工作資料夾」\]** 頁面會顯示伺服器上的可用空間量、同步狀態等。 如有需要，您可以在此重新輸入您的憑證。 關閉視窗。  
  
9. 您的工作資料夾會自動開啟。 您可以將內容新增到此資料夾以便與您的裝置同步。  
  
    基於測試範例的目的，可新增一個測試檔到此「工作資料夾」。 您在未加入網域的電腦上設定「工作資料夾」之後，將無法在每部電腦上的「工作資料夾」之間同步檔案。  
  
## <a name="set-up-a-non-domain-joined-client"></a>設定未加入網域的用戶端  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>安裝 AD FS 和工作資料夾憑證  
使用您在加入網域的電腦上安裝 AD FS 和「工作資料夾」憑證的相同程序，在未加入網域的電腦上進行安裝。  
  
### <a name="update-the-hosts-file"></a>更新主機檔案  
未加入網域用戶端上的主機檔案必須為測試環境進行更新，因為沒有建立「工作資料夾」的公用 DNS 記錄。 新增這些項目到主機檔案︰  
  
-  workfolders.domain  
  
-  AD FS service name.domain  
  
-  enterpriseregistration.domain  
  
如需測驗範例，請使用這些值︰  
  
-  **10.0.0.10 workfolders.contoso.com**  
  
-  **10.0.0.10 blueadfs.contoso.com**  
  
-  **10.0.0.10 enterpriseregistration.contoso.com**  
  
### <a name="configure-work-folders-on-the-client"></a>在用戶端上設定工作資料夾  
使用您在已加入網域的電腦上設定的相同程序，在未加入網域的電腦上設定工作資料夾。  
  
當新的「工作資料夾」資料夾在此用戶端上開啟時，您可以看到已加入網域的電腦中的檔案已經與未加入網域的電腦同步。 您可以開始新增內容到資料夾以便在裝置之間同步。  
  
這包含透過 Windows Server UI 部署工作資料夾、AD FS 和 Web 應用程式 Proxy 的程序。  
  
## <a name="see-also"></a>另請參閱  
[工作資料夾概觀](Work-Folders-Overview.md)  
  

