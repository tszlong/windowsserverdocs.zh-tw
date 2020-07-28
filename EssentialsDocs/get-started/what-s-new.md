---
title: Windows Server 2016 Essentials 中的新功能
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 504e81abb6822fca59eb15afe895c2345cc34f5d
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181614"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Windows Server 2016 Essentials 中的新功能

> 適用于： Windows Server 2016 Essentials

以下是 Windows Server 2016 Essentials 中的新功能和增強功能。

## <a name="integration-with-azure-site-recovery-services"></a>[與 Azure Site Recovery 服務整合](azure-site-recovery-services-integration.md)

**What it does**  -- 用途 &reg;當受保護的虛擬機器失敗，或受保護虛擬機器執行所在的主機伺服器失敗時，使用 Azure Site Recovery 服務進行容錯移轉會維持商務持續性，直到內部部署虛擬機器或主機伺服器修復並可供使用為止。 

**運作方式**--Microsoft Azure 提供的 Azure Site Recovery 服務，可讓您將虛擬機器（VM）即時複寫到 Azure 中的備份保存庫。 如果您的伺服器或網站因硬體或其他失敗而停止運作，您可以使用 Azure Site Recovery 服務損毀修復，讓儲存在備份保存庫中的 VM 映射會布建為 Azure 中執行中的 VM。 與 Azure 虛擬網路結合之後，先前連線到內部部署伺服器的用戶端電腦將會以透明的方式連接到在 Azure 中執行的伺服器。


## <a name="integration-with-azure-virtual-network"></a>[與 Azure 虛擬網路整合](azure-virtual-network-integration.md)

**其作用**--當組織採用雲端運算的方式時，他們很少會一次移動其所有資源。 相反地，他們會將一些資源移到雲端，並保留一些內部部署。 如此一來，在一段時間後，就可以輕鬆地將組織移到雲端。 Azure 虛擬網路整合提供了讓該程式順暢且易於管理的網路基礎結構。

**運作方式**--Azure 虛擬網路是 Microsoft Azure 中提供的一項服務，可讓組織建立點對點（P2P）或站對站（S2S）虛擬私人網路，讓在 Azure 中執行的資源（例如虛擬機器和儲存體）看起來像是在區域網路上，以進行無縫的應用程式和資源存取。



## <a name="support-for-larger-deployments"></a>[支援較大的部署](support-for-larger-deployments.md)

有些較大的小型企業需要更多的功能和容量，才能有效地執行 Windows Server Essentials。 Windows Server 2016 Essentials 藉由新增對較大型部署的支援，提供更高的網域、使用者和裝置管理能力：

 - 多個網域
 - 多個網域控制站
 - 能夠指定指定的網域控制站


<a name="see-also"></a>另請參閱
--------

[Windows Server Essentials 入門](get-started.md)&copy;&reg;