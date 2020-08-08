---
title: 伺服器憑證部署概觀
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 318bab675cc633034731e369b5da2bbb40d810b0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949412"
---
# <a name="server-certificate-deployment-overview"></a>伺服器憑證部署概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題包含下列各節。

-   [伺服器憑證部署元件](#bkmk_components)

-   [伺服器憑證部署程式總覽](#bkmk_process)

## <a name="server-certificate-deployment-components"></a><a name="bkmk_components"></a>伺服器憑證部署元件
您可以使用本指南，將 Active Directory 憑證服務安裝 (AD CS) 作為企業根憑證授權單位 (CA) 並向執行網路原則伺服器 (NPS) 、路由及遠端存取服務 (RRAS) 或 NPS 和 RRAS 的伺服器註冊伺服器憑證。


如果您使用以憑證為基礎的驗證來部署 SDN，伺服器就必須使用伺服器憑證向其他伺服器證明其身分識別，使其能夠達到安全的通訊。

下圖顯示在 SDN 基礎結構中將伺服器憑證部署至伺服器所需的元件。

![伺服器憑證部署所需的基礎結構](../../../media/Nps-Certs/Nps-Certs.jpg)

> [!NOTE]
> 在上圖中，會描述多部伺服器： DC1、CA1、WEB1 和許多 SDN 伺服器。 本指南提供部署和設定 CA1 和 WEB1，以及設定 DC1 的指示，本指南假設您已在網路上安裝。 如果您尚未安裝 Active Directory 網域，可以使用 Windows Server 2016 的[核心網路指南](https://technet.microsoft.com/library/mt604042.aspx)來執行此動作。

如需有關上圖中所描述之每個專案的詳細資訊，請參閱下列各項：

-   [CA1](#bkmk_ca1)

-   [WEB1](#bkmk_web1)

-   [DC1](#bkmk_dc1)

-   [NPS1](#bkmk_nps1)

### <a name="ca1-running-the-ad-cs-server-role"></a><a name="bkmk_ca1"></a>執行 AD CS 伺服器角色的 CA1
在此案例中， (CA) 的企業根憑證授權單位也是發行 CA。 CA 會將憑證頒發給具有正確安全性許可權的伺服器電腦，以註冊憑證。 Active Directory 憑證服務 (AD CS) 安裝在 CA1 上。

對於較大的網路或安全性考慮提供理由，您可以區隔根 CA 和發行 CA 的角色，以及部署發行 Ca 的次級 Ca。

在最安全的部署中，企業根 CA 會離線並受到實際保護。

#### <a name="capolicyinf"></a>Capolicy.inf .inf
在安裝 AD CS 之前，您可以使用部署的特定設定來設定 Capolicy.inf .inf 檔案。

#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>**RAS 與 資訊存取伺服器**憑證範本的複本
當您部署伺服器憑證時，您會建立一個 [ **RAS 和 資訊存取伺服器**] 憑證範本的複本，然後根據您的需求和本指南中的指示來設定範本。

您會利用範本的複本，而不是原始範本，因此會保留原始範本的設定，以供未來使用。 您可以設定 [ **RAS 和 資訊存取伺服器**] 範本的複本，讓 CA 可以建立伺服器憑證，將其發行至您所指定 Active Directory 使用者和電腦中的群組。

#### <a name="additional-ca1-configuration"></a>其他 CA1 設定
CA 會發佈憑證撤銷清單 (CRL) 電腦必須檢查，以確保出示給它們的憑證是有效憑證，而且尚未被撤銷。 您必須使用 CRL 的正確位置來設定您的 CA，讓電腦知道在驗證過程中要尋找 CRL 的位置。

### <a name="web1-running-the-web-services-iis-server-role"></a><a name="bkmk_web1"></a> (IIS) 伺服器角色執行 Web 服務的 WEB1
在執行 Web 服務器 (IIS) 伺服器角色 WEB1 的電腦上，您必須在 Windows Explorer 中建立資料夾，以作為 CRL 和 AIA 的位置。

#### <a name="virtual-directory-for-the-crl-and-aia"></a>CRL 和 AIA 的虛擬目錄
在 Windows Explorer 中建立資料夾之後，您必須將資料夾設定為 Internet Information Services (IIS) 管理員）中的虛擬目錄，以及設定虛擬目錄的存取控制清單，以允許電腦在該處發佈 AIA 和 CRL 後進行存取。

### <a name="dc1-running-the-ad-ds-and-dns-server-roles"></a><a name="bkmk_dc1"></a>DC1 執行 AD DS 和 DNS 伺服器角色
DC1 是您網路上的網域控制站和 DNS 伺服器。

#### <a name="group-policy-default-domain-policy"></a>群組原則預設網域原則
在 CA 上設定憑證範本之後，您可以在群組原則中設定預設網域原則，以便將憑證自動註冊到 NPS 和 RAS 伺服器。 群組原則是在伺服器 DC1 的 AD DS 中設定。

#### <a name="dns-alias-cname-resource-record"></a>DNS 別名 (CNAME) 資源記錄
您必須為 Web 服務器建立 (CNAME) 資源記錄的別名，以確保其他電腦可以找到伺服器，以及伺服器上儲存的 AIA 和 CRL。 此外，使用別名 CNAME 資源記錄可提供彈性，讓您可以針對其他用途（例如，裝載 Web 和 FTP 網站）使用網頁伺服器。

### <a name="nps1-running-the-network-policy-server-role-service-of-the-network-policy-and-access-services-server-role"></a><a name="bkmk_nps1"></a>NPS1 執行網路原則與存取服務伺服器角色的網路原則伺服器角色服務
NPS 會在您執行 Windows Server 2016 核心網路指南中的工作時安裝，因此在您執行本指南中的工作之前，您的網路上應該已安裝一或多個 Nps。

#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>群組原則已套用並已向伺服器註冊憑證
設定憑證範本和自動註冊之後，您可以重新整理所有目標伺服器上的群組原則。 此時，伺服器會從 CA1 註冊伺服器憑證。

### <a name="server-certificate-deployment-process-overview"></a><a name="bkmk_process"></a>伺服器憑證部署程式總覽

> [!NOTE]
> [伺服器憑證部署](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)一節中會提供如何執行這些步驟的詳細資料。

設定伺服器憑證註冊的程式會發生在下列階段：

1.  在 WEB1 上， (IIS) 角色安裝 Web 服務器。

2.  在 DC1 上，為您的 Web 服務器 WEB1 建立 (CNAME) 記錄的別名。

3.  將您的 Web 服務器設定為裝載 CA 的 CRL，然後發佈 CRL 並將企業根 CA 憑證複製到新的虛擬目錄。

4.  在您打算安裝 AD CS 的電腦上，將電腦指派為靜態 IP 位址、將電腦重新命名、將電腦加入網域，然後使用屬於 Domain Admins 和 Enterprise Admins 群組成員的使用者帳戶登入電腦。

5.  在您打算安裝 AD CS 的電腦上，使用您的部署特定的設定來設定 Capolicy.inf 檔案。

6.  安裝 AD CS 伺服器角色，並執行 CA 的其他設定。

7.  將 CRL 和 CA 憑證從 CA1 複製到網頁伺服器 WEB1 上的共用。

8.  在 CA 上，設定 [RAS 和 資訊存取伺服器] 憑證範本的複本。 CA 會根據憑證範本頒發證書，因此您必須先設定伺服器憑證的範本，CA 才能發行憑證。

9.  在群組原則中設定伺服器憑證自動註冊。 當您設定自動註冊時，您使用 Active Directory 群組成員資格指定的所有伺服器，都會在重新整理每部伺服器上的群組原則時，自動接收伺服器憑證。 如果您稍後新增更多伺服器，它們也會自動接收伺服器憑證。

10. 重新整理伺服器上的群組原則。 當群組原則重新整理時，伺服器會收到伺服器憑證，這是以您在上一個步驟中設定的範本為基礎。 伺服器會使用此憑證，在驗證過程中向用戶端電腦和其他伺服器證明其身分識別。

    > [!NOTE]
    > 所有網域成員電腦都會自動接收企業根 CA 的憑證，而不會設定自動註冊。 此憑證不同于您使用自動註冊來設定和散發的伺服器憑證。 CA 的憑證會自動安裝在所有網域成員電腦的「信任的根憑證授權」憑證存放區中，使其信任此 CA 所發行的憑證。

10. 確認所有伺服器都已註冊有效的伺服器憑證。



