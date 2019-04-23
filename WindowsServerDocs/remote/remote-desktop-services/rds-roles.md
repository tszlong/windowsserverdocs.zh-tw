---
title: 遠端桌面服務角色
description: 描述的桌面主機服務的元件。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 48efbffa4f82b707b63e33e6416da43eb105221f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844719"
---
# <a name="remote-desktop-services-roles"></a>遠端桌面服務角色

>適用於：Windows Server （半年通道），Windows Server 2016

這篇文章描述在遠端桌面服務的環境中的角色。

## <a name="remote-desktop-session-host"></a>遠端桌面工作階段主機

遠端桌面工作階段主機 （RD 工作階段主機） 保存的工作階段為基礎的應用程式與您分享的使用者的桌面。 使用者會收到這些桌上型電腦和透過其中一種在 Windows、 MacOS、 iOS 和 Android 執行遠端桌面用戶端應用程式。 使用者也可以使用 web 用戶端連線透過支援的瀏覽器。

您可以將桌面及應用程式組織成一或多個 RD 工作階段主機伺服器，稱為 「 集合 」。 您可以自訂這些特定使用者群組的每個租用戶內的集合。 例如，您可以建立的集合的特定使用者群組可以存取特定的應用程式，但您指定的群組之外的任何人都將無法存取這些應用程式。

針對小型部署，您可以安裝應用程式，直接在 RD 工作階段主機伺服器上。 若是大型部署，建議建置基底映像，並佈建虛擬機器，從該映像。

您可以展開集合，藉由將集合指派給相同的可用性設定組內的每個 RDSH 虛擬機器的集合伺服器陣列中的 RD 工作階段主機伺服器的虛擬機器。 這提供更高的集合可用性，並增加擴充，以支援更多的使用者或耗用大量資源的應用程式。

在大部分情況下，多個使用者會共用相同的 RD 工作階段主機伺服器可最有效率地利用 Azure 桌面主機解決方案的資源。 在此組態中，使用者必須登入使用非系統管理帳戶的集合。 您也可以讓某些使用者完整的系統管理存取他們的遠端桌面藉由建立個人的工作階段桌面集合。

您可以自訂桌面更多建立並上傳具有 Windows Server 作業系統，您可以使用做為範本，建立新的 RD 工作階段主機虛擬機器的虛擬硬碟。

如需詳細資訊，請參閱下列文章：

