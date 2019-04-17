---
title: 設定 WEB1 散發憑證撤銷 (Crl)
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b96fad769638de9835c52137e5165fa32a6b9bd4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>設定 WEB1 散發憑證撤銷 (Crl)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以設定網頁伺服器 WEB1 散發 Crl 使用此程序。  
  
根 CA 的擴充功能，它聲明，ca 從 CRL 是透過http://pki.corp.contoso.com/pki。 目前有不 PKI virtual directory 上 WEB1，因此您必須建立一個。  
  
若要執行此程序，您必須成員的**網域系統管理員**。  
  
> [!NOTE]  
> 在下列程序，取代 account 使用者名稱、網頁伺服器名稱，資料夾名稱位置及其他值的是適用於您的部署。  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>將憑證 WEB1 和 Crl 設定  
  
1.  在 WEB1，系統管理員的身分，以執行 Windows PowerShell 中，輸入`explorer c:\`，然後按 ENTER 鍵。 Windows 檔案總管開啟 c 磁碟機。   
  
2.  建立新資料夾名 PKI c：磁碟機。 若要這樣做，請按一下**Home**，然後按一下 [**新資料夾**。 暫時反白顯示的名稱會建立新的資料夾。 輸入**pki**，然後按 ENTER 鍵。  
  
3.  在 [Windows 檔案總管]，以滑鼠右鍵按一下您剛建立的資料夾，將滑鼠游標暫留在**與**，，然後按一下 [**特定對象**。 **檔案共用**對話方塊。  
  
4.  在**檔案共用**，輸入**憑證發行者**，然後按一下 [**新增**。 在清單中新增了憑證發行者群組。 在清單中，在**權限等級**，按一下旁邊的箭號**憑證發行者**，然後按一下 [**讀取/寫入**。 按一下**共用**，然後按**完成**。  
  
5.  關閉 Windows 檔案總管]。  
  
6.  打開 IIS 主機。 在伺服器管理員中，按一下 [**工具**，然後按**管理員網際網路服務 (IIS)**。  
  
7.  網際網路服務 (IIS) Manager 主控台樹中，展開**WEB1**。 如果您受邀開始使用 Microsoft 網站平台，請按一下**取消**。  
  
8.  展開**網站**，然後以滑鼠右鍵按一下**網站，預設**，然後按一下 [**新增 Virtual Directory**。  
  
9. 在**別名**，輸入**pki**。 在**實體路徑**輸入**C:\pki**，然後按**[確定]**。  
  
10. 讓匿名存取 pki virtual directory，以便任何 client 可以檢查的 CA 憑證 Crl 有效性。 若要這樣做：  
  
    1.  在**連接**窗格中，確定**pki**選取。  
  
    2.  在**pki Home**按**驗證**。  
  
    3.  在**動作**窗格中，按**編輯權限]**。  
  
    4.  在**安全性**索引標籤上，按**編輯**  
  
    5.  在**的權限 pki**對話方塊中，按**新增**。  
  
    6.  在**選擇使用者、電腦、服務帳號或群組**，輸入**匿名的登入。每個人都**，然後按一下 [**檢查名稱]**。 按一下**[確定]**。  
  
    7.  按一下**[確定]**在**選取 [使用者、電腦、服務帳號或群組**對話方塊。  
  
    8.  按一下**[確定]**在**的權限 pki**對話方塊。  
  
11. 按一下**[確定]**在**屬性 pki**對話方塊。  
  
12. 在**pki Home**窗格中，按兩下 [**要求篩選**。  
  
13. **副檔名**索引標籤上選取預設**要求篩選**窗格。 在**動作**窗格中，按**編輯功能設定**。  
  
14. 在**編輯要求篩選設定**、**允許點逸出**，然後按一下 [ **[確定]**。  
  
15. 在網際網路服務 (IIS) 管理員 MMC 中，按一下 [Web 伺服器名稱。 例如，如果您的網頁伺服器命名 WEB1，按一下**WEB1**。  
  
16. 在**動作**，按一下 [**重新開機**。 網際網路服務會停止，且再重新起始。  
  

