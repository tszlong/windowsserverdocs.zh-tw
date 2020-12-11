---
description: 深入瞭解：租使用者的受防護 Vm-使用 Windows Azure 套件部署受防護的 VM
title: 受防護的租使用者 Vm-使用 Windows Azure 套件部署受防護的 VM
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 7eb7078c9df81d4623c0f9655edc11ac991e1844
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043826"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>受防護的租使用者 Vm-使用 Windows Azure 套件部署受防護的 VM

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

如果您的主機服務提供者支援它，您可以使用 Windows Azure 套件來部署受防護的 VM。

完成以下步驟：

1. 訂閱 Windows Azure 套件中提供的一或多個方案。

2. 使用 Windows Azure 套件建立受防護的 VM。

    [使用受防護的虛擬機器](/previous-versions/azure/windows-server-azure-pack/mt720674(v=technet.10))，如下列主題所述：

   - [建立防護資料](/previous-versions/azure/windows-server-azure-pack/mt720672(v=technet.10)) (並上傳防護資料檔案，如主題) 的第二個程式中所述。

     > [!NOTE]
     > 在建立防護資料的過程中，您將會下載您的守護者金鑰檔，這會是 UTF-8 格式的 XML 檔案。 請勿將檔案變更為 UTF-16。

   - 使用「**快速建立**」、透過受防護的範本或一般範本 [建立受防護的虛擬機器](/previous-versions/azure/windows-server-azure-pack/mt720673(v=technet.10))。

       > [!WARNING]
       > 如果您 [使用一般範本建立受防護的虛擬機器](/previous-versions/azure/windows-server-azure-pack/mt720673(v=technet.10)#Anchor_2)，請務必注意，VM 會以非 *遮罩* 的方式布建。 這表示範本磁片不會針對防護資料檔案中受信任的磁片清單進行驗證，也不會對您用來布建 VM 的防護資料檔案中的秘密進行驗證。 如果有受防護的範本可用，最好使用受防護的範本來部署受防護的 VM，以提供秘密的端對端保護。

   - [將第2代虛擬機器轉換為受防護的虛擬機器](/previous-versions/azure/windows-server-azure-pack/mt720670(v=technet.10))

       > [!NOTE]
       > 如果您將虛擬機器轉換成受防護的虛擬機器，現有的檢查點和備份都不會加密。 您應該盡可能刪除舊的檢查點，以防止存取舊的解密資料。

## <a name="additional-references"></a>其他參考資料

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
