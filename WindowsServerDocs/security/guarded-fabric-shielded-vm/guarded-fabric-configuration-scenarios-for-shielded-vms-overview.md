---
description: 深入瞭解：部署受防護的 Vm
title: 部署受防護的 VM
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 015d1a5f4c2fd54a813cacff60fd65b6d8ca99cb
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040376"
---
# <a name="deploy-shielded-vms"></a>部署受防護的 VM


>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

下列主題描述租使用者如何使用受防護的 Vm。

1.  (選用) [建立 Windows 範本磁片](guarded-fabric-create-a-shielded-vm-template.md) 或 [建立 Linux 範本磁片](guarded-fabric-create-a-linux-shielded-vm-template.md)。 您可以透過租使用者或主機服務提供者來建立範本磁片。

2.  (選擇性) [將現有的 WINDOWS VM 轉換成受防護的 vm](guarded-fabric-vm-shielding-helper-vhd.md)。

3. [建立防護資料以定義受防護的 VM](guarded-fabric-tenant-creates-shielding-data.md)。

    如需防護資料檔案的說明和圖表，請參閱 [何謂防護資料，以及為何需要它？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)

    如需建立回應檔案以包含在受防護資料檔案中的相關資訊，請參閱 [受防護的 vm-使用 New-ShieldingDataAnswerFile 函式產生回應](guarded-fabric-sample-unattend-xml-file.md)檔案。

4. 建立受防護的 VM：

    - 使用 **Windows Azure 套件**： [使用 Windows Azure 套件部署受防護的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - 使用 **Virtual Machine Manager**： [使用 Virtual Machine Manager 部署受防護的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="additional-references"></a>其他參考資料

- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
