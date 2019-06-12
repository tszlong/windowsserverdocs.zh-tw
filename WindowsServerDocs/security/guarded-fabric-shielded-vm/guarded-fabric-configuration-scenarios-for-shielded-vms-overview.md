---
title: 部署受防護的 VM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9badbfcb709c29451425aaecc56b46ac98837e18
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443799"
---
# <a name="deploy-shielded-vms"></a>部署受防護的 VM


>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

下列主題說明如何將租用戶可以使用受防護的 Vm。

1. （選擇性）[建立 Windows 範本磁碟](guarded-fabric-create-a-shielded-vm-template.md)或是[建立 Linux 的範本磁碟](guarded-fabric-create-a-linux-shielded-vm-template.md)。 租用戶或主機服務提供者可以建立範本磁碟。 

2. （選擇性）[將現有的 Windows VM 轉換成受防護的 VM](guarded-fabric-vm-shielding-helper-vhd.md)。 

3. [建立要定義受防護的 VM 防護資料](guarded-fabric-tenant-creates-shielding-data.md)。

    如需描述和圖表的防護資料檔案，請參閱[什麼防護資料，以及它為何如此必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    如需建立回應檔案以在防護的資料檔案中包含的資訊，請參閱[受防護的 Vm-使用新 ShieldingDataAnswerFile 函式產生的回應檔案](guarded-fabric-sample-unattend-xml-file.md)。

4. 建立受防護的 VM:
 
    - 使用**Windows Azure Pack**:[使用 Windows Azure Pack 部署受防護的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - 使用**Virtual Machine Manager**:[使用 Virtual Machine Manager 中部署受防護的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>另請參閱

- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
