---
title: 伺服器憑證部署概觀
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0cafa4bdeb80b22d6bac4ad09bcae9436cda97c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877819"
---
# <a name="server-certificate-deployment-overview"></a>伺服器憑證部署概觀

>適用於：Windows Server （半年通道），Windows Server 2016

本主題涵蓋下列各節。  
  
-   [伺服器憑證部署元件](#bkmk_components)
  
-   [伺服器憑證部署程序概觀](#bkmk_process)
  
## <a name="bkmk_components"></a>伺服器憑證部署元件
安裝 Active Directory 憑證服務 (AD CS) 為企業根憑證授權單位 (CA)，並註冊伺服器憑證，以執行網路原則伺服器 (NPS)、 路由及遠端存取服務 (RRAS) 伺服器，您可以使用本指南或同時 NPS 及 RRAS。


如果您使用憑證式驗證部署 SDN，伺服器，才能使用伺服器憑證來證明其身分識別的其他伺服器，以便達成安全的通訊。
  
下圖顯示將伺服器憑證部署到 SDN 基礎結構中的伺服器所需的元件。
  
![伺服器憑證部署必要的基礎結構](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> 上圖中會說明多部伺服器：DC1、 CA1、 WEB1 和許多的 SDN 伺服器。 本指南提供指示，如部署和設定 CA1 和 WEB1，以及設定 DC1，本指南假設您已安裝在您網路上。 如果尚未安裝 Active Directory 網域，則可以使用[核心網路指南](https://technet.microsoft.com/library/mt604042.aspx)適用於 Windows Server 2016。  
  
如需有關每個項目上圖中所描述的詳細資訊，請參閱下列各項：  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 執行 AD CS 伺服器角色  
在此案例中，企業根憑證授權單位 (CA) 也是發行的 CA。 在 CA 簽發憑證給伺服器的電腦具有正確的安全性權限的憑證。 CA1 上安裝 active Directory 憑證服務 (AD CS)。  
  
對於較大型的網路或安全性考量，提供理由，您可以獨立根 CA 和發行 CA 的角色，並部署會發行 Ca 的次級 Ca。  
  
在最安全的部署中，為企業根 CA 已離線且實體安全。   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
在安裝 AD CS 之前，您設定 CAPolicy.inf 檔案使用特定的設定為您的部署。  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>副本**RAS 及 IAS 伺服器**憑證範本  
當您部署伺服器憑證時，您會建立一份**RAS 及 IAS 伺服器**憑證範本]，然後設定本指南中的 [根據您的需求和指示的範本。   
  
原始範本的設定是保留供未來使用，您可以利用範本，而不是原始範本的複本。 您設定的複本**RAS 及 IAS 伺服器**範本，讓 CA 可以為您建立伺服器憑證發行給 Active Directory 使用者與您指定的電腦中的群組。  
  
#### <a name="additional-ca1-configuration"></a>其他 CA1 組態  
CA 發佈憑證撤銷清單 (CRL)，電腦必須檢查以確保會向其作為身分識別證明的憑證是有效的憑證，並已遭到撤銷。 使電腦知道哪裡可以尋求 CRL 驗證程序期間，您必須設定您的 CA CRL 的正確位置。  
  
### <a name="bkmk_web1"></a>WEB1 執行 Web Services (IIS) 伺服器角色  
執行網頁伺服器 (IIS) 伺服器角色，WEB1，在電腦上您必須在 Windows 檔案總管中建立資料夾做為 CRL 和 AIA 位置使用。  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>AIA 和 CRL 的虛擬目錄  
您在 Windows 檔案總管中建立資料夾之後，您必須將資料夾設定為在 Internet Information Services (IIS) 管理員 中，以及設定虛擬目錄的存取控制清單允許電腦存取的 AIA 和 CRL 中的虛擬目錄在發佈到該處。  
  
### <a name="bkmk_dc1"></a>DC1 執行 AD DS 與 DNS 伺服器角色  
DC1 是網域控制站和網路上的 DNS 伺服器。  
  
#### <a name="group-policy-default-domain-policy"></a>群組原則的預設網域原則  
在 CA 上設定憑證範本之後，您可以設定 預設網域原則群組原則中，讓憑證自動註冊到 NPS 和 RAS 伺服器也可以。 在 DC1 的伺服器上的 AD DS 中設定群組原則。  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS 別名 (CNAME) 資源記錄  
您必須建立的 Web 伺服器，以確保伺服器與 AIA 和 CRL 儲存在伺服器上，就可以尋找其他電腦的別名 (CNAME) 資源記錄。 此外，使用別名的 CNAME 資源記錄提供彈性，讓您可以使用 Web 伺服器用於其他用途，例如裝載 Web 和 FTP 站台。  
  
### <a name="bkmk_nps1"></a>Nps1 的 執行網路原則與存取服務伺服器角色的網路原則伺服器角色服務  
當您執行 Windows Server 2016 核心網路指南中的工作，安裝 NPS，讓您執行本指南中的工作之前，您應該已經有一或多個 NPSs 安裝在您網路上。  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>套用群組原則，並向伺服器註冊憑證  
您已設定憑證範本及自動註冊之後，您可以重新整理所有目標伺服器上的 「 群組原則 」。 在此階段中，伺服器會註冊 CA1 的伺服器憑證。  
  
### <a name="bkmk_process"></a>伺服器憑證部署程序概觀  
  
> [!NOTE]  
> 一節提供如何執行這些步驟的詳細資訊[伺服器憑證部署](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)。  
  
設定伺服器憑證註冊的程序分為下列階段：  
  
1.  在 WEB1 上，安裝網頁伺服器 (IIS) 角色。  
  
2.  在 DC1 上，建立您的 Web 伺服器，WEB1 的別名 (CNAME) 記錄。  
  
3.  設定 Web 伺服器來裝載 CRL 從憑證的 CA，然後發佈 CRL，並將企業根 CA 憑證複製到新的虛擬目錄。  
  
4.  您打算安裝 AD CS 的電腦，將電腦指派靜態 IP 位址、 將電腦重新命名、 將電腦加入網域，和電腦的 Domain Admins 與 Enterprise Admins 群組成員的使用者帳戶登入。  
  
5.  您打算安裝 AD CS 的電腦，請使用專屬於您的部署設定中設定 CAPolicy.inf 檔案。  
  
6.  安裝 AD CS 伺服器角色，並執行其他設定的 CA。  
  
7.  CA1 CRL 和 CA 憑證複製到 Web 伺服器 WEB1 上的共用。  
  
8.  在 CA 上，設定 RAS 與 IAS 伺服器憑證範本的複本。 在 CA 簽發取決於憑證範本，所以您必須先在 CA 可以發出的憑證設定伺服器憑證範本的憑證。  
  
9.  群組原則中設定伺服器憑證自動註冊。 當您設定自動註冊時，自動指定具有 Active Directory 群組成員資格的所有伺服器上每一部伺服器的群組原則重新整理時，就會都接收伺服器憑證。 如果您稍後新增更多的伺服器，它們會自動接收伺服器憑證，太。  
  
10. 在伺服器上的 [重新整理群組原則]。 重新整理群組原則時，伺服器會接收伺服器憑證，此作業取決於您在上一個步驟中設定的範本。 用戶端電腦證明其識別身分伺服器與其他伺服器會使用此憑證在驗證程序。  
  
    > [!NOTE]  
    > 所有網域成員電腦自動都接收企業根 CA 的憑證，而不需要設定自動註冊。 此憑證是不同的伺服器憑證，您會設定及發佈使用自動註冊。 如此將會信任此 CA 所簽發的憑證，CA 的憑證會自動安裝在所有網域成員電腦的受信任的根憑證授權單位憑證存放區中。   
  
10. 請確認所有伺服器已都註冊有效的伺服器憑證。  
  


