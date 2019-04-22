---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: 建立同盟伺服器陣列的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816229"
---
# <a name="when-to-create-a-federation-server-farm"></a>建立同盟伺服器陣列的時機

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

請考慮在 Active Directory Federation Services 中建立同盟伺服器陣列\(AD FS\)當您有較大的 AD FS 部署，且您想要提供容錯、 負載\-平衡或延展性，讓您組織 Federation Service。 在相同的網路中建立兩個或多個同盟伺服器、 設定每個使用相同的 Federation Service，以及加入每一部伺服器的公開金鑰的語彙基元 act\-簽章憑證至 AD FS 管理嵌入式管理單元\-中建立同盟伺服器陣列。  
  
您可以建立同盟伺服器陣列，或使用 AD FS 同盟伺服器設定精靈，安裝至現有的伺服陣列的其他同盟伺服器。 如需詳細資訊，請參閱＜ [When to Create a Federation Server](When-to-Create-a-Federation-Server.md)＞。  
  
> [!NOTE]  
> 當您選擇的選項來建立**新的同盟伺服器陣列**使用 AD FS 同盟伺服器設定精靈，精靈會嘗試建立容器物件\(適用於共用憑證\)在 Active Directory。 因此，第一次登入您要在其中設定同盟伺服器角色的電腦時，請務必使用在 Active Directory 中具有足夠權限的帳戶，以便建立這個容器物件。  
  
同盟伺服器可以分組為伺服器陣列之前，他們必須先叢集，如此送達單一完整的要求格式的網域名稱\(FQDN\)會路由傳送至伺服器陣列中的各種同盟伺服器。 您可以建立伺服器叢集部署的網路負載平衡\(NLB\)在公司網路內。 本指南假設 NLB 已適當地設定為叢集每個同盟伺服器陣列中。  
  
如需有關如何設定叢集 FQDN 使用 Microsoft NLB 技術，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?LinkID=74651)。  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署同盟伺服器陣列的最佳作法  
我們建議您部署在生產環境中的同盟伺服器的下列最佳作法：  
  
-   如果您要部署多部同盟伺服器在相同的時間，或您知道，您將新增更多的伺服器到伺服陣列一段時間，請考慮建立伺服器陣列中的現有的同盟伺服器的伺服器映像，並再安裝從該映像，當您需要為 create 其他同盟伺服器快速。  
  
    > [!NOTE]  
    > 如果您決定使用伺服器映像方法來部署其他同盟伺服器，您就不必完成[檢查清單：Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)每當您想要將新的伺服器新增至伺服器陣列。  
  
-   使用 NLB 或其他形式的叢集配置許多同盟伺服器電腦的單一 IP 位址。  
  
-   保留每個同盟伺服器陣列中，並根據您的網域名稱系統的靜態 IP 位址\(DNS\)組態，排除每個 IP 位址在動態主機設定通訊協定中的插入\(DHCP\). Microsoft NLB 技術要求參與 NLB 叢集的每部伺服器都要有指派的靜態 IP 位址。  
  
-   如果 AD FS 設定資料庫會儲存在 SQL 資料庫，避免同時編輯多個同盟伺服器的 SQL 資料庫。  
  
## <a name="configuring-federation-servers-for-a-farm"></a>設定伺服器陣列的同盟伺服器  
下表描述，讓每一部同盟伺服器可以參與陣列環境必須完成的工作。  
  
|工作|描述|  
|--------|---------------|  
|如果您使用 SQL Server 來儲存 AD FS 設定資料庫|同盟伺服器陣列是由共用相同的 AD FS 設定資料庫和語彙基元的兩個或多個同盟伺服器所組成\-簽署憑證。 設定資料庫可以存放在 Windows 內部資料庫或 SQL Server 資料庫中。 如果您打算在 SQL database 中儲存的設定資料庫，請確定組態資料庫已可供存取，使它可以存取所有參與伺服器陣列的新同盟伺服器。 **注意：** 伺服器陣列的情況下，很重要的組態資料庫位於未加入為該伺服器陣列中的同盟伺服器的電腦上。 Microsoft NLB 不允許參與伺服器陣列中的任何電腦互相通訊。 **注意：** 請確認 Internet Information Services 中的 AD FS 應用程式集區的身分識別\(IIS\) \)上每個同盟參與伺服器陣列中的伺服器具有組態資料庫的 「 讀取 」 權限。|  
|取得及共用憑證|您可以取得單一伺服器驗證憑證從公開憑證授權單位\(CA\)— 例如 VeriSign。 如此一來所有同盟伺服器都共用的相同私密金鑰部分的憑證，然後可以設定憑證。 如需如何共用相同的憑證的詳細資訊，請參閱[檢查清單：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。 **注意：** AD FS 管理嵌入式管理單元\-在參考同盟伺服器，做為服務通訊憑證的伺服器驗證憑證。<br /><br />如需詳細資訊，請參閱＜ [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md)＞。|  
|指向相同的 SQL Server 執行個體|如果 AD FS 設定資料庫會儲存在 SQL database 中，新的同盟伺服器必須指向相同的 SQL Server 執行個體，以便新的伺服器可以參與伺服器陣列可由伺服陣列中的其他同盟伺服器。|  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
