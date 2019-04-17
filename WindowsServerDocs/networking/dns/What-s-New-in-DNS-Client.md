---
title: 在 Windows Server DNS Client 中的新功能
description: 本主題提供概觀 DNS Client Windows Server 與 Windows 10 中的新功能
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: defe88fe2a1e4f5393be4d5d5938d9f5bfbfb5d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>在 Windows Server 2016 DNS Client 中的新功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題描述新增或變更 Windows 10 與 Windows Server 2016 和較新版本的下列作業系統中的網域名稱系統」(DNS) client 功能。
  
## <a name="updates-to-dns-client"></a>DNS Client 的更新

**DNS Client 服務繫結**： 在 Windows 10 中的 DNS Client 服務提供的電腦有多個網路介面美化的支援。 多重電腦的 DNS 解析度會以下列方式最佳化：  
  
-   解析 DNS 查詢，使用 DNS 伺服器上的特定介面設定時，將此介面傳送 DNS 查詢之前繫結 DNS Client 服務。  
  
    繫結至特定的介面，DNS client 可清楚指定名稱解析發生的介面，透過這最佳化通訊，使用 DNS client 啟用應用程式網路介面。  
  
-   群組原則設定的名稱解析原則表格 (NRPT) 來指定會使用 DNS 伺服器，如果 DNS Client 服務不是繫結至特定介面。  
  
> [!NOTE]  
> 變更 Windows 10 中的 DNS Client 服務也會出現在執行 Windows Server 2016 和較新版本的電腦。  
  
## <a name="see-also"></a>也了  
  
-   [Windows Server 2016 中的 DNS 伺服器中的新功能](What-s-New-in-DNS-Server.md)  
  

