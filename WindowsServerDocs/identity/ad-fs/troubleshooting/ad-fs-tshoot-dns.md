---
title: AD FS 疑難排解-DNS 解析
description: 本文件說明如何疑難排解 DNS 的 AD FS 的層面
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e0065feac4241b617b8b13c6867d5dc36634bd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815899"
---
# <a name="ad-fs-troubleshooting---dns"></a>AD FS 疑難排解-DNS 
若要檢查，如果 AD FS 未使用或回應，第一件事是 DNS 名稱解析。  這些是基本的測試，以判斷是否 AD FS 伺服器或 WAP 伺服器所要尋找您的網路。  內部使用者，這些測試應該解析為 AD FS 伺服器 (STS)。    對於外部使用者，這些測試應該解析為 WAP 伺服器。

這份文件的其餘部分會顯示如何使用命令列工具的一些快速的名稱解析檢查。

## <a name="ping-test"></a>Ping 測試
驗證與其他 TCP/IP 電腦的 IP 層級連線傳送網際網路控制訊息通訊協定 (ICMP) 回應要求 」 訊息。 會顯示對應的 「 回應回覆 」 訊息回條，以及下往返時間。  如需詳細資訊，請參閱 < [Ping](https://technet.microsoft.com/library/ff961503.aspx)。


>[!NOTE]
>注意，有些公司會阻擋在他們的伺服器上的此連接埠，而且您可能會收到**要求已逾時**回應。

### <a name="to-use-a-ping-test"></a>若要使用 PING 測試
1.  開啟命令提示字元
2. 輸入 PING <name of adfs server> 。 範例：PING sts.contoso.com
3. 您應該會看到來自伺服器的回覆

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
您可以用來診斷網域名稱系統 (DNS) 基礎結構的顯示資訊。  如需詳細資訊，請參閱 < [NSLookup](https://technet.microsoft.com/library/cc725991.aspx)。

### <a name="to-use-a-nslookup"></a>若要使用 NSLookup
1.  開啟命令提示字元
2. 輸入 PING <name of adfs server> 。 範例： nslookup sts.contoso.com
3. 您應該會看到伺服器的 dns 資訊![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
判斷由傳送網際網路控制訊息通訊協定 (ICMP) 回應要求，或以累加方式增加至 Live (TTL) 欄位值的時間目的地的 ICMPv6 訊息採取到目的地的路徑。   如需詳細資訊，請參閱 < [Tracert](https://technet.microsoft.com/library/ff961507.aspx)。


### <a name="to-use-tracert"></a>若要使用 Tracert
1.  開啟命令提示字元
2. 輸入 tracert <name of adfs server> 。 範例： tracert sts.contoso.com
3. 您應該會看到用來連線到伺服器的目的地路徑![Tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)