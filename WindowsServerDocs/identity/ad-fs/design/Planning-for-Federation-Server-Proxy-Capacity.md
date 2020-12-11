---
description: 深入瞭解：規劃同盟伺服器 Proxy 容量
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: 規劃同盟伺服器 Proxy 容量
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a8454210db5c9d15cd308e67e23717e00724b81c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039976"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>規劃同盟伺服器 Proxy 容量

同盟伺服器 proxy 的容量規劃可協助您預估：

-   每個同盟伺服器 proxy 都需要適當的硬體需求。

-   要在每個組織中放置的同盟伺服器和同盟伺服器 proxy 數目。

同盟伺服器 proxy 會將來自公司網路中受保護同盟伺服器的安全性權杖重新導向至同盟使用者。 部署同盟伺服器 proxy 的目的是要讓外部使用者能夠連線到同盟伺服器。 它不會實際簽署權杖或寫入 AD FS 設定資料庫中的資料。 因此，同盟伺服器 proxy 的硬體需求通常會低於同盟伺服器的硬體需求。

因為對同盟伺服器 proxy 的每個要求都會導致對同盟伺服器或同盟伺服器陣列的要求，所以必須以平行方式執行同盟伺服器和同盟伺服器 proxy 的容量規劃。

估計 \- 同盟伺服器 proxy 的每秒尖峰登入次數，需要瞭解將透過同盟伺服器 proxy 登入之同盟使用者的使用模式。 在許多部署中，使用同盟伺服器 proxy 登入的同盟使用者位於網際網路上。 您可以 \- 在將受 AD FS 保護的現有 Web 應用程式上，查看這些同盟使用者的使用模式，以預估每秒的尖峰登入次數。

> [!NOTE]
> 針對生產部署，建議您部署的每個同盟伺服器陣列實例至少要有兩個同盟伺服器 proxy。

## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估計您組織所需的同盟伺服器 proxy 數目
您必須先決定要在組織中部署的同盟伺服器總數，才能估計所需 AD FS 同盟伺服器 proxy 電腦的數目。 如需有關如何進行此作業的詳細資訊，請參閱 [規劃同盟伺服器的容量](Planning-for-Federation-Server-Capacity.md)。

一旦決定同盟伺服器的數目之後，請將此數目的伺服器乘以您預期會從位於公司網路外部的外部使用者發出的傳入同盟驗證要求的百分比 \( \) 。 這項計算的值將會為您提供估計的同盟伺服器 proxy 數目，這些 proxy 會處理您外部使用者的傳入驗證要求。

例如，如果建議的同盟伺服器數目是3，且您預期將從外部使用者進行的驗證要求總數大約是同盟驗證要求總數的60%，則您的計算會等於 1.8 \( 3 X。60， \) 您最多可以進位到2。  因此，在此情況下，您必須部署兩部同盟伺服器 proxy 電腦，以容納三個同盟伺服器的外部使用者驗證要求負載。

在 AD FS 產品小組所執行的測試中，發現每個同盟伺服器 proxy 的整體 CPU 使用率明顯低於相同伺服器陣列的同盟伺服器上觀察到的 CPU 使用率。  在一項測試中，當一部同盟伺服器 CPU 指出它已完全飽和時，為該相同伺服器陣列提供 proxy 服務的同盟伺服器 proxy CPU 會在20% 的使用率內觀察到。 因此，我們的測試顯示同盟伺服器 proxy 的 CPU 負載（使用本節稍早所討論的類似硬體規格）可以合理處理大約三部同盟伺服器的處理負載。

不過，基於容錯目的，我們建議您部署的每個同盟伺服器陣列，至少要有兩個同盟伺服器 proxy。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
