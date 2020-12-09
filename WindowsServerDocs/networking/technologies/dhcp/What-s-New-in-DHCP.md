---
title: DHCP 的新功能
description: 本主題概要說明 Windows Server 2016 中的動態主機設定通訊協定 (DHCP) 的新功能。
manager: brianlic
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5dee105eaf14c92145e1fe70fe4627d37b2baa82
ms.sourcegitcommit: b0c73df80d7b4ff0c332d77e0cc07f7e6e061600
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2020
ms.locfileid: "96925560"
---
# <a name="whats-new-in-dhcp"></a>DHCP 的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的動態主機設定通訊協定 (DHCP) 功能。

DHCP 是 (IETF) 標準的網際網路工程任務推動小組，其設計目的是要降低在 TCP/IP \- 網路（例如私人內部網路）上設定主機的管理負擔和複雜度。 使用 DHCP 伺服器服務可以將 DHCP 用戶端設定 TCP/IP 的程序自動化。

下列各節提供 DHCP 功能的新功能和變更的相關資訊。

## <a name="new-dhcp-client-side-features-in-the-windows-10-may-2020-update"></a>Windows 10 中的新 DHCP 用戶端功能可能是2020更新

Windows 10 中的 DHCP 用戶端已于10月2020更新中更新 (也稱為 Windows 10 2004 版) 。 當您執行 Windows 用戶端，並透過行動網卡 Android 手機連接到網際網路時，連線應該會標示為「計量付費」。 先前，連接已標示為不按方式。 請注意，並非所有 Android 行動網卡電話都將偵測為計量付費，而某些其他網路也可能會顯示為計量付費。

此外，某些 Windows 裝置已更新傳統用戶端廠商名稱。 此值只是 MSFT 5.0。 某些裝置現在會顯示為 MSFT 5.0 XBOX。

## <a name="new-dhcp-client-side-features-in-the-windows-10-april-2018-update"></a>Windows 10 2018 年4月更新中的新 DHCP 用戶端功能

Windows 10 中的 DHCP 用戶端已在 Windows 2018 年4月更新中更新 (也稱為 Windows 10 版本 1803) ，可從您的系統所連接的 DHCP 伺服器中讀取並套用選項119、網域搜尋選項。 網域搜尋選項提供 dns 的 DNS 尾碼來查閱簡短名稱。 在 [RFC 3397](https://tools.ietf.org/html/rfc3397)中指定 DHCP 選項119。

## <a name="dhcp-subnet-selection-options"></a>DHCP 子網選擇選項

DHCP 現在支援選項 82 \( 子選項 5 \) 。 您可以使用此選項來允許 DHCP proxy 用戶端和轉送代理程式要求特定子網的 IP 位址。


如果您使用以 DHCP 選項82（子選項5）設定的 DHCP 轉接代理， \- 則轉送代理程式可以向特定 ip 位址範圍要求 DHCP 用戶端的 IP 位址租用。

如需詳細資訊，請參閱 [DHCP 子網選擇選項](dhcp-subnet-options.md)。

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>DHCP 伺服器 DNS 註冊失敗的新記錄事件

在 DNS 伺服器上 DHCP 伺服器 DNS 記錄註冊失敗的情況下，DHCP 現在包含記錄事件。

如需詳細資訊，請參閱 [DNS 記錄註冊的 DHCP 記錄事件](dhcp-dns-events.md)。

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Windows Server 2016 不支援 DHCP NAP

網路存取保護 \( NAP \) 在 windows Server 2012 R2 中已被取代，而在 windows server 2016 中，DHCP 伺服器角色已不再支援 NAP。 如需詳細資訊，請參閱 [Windows Server 2012 R2 中已移除或過時的功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11))。

NAP 支援是在 Windows Server 2008 的 DHCP 伺服器角色中引進的，在 Windows 10 和 Windows Server 2016 之前，Windows 用戶端和伺服器作業系統都支援此功能。 下表摘要說明 Windows Server 中 NAP 的支援。

|作業系統|NAP 支援|
|--------------------|---------------|
| Windows Server 2008 |支援|
| Windows Server 2008 R2 |支援|
| Windows Server 2012 |支援|
| Windows Server 2012 R2 |支援|
| Windows Server 2016|不支援|

在 NAP 部署中，執行支援 NAP 之作業系統的 DHCP 伺服器，可以作為 NAP DHCP 強制方法的 NAP 強制端點。 如需 NAP 中 DHCP 的詳細資訊，請參閱 [檢查清單：實施 Dhcp 強制設計](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd314186(v=ws.10))。

在 Windows Server 2016 中，DHCP 伺服器不會強制 NAP 原則，且無法啟用 NAP 的 DHCP 領域 \- 。 同時也是 NAP 用戶端的 DHCP 用戶端電腦會傳送健全狀況 \( SoH 聲明 \) 與 DHCP 要求。 如果 DHCP 伺服器執行的是 Windows Server 2016，這些要求的處理方式就如同沒有 SoH 存在一樣。 DHCP 伺服器會將一般 DHCP 租用授與用戶端。

如果執行 Windows Server 2016 的伺服器是 RADIUS proxy，將驗證要求轉寄至支援 NAP 的網路原則伺服器 \( NPS \) ，則這些 NAP 用戶端會被 NPS 評估為不 \- 具備 nap 功能，而 NAP 處理會失敗。

## <a name="additional-references"></a>其他參考資料

-   [動態主機設定通訊協定 (DHCP)](./dhcp-top.md)
