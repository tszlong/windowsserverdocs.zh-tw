---
title: DHCP 的新功能
description: 本主題提供的動態主機設定通訊協定 (DHCP) 在 Windows Server 2016 的新功能的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73cc5134f7af5063c912ad578fa7d660b3194aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840229"
---
# <a name="whats-new-in-dhcp"></a>DHCP 的新功能

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的動態主機設定通訊協定 (DHCP) 功能。
  
DHCP 是一種 Internet Engineering Task Force (IETF) 標準，旨在減輕管理負擔及複雜度上的 TCP/IP 設定主機\-架構的網路，如私人內部網路。 使用 DHCP 伺服器服務可以將 DHCP 用戶端設定 TCP/IP 的程序自動化。

下列各節會提供功能中 DHCP 的新功能和變更的相關資訊。

## <a name="dhcp-subnet-selection-options"></a>子網路選取範圍的 DHCP 選項

DHCP 現在支援選項 118 和 82\(子選項 5\)。 您可以使用這些選項，以允許 DHCP proxy 用戶端和轉接代理來要求特定的子網路，並從特定的 IP 位址範圍和範圍的 IP 位址。


如果您使用的設定為 DHCP 選項 82 DHCP 轉接代理，子\-選項 5，轉接代理程式可以要求來自特定 IP 位址範圍的 DHCP 用戶端的 IP 位址租用。

如需詳細資訊，請參閱 <<c0> [ 子網路選取範圍的 DHCP 選項](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>新的記錄事件，可由 DHCP 伺服器的 DNS 註冊失敗

DHCP 現在包含伺服器 DNS 記錄登錄失敗的 DNS 伺服器的 dhcp 的情況下的記錄事件。

如需詳細資訊，請參閱 < [DHCP 記錄事件的 DNS 記錄登錄](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 中不支援 DHCP 的 NAP

網路存取保護\(NAP\) Windows Server 2012 r2 中已被取代，並在 Windows Server 2016 中的 DHCP 伺服器角色已不再支援 NAP。 如需詳細資訊，請參閱 < [Features Removed or Deprecated in Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx)。  
  
NAP 支援 DHCP 伺服器角色與 Windows Server 2008 中引進，而且支援 Windows 用戶端和伺服器作業系統中 Windows 10 和 Windows Server 2016 之前。 下表摘要說明在 Windows Server 中的 nap 的支援。  
  
|作業系統|NAP 支援|  
|--------------------|---------------|  
| Windows Server 2008 |支援|  
| Windows Server 2008 R2 |支援|  
| Windows Server 2012 |支援|  
| Windows Server 2012 R2 |支援|  
| Windows Server 2016|不支援|  
  
在 NAP 部署中，執行之作業系統的支援 NAP 的 DHCP 伺服器可以做為 NAP DHCP 強制方法的 NAP 強制端點。 如需有關 DHCP 的 NAP 的詳細資訊，請參閱[檢查清單：實作 DHCP 強制設計](https://technet.microsoft.com/library/dd314186.aspx)。  
  
在 Windows Server 2016 中，DHCP 伺服器不會強制執行 NAP 原則，DHCP 領域不能是 NAP\-啟用。 也是 NAP 用戶端的 DHCP 用戶端電腦傳送健康狀態聲明\(SoH\)與 DHCP 要求。 如果 DHCP 伺服器執行 Windows Server 2016，如同任何 SoH 存在於處理這些要求。 DHCP 伺服器中，會授與一般的 DHCP 租用給用戶端。 

如果執行 Windows Server 2016 的伺服器會將驗證要求轉送到網路原則伺服器的 RADIUS proxy \(NPS\)支援 NAP，這些 NAP 用戶端會評估為非 NAP 的 nps\-能力，以及處理失敗的 NAP。
  
## <a name="see-also"></a>另請參閱  
  
-   [動態主機設定通訊協定 (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