* [遠端桌面服務-保護資料存放區](rds-plan-secure-data-storage.md)
* [將一般化的 VHD 上傳並使用它來在 Azure 中建立新的 Vm](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [更新 RDSH 集合 （ARM 範本）](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>遠端桌面連線代理人

遠端桌面連線代理人 （RD 連線代理人） 管理連入遠端桌面連線到 RD 工作階段主機伺服器陣列。 RD 連線代理人會處理連線至這兩個集合的完整桌面和遠端應用程式的集合。 RD 連線代理人在建立新的連接時，可以在集合的伺服器之間平衡負載。 如果工作階段中斷連線，RD 連線代理人會重新連接到正確的 RD 工作階段主機伺服器，以及其中斷的工作階段，仍然在 RD 工作階段主機伺服器陣列中的使用者。

您必須安裝相符的 RD 連線代理人伺服器和用戶端支援單一登入和發佈應用程式上的數位簽章。 當開發或測試網路，您可以使用自我產生及自我簽署憑證。 不過，已發行的服務需要來自受信任的憑證授權單位的數位憑證。 指定憑證名稱必須是相同做為內部完整格式網域名稱 (FQDN) 的 RD 連線代理人虛擬機器。

您可以與以降低成本的 AD DS 相同的虛擬機器上安裝 Windows Server 2016 的 RD 連線代理人。 如果您需要向外延展至多個使用者，您也可以在相同可用性設定組建立 RD 連線代理人叢集新增額外的 RD 連線代理人虛擬機器。

您可以建立 RD 連線代理人叢集之前，您必須部署在租用戶的環境中 Azure SQL Database，或建立 SQL Server AlwaysOn 可用性群組。

如需詳細資訊，請參閱下列文章：

* [將 RD 連線代理人伺服器加入至部署並設定高可用性](rds-connection-broker-cluster.md)
* [SQL database](desktop-hosting-service.md#sql-database)桌面主機服務中。

## <a name="remote-desktop-gateway"></a>遠端桌面閘道

遠端桌面閘道 （RD 閘道） 授與使用者的 Windows 桌面和應用程式裝載於 Microsoft Azure 雲端服務的公用網路存取。

RD 閘道器元件會使用 Secure Sockets Layer (SSL) 加密用戶端與伺服器之間的通訊通道。 RD 閘道虛擬機器必須能夠透過允許連接埠 443 的輸入的 TCP 連線和傳入的 UDP 連接埠 3391 公用 IP 位址。 這可讓使用者透過網際網路使用 HTTPS 通訊的傳輸通訊協定和 UDP 通訊協定，分別連接。

安裝在伺服器和用戶端上的數位簽章必須符合針對此目的。 當您開發或測試網路時，您可以使用自我產生及自我簽署憑證。 不過，已發行的服務需要來自受信任的憑證授權單位的憑證。 憑證的名稱必須符合 FQDN 是否的公用 IP 位址的對外公開的 DNS 名稱或 DNS CNAME 記錄指向公用 IP 位址，用來存取 RD 閘道的 FQDN。

對於具有較少的使用者的租用戶，RD Web 存取和 RD 閘道角色可以結合在單一的虛擬機器，以降低成本。 您也可以新增更多的 RD 閘道虛擬機器至 RD 閘道伺服器陣列以增加服務的可用性和向外延展至多個使用者。 較大的 RD 閘道伺服器陣列中的虛擬機器應該設定負載平衡集合中。 當您使用 RD 閘道在 Windows Server 2016 虛擬機器，但它是在 Windows Server 2012 R2 虛擬機器上執行它時，不需要 IP 同質性。

如需詳細資訊，請參閱下列文章：

* [將 RD Web 和閘道的 web 前端的高可用性](rds-rdweb-gateway-ha.md)
* [遠端桌面服務-隨處存取](rds-plan-access-from-anywhere.md)
* [遠端桌面服務-multi-factor authentication](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>遠端桌面 Web 存取

遠端桌面 Web 存取 （RD Web 存取） 可讓使用者存取桌面並透過 web 入口網站的應用程式，並透過裝置的原生 Microsoft 遠端桌面用戶端應用程式啟動它們。 您可以使用入口網站來發佈 Windows 桌上型電腦和 Windows 和非 Windows 用戶端裝置的應用程式，您也可以選擇性地可以將桌面或應用程式發佈給特定使用者或群組。

RD Web 存取需要網際網路資訊服務 (IIS) 才能正常運作。 超文字傳輸通訊協定安全 (HTTPS) 連線提供用戶端和 RD Web 伺服器之間的加密的通訊通道。 RD Web 存取虛擬機器必須能夠透過允許輸入連接埠 443 允許租用戶的使用者從網際網路連線的 TCP 連線使用 HTTPS 通訊的傳輸通訊協定的公用 IP 位址。

在伺服器和用戶端上必須安裝相符的數位簽章。 用於開發和測試用途，這可以是自我產生及自我簽署憑證。 發行的服務，必須從受信任的憑證授權單位取得的數位憑證。 憑證的名稱必須符合完整格式網域名稱 (FQDN) 用來存取 RD Web 存取。 可能的 Fqdn 包括對外開放的公用 IP 位址和 DNS CNAME 記錄指向公用 IP 位址的 DNS 名稱。

對於具有較少的使用者的租用戶，您可以藉由結合成單一虛擬機器的 RD Web 存取和遠端桌面閘道的工作負載降低成本。 您也可以新增額外的 RD Web 虛擬機器至 RD Web 存取的伺服器陣列以增加服務的可用性和向外延展至多個使用者。 在 RD Web 存取伺服器陣列使用多部虛擬機器，您必須設定負載平衡集中的虛擬機器。

如需如何設定 RD Web 存取的詳細資訊，請參閱下列文章：

* [設定遠端桌面 web 用戶端，為您的使用者](clients/remote-desktop-web-client-admin.md)
* [建立和部署遠端桌面服務集合](rds-create-collection.md)
* [建立桌面和應用程式執行的遠端桌面服務集合](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>遠端桌面授權

已啟用遠端桌面授權 （RD 授權） 的伺服器可讓使用者連線到裝載租用戶的桌面及應用程式的 RD 工作階段主機伺服器。 租用戶環境通常會隨附 RD 授權伺服器已安裝，但是裝載環境的您必須在每個使用者模式下設定伺服器。

服務提供者必須足夠 RDS 訂戶存取授權 (Sal) 涵蓋所有授權 （不同時） 的唯一使用者登入服務每個月。 服務提供者可以直接購買 Microsoft Azure 基礎結構服務，而且可以透過 Microsoft 服務提供者授權合約 (SPLA) 方案購買 Sal。 尋找託管桌面解決方案的客戶必須購買完整的託管的方案 （Azure 和 RDS） 從服務提供者。

小型租用戶可以降低成本，藉由結合到單一虛擬機器上的 RD 授權元件與檔案伺服器。 若要提供更高的服務可用性，租用戶可以部署兩部 RD 授權伺服器虛擬機器，在相同的可用性設定組中。 使用這兩部 RD 授權伺服器，讓使用者能夠連線到新的工作階段，即使其中一部伺服器當機，在租用戶的環境中的所有 RD 伺服器都會產生關聯。

如需詳細資訊，請參閱下列文章：

* [使用用戶端存取使用權 (Cal) 授權您的 RDS 部署](rds-client-access-license.md)
* [啟用遠端桌面服務授權伺服器](rds-activate-license-server.md)
* [追蹤您的遠端桌面服務用戶端存取使用權 (RDS Cal)](rds-track-cals.md)
* [Microsoft 大量授權： 授權服務提供者的選項](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)