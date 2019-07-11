---
title: 遠端桌面服務角色
description: 描述桌面主機服務的元件。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 717f6c714ce95793a9a7b5fcf0ddd304c77daa32
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63753180"
---
# <a name="remote-desktop-services-roles"></a>遠端桌面服務角色

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

本文描述遠端桌面服務環境中的角色。

## <a name="remote-desktop-session-host"></a>遠端桌面工作階段主機

遠端桌面工作階段主機 (RD 工作階段主機) 會保留您與使用者共用的工作階段型應用程式和桌面。 使用者可以透過 Windows、MacOS、iOS 和 Android 上執行的其中一個遠端桌面用戶端，前往這些桌面和應用程式。 使用者也可以使用網頁用戶端透過支援的瀏覽器進行連線。

您可以將桌面和應用程式組織成一或多個 RD 工作階段主機伺服器，稱為「集合」。 您可以自訂這些集合來包含每個租用戶中的特定使用者群組。 例如，您可以建立一個集合，讓其中的特定使用者群組能夠存取特定應用程式，但所指定群組以外的任何人則無法存取這些應用程式。

針對小型部署，您可以直接在 RD 工作階段主機伺服器上安裝應用程式。 針對較大型的部署，建議建置基底映像，然後從該映像佈建虛擬機器。

您可以將 RD 工作階段主機伺服器虛擬機器新增至集合伺服器陣列，並將集合中的每部 RDSH 虛擬機器指派給相同的可用性設定組，藉此擴充集合。 這會提供更高的集合可用性並增加規模，以支援更多的使用者或耗用大量資源的應用程式。

在大部分情況下，多位使用者會共用相同的 RD 工作階段主機伺服器，以最有效率地將 Azure資源用於桌面主機解決方案。 在此設定中，使用者必須使用非系統管理帳戶登入集合。 您也可以透過建立個人工作階段桌面集合，來為某些使用者提供其遠端桌面的完整系統管理存取權。

您可以透過建立和上傳具有 Windows Server 作業系統的虛擬硬碟，作為建立新 RD 工作階段主機虛擬機器的範本，來更進一步地自訂桌面。

如需詳細資訊，請參閱下列文章：

* [Remote Desktop Services - Secure data storage](rds-plan-secure-data-storage.md) (遠端桌面服務 - 安全資料儲存區)
* [Upload a generalized VHD and use it to create new VMs in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) (將一般化 VHD 上傳，並使用它在 Azure 中建立新的 VM)
* [Update RDSH collection (ARM template)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/) (更新 RDSH 集合 (ARM 範本))

## <a name="remote-desktop-connection-broker"></a>遠端桌面連線代理人

遠端桌面連線代理人 (RD 連線代理人) 管理 RD 工作階段主機伺服器陣列的連入遠端桌面連線。 RD 連線代理人處理完整桌面集合與遠端應用程式集合的連線。 RD 連線代理人可以在建立新連線時平衡處理集合伺服器上的負載。 如果工作階段中斷連線，RD 連線代理人會將使用者重新連線到正確的 RD 工作階段主機伺服器，以及其遭到中斷的工作階段 (這些工作階段仍存在於 RD 工作階段主機伺服器陣列中)。

您必須在 RD 連線代理人伺服器和用戶端上安裝相符的數位憑證，以支援單一登入和應用程式發佈。 當您開發或測試網路時，您可以使用自我產生和自我簽署的憑證。 不過，已發行服務需要來自信任憑證授權單位的數位憑證。 您提供給憑證的名稱必須與 RD 連線代理人虛擬機器的內部完整網域名稱 (FQDN) 相同。

您可以將 Windows Server 2016 RD 連線代理人安裝在與 AD DS 相同的虛擬機器上來降低成本。 如果您需要向外延展到更多使用者，您也可以在相同的可用性設定組中新增其他 RD 連線代理人虛擬機器來建立 RD 連線代理人叢集。

您必須在租用戶的環境中部署 Azure SQL Database 或建立 SQL Server AlwaysOn 可用性群組，才能建立 RD 連線代理人叢集。

如需詳細資訊，請參閱下列文章：

