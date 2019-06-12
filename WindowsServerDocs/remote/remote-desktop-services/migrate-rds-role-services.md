---
title: 將您的遠端桌面服務部署移轉至 Windows Server 2016
description: 本文說明如何移轉遠端桌面服務部署至 Windows Server 2016 的新伺服器。
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 0e4736f753fc0ad2ece6135de84d481eecb8b7a1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812592"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>將您的遠端桌面服務部署移轉至 Windows Server 2016

如果您正在 Windows Server 2012 R2 中遠端桌面服務，您可以移至 Windows Server 2016 以善用新功能，例如 Azure SQL 和儲存空間直接存取的支援。

移轉遠端桌面服務部署支援從來源到目的地伺服器執行 Windows Server 2016 執行 Windows Server 2016 的伺服器。 換句話說，沒有任何直接就地移轉從 Windows Server 2012 R2 中的 RDS 到 Windows Server 2016。 相反地，對於大部分的 RDS 元件，您先升級至 Windows Server 2016，然後再移轉資料和授權。 唯一的元件直接移轉為 RD Web，RD 閘道和授權伺服器。

如需有關升級程序和需求的詳細資訊，請參閱 <<c0> [ 升級您的遠端桌面服務部署至 Windows Server 2016](upgrade-to-rds-2016.md)。

若要移轉遠端桌面服務部署中使用下列步驟：

