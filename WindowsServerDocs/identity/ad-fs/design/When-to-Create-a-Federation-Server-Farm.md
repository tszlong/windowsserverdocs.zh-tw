---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: 建立同盟伺服器陣列的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 62aacdf0662eddc7bbc99d8434346fe8e48cc944
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972045"
---
# <a name="when-to-create-a-federation-server-farm"></a>建立同盟伺服器陣列的時機

\( \) 當您有較大的 AD FS 部署，而且想要為貴組織的同盟服務提供容錯、負載平衡或擴充性時，請考慮在 Active Directory 同盟服務 AD FS 中建立同盟伺服器陣列 \- 。 在相同網路中建立兩部或多部同盟伺服器的動作，將每一部伺服器設定為使用相同的同盟服務，並將每個伺服器的權杖 \- 簽署憑證的公開金鑰新增至 [AD FS 管理] 嵌入式管理單元，以 \- 建立同盟伺服器陣列。

您可以使用 [AD FS 同盟伺服器設定] Wizard 來建立同盟伺服器陣列，或將其他的同盟伺服器安裝到現有的伺服器陣列。 如需詳細資訊，請參閱＜ [When to Create a Federation Server](When-to-Create-a-Federation-Server.md)＞。

> [!NOTE]
> 當您選擇使用 [AD FS 同盟伺服器設定] [建立**新的同盟伺服器**陣列] 選項時，此 wizard 會嘗試建立容器物件， \( 以便 \) 在 Active Directory 中共用憑證。 因此，第一次登入您要在其中設定同盟伺服器角色的電腦時，請務必使用在 Active Directory 中具有足夠權限的帳戶，以便建立這個容器物件。

同盟伺服器必須先進行叢集化，才能將它群組為伺服器陣列，如此一來，抵達單一完整功能變數名稱 FQDN 的要求 \( \) 才會路由傳送至伺服器陣列中的各種同盟伺服器。 您可以在 \( 公司網路內部署網路負載平衡 NLB，以建立伺服器叢集 \) 。 本指南假設 NLB 已適當設定，以叢集伺服器陣列中的每部同盟伺服器。

如需有關如何使用 Microsoft NLB 技術來設定叢集 FQDN 的詳細資訊，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?LinkID=74651)。

## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署同盟伺服器陣列的最佳作法
我們建議您在生產環境中部署同盟伺服器的下列最佳作法：

-   如果您要同時部署多部同盟伺服器，或您知道您將會在一段時間後新增更多伺服器至伺服器陣列，請考慮在伺服陣列中建立現有同盟伺服器的伺服器映射，然後在需要快速建立其他同盟伺服器時，從該映射進行安裝。

    > [!NOTE]
    > 如果您決定使用伺服器映射方法來部署其他同盟伺服器，您就不需要在每次想要將新的伺服器加入至伺服器陣列時，完成[檢查清單：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)中的工作。

-   使用 NLB 或其他形式的叢集，為許多同盟伺服器電腦配置單一 IP 位址。

-   為伺服器陣列中的每部同盟伺服器保留靜態 IP 位址，並根據您的網域名稱系統 \( DNS 設定 \) ，在動態主機設定通訊協定 DHCP 中插入每個 IP 位址的 \( 排除 \) 。 Microsoft NLB 技術要求參與 NLB 叢集的每部伺服器都要有指派的靜態 IP 位址。

-   如果 AD FS 設定資料庫會儲存在 SQL 資料庫中，請避免同時從多部同盟伺服器編輯 SQL 資料庫。

## <a name="configuring-federation-servers-for-a-farm"></a>設定伺服器陣列的同盟伺服器
下表說明必須完成的工作，讓每部同盟伺服器都可以參與陣列環境。

|Task|描述|
|--------|---------------|
|如果您使用 SQL Server 來儲存 AD FS 設定資料庫|同盟伺服器陣列是由兩個或多個共用相同 AD FS 設定資料庫和權杖簽署憑證的同盟伺服器所組成 \- 。 設定資料庫可以存放在 Windows 內部資料庫或 SQL Server 資料庫中。 如果您打算將設定資料庫存放在 SQL 資料庫中，請確定設定資料庫可供存取，以供所有參與伺服器陣列的新同盟伺服器存取。 **注意：** 對於伺服器陣列案例而言，設定資料庫必須位於不會同時參與該伺服器陣列中同盟伺服器的電腦上，這點很重要。 Microsoft NLB 不允許參與伺服器陣列中的任何電腦互相通訊。 **注意：** 請確定 \( \) 參與伺服器陣列的每一部同盟伺服器上，Internet Information Services IIS 中的 AD FS AppPool 的識別 \) 具有設定資料庫的讀取權限。|
|取得及共用憑證|您可以從公開憑證授權單位單位 \( CA （例如 VeriSign）取得單一伺服器驗證憑證 \) 。 接著，您可以設定憑證，讓所有同盟伺服器共用憑證的相同私密金鑰部分。 如需有關如何共用相同憑證的詳細資訊，請參閱＜ [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)＞。 **注意：** 中的 [AD FS 管理] 嵌入式管理單元 \- 是指做為服務通訊憑證之同盟伺服器的伺服器驗證憑證。<p>如需詳細資訊，請參閱＜ [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md)＞。|
|指向相同的 SQL Server 執行個體|如果 AD FS 設定資料庫會儲存在 SQL 資料庫中，新的同盟伺服器必須指向伺服器陣列中其他同盟伺服器所使用的相同 SQL Server 實例，如此新的伺服器才可以參與伺服器陣列。|

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
