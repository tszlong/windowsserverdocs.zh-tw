---
title: 什麼是 Windows Server 中的 DNS 用戶端的新功能
description: 本主題提供在 Windows Server 和 Windows 10 中的 DNS 用戶端的新功能的概觀
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34b1a64465e217fbd7e6b3ae55e89832a7a4e48c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860559"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Windows Server 2016 中 DNS 用戶端的新功能

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明新增或變更 Windows 10 和 Windows Server 2016 和更新版本中的這些作業系統的網域名稱系統 (DNS) 用戶端功能。
  
## <a name="updates-to-dns-client"></a>DNS 用戶端的更新

**DNS 用戶端服務繫結**:在 Windows 10 中，DNS 用戶端服務會提供電腦與一個以上的網路介面的增強的支援。 對於多重主目錄電腦，DNS 解析已透過下列方式最佳化：  
  
-   設定特定介面的 DNS 伺服器來解析 DNS 查詢，DNS 用戶端服務會繫結至這個介面再傳送 DNS 查詢。  
  
    清楚地繫結至特定的介面，DNS 用戶端可以指定發生名稱解析的介面，啟用應用程式，以最佳化與 DNS 用戶端通訊透過此網路介面。  
  
-   如果 DNS 伺服器是所指定的群組原則設定的名稱解析原則表格 (NRPT)，DNS 用戶端服務不繫結至特定的介面。  
  
> [!NOTE]  
> 在 Windows 10 中的 DNS 用戶端服務的變更也會在執行 Windows Server 2016 和更新版本的電腦。  
  
## <a name="see-also"></a>另請參閱  
  
-   [什麼是 Windows Server 2016 中 DNS 伺服器的新功能](What-s-New-in-DNS-Server.md)  
  

