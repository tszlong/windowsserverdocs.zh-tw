---
title: DHCP 的新功能
description: 本主題概要說明 Windows Server 2016 中的動態主機設定通訊協定 (DHCP) 的新功能。
manager: brianlic
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9b0b23b6058655baf599ed66877228b39eb807f9
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993929"
---
# <a name="whats-new-in-dhcp"></a>DHCP 的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更 (DHCP) 功能的動態主機設定通訊協定。

DHCP 是一項網際網路工程任務推動小組 (IETF) standard，其設計目的是降低在 TCP/IP \- 網路（例如私人內部網路）上設定主機的管理負擔和複雜度。 使用 DHCP 伺服器服務可以將 DHCP 用戶端設定 TCP/IP 的程序自動化。

下列各節提供有關 DHCP 的新功能和功能變更的資訊。

## <a name="new-dhcp-client-side-features-in-the-windows-10-may-2020-update"></a>Windows 10 中的新 DHCP 用戶端功能5月2020更新

Windows 10 中的 DHCP 用戶端已在10月2020更新中更新 (也稱為 Windows 10 2004 版) 。 當您執行 Windows 用戶端並透過行動網卡 Android 手機連線到網際網路時，應該將連線標示為「計量付費」。 先前，連線已標示為不按過的順序。 請注意，並非所有 Android 行動網卡電話都會以計量方式偵測到，而其他網路也可能會顯示為計量付費。

此外，某些 Windows 裝置的傳統用戶端廠商名稱已經過更新。 這個值只是 MSFT 5.0。 有些裝置現在會顯示為 MSFT 5.0 XBOX。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子網選擇選項

DHCP 現在支援選項 82 \( 子選項 5 \) 。 您可以使用此選項，讓 DHCP proxy 用戶端和轉送代理程式要求特定子網的 IP 位址。


如果您使用的 DHCP 轉送代理已設定 DHCP 選項82，子 \- 選項5，則轉送代理程式可以從特定的 ip 位址範圍要求 DHCP 用戶端的 ip 位址租用。

如需詳細資訊，請參閱[DHCP 子網選擇選項](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>DHCP 伺服器 DNS 註冊失敗的新記錄事件

DHCP 現在包含 DNS 伺服器上 DHCP 伺服器 DNS 記錄註冊失敗的情況的記錄事件。

如需詳細資訊，請參閱[DNS 記錄註冊的 DHCP 記錄事件](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 不支援 DHCP NAP

網路存取保護 \( NAP \) 在 windows Server 2012 R2 中已被取代，而在 windows server 2016 中，DHCP 伺服器角色已不再支援 nap。 如需詳細資訊，請參閱[Windows Server 2012 R2 中已移除或過時的功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11))。

Windows Server 2008 的 DHCP 伺服器角色引進了 NAP 支援，windows 10 和 Windows Server 2016 之前的 Windows 用戶端和伺服器作業系統都支援此功能。 下表摘要說明 Windows Server 中 NAP 的支援。

|作業系統|NAP 支援|
|--------------------|---------------|
| Windows Server 2008 |支援|
| Windows Server 2008 R2 |支援|
| Windows Server 2012 |支援|
| Windows Server 2012 R2 |支援|
| Windows Server 2016|不支援|

在 NAP 部署中，執行作業系統以支援 NAP 的 DHCP 伺服器，可以做為 NAP DHCP 強制方法的 NAP 強制端點。 如需 NAP 中 DHCP 的詳細資訊，請參閱[檢查清單：實施 Dhcp 強制設計](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd314186(v=ws.10))。

在 Windows Server 2016 中，DHCP 伺服器不會強制執行 NAP 原則，DHCP 領域也不能 \- 啟用 nap。 DHCP 用戶端電腦，也就是 NAP 用戶端，會 \( 使用 dhcp 要求傳送健康情況 SoH 的聲明 \) 。 如果 DHCP 伺服器執行的是 Windows Server 2016，則這些要求的處理方式會如同沒有 SoH 存在。 DHCP 伺服器會將一般 DHCP 租用授與用戶端。

如果執行 Windows Server 2016 的伺服器是 RADIUS proxy，將驗證要求轉送到支援 NAP 的網路原則伺服器 \( NPS \) ，這些 NAP 用戶端會由 NPS 評估為不支援 nap \- ，而 nap 處理會失敗。

## <a name="additional-references"></a>其他參考資料

-   [動態主機設定通訊協定 (DHCP)](./dhcp-top.md)