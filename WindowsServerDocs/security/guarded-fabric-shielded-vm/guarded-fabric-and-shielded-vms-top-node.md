---
title: 受防護網狀架構與受防護的 VM
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: c3f17320432d44a84be05040e23e7af30995c969
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997559"
---
# <a name="guarded-fabric-and-shielded-vms"></a>受防護網狀架構與受防護的 VM

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

提供裝載環境的其中一個最重要的目標，就是保證在環境中執行之虛擬機器的安全性。 身為雲端服務提供者或企業私人雲端系統管理員，您可以使用受防護網狀架構為 VM 提供更安全的環境。 受防護網狀架構包含一個主機守護者服務 (HGS) (通常是有三個節點的叢集)，加上一或多個受防護的主機，以及一組受防護的虛擬機器 (VM)。

> [!IMPORTANT]
> 在生產環境中部署受防護的虛擬機器之前，請確定您已安裝最新的累計更新。

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>有關受保護網狀架構和受防護 Vm 的影片、blog 和總覽主題

- 影片：[如何保護您的虛擬化網狀架構與 Windows Server 2019 的內部威脅](https://myignite.techcommunity.microsoft.com/sessions/64690)
- 影片： [Windows Server 2016 中受防護的虛擬機器簡介](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- 影片：[使用 Windows Server 2016 Hyper-v 深入探索受防護的 vm](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- 影片：[使用 Windows Server 2016 部署受防護的 vm 和受防護的網狀架構](https://mva.microsoft.com/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog：[資料中心和私用雲端安全性 Blog](/archive/blogs/datacentersecurity/)
- 總覽：[受保護的網狀架構和受防護的 vm 總覽](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>規劃主題

- [主控者規劃指南](guarded-fabric-planning-for-hosters.md)
- [租使用者規劃指南](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>部署主題

- [部署指南](guarded-fabric-deploying-hgs-overview.md)
    - [快速入門](guarded-fabric-deployment-overview.md)
    - [部署 HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [部署受防護主機](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [針對將會變成受防護主機的主機設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns.md)
        - [使用 AD 模式部署受防護主機](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [使用 TPM 模式部署受防護主機](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [受防護的 VM - 託管服務提供者會在 VMM 中部署受防護的主機](/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019)
    - [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)
        - [準備 VM 防護協助程式 VHD](guarded-fabric-vm-shielding-helper-vhd.md)
        - [設定 Windows Azure 套件](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [建立防護資料檔案](guarded-fabric-tenant-creates-shielding-data.md)
        - [使用 Microsoft Azure 套件部署受防護的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [使用 Virtual Machine Manager 部署受防護的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>作業和管理主題

- [管理主機守護者服務](guarded-fabric-manage-hgs.md)