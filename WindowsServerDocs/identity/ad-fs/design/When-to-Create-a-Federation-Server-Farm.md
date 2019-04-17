---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: "建立聯盟伺服器陣列的時機"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-farm"></a>建立聯盟伺服器陣列的時機

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

請考慮 \(AD FS\) Active Directory 同盟服務中建立聯盟伺服器陣列，當您有較大的 AD FS 部署，而您想要提供容錯、 load\ 平衡或延展性到您的組織同盟服務。 有兩個或更多聯盟伺服器建立在相同網路的動作，每個使用相同的同盟服務、 設定和 AD FS 管理 snap\ 中新增的每個伺服器的 token\ 簽署的憑證會建立聯盟伺服器陣列。  
  
您可以建立聯盟伺服器陣列或使用 AD FS 聯盟伺服器設定精靈現有發電廠安裝其他聯盟伺服器。 如需詳細資訊，請查看[當建立聯盟伺服器](When-to-Create-a-Federation-Server.md)。  
  
> [!NOTE]  
> 當您選擇的選項來建立**新聯盟伺服器陣列**使用 AD FS 聯盟伺服器設定精靈，精靈將會嘗試建立容器物件 \(for sharing certificates\) Active Directory 中。 因此，這很重要，您第一次登入電腦，您會在位置設定聯盟伺服器角色，以建立此容器物件 Active Directory 中具有權限不足帳號。  
  
聯盟伺服器可以群組發電廠之前，他們必須第一次叢集使完全抵達單一要求限定 \(FQDN\) 傳送到不同聯盟伺服器伺服器的網域名稱。 您可以藉由部署公司網路中的網路負載平衡 \(NLB\) 建立伺服器叢集。 本指南假設 NLB 已正確設定為每個聯盟伺服器叢集。  
  
如需詳細資訊，了解如何設定叢集 FQDN 使用 Microsoft NLB 技術，查看[指定叢集參數](https://go.microsoft.com/fwlink/?LinkID=74651)。  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署聯盟伺服器陣列最佳做法  
我們建議部署 production 環境中的聯盟伺服器下列最佳做法：  
  
-   如果您將會在此同時部署多個聯盟伺服器，或您知道，您將加入更多的伺服器發電廠段時間，請考慮陣列中建立現有聯盟伺服器伺服器影像時，您需要快速建立其他聯盟伺服器安裝從該映像。  
  
    > [!NOTE]  
    > 如果您選擇執行其他聯盟伺服器部署使用伺服器影像方法，您不需要完成事務於[檢查清單： 聯盟伺服器上設定](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)每個您想要新增至陣列伺服器的時間。  
  
-   使用 NLB 或其他形式叢集的許多聯盟伺服器電腦配置單一 IP 位址。  
  
-   保留靜態陣列中每個聯盟伺服器的 IP 位址，根據您的網域名稱系統 \(DNS\) 設定動態主機設定通訊協定 \(DHCP\) 插入每個 IP 位址排除項目。 Microsoft NLB 技術需要的每個參與 NLB 叢集伺服器指派靜態 IP 位址。  
  
-   如果 AD FS 資料庫設定會儲存在 SQL 資料庫，避免在此同時編輯多個聯盟伺服器 SQL 資料庫。  
  
## <a name="configuring-federation-servers-for-a-farm"></a>設定為發電廠聯盟伺服器  
下表描述，因此每個聯盟伺服器可以參與陣列來說環境必須完成的工作。  
  
|工作|描述|  
|--------|---------------|  
|如果您正在使用 SQL Server 儲存 AD FS 資料庫設定|聯盟伺服器陣列包含的兩個或更多聯盟伺服器共用相同 AD FS 設定資料庫和 token\ 簽署的憑證。 在 Windows 內部資料庫或 SQL Server 資料庫中可以儲存設定資料庫。 如果您想要儲存在 SQL 資料庫設定資料庫，請確定該設定資料庫存取使其可以存取所有新聯盟伺服器參與發電廠。 **注意：**發電廠案例中，這很重要，也不會參與為聯盟伺服器在農地的電腦上找到設定資料庫。 Microsoft NLB 不允許任何參與發電廠到彼此的電腦。 **注意：**確保中加入了 eu-u.s 發電廠每個聯盟伺服器上網際網路資訊服務 \(IIS\)\) AD FS AppPool 的身分已設定資料庫讀取權限。|  
|取得並分享憑證|您可以取得單一伺服器驗證憑證的公用憑證授權單位 \ (CA\)，例如 VeriSign。 然後，讓所有聯盟伺服器都共用相同金鑰的私密部分憑證，您可以設定的憑證。 如需有關如何共用相同的憑證的詳細資訊，請查看[檢查清單︰ 設定好聯盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。 **注意：** AD FS 管理 snap\ 中指的是伺服器驗證憑證的同盟服務通訊的憑證以的伺服器。<br /><br />如需詳細資訊，請查看[聯盟伺服器的憑證需求](Certificate-Requirements-for-Federation-Servers.md)。|  
|指向相同 SQL Server 執行個體|如果 AD FS 資料庫設定會儲存在 SQL 資料庫，新的聯盟伺服器必須指向，讓新的伺服器可以參與發電廠是由其他聯盟伺服器相同 SQL Server 執行個體。|  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
