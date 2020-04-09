---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: 檢閱 DNS 概念
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 37c33ca181394c66ef149715c3f1477774061660
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822061"
---
# <a name="reviewing-dns-concepts"></a>檢閱 DNS 概念

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網域名稱系統（DNS）是代表命名空間的分散式資料庫。 命名空間包含任何用戶端查閱任何名稱所需的所有資訊。 任何 DNS 伺服器都可以回應其命名空間內任何名稱的查詢。 DNS 伺服器會透過下列其中一種方式來回答查詢：  
  
- 如果答案是在其快取中，則會從快取回應查詢。  
- 如果答案是在 DNS 伺服器裝載的區域中，則會從其區域回答查詢。 「區域」（zone）是儲存在 DNS 伺服器上的 DNS 樹狀結構的一部分。 當 DNS 伺服器裝載區域時，它會受到該區域中的名稱的授權（也就是，DNS 伺服器可以回答區域中任何名稱的查詢）。 例如，裝載區域 contoso.com 的伺服器可以回答 contoso.com 中任何名稱的查詢。  
- 如果伺服器無法接聽其快取或區域的查詢，它會查詢其他伺服器以取得答案。  

請務必瞭解 DNS 的核心功能，例如委派、遞迴名稱解析，以及 Active Directory 整合的 DNS 區域，因為它們會直接影響您的 Active Directory 邏輯結構設計。  
  
如需 DNS 和 Active Directory Domain Services （AD DS）的詳細資訊，請參閱[dns 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)。  
  
## <a name="delegation"></a>委派

若要讓 DNS 伺服器回答有關任何名稱的查詢，它必須具有命名空間中每個區域的直接或間接路徑。 這些路徑是透過委派來建立的。 委派是父區域中的一筆記錄，其中列出階層的下一個層級中區域的授權名稱伺服器。 委派可讓一個區域中的伺服器將用戶端參考到其他區域中的伺服器。 下圖顯示委派的其中一個範例。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
DNS 根功能變數名稱伺服器裝載以點（. 表示的根區域）。 )。 根區域包含階層的下一個層級（com 區域）中區域的委派。 根區域中的委派會告訴 DNS 根功能變數名稱伺服器，若要尋找 com 區域，它必須與 Com 伺服器聯繫。 同樣地，com 區域中的委派會告訴 Com 伺服器，若要尋找 contoso.com 區域，它必須與 Contoso 伺服器聯繫。  
  
> [!NOTE]  
> 委派會使用兩種類型的記錄。 名稱伺服器（NS）資源記錄會提供授權伺服器的名稱。 主機（A）和主機（AAAA）資源記錄提供授權伺服器的 IP 第4版（IPv4）和 IP 第6版（IPv6）位址。  
  
這個區域和委派系統會建立一個代表 DNS 命名空間的階層式樹狀結構。 每個區域都代表階層中的一個圖層，而每個委派代表一個樹狀結構的分支。  
  
藉由使用區域和委派的階層，DNS 根功能變數名稱伺服器可以在 DNS 命名空間中找到任何名稱。 根區域包含直接或間接導向階層中所有其他區域的委派。 任何可以查詢 DNS 根功能變數名稱伺服器的伺服器都可以使用委派中的資訊來尋找命名空間中的任何名稱。  
  
## <a name="recursive-name-resolution"></a>遞迴名稱解析

遞迴名稱解析是一種程式，DNS 伺服器會使用區域和委派的階層來回應其未授權的查詢。  
  
在某些設定中，DNS 伺服器會包含根提示（也就是名稱和 IP 位址的清單），讓它們能夠查詢 DNS 根功能變數名稱伺服器。 在其他設定中，伺服器會將其無法回答的所有查詢轉送至另一部伺服器。 轉送和根提示是 DNS 伺服器可用來解析不具授權之查詢的兩種方法。  
  
### <a name="resolving-names-by-using-root-hints"></a>使用根提示解析名稱

根提示可讓任何 DNS 伺服器找出 DNS 根功能變數名稱伺服器。 DNS 伺服器找到 DNS 根功能變數名稱伺服器之後，就可以解析該命名空間的任何查詢。 下圖說明 DNS 如何使用根提示來解析名稱。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
在此範例中，會發生下列事件：  
  
1. 用戶端會將遞迴查詢傳送至 DNS 伺服器，以要求對應至名稱 ftp.contoso.com 的 IP 位址。 遞迴查詢表示用戶端想要有其查詢的確切解答。 遞迴查詢的回應必須是有效的位址或訊息，指出找不到該位址。  
2. 因為 DNS 伺服器不是名稱的授權，而且在其快取中沒有回應，所以 DNS 伺服器會使用根提示來尋找 DNS 根功能變數名稱伺服器的 IP 位址。  
3. DNS 伺服器會使用反復查詢來要求 DNS 根功能變數名稱伺服器解析名稱 ftp.contoso.com。 反復查詢表示伺服器將接受另一部伺服器的參照，以取代查詢的確切答案。 因為名稱 ftp.contoso.com 的結尾是標籤 com，所以 DNS 根功能變數名稱伺服器會將參考傳回給裝載 com 區域的 Com 伺服器。  
4. DNS 伺服器會使用反復查詢來要求 Com 伺服器解析名稱 ftp.contoso.com。 因為名稱 ftp.contoso.com 的結尾名稱為 contoso.com，所以 Com 伺服器會將參考傳回給裝載 contoso.com 區域的 Contoso 伺服器。  
5. DNS 伺服器會使用反復查詢來要求 Contoso 伺服器解析名稱 ftp.contoso.com。 Contoso 伺服器會在其區域資料中尋找答案，然後將答案傳回給伺服器。  
6. 接著，伺服器會將結果傳回給用戶端。  
  
### <a name="resolving-names-by-using-forwarding"></a>使用轉送來解析名稱

轉送可讓您透過特定伺服器來路由名稱解析，而不是使用根提示。 下圖說明 DNS 如何使用轉送來解析名稱。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
在此範例中，會發生下列事件：  
  
1. 用戶端會查詢 DNS 伺服器中的名稱 ftp.contoso.com。  
2. DNS 伺服器會將查詢轉送到另一部 DNS 伺服器，也稱為轉寄站。  
3. 因為轉寄站不是名稱的授權，而且在其快取中沒有回應，所以它會使用根提示來尋找 DNS 根功能變數名稱伺服器的 IP 位址。  
4. 轉寄站會使用反復查詢來要求 DNS 根功能變數名稱伺服器解析名稱 ftp.contoso.com。 因為名稱 ftp.contoso.com 的結尾是「com」，所以 DNS 根功能變數名稱伺服器會將參考傳回給裝載 com 區域的 Com 伺服器。  
5. 轉寄站會使用反復查詢來要求 Com 伺服器解析名稱 ftp.contoso.com。 因為名稱 ftp.contoso.com 的結尾名稱為 contoso.com，所以 Com 伺服器會將參考傳回給裝載 contoso.com 區域的 Contoso 伺服器。  
6. 轉寄站會使用反復查詢來要求 Contoso 伺服器解析名稱 ftp.contoso.com。 Contoso 伺服器會在其區域檔案中尋找答案，然後將答案傳回給伺服器。  
7. 轉寄站接著會將結果傳回到原始的 DNS 伺服器。  
8. 原始的 DNS 伺服器接著會將結果傳回給用戶端。  
