---
title: 步驟 1-安裝 WSUS 伺服器角色
description: Windows Server Update Service （WSUS）主題-描述如何使用伺服器管理員安裝伺服器角色
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: fabc8619-350e-403b-96f8-116424931300
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22554a9669c30cc827c509824f187fbaaedb1272
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361696"
---
# <a name="step-1-install-the-wsus-server-role"></a>步驟 1：安裝 WSUS 伺服器角色

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

部署 WSUS 伺服器的下一個步驟是安裝 WSUS 伺服器角色。 下列程序描述如何使用伺服器管理員安裝 WSUS 伺服器角色。

> [!IMPORTANT]
> 這個安裝程序只涵蓋如何使用 Windows 內部資料庫 (WID) 安裝 WSUS 的內容。 使用 Microsoft SQL Server 安裝 WSUS 的程序記錄在 [這篇文章](https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx)中。

### <a name="to-install-the-wsus-server-role"></a>安裝 WSUS 伺服器角色

1.  使用屬於本機 Administrators 群組成員的帳戶，登入到計劃安裝 WSUS 伺服器角色的伺服器上。

2.  在**伺服器管理員**中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。

3.  在 [在您開始前] 頁面上，按一下 [下一步]。

4.  在 [**選取安裝類型**] 頁面上，確認已選取 [以**角色為基礎或以功能為基礎的安裝**選項]，然後按 **[下一步]** 。

5.  在 [**選取目的地伺服器**] 頁面上，選擇伺服器所在的位置（從伺服器集區或虛擬硬碟）。 選取位置之後，選擇要安裝 WSUS 伺服器角色的伺服器，然後按 [下一步]。

6.  在 [**選取伺服器角色**] 頁面上，選取 [ **Windows Server Update Services**]。  [新增 Windows Server Update Services 所需的功能] 隨即開啟。 按一下 [新增功能]，然後按 [下一步]。

7.  在 [**選取功能**] 頁面上。 保留預設選項，然後按 **[下一步]** 。

    > [!IMPORTANT]
    > WSUS 只需要預設的網頁伺服器角色設定。 設定 WSUS 時，如果系統提示您進行其他網頁伺服器角色設定，您可以有把握地接受預設值並繼續設定 WSUS。

8.  在 [Windows Server Update Services] 頁面上，按 [下一步]。

9. 在 [選取角色服務] 頁面上，保留預設選取項目，然後按 [下一步]。

    > [!TIP]
    > 您必須選取一個資料庫類型。 如果資料庫選項全部清除 (未選取)，後續安裝工作將會失敗。

10. 在 [內容位置選取] 頁面上，輸入用來儲存更新的有效位置。 例如，您可以特別為此在磁碟機 K 的根目錄上建立名 WSUS_database 的資料夾，然後輸入 **k:\WSUS_database** 做為有效位置。

11. 按一下 **\[下一步\]** 。 [網頁伺服器 (IIS) 角色] 頁面隨即開啟。 檢視資訊，然後按 [下一步]。 在 **[選取要為網頁伺服器（IIS）安裝的角色服務**] 中保留預設值，然後按 **[下一步]** 。

12. 在 [確認安裝選項] 頁面上，檢視選取的選項，然後按一下 [安裝]。 WSUS 安裝精靈隨即執行。 這需要數分鐘的時間才能完成。

13. WSUS 安裝完成後，在 [安裝進度] 頁面上的摘要視窗中，按一下 [啟動後續安裝工作]。 文字會變更，要求您： **伺服器正在設定，請稍候**。 工作完成時，文字會變更為： **已順利完成設定**。 按一下 **關閉**。

14. 在 [伺服器管理員]中，確認是否出現要求您重新啟動的通知。 這可能會依安裝的伺服器角色而有不同。 如果需要重新啟動，請務必重新啟動伺服器以完成安裝。

> [!IMPORTANT]
> 此時，安裝程式已完成，不過若要讓 WSUS 正常運作，您需要繼續進行[步驟2：設定 WSUS](2-configure-wsus.md)。

