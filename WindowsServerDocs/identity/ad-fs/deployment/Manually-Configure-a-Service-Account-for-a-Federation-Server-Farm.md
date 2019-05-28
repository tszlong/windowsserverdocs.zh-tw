---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: 手動設定同盟伺服器陣列的服務帳戶
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b027bff4645203c44e228f11c651b767fa4502e0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192064"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>手動設定同盟伺服器陣列的服務帳戶

如果您想要在 Active Directory Federation Services 中設定同盟伺服器陣列環境\(AD FS\)，您必須建立並設定專用的服務帳戶在 Active Directory 網域服務中\(AD DS\)伺服器陣列所在的位置。 接著，您必須將該陣列中的每部同盟伺服器設定為使用此帳戶。 當您想要讓用戶端電腦驗證同盟伺服器使用 Windows 整合式驗證 AD FS 伺服器陣列中的任何公司網路上，您必須在組織中完成下列工作。  

> [!IMPORTANT]
> 從 AD FS 3.0 (Windows Server 2012 R2) 中，AD FS 支援使用[群組受控服務帳戶](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\)做為服務帳戶。  因為它不需要管理的服務帳戶密碼經過一段時間，這是建議的選項。  本文件說明如何使用傳統的服務帳戶，例如，仍在執行 Windows Server 2008 R2 或舊版的網域功能等級的網域中的替代案例\(網域功能等級\)。

> [!NOTE]  
> 針對整個同盟伺服器陣列，您必須只執行此程序中的工作一次。 稍後，當您使用 AD FS 同盟伺服器設定精靈建立同盟伺服器，您必須指定這個相同的帳戶**服務帳戶**伺服陣列中每部同盟伺服器上的精靈頁面。  
  
#### <a name="create-a-dedicated-service-account"></a>建立專用服務帳戶  
  
1.  建立專用的使用者\/服務身分識別提供者組織中的 Active Directory 樹系中的帳戶。 此帳戶，才能在伺服陣列案例中運作，並允許通過的 Kerberos 驗證通訊協定\-透過每個同盟伺服器上的驗證。 此帳戶僅適用於同盟伺服器陣列的用途。  
  
2.  編輯使用者帳戶內容，然後選取 [密碼永久有效]  核取方塊。 此動作可確保此服務帳戶的功能不會因為網域密碼變更需求而中斷。  
  
    > [!NOTE]  
    > 針對此專用帳戶使用 Network Service 帳戶將會導致隨機失敗 (當嘗試透過「Windows 整合式驗證」存取時)，因為 Kerberos 票證無法在伺服器之間彼此驗證。  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>設定服務帳戶的 SPN  
  
1.  因為 AD FS 應用程式集區的應用程式集區識別以網域使用者身分執行\/服務帳戶，您必須將服務主體名稱\(SPN\)使用 Setspn.exe 命令網域中的該帳戶\-線條 工具。 執行 Windows Server 2008 的電腦上預設會安裝 Setspn.exe。 已加入相同網域的電腦上執行下列命令，使用者\/service 帳戶位於：  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    比方說，在案例中的所有同盟伺服器已叢集化網域名稱系統下\(DNS\)主機名稱 fs.fabrikam.com 解析並指派給 AD FS 應用程式集區的服務帳戶名稱 adfs2farm 中，輸入命令為遵循，，然後按 ENTER 鍵：  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    只需要為此帳戶完成一次此工作。  
  
2.  AD FS 應用程式集區身分識別變更為服務帳戶之後，設定存取控制清單\(Acl\)上的 SQL Server 資料庫，以允許對此新帳戶的讀取權限，使 AD FS AppPool 可以讀取原則資料。  
  

