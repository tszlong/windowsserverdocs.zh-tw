---
title: 啟用使用 Azure Site Recovery 的 RDS 的災害復原
description: 了解如何啟用使用 Azure Site Recovery 的 RDS 的災害復原。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 7aa25602c71e5d114be7ae59c5e3ce168844d700
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446555"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>啟用使用 Azure Site Recovery 的 RDS 的災害復原

>適用於：Windows Server （半年通道），Windows Server 2019，Windows Server 2016

若要確保使用的 RDS 部署適當地設定災害復原，您需要保護所有的元件，可讓您的 RDS 部署：

- Active Directory
- SQL Server 層
- RDS 元件
- 網路元件

## <a name="configure-active-directory-and-dns-replication"></a>設定 Active Directory 和 DNS 複寫

您需要為您的 RDS 部署災害復原站台的 Active Directory 才能運作。 您有根據的複雜程度的 RDS 部署的兩個選擇：

- 選項 1-如果您有少數的應用程式和單一網域控制站的整個內部部署網站，而且您將容錯移轉整個網站一起使用 ASR 複寫將網域控制站複寫至次要站台 (同時為 true站對站和站台至 Azure 案例）。
- 選項 2-如果您有大量的應用程式和您正在執行 Active Directory 樹系中，您將會容錯移轉少數應用程式一次設定災害復原站台上的其他網域控制站 (次要網站或 Azure 中)。

請參閱[保護 Active Directory 和 DNS 使用 Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory)如需有關在災害復原網站上提供的網域控制站。 本指南的其餘部分，我們假設您已遵循這些步驟，且有可用的網域控制站。

## <a name="set-up-sql-server-replication"></a>設定 SQL Server 複寫

請參閱[使用 SQL Server 災害復原和 Azure Site Recovery 保護 SQL Server](/azure/site-recovery/site-recovery-sql)如需設定 SQL Server 複寫的步驟。

## <a name="enable-protection-for-the-rds-application-components"></a>啟用保護 RDS 應用程式元件

根據您的 RDS 部署類型中，您可以啟用 Azure Site Recovery 中 （如在下表中所列） 的不同元件 Vm 的保護。 設定相關的 Azure Site Recovery 元素根據您的 Vm 會部署 HYPER-V 或 VMWare 上。


|               部署類型                |                                                                                                     保護的步驟                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     (Unmanaged) 的個人虛擬桌面     | 1.請確定所有虛擬化主機已準備安裝 RDVH 角色。    </br>2.連接代理程式。  </br>3.個人桌面。 </br>4.金級範本 VM。 </br>5.Web 存取，授權伺服器與閘道伺服器 |
| （使用任何 UPD 管理） 的集區虛擬桌面 |                    1.所有虛擬化主機準備安裝 RDVH 角色。  </br>2.連接代理程式。  </br>3.金級範本 VM。 </br>4.Web 存取，授權伺服器與閘道伺服器。                    |
|   Remoteapp 和桌面工作階段 (沒有 UPD)   |                                                          1.工作階段主機。  </br>2.連接代理程式。 </br>3.Web 存取，授權伺服器與閘道伺服器。                                                           |

