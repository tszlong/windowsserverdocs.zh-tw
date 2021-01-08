---
title: 桌面主機服務
description: 深入了解在 Windows Server 中桌面代管服務的元件。
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: lizross
ms.openlocfilehash: 4453c63b364cf664b9f1a0bd0325bcb88b6d3b2b
ms.sourcegitcommit: 5f234fb15c1d0365b60e83a50bf953e317d6239c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879427"
---
# <a name="desktop-hosting-service"></a>桌面主機服務

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

本文將進一步為您介紹桌面主機服務的元件。

## <a name="tenant-environment"></a>租用戶環境

如[遠端桌面服務角色](rds-roles.md)中所述，每個角色都會在租用戶環境中發揮不同的作用。

提供者的桌面主機服務會實作為一組經隔離租用戶環境。 每個租用戶的環境都由儲存體容器、一組虛擬機器和 Azure 服務所組成，這些服務全都透過隔離的虛擬網路進行通訊。 每部虛擬機器都包含組成租用戶裝載桌面環境的一或多個元件。 下列各小節介紹組成每個租用戶裝載桌面環境的元件。

## <a name="active-directory-domain-services"></a>Active Directory 網域服務

Active Directory 網域服務 (AD DS) 提供網域和樹系資訊，讓租用戶的使用者可以登入桌面和應用程式來執行其工作負載。 這也可讓您設定或連線到 Windows 應用程式可能需要的必要檔案共用和資料庫。

租用戶樹系不需要與提供者的管理樹系具有任何信任關係。 您可以在租用戶的網域中設定網域系統管理員帳戶，以允許提供者的技術人員在租用戶環境中執行管理工作 (例如監視系統狀態與套用軟體更新)，並協助進行疑難排解和設定。

有多種方式可以部署 AD DS：

1. 在租用戶的虛擬網路環境中啟用 Azure Active Directory 網域服務。 這會根據存在於 Azure AD 的使用者和群組為租用戶建立受控 AD DS 執行個體。
2. 在租用戶虛擬網路環境中設定獨立的 AD DS 伺服器。 這可讓您完整控制所有在虛擬機器上執行的 AD DS 執行個體。
3. 建立連線到位於租用戶內部 AD DS 伺服器的站對站 VPN。 這可讓租用戶連線到其現有的 AD DS 執行個體，並減少使用者、群組、組織單位等項目的重複資料。

如需詳細資訊，請參閱下列文章：

* [Azure Active Directory 網域服務文件](/azure/active-directory-domain-services/)
* [Windows Server 2012 R2 的桌面主機參考架構指南](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [在 Azure 入口網站中建立站對站連線](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>SQL 資料庫

高度可用的 SQL 資料庫使用遠端桌面連線代理人來儲存部署資訊，例如目前使用者連線與主機伺服器的對應。

有多種方式可以部署 SQL 資料庫：

1. 在租用戶的環境中建立 Azure SQL Database。 這為您提供備援 SQL 資料庫的功能，而不必自行管理伺服器。 也可讓您隨用隨付，不需投資於基礎結構。
2. 建立 SQL Server AlwaysOn 叢集。 這可讓您運用現有的 SQL Server 基礎結構，並可讓您完整控制 SQL Server 執行個體。

如需如何設定高度可用之 SQL 資料庫基礎結構的詳細資訊，請參閱下列文章：

* [什麼是 Azure SQL Database 服務？](/azure/sql-database/sql-database-technical-overview)
* [建立及設定可用性群組 (SQL Server)](/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017&preserve-view=true)。
* [將 RD 連線代理人伺服器新增至部署並設定高可用性](rds-connection-broker-cluster.md)。

## <a name="file-server"></a>檔案伺服器

檔案伺服器使用伺服器訊息區 (SMB) 3.0 通訊協定來提供共用資料夾。 這些共用資料夾用於建立並儲存使用者設定檔磁碟檔案 (.vhdx) 以備份資料，並可讓使用者在租用戶的雲端服務中互相共用資料。

用於部署檔案伺服器的虛擬機器，必須已連結 Azure 資料磁碟並設定共用資料夾。 Azure 資料磁碟使用直接寫入式快取，可保證在重新啟動虛擬機器時不會清除對磁碟的寫入。

小型租用戶可以將檔案伺服器和 [RD 授權角色](rds-roles.md#remote-desktop-licensing)合併到租用戶環境中的單一虛擬機器來降低成本。

如需詳細資訊，請參閱下列文章：

* [Windows Server 的儲存空間](../../storage/storage.yml)
* [如何將受控資料磁碟連結至 Azure 入口網站中的 Windows VM](/azure/virtual-machines/windows/attach-managed-disk-portal?toc=/azure/virtual-machines/windows/classic/toc.json)

### <a name="user-profile-disks"></a>使用者設定檔磁碟

使用者設定檔磁碟可讓使用者在登入至某個集合中 RD 工作階段主機伺服器上的工作階段時儲存個人設定和檔案，在登入至集合中不同的 [RD 工作階段主機](rds-roles.md#remote-desktop-session-host)伺服器時存取相同設定和檔案。 使用者第一次登入時，租用戶的檔案伺服器會建立使用者設定檔磁碟，該磁碟會掛接到使用者目前連線的 RD 工作階段主機伺服器。 之後每次登入時，使用者設定檔磁碟都會掛接到適當的 RD 工作階段主機伺服器，並在每次登出時取消掛接。只有該使用者可以存取該設定檔磁碟的內容。
