---
title: 受防護網狀架構與受防護的 VM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c2c16574439569eeb1181977f23bae238f43865c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857429"
---
# <a name="guarded-fabric-and-shielded-vms"></a>受防護網狀架構與受防護的 VM

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

提供的託管的環境的最重要目標之一是確保在環境中執行的虛擬機器的安全性。 身為雲端服務提供者或企業私人雲端系統管理員，您可以使用受防護網狀架構為 VM 提供更安全的環境。 受防護網狀架構包含一個主機守護者服務 (HGS) (通常是有三個節點的叢集)，加上一個或多個受防護主機，以及一組受防護虛擬機器 (VM)。

> [!IMPORTANT]
> 請確定您已部署在生產環境中的受防護的虛擬機器之前，先安裝最新的累積更新。

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>影片、 部落格和概觀主題有關受防護網狀架構與受防護的 Vm

- 視訊：[如何從使用 Windows Server 2019 內部威脅保護您的虛擬化網狀架構](https://myignite.techcommunity.microsoft.com/sessions/64690)
- 視訊：[Windows Server 2016 中受防護的虛擬機器簡介](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- 視訊：[了解受防護的 Vm，使用 Windows Server 2016 HYPER-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- 視訊：[部署受防護的 Vm 和使用 Windows Server 2016 的受防護網狀架構](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- 部落格：[資料中心和私用雲端安全性部落格](https://blogs.technet.microsoft.com/datacentersecurity/)
- 概觀：[受防護網狀架構與受防護的 Vm 概觀](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>規劃主題

- [主機服務提供者的規劃指南](guarded-fabric-planning-for-hosters.md)
- [租用戶的規劃指南](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>部署主題

- [部署指南](guarded-fabric-deploying-hgs-overview.md)
    - [快速入門](guarded-fabric-deployment-overview.md)
    - [部署 HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [部署受防護的主機](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [設定要做為受防護的主機的主機網狀架構 DNS](guarded-fabric-configuring-fabric-dns.md)
        - [部署受防護的主機使用 AD 模式](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [部署受防護的主機使用 TPM 模式](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [確認可以證明的受防護的主機](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [受防護的 Vm-託管服務提供者會部署在 VMM 中的受防護的主機](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [部署受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)
        - [準備 VM 防護協助程式 VHD](guarded-fabric-vm-shielding-helper-vhd.md)
        - [設定 Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [建立虛擬機器防護資料檔案](guarded-fabric-tenant-creates-shielding-data.md)
        - [使用 Windows Azure Pack 部署受防護的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [使用 Virtual Machine Manager 中部署受防護的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>作業和管理主題

- [管理主機守護者服務](guarded-fabric-manage-hgs.md)
