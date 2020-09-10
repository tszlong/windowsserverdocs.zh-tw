---
title: Windows Server 2016 Essentials 中的新功能
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: baf674b75f9476b65ba508f2841e0973783f1b9a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624176"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Windows Server 2016 Essentials 中的新功能

> 適用于： Windows Server 2016 Essentials

以下是 Windows Server 2016 Essentials 中的新功能和增強功能。

## <a name="integration-with-azure-site-recovery-services"></a>[與 Azure Site Recovery 服務整合](azure-site-recovery-services-integration.md)

**What it does**  -- 用途 &reg;當受保護的虛擬機器失敗，或受保護的虛擬機器執行所在的主機伺服器失敗時，使用 Azure Site Recovery 服務容錯移轉會維持商務持續性，直到內部部署虛擬機器或主機伺服器已修復並可供使用為止。 

**其運作方式** （Microsoft Azure 所提供的 Azure Site Recovery 服務）可讓您將虛擬機器 (VM) 即時複寫到 Azure 中的備份保存庫。 如果您的伺服器或網站因為硬體或其他失敗而關閉，您可以使用 Azure Site Recovery 服務進行容錯移轉，以便將備份保存庫中儲存的 VM 映射布建為 Azure 中的執行中 VM。 相較于 Azure 虛擬網路，先前連線至內部部署伺服器的用戶端電腦，將會以透明方式連線到在 Azure 中執行的伺服器。


## <a name="integration-with-azure-virtual-network"></a>[與 Azure 虛擬網路整合](azure-virtual-network-integration.md)

**它的作用**--隨著組織在雲端運算方面的進展，他們很少會一次移動所有的資源。 相反地，他們會將某些資源移至雲端，並將某些資源保留在內部部署環境。 如此一來，您就可以輕鬆地在一段時間內將組織移至雲端。 Azure 虛擬網路整合提供的網路基礎結構，可讓該程式順暢且容易管理。

**運作方式** ： Azure 虛擬網路是 Microsoft Azure 中提供的一種服務，可讓組織建立點對點 (P2P) 或站對站 (S2S) 虛擬私人網路，讓在 Azure 中執行的資源 (例如虛擬機器和儲存體) 看起來像是在區域網路上，以進行無縫的應用程式和資源存取。



## <a name="support-for-larger-deployments"></a>[支援大型部署](support-for-larger-deployments.md)

有些較大型的企業需要更多功能和容量，才能有效地執行 Windows Server Essentials。 Windows Server 2016 Essentials 藉由新增對較大型部署的支援，提供更高的網域、使用者和裝置管理能力：

 - 多個網域
 - 多個網域控制站
 - 能夠指定指定的網域控制站


<a name="see-also"></a>另請參閱
--------

[開始使用 Windows Server Essentials](get-started.md)&copy;&reg;