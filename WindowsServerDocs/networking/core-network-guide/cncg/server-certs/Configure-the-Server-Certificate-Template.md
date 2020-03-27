---
title: 設定伺服器憑證範本
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc4ba05466c2d87e6d6e9f7c0d5cee1bb405c79a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318395"
---
# <a name="configure-the-server-certificate-template"></a>設定伺服器憑證範本

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此程式設定 Active Directory&reg; 憑證服務（AD CS）的憑證範本，做為向網路上的伺服器註冊之伺服器憑證的基礎。  
  
設定此範本時，您可以 Active Directory 群組來指定要從 AD CS 自動接收伺服器憑證的伺服器。   
  
下列套裝程式含設定範本以頒發證書給下列所有伺服器類型的指示：  
  
- 執行遠端存取服務的伺服器，包括 RAS 閘道伺服器，其為**ras 和 資訊存取伺服器**群組的成員。  
- 執行網路原則伺服器（NPS）服務的伺服器，其為**RAS 和 資訊存取伺服器**群組的成員。  
  
若要完成此程式，至少需要**Enterprise Admins**和根域的**domain admins**群組的成員資格。  
  
### <a name="to-configure-the-certificate-template"></a>設定憑證範本  
  
1.  在 CA1 的伺服器管理員中，按一下 [**工具**]，然後按一下 [**憑證授權單位**單位]。 [憑證授權單位單位] Microsoft Management Console （MMC）隨即開啟。  
  
2.  在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下 [**憑證範本**]，然後按一下 [**管理**]。  
  
3.  [憑證範本] 主控台隨即開啟。 所有的憑證範本都會顯示在詳細資料窗格中。  
  
4.  在詳細資料窗格中，按一下 [ **RAS 和 資訊存取伺服器**] 範本。  
  
5.  按一下 [**動作**] 功能表，然後按一下 [**複製範本**]。 [範本**屬性**] 對話方塊隨即開啟。  
  
6.  按一下 **[安全性]** 索引標籤。   
  
7.  在 [**安全性**] 索引標籤的 [**群組或使用者名稱**] 中，按一下 [ **RAS 和 資訊存取伺服器**]。  
  
8.  在 [ **RAS 和 資訊存取伺服器的許可權**] 的 [**允許**] 下，確定已選取 [**註冊**]，然後選取 [**自動註冊**] 核取方塊。 按一下 **[確定]** ，然後關閉 [憑證範本] MMC。  
  
9.  在 [憑證授權單位單位] MMC 中，按一下 [**憑證範本**]。 在 [**動作**] 功能表上，指向 [**新增**]，然後按一下 [**要發出的憑證範本**]。 [**啟用憑證範本**] 對話方塊隨即開啟。  
  
10. 在 [**啟用憑證範本**] 中，按一下您剛才設定的憑證範本名稱，然後按一下 **[確定]** 。 例如，如果您沒有變更預設憑證範本名稱，請按一下 [ **RAS 和 資訊存取伺服器的複本**]，然後按一下 **[確定]** 。  
  


