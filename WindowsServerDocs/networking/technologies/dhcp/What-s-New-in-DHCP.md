---
title: DHCP 的新功能
description: 本主題提供 Windows Server 2016 中動態主機設定通訊協定（DHCP）的新功能總覽。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8032b7c8e78170d57b0367775672577d9fd900e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355447"
---
# <a name="whats-new-in-dhcp"></a>DHCP 的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的動態主機設定通訊協定（DHCP）功能。
  
DHCP 是一項網際網路工程任務推動小組（IETF）標準，其設計目的是為了降低在 TCP/IP @ no__t-0based 網路（例如私人內部網路）上設定主機的管理負擔和複雜度。 使用 DHCP 伺服器服務可以將 DHCP 用戶端設定 TCP/IP 的程序自動化。

下列各節提供有關 DHCP 的新功能和功能變更的資訊。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子網選擇選項

DHCP 現在支援選項118和 82 \(sub-option 5 @ no__t-1。 您可以使用這些選項，讓 DHCP proxy 用戶端和轉送代理程式要求特定子網的 IP 位址，以及特定的 IP 位址範圍和範圍。


如果您使用的 DHCP 轉送代理是使用 DHCP 選項82，sub @ no__t-0option 5 設定，則轉送代理程式可以從特定的 IP 位址範圍要求 DHCP 用戶端的 IP 位址租用。

如需詳細資訊，請參閱[DHCP 子網選擇選項](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>DHCP 伺服器 DNS 註冊失敗的新記錄事件

DHCP 現在包含 DNS 伺服器上 DHCP 伺服器 DNS 記錄註冊失敗的情況的記錄事件。

如需詳細資訊，請參閱[DNS 記錄註冊的 DHCP 記錄事件](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 不支援 DHCP NAP

網路存取保護 \(NAP @ no__t-1 在 Windows Server 2012 R2 中已被取代，而在 Windows Server 2016 中，DHCP 伺服器角色已不再支援 NAP。 如需詳細資訊，請參閱[Windows Server 2012 R2 中已移除或過時的功能](https://technet.microsoft.com/library/dn303411.aspx)。  
  
Windows Server 2008 的 DHCP 伺服器角色引進了 NAP 支援，windows 10 和 Windows Server 2016 之前的 Windows 用戶端和伺服器作業系統都支援此功能。 下表摘要說明 Windows Server 中 NAP 的支援。  
  
|作業系統|NAP 支援|  
|--------------------|---------------|  
| Windows Server 2008 |支援|  
| Windows Server 2008 R2 |支援|  
| Windows Server 2012 |支援|  
| Windows Server 2012 R2 |支援|  
| Windows Server 2016|不支援|  
  
在 NAP 部署中，執行作業系統以支援 NAP 的 DHCP 伺服器，可以做為 NAP DHCP 強制方法的 NAP 強制端點。 如需有關 NAP 中 DHCP 的詳細資訊，請參閱 @no__t 0Checklist：執行 DHCP 強制設計 @ no__t-0。  
  
在 Windows Server 2016 中，DHCP 伺服器不會強制執行 NAP 原則，且 DHCP 領域不可以是 NAP @ no__t-0enabled。 DHCP 用戶端電腦（也就是 NAP 用戶端）會使用 DHCP 要求傳送健全狀況聲明 \(SoH @ no__t-1。 如果 DHCP 伺服器執行的是 Windows Server 2016，則這些要求的處理方式會如同沒有 SoH 存在。 DHCP 伺服器會將一般 DHCP 租用授與用戶端。 

如果執行 Windows Server 2016 的伺服器是 RADIUS proxy，將驗證要求轉送到網路原則伺服器 \(NPS @ no__t-1 （支援 NAP），則 NPS 會將這些 NAP 用戶端評估為非 NAP @ no__t-2capable，而 NAP 處理會失敗。
  
## <a name="see-also"></a>另請參閱  
  
-   [動態主機設定通訊協定 (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

