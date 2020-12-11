---
description: 深入瞭解：同盟伺服器 proxy 的名稱解析需求
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: 同盟伺服器 Proxy 的名稱解析需求
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ed871b8fb0f904fee246714a852ab5934c4a6e1c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046526"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>同盟伺服器 Proxy 的名稱解析需求

當網際網路上的用戶端電腦嘗試存取 Active Directory 同盟服務 AD FS 所保護的應用程式 \( 時 \) ，他們必須先向同盟伺服器進行驗證。 在大多數情況下，同盟伺服器通常無法直接從網際網路存取。 因此，網際網路用戶端電腦必須改為重新導向至同盟伺服器 proxy。 您可以將適當的網域名稱系統 \( DNS \) 記錄新增到您的 DNS 區域或面對網際網路的區域，以完成重新導向的成功。

您用來將網際網路用戶端重新導向至同盟伺服器 proxy 的方法，取決於您如何在周邊網路中設定 DNS 區域，或是如何設定您在網際網路上控制的 DNS 區域。 同盟伺服器 proxy 是用於周邊網路。 只有在您所控制的所有網際網路面向區域中正確設定 DNS 時，才會成功將網際網路用戶端要求重新導向至同盟伺服器 \- 。 因此，您的網際網路對向 \- 區域的設定—無論您的 dns 區域是否只提供周邊網路，或是同時提供周邊網路和網際網路用戶端的 dns 區域，都很重要。

本主題說明當您在周邊網路中放置同盟伺服器 proxy 時，可採取的步驟來設定名稱解析。 若要判斷應遵循哪些步驟，請先判斷下列哪一個 DNS 案例最符合您組織的周邊網路中的 DNS 基礎結構。 然後，遵循該案例中的步驟。

## <a name="dns-zone-serving-only-the-perimeter-network"></a>僅對周邊網路提供服務的 DNS 區域
在此案例中，您的組織在周邊網路中有一或兩個 DNS 區域，且您的組織不會控制在網際網路上的任何 DNS 區域。 在僅提供周邊網路案例的 DNS 區域中，同盟伺服器 proxy 的成功名稱解析，取決於下列條件：

-   同盟伺服器 proxy 必須有主機檔案中的設定，才能將 \( 同盟伺服器端點 URL 的完整功能變數名稱 FQDN 解析 \) 為同盟伺服器或同盟伺服器叢集的 IP 位址。

-   必須設定帳戶夥伴周邊網路中的 DNS，以便將同盟伺服器端點 URL 的 FQDN 解析為同盟伺服器 proxy 的 IP 位址。

下圖和相對應的步驟顯示如何針對指定的範例達成每一個條件。 在此圖中，Microsoft 網路負載平衡 \( NLB \) 技術為現有的同盟伺服器陣列提供單一叢集 FQDN 和單一叢集 IP 位址。

![名稱需求](media/adfs2_deploy_single_fs.gif)

如需使用 NLB 設定叢集 IP 位址或叢集 FQDN 的詳細資訊，請參閱 [指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。

### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.在同盟伺服器 Proxy 上設定主機檔案。
因為周邊網路中的 DNS 是設定來解析 fs.fabrikam.com 至帳戶同盟伺服器 proxy 的所有要求，所以帳戶夥伴同盟伺服器 proxy 在其本機 hosts 檔案中有一個專案，可將 fs.fabrikam.com 解析為 \( \) 連接到公司網路之同盟伺服器陣列的實際帳戶同盟伺服器或叢集 DNS 名稱的 IP 位址。 如此一來，帳戶同盟伺服器 proxy 就可以將主機名稱 fs.fabrikam.com 解析為帳戶同盟伺服器，而不是本身，就像是嘗試使用周邊 DNS 查閱 fs.fabrikam.com 一樣，如此一來，同盟伺服器 proxy 就可以與同盟伺服器通訊。

### <a name="2-configure-perimeter-dns"></a>2.設定周邊 DNS
由於用戶端電腦只有一部 AD FS 主機名稱，不論它們是在內部網路或網際網路上—使用周邊 DNS 伺服器的網際網路上的用戶端電腦，都必須將帳戶同盟伺服器 fs.fabrikam.com 的 FQDN 解析 \( \) 為周邊網路上帳戶同盟伺服器 PROXY 的 IP 位址。 如此一來，當用戶端嘗試解析 fs.fabrikam.com 時，就可以將用戶端轉寄到帳戶同盟伺服器 proxy，而周邊 DNS 則包含受限的 corp.fabrikam.com DNS 區域，其中有一個 \( \) 適用于 fs fs.fabrikam.com 的資源記錄 \( \) 和周邊網路上的帳戶同盟伺服器 proxy 的 IP 位址。

如需有關如何修改同盟伺服器 proxy 的 hosts 檔案，以及在周邊網路中設定 DNS 的詳細資訊，請參閱 [在僅提供周邊網路的 Dns 區域中設定同盟伺服器 proxy 的名稱解析](../deployment/configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md)。

## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>對周邊網路和網際網路用戶端提供服務的 DNS 區域
在此案例中，您的組織控制周邊網路中的 DNS 區域，和至少一個網際網路上的 DNS 區域。 在此案例中，同盟伺服器 proxy 的成功名稱解析取決於下列條件：

-   帳戶夥伴的網際網路區域中必須設定 DNS，以便將同盟伺服器主機名稱的 FQDN 解析為周邊網路中同盟伺服器 proxy 的 IP 位址。

-   您必須設定帳戶夥伴周邊網路中的 DNS，以便將同盟伺服器主機名稱的 FQDN 解析為公司網路中同盟伺服器的 IP 位址。

下圖和相對應的步驟顯示如何針對指定的範例達成每一個條件。

![名稱需求](media/adfs2_deploy_fsp_3DNS.gif)

### <a name="1-configure-perimeter-dns"></a>1.設定周邊 DNS
針對此案例，因為假設您將設定網際網路 DNS 區域，以針對特定端點 URL （也 \( 就是 fs.fabrikam.com \) 至周邊網路中的同盟伺服器 proxy）所提出的要求進行解析，您也必須設定周邊 DNS 中的區域，將這些要求轉送到公司網路中的同盟伺服器。

如此一來，當用戶端嘗試解析 fs.fabrikam.com 時，就可以將用戶端轉送到帳戶同盟伺服器，周邊 DNS 則是以單一主機 \( a a a a a a a a a a a a a a a a a a a a a a a a a a a a a a a a \) \( \) 如此一來，帳戶同盟伺服器 proxy 就可以將主機名稱 fs.fabrikam.com 解析為帳戶同盟伺服器，而不是本身，就像是嘗試使用網際網路 DNS 來查閱 fs.fabrikam.com 一樣，如此一來，同盟伺服器 proxy 才能與同盟伺服器通訊。

### <a name="2-configure-internet-dns"></a>2.設定網際網路 DNS
若要讓此案例的名稱解析成功，來自網際網路上用戶端電腦至 fs.fabrikam.com 的所有要求，必須透過您控制的網際網路 DNS 區域進行解析。 因此，您必須設定網際網路 DNS 區域，將 fs.fabrikam.com 的用戶端要求轉寄至周邊網路中帳戶同盟伺服器 proxy 的 IP 位址。

如需有關如何修改周邊網路和網際網路 DNS 區域的詳細資訊，請參閱 [在同時提供周邊網路和網際網路用戶端的 DNS 區域中，設定同盟伺服器 Proxy 的名稱解析](../deployment/configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
