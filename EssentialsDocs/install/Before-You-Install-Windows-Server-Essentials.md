---
title: 在您安裝 Windows Server Essentials 之前
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ceb5d81490b46513b540901413d8923d61d87e40
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623940"
---
# <a name="before-you-install-windows-server-essentials"></a>在您安裝 Windows Server Essentials 之前

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="before-you-begin-your-installation-of--windows-server-essentials-perform-the-following-tasks"></a><a name="BKMK_BeforeYouBegin"></a> 開始安裝 Windows Server Essentials 之前，請先執行下列工作：

-   **確認您的電腦符合最低硬體需求**。 這包括判斷您是否需要額外的硬體，以及驗證 Windows Server Essentials 是否支援硬體的驅動程式。 如需詳細資訊，請參閱 [Windows Server Essentials 的系統需求](../get-started/system-requirements.md)。

> [!IMPORTANT]
> 在預先存在的電腦上安裝 Windows Server Essentials 之前，建議您先完整格式化，然後重新分割預先存在電腦的硬碟。 透過硬碟格式化以及重新分割，您將移除硬碟上可能殘留在的隱藏式磁碟分割。

- **準備您的網路** 若要準備您的網路以安裝 Windows Server Essentials，請執行下列動作：


  - **升級用戶端電腦上的作業系統**  Windows Server Essentials 支援下列作業系統： Windows 8、Windows 7、Windows 10 和 Macintosh OS X Lion 或更新版本。 這些作業系統可提供區域網路必要的安全功能、可靠性效能以及功能。

  - **設定您的路由器** 請確認您的路由器已按照下面的方式設定：

    -   已在路由器上啟用 UPnP 架構。

    -   可以啟用或停用 LAN 的動態主機設定通訊協定 (DHCP) 伺服器服務。  Windows Server Essentials 可確保 DHCP 不會同時在伺服器與路由器上執行？ 當路由器上已啟用 DHCP 時，在安裝期間不會在伺服器上啟用 DHCP。

    -   您擁有由網際網路服務提供者 (ISP) 提供的路由器外部介面的 IP 位址。 IP 位址可由位在 ISP 的 DHCP 伺服器服務動態指派，否則您就需要使用路由器管理主控台手動設定靜態 IP 位址。

    -   如果您的網際網路連線要求使用者名稱與密碼 (也稱為乙太網路上的點對點通訊協定 (PPPoE))，即使裝置支援 UPnP 架構，這些設定也都會配置在您的路由器上。

    -   路由器已連線至 LAN 以及網路網路，已經開啟並且正常作用。

    如果您的路由器不支援 UPnP 架構，或者無法在安裝期間設定路由器，您就必須以您的網路設定手動配置。 請確定下列連接埠皆已開啟並被導向至目的地伺服器的 IP 位址：

  |連接埠號碼|應用程式|
  |-----------------|-----------------|
  |連接埠 80|HTTP 網路流量|
  |連接埠 443|HTTPS 網路流量|


- **閱讀 Windows Server Essentials 版本檔**。 版本檔包含正確安裝和設定 Windows Server Essentials 的最新資訊。 若要查看或列印版本檔，請參閱 [Windows Server Essentials 的版本檔](../get-started/release-notes.md)。

## <a name="additional-references"></a>其他參考資料

-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)