- [移轉 RD 連線代理人伺服器](#migrate-rdconnection-broker-servers)

- [移轉工作階段集合](#migrate-session-collections)

- [移轉虛擬桌面集合](#migrate-virtual-desktop-collections)

- [移轉 RD Web 存取伺服器](#migrate-rdweb-access-servers)

- [移轉 RD 閘道伺服器](#migrate-rdgateway-servers)

- [移轉 RD 授權伺服器](#migrate-rdgateway-servers)

- [移轉憑證](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>移轉 RD 連線代理人伺服器

這是移轉的第一個也是最重要的步驟： 移轉到目的地伺服器執行 Windows Server 2016 的 RD 連線代理人。

> [!IMPORTANT]
> 遠端桌面連線代理人 （RD 連線代理人） 的來源伺服器必須設定為高可用性以支援移轉。 如需詳細資訊，請參閱 <<c0> [ 部署遠端桌面連線代理人叢集](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)。

1. 如果您在高可用性設定中擁有多部 RD 連線代理人伺服器，則除了目前使用中的 RD 連線代理人伺服器之外，請移除所有的這類伺服器。

2. [升級](upgrade-to-rds-2016.md)部署至 Windows Server 2016 中剩餘的 RD 連線代理人伺服器。

3. 將 Windows Server 2016 的 RD 連線代理人伺服器新增至高可用性部署。

> [!NOTE] 
> Windows Server 2016 和 Windows Server 2012 R2 的混合式高可用性組態不支援針對 RD 連線代理人伺服器。 執行 Windows Server 2016 RD 連線代理人可以提供含有執行 Windows Server 2012 R2 中，RD 工作階段主機伺服器的工作階段集合，並且可以提供虛擬桌面集合與執行 Windows Server 2012 R2 的 rd 工作階段主機伺服器。

## <a name="migrate-session-collections"></a>移轉工作階段集合

請遵循下列步驟來將 Windows Server 2012 R2 中的工作階段集合移轉到 Windows Server 2016 中的工作階段集合。

> [!IMPORTANT]
> 只有在成功完成上一個步驟中之後，才能移轉工作階段集合[移轉的 RD 連線代理人伺服器](#migrate-rdconnection-broker-servers)。

1. [升級的工作階段集合](Upgrade-to-RDSH-2016.md)從 Windows Server 2012 R2 到 Windows Server 2016。

2. 新增到工作階段集合中執行 Windows Server 2016 的新 RD 工作階段主機伺服器。

3. 登出 RD 工作階段主機伺服器上的所有工作階段，然後從工作階段集合中移除需要移轉的伺服器。

   > [!NOTE]
   > 如果在工作階段集合中啟用 UVHD 範本 (uvhd-template.vhdx) 且已檔案伺服器移轉至新的伺服器，更新使用者設定檔磁碟：使用新路徑的位置集合屬性。 使用者設定檔磁碟必須可以在新位置中與它們在來源伺服器上相同的相對路徑上取得。
   >
   > 不支援 RD 工作階段主機伺服器的工作階段集合，混合使用執行 Windows Server 2012 R2 和 Windows Server 2016 的伺服器。

## <a name="migrate-virtual-desktop-collections"></a>移轉虛擬桌面集合

請遵循下列步驟來移轉虛擬桌面集合從執行 Windows Server 2016 之目的地伺服器執行 Windows Server 2012 R2 的來源伺服器。

> [!IMPORTANT]
> 只有在成功完成上一個步驟中之後，才能移轉虛擬桌面集合[移轉的 RD 連線代理人伺服器](#migrate-rdconnection-broker-servers)。

1. [升級虛擬桌面集合](Upgrade-to-RDVH-2016.md)從伺服器執行 Windows Server 2012 R2 到 Windows Server 2016。

2. 將新的 Windows Server 2016 RD 虛擬主機伺服器新增到虛擬桌面集合。

3. 將目前虛擬桌面集合中所有在 RD 虛擬主機伺服器上執行的虛擬機器移轉到新伺服器。

4. 在來源伺服器中，移除所有需要從虛擬桌面集合移轉的 RD 虛擬主機伺服器。

> [!NOTE]
> 如果在工作階段集合中啟用 UVHD 範本 (uvhd-template.vhdx) 且已檔案伺服器移轉至新的伺服器，更新使用者設定檔磁碟：使用新路徑的位置集合屬性。 使用者設定檔磁碟必須可以在新位置中與它們在來源伺服器上相同的相對路徑上取得。
>
> 不支援混合使用執行 Windows Server 2012 R2 和 Windows Server 2016 的伺服器 RD 虛擬主機伺服器的虛擬桌面集合。

## <a name="migrate-rdweb-access-servers"></a>移轉 RD Web 存取伺服器

請遵循下列步驟以移轉 RD Web 存取伺服器：

- 將目的地伺服器的遠端桌面服務部署到執行 Windows Server 2016 並安裝 RD Web 角色

- 使用[IIS Web Deploy 工具](https://www.iis.net/)若要從目前的 RD Web 存取伺服器時使用的 RD Web 網站設定移轉到執行 Windows Server 2016 的目的地伺服器。

- [移轉憑證](#migrate-certificates)到目的地伺服器執行 Windows Server 2016。

- 從遠端桌面服務的部署移除來源伺服器。

## <a name="migrate-rdgateway-servers"></a>移轉 RD 閘道伺服器

請遵循下列步驟來移轉 RD 閘道伺服器：

- 將目的地伺服器的遠端桌面服務部署到執行 Windows Server 2016 並安裝 RD 閘道角色

- 使用[IIS Web Deploy 工具](https://www.iis.net/)將從目前的 RD 閘道伺服器的 RD 閘道端點設定移轉到目的地伺服器執行 Windows Server 2016。

- [移轉憑證](#migrate-certificates)到目的地伺服器執行 Windows Server 2016。

- 從遠端桌面服務的部署移除來源伺服器。

## <a name="migrate-rdlicensing-servers"></a>移轉 RD 授權伺服器

請遵循下列步驟以從執行 Windows Server 2016 之目的地伺服器執行 Windows Server 2012 或 Windows Server 2012 R2 的來源伺服器移轉 RD 授權伺服器。

1. [移轉遠端桌面服務用戶端存取使用權 (RDS Cal)](migrate-rds-cals.md)從來源伺服器到目的地伺服器。

2. 編輯**部署內容**中**伺服器管理員**（這通常是在第一部 RD 連線代理人伺服器上執行） 的遠端桌面管理伺服器上將只有新的 RD 授權執行 Windows Server 2016 的伺服器。

3. 停用來源 RD 授權伺服器：在 **遠端桌面授權管理員**、 以滑鼠右鍵按一下適當的伺服器、 將滑鼠停留**進階**選取**停用伺服器**，然後遵循精靈中的步驟.

4. 從中的部署移除來源 RD 授權伺服器**伺服器管理員**遠端桌面管理伺服器上。

## <a name="migrate-certificates"></a>移轉憑證

成功的憑證移轉需要這兩個實際的移轉憑證和更新遠端桌面服務部署的內容中的憑證資訊程序。

一般憑證移轉包含下列步驟：

- 匯出為 PFX 檔案的憑證的私密金鑰。

- 匯入憑證從 PFX 檔案。

移轉之後適當的憑證，請更新所需的下列憑證，在 伺服器管理員或 PowerShell 的遠端桌面服務部署：

- RD 連線代理人-在單一登入

- RD 連線代理人-發行 RDP 檔案

- RD 閘道-HTTPS 連線

- RD Web 存取-HTTPS 連線，且 RemoteApp/桌面連接的訂用帳戶