* [Add the RD Connection Broker server to the deployment and configure high availability](rds-connection-broker-cluster.md) (將 RD 連線代理人伺服器新增至部署並設定高可用性)
* ＜桌面主機服務＞中的 [SQL 資料庫](desktop-hosting-service.md#sql-database)。

## <a name="remote-desktop-gateway"></a>遠端桌面閘道

遠端桌面閘道 (RD 閘道) 授權公用網路上的使用者存取 Microsoft Azure 雲端服務中所裝載 Windows 桌面和應用程式。

RD 閘道元件使用安全通訊端層 (SSL) 來加密用戶端與伺服器之間的通訊通道。 RD 閘道虛擬機器必須可透過公用 IP 位址存取，該位址允許連接埠 443 的輸入 TCP 連線和連接埠 3391 的輸入 UDP 連線。 這讓使用者可以分別使用 HTTPS 通訊傳輸通訊協定和 UDP 通訊協定透過網際網路進行連線。

安裝在伺服器和用戶端上的數位憑證必須相符才能運作。 當您開發或測試網路時，您可以使用自我產生和自我簽署憑證。 不過，已發行服務需要來自信任憑證授權單位的憑證。 憑證名稱必須與用來存取 RD 閘道的 FQDN 相符，無論 FQDN 是公用 IP 位址對外的 DNS 名稱，還是指向公用 IP 位址的 DNS CNAME 記錄。

對於具有較少使用者的租用戶，您可以將 RD Web 存取和 RD 閘道角色合併到單一虛擬機器來降低成本。 您也可以將更多 RD 閘道虛擬機器新增至 RD 閘道伺服器陣列，以提高服務可用性並向外延展到更多使用者。 較大型 RD 閘道伺服器陣列中的虛擬機器應該在負載平衡集中設定。 當您在 Windows Server 2016 虛擬機器上使用 RD 閘道時，不需要 IP 親和性；但當您在 Windows Server 2012 R2 虛擬機器上執行 RD 閘道時，則需要 IP 親和性。

如需詳細資訊，請參閱下列文章：

* [Add high availability to the RD Web and Gateway web front](rds-rdweb-gateway-ha.md) (新增高可用性到 RD Web 和閘道 Web 前端)
* [遠端桌面服務 - 隨處存取](rds-plan-access-from-anywhere.md)
* [Remote Desktop Services - Multi-factor authentication](rds-plan-mfa.md) (遠端桌面服務 - 多重要素驗證)

## <a name="remote-desktop-web-access"></a>遠端桌面 Web 存取

遠端桌面 Web 存取 (RD Web 存取) 可讓使用者透過入口網站存取桌面和應用程式，並透過裝置的原生 Microsoft 遠端桌面用戶端應用程式加以啟動。 您可以使用入口網站將 Windows 桌面和應用程式發佈到 Windows 和非 Windows 用戶端裝置，您也可以選擇性地將桌面或應用程式發佈給特定使用者或群組。

RD Web 存取需要 Internet Information Services (IIS) 才能正常運作。 超文字安全傳輸通訊協定 (HTTPS) 連線在用戶端與 RD Web 伺服器之間提供加密的通訊通道。 RD Web 存取虛擬機器必須可透過公用 IP 位址存取，該位址允許連接埠 443 的輸入 TCP 連線，以允許租用戶的使用者使用 HTTPS 通訊傳輸通訊協定從網際網路進行連線。

必須將相符的數位憑證安裝在伺服器和用戶端上。 基於開發和測試目的，這可以是自我產生和自我簽署的憑證。 若是已發行的服務，則必須從信任的憑證授權單位取得數位憑證。 憑證名稱必須與用來存取 RD Web 存取的完整網域名稱 (FQDN) 相符。 可能的 FQDN 包括公用 IP 位址對外 DNS 名稱，以及指向公用 IP 位址的 CNAME DNS 記錄。

對於具有較少使用者的租用戶，您可以將 RD Web 存取和遠端桌面閘道工作負載合併到單一虛擬機器來降低成本。 您也可以將其他 RD Web 虛擬機器新增至 RD Web 存取伺服器陣列，以提高服務可用性並向外延展到更多使用者。 在具有多部虛擬機器的 RD Web 存取伺服器陣列中，您必須在負載平衡集中設定虛擬機器。

如需如何設定 RD Web 存取的詳細資訊，請參閱下列文章：

* [為您的使用者設定遠端桌面 Web 用戶端](clients/remote-desktop-web-client-admin.md)
* [建立和部署遠端桌面服務集合](rds-create-collection.md)
* [建立遠端桌面服務集合，以執行桌面和應用程式](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>遠端桌面授權

已啟用遠端桌面授權 (RD 授權) 之伺服器可讓使用者連線到裝載租用戶桌面和應用程式的 RD 工作階段主機伺服器。 租用戶環境通常已隨附安裝 RD 授權伺服器，但針對託管環境，您必須在每位使用者模式下設定伺服器。

服務提供者需要足夠的 RDS 訂閱者存取使用權 (SAL)，才能涵蓋每月登入服務的所有已授權非重複 (非並行) 使用者。 服務提供者可以直接購買 Microsoft Azure 基礎結構服務，且可以透過 Microsoft 服務提供者授權合約 (SPLA) 方案購買 SAL。 需要託管桌面解決方案的客戶必須向服務提供者購買完整託管解決方案 (Azure 和 RDS)。

小型租用戶可以將檔案伺服器和 RD 授權元件合併到單一虛擬機器來降低成本。 若要提供更高的服務可用性，租用戶可以在相同的可用性設定組中部署兩部 RD 授權伺服器虛擬機器。 租用戶環境中的所有 RD 伺服器都會與這兩部 RD 授權伺服器建立關聯，讓使用者能夠連線到新的工作階段，即使其中一部伺服器離線也一樣。

如需詳細資訊，請參閱下列文章：

* [使用用戶端存取使用權 (CAL) 授權您的 RDS 部署](rds-client-access-license.md)
* [啟用遠端桌面服務授權伺服器](rds-activate-license-server.md)
* [追蹤您的遠端桌面服務用戶端存取使用權 (RDS CAL)](rds-track-cals.md)
* [Microsoft 大量授權：服務提供者授權選項](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)