---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: 手動設定同盟伺服器陣列的服務帳戶
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5d0495d43eecd2508a6535411ef4e226db0ebacf
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964790"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>手動設定同盟伺服器陣列的服務帳戶

如果您想要在 Active Directory 同盟服務 AD FS 中設定同盟伺服器陣列 \( 環境 \) ，您必須在 \( 伺服器陣列所在的 Active Directory Domain Services AD DS 中，建立並設定專屬的服務帳戶 \) 。 接著，您必須將該陣列中的每部同盟伺服器設定為使用此帳戶。 當您想要讓公司網路上的用戶端電腦使用 Windows 整合式驗證，向 AD FS 伺服器陣列中的任何同盟伺服器進行驗證時，您必須在組織中完成下列工作。  

> [!IMPORTANT]
> 從 AD FS 3.0 （Windows Server 2012 R2），AD FS 支援使用[群組受管理的服務帳戶](../../../security/group-managed-service-accounts/group-managed-service-accounts-overview.md) \( gMSA \) 做為服務帳戶。  這是建議的選項，因為它不需要在一段時間內管理服務帳戶密碼。  本檔涵蓋使用傳統服務帳戶的替代案例，例如，在仍執行 Windows Server 2008 R2 或更早網域功能等級 DFL 的網域 \( 中 \) 。

> [!NOTE]  
> 針對整個同盟伺服器陣列，您必須只執行此程序中的工作一次。 稍後，當使用 AD FS 同盟伺服器組態精靈建立同盟伺服器時，您必須在伺服器陣列中每一部同盟伺服器的 [服務帳戶]**** 精靈頁面上指定這個相同帳戶。  
  
#### <a name="create-a-dedicated-service-account"></a>建立專用服務帳戶  
  
1.  \/在位於身分識別提供者組織的 Active Directory 樹系中，建立專用的使用者服務帳戶。 Kerberos 驗證通訊協定必須要有此帳戶，才能在伺服器陣列案例中使用，並允許在每部 \- 同盟伺服器上通過驗證。 此帳戶僅適用于同盟伺服器陣列的用途。  
  
2.  編輯使用者帳戶內容，然後選取 [密碼永久有效]**** 核取方塊。 此動作可確保此服務帳戶的功能不會因為網域密碼變更需求而中斷。  
  
    > [!NOTE]  
    > 針對此專用帳戶使用 Network Service 帳戶將會導致隨機失敗 (當嘗試透過「Windows 整合式驗證」存取時)，因為 Kerberos 票證無法在伺服器之間彼此驗證。  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>設定服務帳戶的 SPN  
  
1.  因為 AD FS AppPool 的應用程式集區識別是以網域使用者服務帳戶的身分執行 \/ ，所以您必須 \( \) 在網域中使用 Setspn.exe 命令列工具來設定該帳戶的服務主體名稱 SPN \- 。 Setspn.exe 預設會安裝在執行 Windows Server 2008 的電腦上。 在加入使用者服務帳戶所在之相同網域的電腦上執行下列命令 \/ ：  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    例如，在所有同盟伺服器都在網域名稱系統 DNS 主機名稱 fs.fabrikam.com 下叢集化的案例中， \( \) 以及指派給 AD FS AppPool 的服務帳戶名稱命名為 adfs2farm 後，請如下所示輸入命令，然後按 enter 鍵：  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    只需要為此帳戶完成一次此工作。  
  
2.  在 AD FS AppPool 身分識別變更為服務帳戶之後，請設定存取控制清單 \( \) SQL Server 資料庫上的 acl，以允許此新帳戶的讀取存取權，讓 AD FS AppPool 可以讀取原則資料。  
  
