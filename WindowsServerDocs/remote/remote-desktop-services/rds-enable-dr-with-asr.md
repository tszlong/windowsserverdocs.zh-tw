---
title: 使用 Azure Site Recovery 啟用 RDS 的災害復原
description: 了解如何使用 Azure Site Recovery 啟用 RDS 的災害復原。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 0c7af18be4aa767009f1dd0b82f145ffe6874768
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861401"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>使用 Azure Site Recovery 啟用 RDS 的災害復原

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

為確保適當地針對災害復原設定 RDS 部署，您需要保護組成 RDS 部署的所有元件：

- Active Directory
- SQL Server 層
- RDS 元件
- 網路元件

## <a name="configure-active-directory-and-dns-replication"></a>設定 Active Directory 和 DNS 複寫

災害復原網站需要有 Active Directory，RDS 部署才能運作。 根據 RDS 部署的複雜程度，您有兩個選擇：

- 選項 1 - 如果您在整個內部部署網站上有少數的應用程式和單一網域控制站，而且您將一起容錯移轉整個網站，請使用 ASR-Replication 將網域控制站複寫至次要網站 (同時適用於站台對站台和站台對 Azure 案例)。
- 選項 2 - 如果您有大量的應用程式而且您正在執行 Active Directory 樹系，您將一次容錯移轉少數應用程式，並在災害復原網站上設定額外的網域控制站 (次要網站或在 Azure 中)。

如需有關在災害復原網站上提供網域控制站的詳細資訊，請參閱[使用 Azure Site Recovery 保護 Active Directory 和 DNS](/azure/site-recovery/site-recovery-active-directory)。 在本指南的其餘部分，我們假設您已經依照這些步驟進行，而且有可用的網域控制站。

## <a name="set-up-sql-server-replication"></a>設定 SQL Server 複寫

如需設定 SQL Server 複寫的步驟，請參閱[使用 SQL Server 災害復原和 Azure Site Recovery 保護 SQL Server](/azure/site-recovery/site-recovery-sql)。

## <a name="enable-protection-for-the-rds-application-components"></a>針對 RDS 應用程式元件啟用保護

根據您的 RDS 部署類型，您可以在 Azure Site Recovery 中，針對不同的元件 VM 啟用保護 (如下表中所列)。 根據您的 VM 是在 Hyper-V 還是 VMWare 上部署，設定相關的 Azure Site Recovery 元素。


|               部署類型                |                                                                                                     保護步驟                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     個人虛擬桌面 (未受管理)     | 1.請確定所有虛擬化主機都已經準備好安裝 RDVH 角色。    </br>2.連線代理人。  </br>3.個人桌面。 </br>4.金級範本 VM。 </br>5.Web 存取、授權伺服器與閘道伺服器 |
| 集區虛擬桌面 (受管理、無 UPD) |                    1.所有虛擬化主機都已經準備好安裝 RDVH 角色。  </br>2.連線代理人。  </br>3.金級範本 VM。 </br>4.Web 存取、授權伺服器與閘道伺服器。                    |
|   RemoteApp 和桌面工作階段 (無 UPD)   |                                                          1.工作階段主機。  </br>2.連線代理人。 </br>3.Web 存取、授權伺服器與閘道伺服器。                                                           |

