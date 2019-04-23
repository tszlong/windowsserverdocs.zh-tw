---
title: 桌面主機服務
description: 描述的桌面主機服務的元件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: adbb9fd69bc61d2e877cadb0484a4e42093f262a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840469"
---
# <a name="desktop-hosting-service"></a>桌面主機服務

>適用於：Windows Server （半年通道），Windows Server 2016

這篇文章將告訴您更多關於桌面主機服務的元件。

## <a name="tenant-environment"></a>租用戶環境

中所述[遠端桌面服務角色](rds-roles.md)，每個角色的租用戶環境中扮演不同部分。

提供者的桌面主機服務會實作為一組隔離的租用戶環境中。 每個租用戶的環境是由儲存體容器、 一組的虛擬機器，以及透過隔離的虛擬網路的所有通訊的 Azure 服務，組合所組成。 每個虛擬機器包含一或多個元件組成的租用戶裝載的桌面環境。 下列各小節會描述組成每個租用戶裝載的桌面環境的元件。

## <a name="active-directory-domain-services"></a>Active Directory Domain Services

Active Directory 網域服務 (AD DS) 中將提供的網域和樹系的資訊，使租用戶的使用者可以登入桌上型電腦和應用程式來執行其工作負載。 這也可讓您設定或連接到必要的檔案共用和可能需要的 Windows 應用程式的資料庫。

租用戶的樹系不需要任何與提供者的管理樹系的信任關係。 網域系統管理員帳戶可以設定租用戶的網域中允許 （例如，監視系統狀態和套用軟體更新） 的租用戶的環境中執行管理工作，並協助提供者的技術人員疑難排解和組態。

有多種方式可以部署 AD DS:

1. 租用戶的虛擬網路環境中啟用 Azure Active Directory 網域服務。 這會根據使用者和群組存在於 Azure AD 租用戶建立受管理的 AD DS 執行個體。
2. 設定租用戶的虛擬網路環境中獨立的 AD DS 伺服器。 這可讓您所有的虛擬機器上執行的 AD DS 執行個體的完整控制。
3. 建立站對站 VPN 連線到位於租用戶的內部部署 AD DS 伺服器。 這可讓租用戶，以連接到其現有的 AD DS 執行個體，並減少重複資料刪除的使用者、 群組、 組織單位，等等。

如需詳細資訊，請參閱下列文章：

* [Azure Active Directory 網域服務文件](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [桌面主機參考架構指南適用於 Windows Server 2012 R2](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [在 Azure 入口網站中建立站對站連線](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>SQL 資料庫

高度可用的 SQL 資料庫使用遠端桌面連線代理人來儲存部署資訊，例如目前的使用者連線到主機伺服器的對應。

有多種方式可以部署的 SQL database:

1. 在租用戶的環境中建立 Azure SQL Database。 這為您提供的多餘的 SQL database 功能而不必管理伺服器本身。 這也可讓您將需支付您使用而不是基礎結構投資。
2. 建立 SQL Server AlwaysOn 叢集。 這可讓您運用現有的 SQL Server 基礎結構，並可讓您完整控制 SQL Server 執行個體。

如需如何設定高可用性的 SQL 資料庫基礎結構的詳細資訊，請參閱下列文章：

* [什麼是 Azure SQL Database 服務？](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [建立及設定可用性群組 (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017)。
* [將 RD 連線代理人伺服器加入至部署並設定高可用性](rds-connection-broker-cluster.md)。

## <a name="file-server"></a>檔案伺服器

檔案伺服器會使用伺服器訊息區塊 (SMB) 3.0 通訊協定來提供共用的資料夾。 這些共用的資料夾用來建立及儲存使用者設定檔磁碟檔案 (.vhdx) 備份資料，並讓使用者彼此共用資料，租用戶的雲端服務內。

部署檔案伺服器虛擬機器必須連接並使用 共用資料夾設定的 Azure 資料磁碟。 Azure 資料磁碟使用直接寫入式快取，保證在重新啟動虛擬機器時，不會清除寫入至磁碟。

小型租用戶可以降低成本，藉由結合檔案伺服器和[RD 授權的角色](rds-roles.md#remote-desktop-licensing)租用戶的環境中的單一虛擬機器上。

如需詳細資訊，請參閱下列文章：

* [Windows Server 中的儲存體](../../storage/storage.md)
* [如何將受控的資料磁碟連接至 Azure 入口網站中的 Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>使用者設定檔磁碟

使用者設定檔磁碟讓使用者儲存個人設定和檔案，當使用者在一個集合中，RD 工作階段主機伺服器上的工作階段登入，然後存取相同的設定和檔案時登入到不同[RD 工作階段主機](rds-roles.md#remote-desktop-session-host)集合中的伺服器。 當使用者第一次登入時，租用戶的檔案伺服器就會建立使用者設定檔磁碟，掛接到使用者目前連線到 RD 工作階段主機伺服器。 針對每個後續登入時，使用者設定檔磁碟會掛接到適當的 RD 工作階段主機伺服器，和與每個卸載登出。只有使用者可以存取設定檔磁碟的內容。