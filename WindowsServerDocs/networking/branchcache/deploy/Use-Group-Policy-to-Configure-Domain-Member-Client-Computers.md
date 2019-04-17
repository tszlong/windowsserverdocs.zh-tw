---
title: 使用群組原則設定的網域成員 Client 電腦
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 96bf84df14eac8016a898e92eb96f90435c53c98
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>使用群組原則設定的網域成員 Client 電腦

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用下列程序，設定的網域成員 client 電腦分散式快取模式或裝載快取模式，並設定 Windows 防火牆進階安全性允許 BranchCache 資料傳輸與在組織中建立的所有電腦群組原則物件。  
  
本章節包含下列程序。  
  
1.  [建立群組原則物件和設定 BranchCache 模式](#bkmk_gp)  
  
2.  [設定 Windows 防火牆使用進階安全性輸入流量規則](#bkmk_inbound)  
  
3.  [設定 Windows 防火牆使用進階安全性輸出交通規則](#bkmk_outbound)  
  
> [!TIP]  
> 下列程序，要求您的預設網域原則中建立群組原則物件，但是您可以建立物件 （組織單位） 組織單位或其他容器的是適用於您的部署。  
  
您必須成員的**網域系統管理員**，或相當於執行這些程序。  
  
## <a name="bkmk_gp"></a>建立群組原則物件和設定 BranchCache 模式  
  
1.  安裝的電腦上的 Active Directory Domain Services 伺服器角色是，在伺服器管理員中，按一下 [**工具**，然後按**群組原則管理**。 群組原則管理主控台開啟。  
  
2.  在群組原則管理主控台中，展開下列路徑：**樹系：** *example.com*、**網域**、 *example.com*，**群組原則物件**，其中*example.com* BranchCache client 電腦帳號您想要設定的網域名稱。  
  
3.  以滑鼠右鍵按一下**群組原則物件**，然後按一下 [**新增]**。 **新的 GPO**對話方塊。 在**名稱**，輸入名稱的新群組原則物件 (GPO)。 如果您想要的物件 BranchCache Client 電腦的名稱，例如，輸入**BranchCache Client 電腦**。 按一下**[確定]**。  
  
4.  在群組原則管理主控台中，確定**群組原則物件**已選取，然後在詳細資料窗格中您剛建立 GPO 上按一下滑鼠右鍵。 例如，如果您 GPO BranchCache Client 電腦，以滑鼠右鍵按一下**BranchCache Client 電腦**。 按一下**編輯**。 群組原則編輯器] 管理主控台開啟。  
  
5.  在群組原則編輯器] 管理主控台中，展開下列路徑：**電腦設定**，**原則**、**系統管理範本： 原則 （ADMX 擷取定義檔案） 從本機電腦**、**網路**、 **BranchCache**。  
  
6.  按一下**BranchCache**，然後在詳細資料窗格中，按兩下**上 BranchCache 關閉**。 原則設定對話方塊。  
  
7.  在**上 BranchCache 關閉**對話方塊中，按一下 [**啟用**，，然後按一下 [ **[確定]**。  
  
8.  若要讓 BranchCache 分散式快取模式中的詳細資料窗格中，按兩下 [**設定 BranchCache 分散式快取模式**。 原則設定對話方塊。  
  
9. 在**設定 BranchCache 散發快取模式**對話方塊中，按一下**啟用**，然後按一下 [ **[確定]**。  
  
10. 如果您有一或多個分公司位置您要部署 BranchCache 裝載快取模式，以及您已部署裝載快取伺服器這些辦公室中的，按兩下 [**讓自動裝載快取探索服務連接點的**。 原則設定對話方塊。  
  
11. 在**讓自動裝載快取探索服務連接點，**對話方塊中，按**啟用**，，然後按一下 [ **[確定]**。  
  
    > [!NOTE]  
    > 支援兩**設定 BranchCache 分散式快取模式**和**可讓自動裝載快取探索服務連接點，**原則設定，client 電腦操作 BranchCache 分散式快取模式除非它們找到裝載快取伺服器分公司操作裝載快取模式中的時間點。  
  
12. 使用下列程序防火牆電腦上設定 client 使用群組原則。  
  
## <a name="bkmk_inbound"></a>設定 Windows 防火牆使用進階安全性輸入流量規則  
  
1.  在群組原則管理主控台中，展開下列路徑：**樹系：** *example.com*、**網域**、 *example.com*，**群組原則物件**，其中*example.com* BranchCache client 電腦帳號您想要設定的網域名稱。  
  
2.  在群組原則管理主控台中，確定**群組原則物件**已選取，然後在詳細資料窗格中 BranchCache client 電腦您先前建立 GPO 上按一下滑鼠右鍵。 例如，如果您 GPO BranchCache Client 電腦，以滑鼠右鍵按一下**BranchCache Client 電腦**。 按一下**編輯**。 群組原則編輯器] 管理主控台開啟。  
  
3.  在群組原則編輯器] 管理主控台中，展開下列路徑：**電腦設定**，**原則**， **Windows 設定**、**的安全性設定**， **Windows 防火牆使用進階安全性**，**使用進階安全性-LDAP Windows 防火牆**、**輸入規則**。  
  
4.  以滑鼠右鍵按一下**輸入規則**，然後按**新規則**。 [新增輸入規則精靈開啟。  
  
5.  在**規則類型**，按一下 [**預先定義的**，展開清單中選擇，然後按一下 [ **BranchCache-擷取內容 (使用 HTTP)**。 按一下**下一步**。  
  
6.  在**預先定義的規則**，按一下 [**下**。  
  
7.  在**動作**，確保**允許連接**已選取，然後按一下 [**完成]**。  
  
    > [!IMPORTANT]  
    > 您必須選取 [**允許連接**，才能接收此連接埠流量 BranchCache client 的。  
  
8.  若要建立 WS 探索防火牆例外，再以滑鼠右鍵按一下**輸入規則**，然後按一下 [**新規則**。 [新增輸入規則精靈開啟。  
  
9. 在**規則類型**，按一下 [**預先定義的**，展開清單中選擇，然後按一下 [ **BranchCache-等探索 (使用 WSD)**。 按一下**下一步**。  
  
10. 在**預先定義的規則**，按一下 [**下**。  
  
11. 在**動作**，確保**允許連接**已選取，然後按一下 [**完成]**。  
  
    > [!IMPORTANT]  
    > 您必須選取 [**允許連接**，才能接收此連接埠流量 BranchCache client 的。  
  
## <a name="bkmk_outbound"></a>設定 Windows 防火牆使用進階安全性輸出交通規則  
  
1.  在群組原則編輯器] 管理主控台中，以滑鼠右鍵按一下**輸出規則**，然後按**新規則**。 新的輸出規則精靈開啟。  
  
2.  在**規則類型**，按一下 [**預先定義的**，展開清單中選擇，然後按一下 [ **BranchCache-擷取內容 (使用 HTTP)**。 按一下**下一步**。  
  
3.  在**預先定義的規則**，按一下 [**下**。  
  
4.  在**動作**，確保**允許連接**已選取，然後按一下 [**完成]**。  
  
    > [!IMPORTANT]  
    > 您必須選取 [**允許連接**以傳送此連接埠 BranchCache client 的。  
  
5.  若要建立 WS 探索防火牆例外，再以滑鼠右鍵按一下**輸出規則**，然後按一下 [**新規則**。 新的輸出規則精靈開啟。  
  
6.  在**規則類型**，按一下 [**預先定義的**，展開清單中選擇，然後按一下 [ **BranchCache-等探索 (使用 WSD)**。 按一下**下一步**。  
  
7.  在**預先定義的規則**，按一下 [**下**。  
  
8.  在**動作**，確保**允許連接**已選取，然後按一下 [**完成]**。  
  
    > [!IMPORTANT]  
    > 您必須選取 [**允許連接**以傳送此連接埠 BranchCache client 的。  
  


