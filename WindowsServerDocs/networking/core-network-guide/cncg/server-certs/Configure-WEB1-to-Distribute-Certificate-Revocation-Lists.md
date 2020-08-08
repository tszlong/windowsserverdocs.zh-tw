---
title: '設定 WEB1 以發佈憑證撤銷清單 (Crl) '
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 29c9cfb0a229200e1a62f0187e0bf277dbb99718
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969645"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>設定 WEB1 以發佈憑證撤銷清單 (Crl) 

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式來設定 web 伺服器 WEB1 來散發 Crl。

在根 CA 的延伸模組中，已聲明可以透過使用根 CA 的 CRL https://pki.corp.contoso.com/pki 。 目前，WEB1 上沒有 PKI 虛擬目錄，因此必須建立一個。

若要執行此程式，您必須是**Domain Admins**的成員。

> [!NOTE]
> 在下列程式中，以適用于您部署的使用者帳戶名稱、Web 服務器名稱、資料夾名稱和位置，以及其他值來取代。

#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>設定 WEB1 以散發憑證和 Crl

1.  在 WEB1 上，以系統管理員身分執行 Windows PowerShell，輸入 `explorer c:\` ，然後按 enter。 Windows Explorer 會開啟到 C 磁片磁碟機。

2.  在 C：磁片磁碟機上建立名為 PKI 的新資料夾。 若要這麼做，請按一下 [**首頁**]，然後按一下 [**新增資料夾**]。 隨即會建立新的資料夾，並反白顯示暫時名稱。 輸入**pki** ，然後按 enter。

3.  在 Windows Explorer 中，以滑鼠右鍵按一下您剛才建立的資料夾，將滑鼠游標停留在 [**共用**] 上方，然後按一下 [**特定人員**]。 [檔案**共用**] 對話方塊隨即開啟。

4.  在 [檔案**共用**] 中，輸入**Cert 發行者**，然後按一下 [**新增**]。 [憑證發行者] 群組會新增至清單。 在清單的 [**許可權等級**] 中，按一下 [ **Cert 發行者**] 旁的箭號，然後按一下 [**讀取/寫入**]。 按一下 [**共用**]，然後按一下 [**完成**]。

5.  關閉 Windows 檔案總管。

6.  開啟 IIS 主控台。 在 [伺服器管理員] 按一下 [工具]****，然後按一下 [Internet Information Services (IIS) 管理員]****。

7.  在 [Internet Information Services (IIS) 管理員] 主控台樹中，展開 [ **WEB1**]。 如果系統邀請您開始使用 Microsoft Web Platform，請按一下 [取消]****。

8.  展開 [站台]**** 後，以滑鼠右鍵按一下 [Default Web Site]****，然後按一下 [新增虛擬目錄]****。

9. 在 [**別名**] 中，輸入**pki**。 在 [**實體路徑**] 中輸入**C:\pki**，然後按一下 **[確定]**。

10. 啟用 pki 虛擬目錄的匿名存取，讓任何用戶端都可以檢查 CA 憑證和 Crl 的有效性。 若要這樣做：

    1.  在 [連線]**** 窗格中，確認已選取 [pki]****。

    2.  在 [pki 首頁]**** 中，按一下 [驗證]****。

    3.  在 [動作]**** 窗格中，按一下 [編輯權限]****。

    4.  在 [安全性]**** 索引標籤上，按一下 [編輯] ****。

    5.  在 [pki 的權限]**** 對話方塊上，按一下 [新增]****。

    6.  在 [**選取使用者、電腦、服務帳戶或群組**] 中，輸入**ANONYMOUS LOGON;[所有人**]，然後按一下 [**檢查名稱**]。 按一下 [確定]  。

    7.  按一下 [**選取使用者、電腦、服務帳戶或群組**] 對話方塊上的 **[確定]** 。

    8.  按一下 [pki 的**許可權**] 對話方塊上的 **[確定**]。

11. 按一下 [ **Pki 屬性**] 對話方塊上的 **[確定]** 。

12. 在 [pki 首頁]**** 窗格中，按兩下 [要求篩選]****。

13. 在 [要求篩選]**** 窗格中，預設會選取 [副檔名]**** 索引標籤。 在 [動作]**** 窗格中，按一下 [編輯功能設定]****。

14. 在 [編輯要求篩選設定]**** 中，選取 [允許雙重逸出]****，然後按一下 [確定]****。

15. 在 [Internet Information Services (IIS) 管理員 MMC] 中，按一下您的 Web 服務器名稱。 例如，如果您的 Web 服務器名為 WEB1，請按一下 [ **WEB1**]。

16. 在 [**動作**] 中，按一下 [**重新開機**]。 網際網路服務會停止，然後重新開機。


