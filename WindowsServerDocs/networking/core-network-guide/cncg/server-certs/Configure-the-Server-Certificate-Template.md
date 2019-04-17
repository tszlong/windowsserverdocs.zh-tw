---
title: 設定伺服器的憑證範本
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e747fedd74dfc55c2c4df457d7f107b618423b8e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-server-certificate-template"></a>設定伺服器的憑證範本

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序設定憑證範本 Active Directory&reg;憑證 Services (AD CS) 使用為基礎的伺服器上您的網路退出伺服器的憑證。  
  
設定此範本，您可以指定的伺服器來應該伺服器的憑證會自動接收 AD CS 的 Active Directory 群組。   
  
下列程序包含設定伺服器下列類型的所有發行憑證範本指示：  
  
- 執行遠端存取服務，包括 RAS 閘道伺服器成員的伺服器] **RAS 及 IAS 伺服器]**群組。  
- 正在執行的網路原則 Server (NPS) 服務，伺服器的成員**RAS 及 IAS 伺服器]**群組。  
  
同時成員資格**企業系統管理員**並根網域的**網域系統管理員」**群組是才能完成此程序最小值。  
  
### <a name="to-configure-the-certificate-template"></a>若要設定憑證範本  
  
1.  CA1，在伺服器管理員中，按一下 [**工具**，然後按**憑證授權單位**。 開啟憑證授權單位 Microsoft Management Console (MMC)。  
  
2.  在 MMC 中，按兩下 [CA 名稱，以滑鼠右鍵按一下**憑證範本**，然後按**管理**。  
  
3.  [憑證範本主控台開啟。 詳細資料窗格中會顯示所有的憑證範本。  
  
4.  在詳細資料窗格中，按一下**RAS 及 IAS 伺服器**範本。  
  
5.  按一下**動作**，然後再按**複製範本**。 範本**屬性**對話方塊。  
  
6.  按一下**安全性**索引標籤。   
  
7.  在**安全性**索引標籤的**群組或使用者名稱**，按一下 [ **RAS 及 IAS 伺服器]**。  
  
8.  在**伺服器 RAS 及 IAS 的權限**的**允許**，確保**Enroll**會已選取，然後選取**註冊**核取方塊。 按一下**[確定]**，然後關閉 [憑證範本 MMC。  
  
9.  在憑證授權單位 MMC 中，按一下 [**憑證範本**。 在**動作**功能表上，指向 [**新**，，然後按一下 [**憑證範本]**。 **讓憑證範本**對話方塊。  
  
10. 在**讓憑證範本**，按一下 [設定] 的憑證範本您剛才的名稱，然後按一下**[確定]**。 例如，如果您未變更預設的憑證範本名稱，按一下**的 RAS 複製及 IAS 伺服器**，然後按一下 [ **[確定]**。  
  


