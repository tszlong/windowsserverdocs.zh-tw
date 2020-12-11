---
title: 部署主機守護者服務
description: 深入瞭解：部署主機守護者服務
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 01/14/2020
ms.openlocfilehash: a6a885661107c4e16b495f845ee28677dd5bbba4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049836"
---
# <a name="deploying-the-host-guardian-service"></a>部署主機守護者服務

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

提供託管環境的最重要目標之一，就是確保在環境中執行的虛擬機器安全性。 身為雲端服務提供者或企業私人雲端系統管理員，您可以使用受防護網狀架構為 VM 提供更安全的環境。 受防護網狀架構包含一個主機守護者服務 (HGS) (通常是有三個節點的叢集)，加上一或多個受防護的主機，以及一組受防護的虛擬機器 (VM)。

## <a name="video-deploying-a-guarded-fabric"></a>影片：部署受防護網狀架構

> [!VIDEO https://www.microsoft.com/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>受防護網狀架構與受防護 Vm 的部署工作

下表將細分工作以部署受防護網狀架構，並根據不同的系統管理員角色建立受防護的 Vm。 請注意，當 HGS 系統管理員以授權的 Hyper-v 主機設定 HGS 時，網狀架構系統管理員會同時收集和提供主機的識別相關資訊。

| 步驟和連結至內容 | 映像 |
|--|--|--|
| 1- [驗證 HGS 必要條件](guarded-fabric-prepare-for-hgs.md) | ![步驟1：確認必要條件](../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png) |
| 2- [設定第一個 HGS 節點](guarded-fabric-choose-where-to-install-hgs.md) | ![步驟2：設定第一個 HGS 節點](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png) |
| 3- [設定額外的 HGS 節點](guarded-fabric-configure-additional-hgs-nodes.md) | ![步驟3：設定額外的 HGS 節點](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png) |
| 4- [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns.md) | ![步驟4，設定網狀架構 DNS](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png) |
| 5- [確認主機必要條件 (金鑰) ](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation) 並 [確認主機必要條件 (TPM) ](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation) | ![步驟5：確認主機必要條件金鑰和主機先決條件 TPM](../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png) |
| 6- [建立主機金鑰 (金鑰) ](guarded-fabric-create-host-key.md) ，並[ (TPM) 收集主機資訊 ](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) | ![步驟6：建立主機金鑰並收集主機資訊](../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png) |
| 7- [使用主機資訊設定 HGS](guarded-fabric-add-host-information-to-hgs.md) | ![步驟7：將主機資訊新增至 HGS](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png) |
| 8-  [確認主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md) | ![步驟8：確認主機可以證明](../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png) |
| 9- [設定 VMM (選用) ](/system-center/vmm/deploy-guarded-host-fabric) | ![步驟9，設定 VMM (選用) ](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png) |
| 10- [建立範本磁片](guarded-fabric-create-a-shielded-vm-template.md) | ![步驟10：建立範本磁片](../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png) |
| 11- [為 VMM 建立 VM 防護協助程式磁片 (選用) ](guarded-fabric-vm-shielding-helper-vhd.md) | ![步驟11：建立 VMM 的 VM 防護說明磁片](../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png) |
| 12- [設定 Windows Azure 套件 (選擇性) ](guarded-fabric-shielded-vm-windows-azure-pack.md) | ![步驟12，設定 Windows Azure 套件 (選擇性) ](../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png) |
| 13- [建立防護資料檔案](guarded-fabric-tenant-creates-shielding-data.md) | ![步驟13：建立防護資料檔案](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png) |
| 14- [使用 Windows Azure 套件建立受防護的 vm](guarded-fabric-shielded-vm-windows-azure-pack.md) | ![步驟14：使用 Windows Azure 套件建立受防護的 Vm](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png) |
| 15- [使用 VMM 建立受防護的 vm](/system-center/vmm/guarded-deploy-vm) | ![步驟15：使用 VMM 建立受防護的 Vm](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png) |

## <a name="additional-references"></a>其他參考資料

- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
