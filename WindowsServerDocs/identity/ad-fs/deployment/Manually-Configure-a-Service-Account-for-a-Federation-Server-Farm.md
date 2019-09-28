---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: 手動設定同盟伺服器陣列的服務帳戶
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8240903b3c446d4f02ca93dc053e520480f5e8ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359486"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>手動設定同盟伺服器陣列的服務帳戶

如果您想要在 Active Directory 同盟服務中設定同盟伺服器陣列環境 \(AD FS @ no__t-1，您必須在伺服器陣列的 Active Directory Domain Services \(AD DS @ no__t-3 中建立並設定專屬的服務帳戶會位於。 接著，您必須將該陣列中的每部同盟伺服器設定為使用此帳戶。 當您想要讓公司網路上的用戶端電腦使用 Windows 整合式驗證，向 AD FS 伺服器陣列中的任何同盟伺服器進行驗證時，您必須在組織中完成下列工作。  

> [!IMPORTANT]
> 從 AD FS 3.0 （Windows Server 2012 R2），AD FS 支援使用[群組受管理的服務帳戶](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)\(gMSA @ no__t-2 作為服務帳戶。  這是建議的選項，因為它不需要在一段時間內管理服務帳戶密碼。  本檔涵蓋使用傳統服務帳戶的替代案例，例如在仍執行 Windows Server 2008 R2 或更早版本網域功能等級的網域中，\(DFL @ no__t-1。

> [!NOTE]  
> 針對整個同盟伺服器陣列，您必須只執行此程序中的工作一次。 之後，當您使用 [AD FS 同盟伺服器設定] Wizard 建立同盟伺服器時，必須在伺服器陣列中每部同盟伺服器的 [**服務帳戶**] [Wizard] 頁面上指定這個相同的帳戶。  
  
#### <a name="create-a-dedicated-service-account"></a>建立專用服務帳戶  
  
1.  在位於身分識別提供者組織的 Active Directory 樹系中，建立專用的使用者 @ no__t-0service 帳戶。 Kerberos 驗證通訊協定必須要有此帳戶，才能在伺服器陣列案例中使用，並允許在每部同盟伺服器上通過 @ no__t-0through 驗證。 此帳戶僅適用于同盟伺服器陣列的用途。  
  
2.  編輯使用者帳戶內容，然後選取 [密碼永久有效] 核取方塊。 此動作可確保此服務帳戶的功能不會因為網域密碼變更需求而中斷。  
  
    > [!NOTE]  
    > 針對此專用帳戶使用 Network Service 帳戶將會導致隨機失敗 (當嘗試透過「Windows 整合式驗證」存取時)，因為 Kerberos 票證無法在伺服器之間彼此驗證。  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>設定服務帳戶的 SPN  
  
1.  因為 AD FS AppPool 的應用程式集區識別是以網域使用者 @ no__t-0service 帳戶的身分執行，所以您必須使用 Setspn 命令 @ no__t-3line 工具，在網域中設定該帳戶的服務主體名稱 \(SPN @ no__t-2。 在執行 Windows Server 2008 的電腦上，預設會安裝 Setspn .exe。 在加入與 user @ no__t-0service 帳戶所在相同網域的電腦上執行下列命令：  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    例如，如果所有同盟伺服器都在網域名稱系統下叢集化的情況下 \(DNS @ no__t-1 主機名稱 fs.fabrikam.com，而指派給 AD FS AppPool 的服務帳戶名稱則會命名為 adfs2farm，請如下所示輸入命令，並然後按 ENTER 鍵：  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    只需要為此帳戶完成一次此工作。  
  
2.  在 AD FS AppPool 身分識別變更為服務帳戶之後，請在 SQL Server 資料庫上設定存取控制清單 \(ACLs @ no__t-1，以允許此新帳戶的讀取存取權，讓 AD FS AppPool 可以讀取原則資料。  
  

