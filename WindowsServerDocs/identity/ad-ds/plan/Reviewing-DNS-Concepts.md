---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: "審查 DNS 概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>審查 DNS 概念

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網域名稱系統」(DNS) 時，表示命名空間分散式的資料庫。 命名空間包含所有所需的任何查詢任何名稱 client 的資訊。 任何 DNS 伺服器可回答關於其命名空間任何知名查詢。 DNS 伺服器解答查詢其中一項下列方法：  
  
-   快取中解答時，它會解答查詢從快取。  
  
-   如果解答裝載的 DNS 伺服器區中，它會解答查詢從它的區域。 區域是 DNS 樹 DNS 伺服器上儲存的部分。 時 DNS 伺服器裝載區域，就授權的區域中的名稱（也就是 DNS 伺服器可以回應查詢區域中的任何名稱）。 例如裝載區域 contoso.com 伺服器可回答查詢 contoso.com 中的任何名稱。  
  
-   如果伺服器無法回應查詢其快取或區域，它會查詢其他伺服器的解答。  
  
請務必以了解核心的功能 DNS，例如委派、遞迴名稱解析度和 Active Directory 整合 DNS 區域，因為它們影響直接在您的 Active Directory 邏輯結構設計。  
  
如需有關 DNS Active Directory Domain Services (AD DS)，請查看[DNS 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)。  
  
## <a name="delegation"></a>委派  
回答關於任何名稱查詢的 DNS 伺服器，您必須先直接或間接路徑每命名空間中的時區。 利用委派所建立這些路徑。 委派是會列出名稱伺服器區中的下一步層級階層的授權家長區域中記錄。 委派讓參考其他區域中的伺服器戶端一個區域中的伺服器。 下圖顯示委派的一個例子。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
根 DNS 伺服器主控表示為點根區域 (。 ). 根區域包含區域中階層、com 區域的下一步層級的委派。 委派根區域中的會顯示根 DNS 伺服器，若要尋找 com 區域，就必須連絡 Com 伺服器。 同樣地，委派 com 區域中的會顯示 Com 伺服器，尋找 contoso.com 區域，就必須連絡 Contoso 伺服器。  
  
> [!NOTE]  
> 委派使用兩種類型的記錄。 名稱（奈秒）伺服器資源記錄提供授權伺服器的名稱。 主機 (A) 和主機 (AAAA) 資源記錄提供 IP 版本 4 (IPv4) 與 IP 6 授權伺服器 (IPv6) 位址。  
  
這個區域和委派系統建立階層樹代表 DNS 命名空間。 每個區域表示階層、中的層級，每個委派表示樹的分支。  
  
使用的區域和委派階層、DNS 伺服器根可以找到 DNS 命名空間任何名稱。 根區域包含委派導致直接或間接其他階層中的所有區域。 可以查詢根 DNS 伺服器的任何伺服器可以使用委派中的資訊，來命名空間中尋找任何名稱。  
  
## <a name="recursive-name-resolution"></a>遞迴名稱解析  
遞迴名稱解析為程序的 DNS 伺服器使用的區域和委派階層回應查詢的不是授權。  
  
在某些設定、DNS 伺服器包含根提示（也就是一份名稱與 IP 位址），讓它們查詢根 DNS 伺服器。 在其他設定，他們無法回應到另一個伺服器的所有查詢都轉寄伺服器。 轉寄和根提示有兩種方法可以使用解析的查詢，它們不會 DNS 伺服器。  
  
### <a name="resolving-names-by-using-root-hints"></a>使用根提示解析名稱  
根提示可讓任何 DNS 伺服器找出根 DNS 伺服器。 DNS 伺服器找出 DNS 伺服器根之後，它可以解析任何查詢命名空間。 下圖告訴您如何 DNS 名稱解析使用根提示。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
在此範例中，進行下列事件：  
  
1.  Client 傳送遞迴查詢 DNS 伺服器，以要求名稱 ftp.contoso.com 相對應的 IP 位址。 遞迴查詢指示 client 想其查詢明確的解答。 回應遞迴查詢必須有效的地址或訊息，指出找不到的位址。  
  
2.  因為 DNS 伺服器不適用於名稱，在該快取不需要解答的 DNS 伺服器使用根提示尋找根 DNS 伺服器的 IP 位址。  
  
3.  DNS 伺服器使用反覆查詢要求 DNS 根伺服器解析 ftp.contoso.com 的名稱。 反覆查詢指示伺服器會接收到另一個的伺服器來取代查詢明確回答推薦。 因為名稱 ftp.contoso.com 結束標籤與 com，根 DNS 伺服器傳回推薦 Com 伺服器裝載 com 區域。  
  
4.  DNS 伺服器使用反覆查詢要求 Com 伺服器解析 ftp.contoso.com 的名稱。 因為名稱 ftp.contoso.com 結束 contoso.com 名稱、Com 伺服器傳回推薦 Contoso 伺服器該主機 contoso.com 區域。  
  
5.  DNS 伺服器使用反覆查詢要求 Contoso 伺服器解析 ftp.contoso.com 的名稱。 Contoso 伺服器其時區資料中找到答案，然後傳回伺服器的解答。  
  
6.  伺服器然後傳回至 client 的結果。  
  
### <a name="resolving-names-by-using-forwarding"></a>使用轉接解析名稱  
轉送可讓您透過特定的伺服器，而不是使用根提示路由名稱解析。 下圖告訴您如何 DNS 名稱解析使用轉接。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
在此範例中，進行下列事件：  
  
1.  Client 查詢名稱 ftp.contoso.com DNS 伺服器。  
  
2.  DNS 伺服器將其他 DNS 伺服器，稱為 [轉寄查詢。  
  
3.  轉寄名稱的授權並不在該快取不需要回應，因為它會使用根提示尋找根 DNS 伺服器的 IP 位址。  
  
4.  轉寄使用反覆查詢要求 DNS 根伺服器解析 ftp.contoso.com 的名稱。 因為名稱 ftp.contoso.com 結束名稱 com，根 DNS 伺服器傳回推薦 Com 伺服器裝載 com 區域。  
  
5.  轉寄使用反覆查詢要求 Com 伺服器解析 ftp.contoso.com 的名稱。 因為名稱 ftp.contoso.com 結束 contoso.com 名稱、Com 伺服器傳回推薦 Contoso 伺服器該主機 contoso.com 區域。  
  
6.  轉寄使用反覆查詢要求 Contoso 伺服器解析 ftp.contoso.com 的名稱。 Contoso 伺服器區檔案中, 尋找解答，然後傳回伺服器的答案。  
  
7.  然後轉寄會傳回結果原始的 DNS 伺服器。  
  
8.  原始 DNS 伺服器然後傳回 client 結果。  
  


