---
title: 設定伺服器憑證範本
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 238579c945821d19e45dad7e623450d598830596
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886119"
---
# <a name="configure-the-server-certificate-template"></a>設定伺服器憑證範本

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序設定憑證範本的 Active Directory&reg;憑證服務 (AD CS) 用做為基礎向您的網路上的伺服器註冊的伺服器憑證。  
  
在設定此範本時，您可以指定伺服器應該自動接收伺服器憑證從 AD CS 的 Active Directory 群組。   
  
下列程序包含設定將憑證發行至下列伺服器類型的所有範本的指示：  
  
- 執行遠端存取服務，包括 RAS 閘道伺服器，這是成員伺服器**RAS 與 IAS 伺服器**群組。  
- 伺服器正在執行網路原則伺服器 (NPS) 服務，屬於**RAS 與 IAS 伺服器**群組。  
  
中的成員資格**Enterprise Admins**與根網域**Domain Admins**群組是要完成此程序，至少需要。  
  
### <a name="to-configure-the-certificate-template"></a>若要設定憑證範本  
  
1.  在 CA1，在 伺服器管理員 中，按一下 **工具**，然後按一下**憑證授權單位**。 此時會開啟 憑證授權單位 Microsoft Management Console (MMC)。  
  
2.  在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下**憑證範本**，然後按一下**管理**。  
  
3.  憑證範本主控台隨即開啟。 在 [詳細資料] 窗格中，會顯示所有的憑證範本。  
  
4.  在 [詳細資料] 窗格中，按一下**RAS 及 IAS 伺服器**範本。  
  
5.  按一下 **動作**功能表，然後再按一下**複製範本**。 範本**屬性**對話方塊隨即開啟。  
  
6.  按一下 [安全性] 索引標籤。   
  
7.  在 **安全性**索引標籤**群組或使用者名稱**，按一下  **RAS 及 IAS 伺服器**。  
  
8.  在**RAS 與 IAS 伺服器的權限**下方**允許**，確定**註冊**已選取，然後選取**自動註冊**檢查方塊。 按一下 **確定**，並關閉 憑證範本 MMC。  
  
9.  在 [憑證授權單位] MMC 中，按一下**憑證範本**。 在上**動作**功能表上，指向**新增**，然後按一下**要發出的憑證範本**。 **啟用憑證範本**對話方塊隨即開啟。  
  
10. 在 **啟用憑證範本**，按一下您剛才的憑證範本名稱 設定，然後按一下**確定**。 例如，如果您沒有變更預設憑證範本名稱，按一下**複製的 RAS 及 IAS 伺服器**，然後按一下**確定**。  
  


