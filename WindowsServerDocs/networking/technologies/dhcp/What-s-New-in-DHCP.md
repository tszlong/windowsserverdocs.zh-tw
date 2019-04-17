---
title: DHCP 中的新功能
description: 本主題提供的動態主機設定通訊協定 (DHCP) 在 Windows Server 2016 中的新功能的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f67e5dfd8aefcf408a41f6fc919665419f0ff1c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dhcp"></a>DHCP 中的新功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題描述新增或變更 Windows Server 2016 中的 「 動態主機設定通訊協定 」 (DHCP) 功能。
  
DHCP 是設計用來減少的系統管理負擔和複雜的 TCP 日 IP\ 型網路，例如私人內部網路設定主機網際網路工程設計工作推動 (IETF) 標準。 使用 DHCP 伺服器服務，程序的 TCP/IP 設定戶端 DHCP 會自動執行。

下列章節 DHCP 功能提供新功能和變更的相關資訊。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子網路選取選項

DHCP 現在支援 \(sub-option 5\) 118 與 82 選項。 您可以使用這些選項來讓您要求的 IP 位址特定子網路，以及從指定 IP 位址和範圍 DHCP proxy 戶端及轉送代理程式。

如果您使用 DHCP 選項 118，例如執行 Windows Server 2016 和遠端存取伺服器角色 virtual 私人網路 \(VPN\) 伺服器設定 DHCP proxy client VPN 伺服器可以要求 IP 位址租用 VPN 戶端的特定的 IP 位址。

如果您使用 DHCP 轉送代理 DHCP 選項 82，sub\ 選項 5 設定轉接可以要求 IP 位址租用 DHCP 戶端的特定的 IP 位址。

如需詳細資訊，請查看[選擇 DHCP 子網路的選項](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>新的登入事件 DHCP 伺服器 DNS 登記失敗

DHCP 現在包含登入的環境中 DHCP 伺服器 DNS 使用碼表進行登錄失敗 DNS 伺服器的活動。

如需詳細資訊，請查看[DHCP 登入事件 DNS 記錄登錄的](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 不支援 DHCP NAP

網路存取保護 \(NAP\) 已在 Windows Server 2012 R2 取代和 Windows Server 2016 中 DHCP 伺服器角色不再支援 NAP。 如需詳細資訊，請查看[的功能已經移除或 Deprecated Windows Server 2012 R2 在](https://technet.microsoft.com/library/dn303411.aspx)。  
  
NAP 支援 DHCP 伺服器角色與 Windows Server 2008、 推出和支援 Windows client 和伺服器作業系統之前的 Windows 10 與 Windows Server 2016。 下表摘要 NAP 在 Windows Server 的支援。  
  
|作業系統|NAP 支援|  
|--------------------|---------------|  
| Windows Server 2008 |支援|  
| Windows Server 2008 R2 |支援|  
| Windows Server 2012 |支援|  
| Windows Server 2012 R2 |支援|  
| Windows Server 2016|不支援|  
  
NAP 部署，在執行作業系統，其支援 NAP DHCP 伺服器可以作為 NAP DHCP 執法方法 NAP 執法點。 如需 DHCP NAP 中相關資訊，請查看[檢查清單︰ 實作 DHCP 執法設計](https://technet.microsoft.com/library/dd314186.aspx)。  
  
在 Windows Server 2016 DHCP 伺服器不會執行 NAP 原則，且 DHCP 領域無法 NAP\ 功能。 這也是 NAP 戶端 DHCP client 電腦傳送健康 \(SoH\) DHCP 要求的聲明。 如果執行 Windows Server 2016 的 DHCP 伺服器，如同任何 SoH 存在於處理這些要求。 DHCP 伺服器授與一般 DHCP 租用 client。 

如果執行 Windows Server 2016 的伺服器，轉送給支援 NAP 網路原則伺服器 \(NPS\) 驗證要求 RADIUS proxy，這些 NAP 戶端會評估 NPS 非 NAP\ 處理能力，和 NAP 處理失敗。
  
## <a name="see-also"></a>也了  
  
-   [動態主機設定通訊協定」(DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

