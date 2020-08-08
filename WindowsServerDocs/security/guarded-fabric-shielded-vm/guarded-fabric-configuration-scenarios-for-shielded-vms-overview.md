---
title: 部署受防護的 VM
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 0eed5d3166410cd006469eaac97ff152dfc8304e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971345"
---
# <a name="deploy-shielded-vms"></a>部署受防護的 VM


>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

下列主題說明租使用者可以如何與受防護的 Vm 搭配使用。

1.  (選擇性) [建立 Windows 範本磁片](guarded-fabric-create-a-shielded-vm-template.md)或[建立 Linux 範本磁片](guarded-fabric-create-a-linux-shielded-vm-template.md)。 租使用者或主機服務提供者可以建立範本磁片。

2.  (選擇性) [將現有的 WINDOWS VM 轉換為受防護的 vm](guarded-fabric-vm-shielding-helper-vhd.md)。

3. [建立防護資料以定義受防護的 VM](guarded-fabric-tenant-creates-shielding-data.md)。

    如需防護資料檔案的說明和圖表，請參閱[何謂防護資料和為何需要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)

    如需建立回應檔案以包含在受防護資料檔案中的相關資訊，請參閱[受防護的 vm-使用 ShieldingDataAnswerFile 函式產生回應](guarded-fabric-sample-unattend-xml-file.md)檔案。

4. 建立受防護的 VM：

    - 使用**Windows Azure 套件**：[使用 Windows Azure 套件部署受防護的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - 使用**Virtual Machine Manager**：[使用 Virtual Machine Manager 部署受防護的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="additional-references"></a>其他參考資料

- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
