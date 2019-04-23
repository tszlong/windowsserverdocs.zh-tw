---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: 檢閱 DNS 概念
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d077ecc30caca9b8f3b382af624121fec729afae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890499"
---
# <a name="reviewing-dns-concepts"></a>檢閱 DNS 概念

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

網域名稱系統 (DNS) 是一個分散式的資料庫，表示命名空間。 命名空間包含的所有資訊所需的任何用戶端來查詢任何名稱。 任何 DNS 伺服器可以回應其命名空間內的任何名稱的相關的查詢。 DNS 伺服器回答查詢中的下列方法之一：  
  
- 如果答案就在其快取，它會回應來自快取的查詢。  
- 如果答案是 DNS 伺服器所裝載之區域中，它會回應來自其區域的查詢。 區域是儲存在 DNS 伺服器上的 DNS 樹狀結構的一部分。 當 DNS 伺服器所裝載的區域時，是否管轄該區域中的名稱 （也就是 DNS 伺服器可以回應的查詢區域中的任何名稱）。 例如，裝載區域 contoso.com 的伺服器可以回應的 contoso.com 中的任何名稱的查詢。  
- 如果伺服器無法從其快取或區域來回答查詢，它會查詢其他伺服器的回應。  

務必了解 DNS 的核心功能，例如委派、 遞迴性名稱解析，以及 Active Directory 整合 DNS 區域，因為它們將直接影響對您的 Active Directory 邏輯結構設計。  
  
如需有關 DNS 和 Active Directory 網域服務 (AD DS) 的詳細資訊，請參閱 < [DNS 與 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)。  
  
## <a name="delegation"></a>委派

回應查詢有關的任何名稱的 DNS 伺服器，它就必須在命名空間中的每個區域中具有直接或間接的路徑。 這些路徑會透過委派來建立。 委派是在上層區域會列出有權管理階層的下一個層級中區域的名稱伺服器記錄。 委派可以讓用戶端指向伺服器的其他區域中的某個區域中的伺服器。 下圖顯示一個範例中的委派。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
DNS 根伺服器裝載根區域以點表示 (。 )。 根區域包含在下一個層級，階層，也就是 com 區域的區域的委派。 在根區域委派會告訴 DNS 根伺服器，若要尋找 com 區域，它就必須連絡 Com 伺服器。 同樣地，在 com 區域委派會告知 Com 伺服器，若要尋找 「 contoso.com 」 區域，它就必須連絡 Contoso 伺服器。  
  
> [!NOTE]  
> 委派會使用兩種類型的記錄。 名稱伺服器 (NS) 資源記錄會提供授權伺服器的名稱。 主機 (A) 與主機 (AAAA) 資源記錄提供 IP 第 4 版 (IPv4) 和 IP 版本 6 (IPv6) 位址的授權伺服器。  
  
此區域和委派的系統會建立階層式樹狀圖，表示 DNS 命名空間。 每個區域代表圖層在階層中，而每個委派則代表樹狀結構的分支。  
  
藉由使用區域和委派的階層，DNS 根伺服器可以在 DNS 命名空間中找到的任何名稱。 根區域包含直接或間接指向在階層中的所有其他區域的委派。 任何可以查詢 DNS 根伺服器的伺服器可以命名空間中找不到任何名稱，以使用委派中的資訊。  
  
## <a name="recursive-name-resolution"></a>遞迴性名稱解析

遞迴性名稱解析是讓 DNS 伺服器使用區域和委派的階層架構來回應的查詢，它不是授權程序。  
  
在某些組態中，DNS 伺服器會包含根提示 （也就是清單的名稱和 IP 位址），來查詢 DNS 根伺服器。 在其他組態中，伺服器會轉送另一部伺服器無法回答他們的所有查詢。 轉寄和根提示是這兩種方法，DNS 伺服器可以用來解決它們的未授權的查詢。  
  
### <a name="resolving-names-by-using-root-hints"></a>使用根提示來解析名稱

根提示，讓任何 DNS 伺服器，以找出 DNS 根伺服器。 DNS 伺服器會找出 DNS 根伺服器之後，它可以解決該命名空間的任何查詢。 下圖說明如何 DNS 名稱解析使用根提示。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
在此範例中，會發生下列事件：  
  
1. 用戶端會傳送至要求對應至名稱 ftp.contoso.com 的 IP 位址的 DNS 伺服器的遞迴查詢。 遞迴查詢表示用戶端想要明確的答案，到其查詢。 遞迴查詢的回應必須是有效的地址或訊息，指出找不到地址。  
2. 因為 DNS 伺服器不是名稱的授權，並在其快取中沒有回應，DNS 伺服器會使用根提示來尋找 DNS 根伺服器的 IP 位址。  
3. DNS 伺服器會使用要求 DNS 根伺服器，來解決名稱 ftp.contoso.com 反覆查詢。 反覆查詢表示伺服器將會接受轉介至另一部伺服器來查詢明確的答案取代。 因為名稱 ftp.contoso.com 結尾標籤 com，DNS 根伺服器傳回轉介至 Com 伺服器裝載 com 區域。  
4. DNS 伺服器會使用要求的 Com 伺服器，以解析名稱 ftp.contoso.com 反覆查詢。 因為名稱 ftp.contoso.com 結束名稱為 contoso.com，Com 伺服器就會傳回轉介到 Contoso 伺服器裝載 contoso.com 區域。  
5. DNS 伺服器會向 Contoso 伺服器來解決名稱 ftp.contoso.com 使用反覆查詢。 Contoso 伺服器在其區域資料中尋找答案，並傳回伺服器回應。  
6. 伺服器接著會傳回給用戶端的結果。  
  
### <a name="resolving-names-by-using-forwarding"></a>使用轉送解析名稱

轉送可讓您透過特定的伺服器，而不是使用根提示的路由名稱解析。 下圖說明 DNS 如何使用轉送解析名稱。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
在此範例中，會發生下列事件：  
  
1. 用戶端查詢名稱 ftp.contoso.com 的 DNS 伺服器。  
2. DNS 伺服器將查詢轉寄到另一部 DNS 伺服器，稱為 轉寄站。  
3. 轉寄站不是名稱的授權，並在其快取中沒有答案，因為它會使用根提示，來尋找 DNS 根伺服器的 IP 位址。  
4. 轉寄站會使用要求 DNS 根伺服器，來解決名稱 ftp.contoso.com 反覆查詢。 因為名稱結尾是名稱 ftp.contoso.com com，DNS 根伺服器傳回轉介至 Com 伺服器裝載 com 區域。  
5. 轉寄站會向 Com 伺服器，以解析名稱 ftp.contoso.com 使用反覆查詢。 因為名稱 ftp.contoso.com 結束名稱為 contoso.com，Com 伺服器就會傳回轉介到 Contoso 伺服器裝載 contoso.com 區域。  
6. 轉寄站會向 Contoso 伺服器來解決名稱 ftp.contoso.com 使用反覆查詢。 Contoso 伺服器在其區域檔案中，尋找答案，並傳回伺服器回應。  
7. 轉寄站接著會將結果傳回至原始的 DNS 伺服器。  
8. 原始的 DNS 伺服器然後會將結果傳回至用戶端。  
