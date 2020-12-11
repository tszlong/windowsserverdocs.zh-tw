---
description: 深入瞭解：何時建立同盟伺服器陣列
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: 建立同盟伺服器陣列的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8728f7da22618d9c05a4016c8156b18a319da5a5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049946"
---
# <a name="when-to-create-a-federation-server-farm"></a>建立同盟伺服器陣列的時機

\( \) 當您有較大的 AD FS 部署，而您想要提供容錯、負載 \- 平衡或您組織的同盟服務的擴充能力時，請考慮在 Active Directory 同盟服務 AD FS 中建立同盟伺服器陣列。 在相同網路中建立兩部或多部同盟伺服器、將每個同盟伺服器設定為使用相同的同盟服務，以及將每個伺服器的權杖簽署憑證的公開金鑰新增 \- 至中的 [AD FS 管理] 嵌入式管理單元的動作， \- 都會建立同盟伺服器陣列。

您可以使用 AD FS 同盟伺服器設定向導]，建立同盟伺服器陣列或安裝其他同盟伺服器至現有的伺服器陣列。 如需詳細資訊，請參閱＜ [When to Create a Federation Server](When-to-Create-a-Federation-Server.md)＞。

> [!NOTE]
> 當您使用 AD FS 同盟伺服器設定向導] 選擇建立 **新同盟伺服器** 陣列的選項時，嚮導會嘗試建立容器物件， \( 以便 \) 在 Active Directory 中共用憑證。 因此，第一次登入您要在其中設定同盟伺服器角色的電腦時，請務必使用在 Active Directory 中具有足夠權限的帳戶，以便建立這個容器物件。

同盟伺服器必須先叢集化，才能將同盟伺服器群組為 \( \) 伺服器陣列中的不同同盟伺服器，才能將同盟伺服器群組為伺服器陣列。 您可以藉由在 \( 公司網路內部署網路負載平衡 NLB 來建立伺服器叢集 \) 。 本指南假設 NLB 已適當設定，以叢集伺服器陣列中的每部同盟伺服器。

如需有關如何使用 Microsoft NLB 技術來設定叢集 FQDN 的詳細資訊，請參閱 [指定叢集參數](https://go.microsoft.com/fwlink/?LinkID=74651)。

## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署同盟伺服器陣列的最佳作法
建議您在實際執行環境中部署同盟伺服器的最佳作法如下：

-   如果您要同時部署多部同盟伺服器，或您知道將會在一段時間內新增更多伺服器到伺服器陣列，請考慮在伺服器陣列中建立現有同盟伺服器的伺服器映射，然後在需要快速建立額外的同盟伺服器時，從該映射進行安裝。

    > [!NOTE]
    > 如果您決定使用伺服器映射方法來部署其他同盟伺服器，就不需要在每次想要將新的伺服器加入至伺服器陣列時，完成檢查清單中的工作 [：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) 。

-   使用 NLB 或某些其他形式的叢集，為許多同盟伺服器電腦配置單一 IP 位址。

-   針對伺服器陣列中的每部同盟伺服器保留一個靜態 IP 位址，而且根據您的網域名稱系統 \( DNS 設定 \) ，請在動態主機設定通訊協定 DHCP 中插入每個 IP 位址的排除 \( \) 。 Microsoft NLB 技術要求參與 NLB 叢集的每部伺服器都要有指派的靜態 IP 位址。

-   如果 AD FS 設定資料庫會儲存在 SQL 資料庫中，請避免同時從多部同盟伺服器編輯 SQL database。

## <a name="configuring-federation-servers-for-a-farm"></a>設定伺服器陣列的同盟伺服器
下表說明必須完成才能讓每部同盟伺服器參與陣列環境的工作。

|Task|描述|
|--------|---------------|
|如果您使用 SQL Server 來儲存 AD FS 設定資料庫|同盟伺服器陣列是由兩部或多部共用相同 AD FS 設定資料庫和權杖簽署憑證的同盟伺服器所組成 \- 。 設定資料庫可以存放在 Windows 內部資料庫或 SQL Server 資料庫中。 如果您打算將設定資料庫儲存在 SQL database 中，請確定設定資料庫可供存取，以供參與伺服器陣列的所有新同盟伺服器存取。 **注意：** 對於伺服器陣列案例而言，設定資料庫必須位於尚未同時參與該伺服器陣列中的同盟伺服器的電腦上。 Microsoft NLB 不允許參與伺服器陣列中的任何電腦互相通訊。 **注意：** 請確定參與陣列的每一部同盟伺服器上，Internet Information Services IIS 中 AD FS AppPool 的身分識別 \( \) \) 都具有設定資料庫的讀取權限。|
|取得及共用憑證|您可以從公開憑證授權單位單位 CA 取得單一伺服器驗證憑證， \( \) 例如 VeriSign。 然後，您可以設定憑證，讓所有同盟伺服器共用憑證的相同私密金鑰部分。 如需有關如何共用相同憑證的詳細資訊，請參閱＜ [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)＞。 **注意：** AD FS 管理嵌入式管理單元會 \- 將同盟伺服器的伺服器驗證憑證視為服務通訊憑證。<p>如需詳細資訊，請參閱＜ [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md)＞。|
|指向相同的 SQL Server 執行個體|如果 AD FS 設定資料庫會儲存在 SQL 資料庫中，則新的同盟伺服器必須指向伺服器陣列中其他同盟伺服器所使用的相同 SQL Server 實例，讓新的伺服器可以參與伺服器陣列。|

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
