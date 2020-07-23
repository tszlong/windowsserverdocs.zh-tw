---
title: AD FS 疑難排解-DNS 解析
description: 本檔說明如何針對 AD FS 的 DNS 層面進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a9be4a72cd60cfdd5807c67132dba837093be4db
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959020"
---
# <a name="ad-fs-troubleshooting---dns"></a>AD FS 疑難排解-DNS 
要檢查的第一件事是，如果 AD FS 沒有作用或回應，則是 DNS 名稱解析。  這些是基本測試，可判斷是否在您的網路上找到 AD FS 伺服器或 WAP 伺服器。  對於內部使用者，這些測試應該會解析為 AD FS 伺服器（STS）。    對於外部使用者，這些測試應解析為 WAP 伺服器。

本檔的其餘部分將示範如何使用命令列工具進行快速名稱解析檢查。

## <a name="ping-test"></a>Ping 測試
藉由傳送網際網路控制訊息通訊協定（ICMP） Echo 要求訊息，驗證另一個 TCP/IP 電腦的 IP 層級連線能力。 會顯示對應回應回復訊息的回條，以及來回行程時間。  如需詳細資訊，請參閱[Ping](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff961503(v=ws.11))。


>[!NOTE]
>請注意，有些組織會在其伺服器上封鎖此埠，而您可能會收到**要求超時**回應。

### <a name="to-use-a-ping-test"></a>若要使用 PING 測試
1.  開啟命令提示字元
2. 輸入 PING <name of adfs server> a。 範例： PING sts.contoso.com
3. 您應該會看到來自伺服器的回復

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
顯示您可以用來診斷網域名稱系統（DNS）基礎結構的資訊。  如需詳細資訊，請參閱[NSLookup](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc725991(v=ws.11))。

### <a name="to-use-a-nslookup"></a>使用 NSLookup
1.  開啟命令提示字元
2. 輸入 PING <name of adfs server> a。 範例： nslookup sts.contoso.com
3. 您應該會看到伺服器 NSLookup 的 dns 資訊 ![](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
藉由將網際網路控制訊息通訊協定（ICMP） Echo 要求或 ICMPv6 訊息傳送至目的地，並以累加方式增加存留時間（TTL）域值，來決定目的地所採用的路徑。   如需詳細資訊，請參閱[Tracert](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff961507(v=ws.11))。


### <a name="to-use-tracert"></a>若要使用 Tracert
1.  開啟命令提示字元
2. 輸入 tracert <name of adfs server> a。 範例： tracert sts.contoso.com
3. 您應該會看到用來存取伺服器 Tracert 的目的地路徑 ![](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
