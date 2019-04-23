---
title: 設定 WEB1 以發佈憑證撤銷清單 (Crl)
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 57fa45eff87a1f0cdaae8b780d7f605e54ff6871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839189"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>設定 WEB1 以發佈憑證撤銷清單 (Crl)

>適用於：Windows Server （半年通道），Windows Server 2016

若要設定 web 伺服器 WEB1 以發佈 Crl，您可以使用此程序。  
  
在 根 CA 的延伸模組，它所述，從根 CA 的 CRL 可以透過 https://pki.corp.contoso.com/pki。 目前，沒有 PKI 虛擬目錄在 WEB1 上，因此必須建立一個。  
  
若要執行此程序，您必須隸屬**Domain Admins**。  
  
> [!NOTE]  
> 在下列程序，取代的使用者帳戶名稱、 Web 伺服器名稱、 資料夾名稱和位置和其他值的適用於您的部署。  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>若要設定 WEB1 以發佈憑證與 Crl  
  
1.  在 WEB1 上，以系統管理員身分執行 Windows PowerShell 中，輸入`explorer c:\`，然後按 ENTER 鍵。 Windows 檔案總管會開啟至 c 磁碟機。   
  
2.  建立新的資料夾 c： 磁碟機上名為 PKI。 若要這樣做，請按一下**首頁**，然後按一下**新資料夾**。 反白顯示出暫存名稱建立新的資料夾。 型別**pki**然後按 ENTER 鍵。  
  
3.  在 Windows 檔案總管中，以滑鼠右鍵按一下您剛才建立的資料夾中，滑鼠游標暫留**分享**，然後按一下**特定人員**。 **檔案共用**對話方塊隨即開啟。  
  
4.  在 **檔案共用**，型別**Cert Publishers**，然後按一下**新增**。 Cert Publishers 群組新增至清單。 在清單中，在**權限層級**，按一下箭號旁**Cert Publishers**，然後按一下 **讀取/寫入**。 按一下 **共用**，然後按一下**完成**。  
  
5.  關閉 Windows 檔案總管。  
  
6.  開啟 IIS 主控台。 在 [伺服器管理員] 按一下 [工具]，然後按一下 [Internet Information Services (IIS) 管理員]。  
  
7.  在 Internet Information Services (IIS) 管理員主控台樹狀目錄中，依序展開**WEB1**。 如果系統邀請您開始使用 Microsoft Web Platform，請按一下 [取消]。  
  
8.  展開 [站台] 後，以滑鼠右鍵按一下 [Default Web Site]，然後按一下 [新增虛擬目錄]。  
  
9. 在 **別名**，型別**pki**。 在 **實體路徑**型別**C:\pki**，然後按一下**確定**。  
  
10. 啟用匿名存取 pki 虛擬目錄，以便任何用戶端檢查有效性的 CA 憑證與 Crl。 方法如下：  
  
    1.  在 [連線] 窗格中，確認已選取 [pki]。  
  
    2.  在 [pki 首頁] 中，按一下 [驗證]。  
  
    3.  在 [動作] 窗格中，按一下 [編輯權限]。  
  
    4.  在 [安全性] 索引標籤上，按一下 [編輯] 。  
  
    5.  在 [pki 的權限]  對話方塊上，按一下 [新增] 。  
  
    6.  在 **選取使用者、 電腦、 服務帳戶或群組**，輸入**匿名登入;每個人都**，然後按一下 **檢查名稱**。 按一下 [確定] 。  
  
    7.  按一下 [ **[確定]** 上**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊。  
  
    8.  按一下 [ **[確定]** 上**pki 的權限**] 對話方塊。  
  
11. 按一下 [ **[確定]** 上**pki 屬性**] 對話方塊。  
  
12. 在 [pki 首頁]  窗格中，按兩下 [要求篩選] 。  
  
13. 在 [要求篩選] 窗格中，預設會選取 [副檔名] 索引標籤。 在 [動作]  窗格中，按一下 [編輯功能設定] 。  
  
14. 在 [編輯要求篩選設定] 中，選取 [允許雙重逸出]  ，然後按一下 [確定] 。  
  
15. 在 Internet Information Services (IIS) 管理員 MMC 中，按一下您的 Web 伺服器名稱。 例如，如果您的 Web 伺服器名為 WEB1，按一下**WEB1**。  
  
16. 在 **動作**，按一下**重新啟動**。 網際網路服務停止，然後重新啟動。  
  

