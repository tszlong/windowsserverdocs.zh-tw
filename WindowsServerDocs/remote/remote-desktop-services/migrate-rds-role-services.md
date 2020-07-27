---
title: 將遠端桌面服務部署移轉至 Windows Server 2016
description: 本文描述如何將遠端桌面服務部署移轉到新的 Windows Server 2016 伺服器。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 2e4628e9ec7b5d75b88bb07282e2183a2f16fff2
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959150"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>將遠端桌面服務部署移轉至 Windows Server 2016

如果您目前在 Windows Server 2012 R2 中執行遠端桌面服務，您可以移至 Windows Server 2016 來利用 Azure SQL 和儲存空間直接存取支援等新功能。

支援將遠端桌面服務部署從執行 Windows Server 2016 之來源伺服器移轉到執行 Windows Server 2016 的目的地伺服器。 換句話說，無法從 Windows Server 2012 R2 中的 RDS 直接就地移轉到 Windows Server 2016。 相反地，對於大部分的 RDS 元件，您會先升級至 Windows Server 2016，再移轉資料和授權。 唯一可直接移轉的元件包括 RD Web、RD 閘道和授權伺服器。

如需升級程序和需求的詳細資訊，請參閱[將遠端桌面服務部署升級至 Windows Server 2016](./upgrade-to-rds.md)。

使用下列步驟來移轉遠端桌面服務部署：

- [移轉 RD 連線代理人伺服器](#migrate-rdconnection-broker-servers)

- [移轉工作階段集合](#migrate-session-collections)

- [移轉虛擬桌面集合](#migrate-virtual-desktop-collections)

- [移轉 RD Web 存取伺服器](#migrate-rdweb-access-servers)

- [移轉 RD 閘道伺服器](#migrate-rdgateway-servers)

- [移轉 RD 授權伺服器](#migrate-rdgateway-servers)

- [移轉憑證](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>移轉 RD 連線代理人伺服器

這是移轉的第一個且最重要步驟：將 RD 連線代理人移轉到執行 Windows Server 2016 的目的地伺服器。

> [!IMPORTANT]
> 遠端桌面連線代理人 (RD 連線代理人) 來源伺服器必須設定為高可用性以支援移轉。 如需詳細資訊，請參閱[部署遠端桌面連線代理人叢集](./rds-connection-broker-cluster.md)。

1. 如果您在高可用性設定中擁有多部 RD 連線代理人伺服器，則除了目前使用中的 RD 連線代理人伺服器之外，請移除所有的這類伺服器。

2. 將部署中剩餘的 RD 連線代理人伺服器[升級](./upgrade-to-rds.md)至 Windows Server 2016。

3. 將 Windows Server 2016 RD 連線代理人伺服器新增至高可用性部署。

> [!NOTE] 
> RD 連線代理人伺服器不支援 Windows Server 2016 和 Windows Server 2012 R2 的混合高可用性設定。 執行 Windows Server 2016 的 RD 連線代理人可以提供具有執行 Windows Server 2012 R2 之 RD 工作階段主機伺服器的工作階段集合，且可提供具有執行 Windows Server 2012 R2 之 RD 虛擬主機伺服器的虛擬桌面集合。

## <a name="migrate-session-collections"></a>移轉工作階段集合

遵循下列步驟，將 Windows Server 2012 R2 中工作階段集合移轉到 Windows Server 2016 中工作階段集合。

> [!IMPORTANT]
> 只有在成功完成前一個步驟 ([移轉 RD 連線代理人伺服器](#migrate-rdconnection-broker-servers)) 之後，才能移轉工作階段集合。

1. [將工作階段集合從 Windows Server 2012 R2 升級至 Windows Server 2016](./upgrade-to-rdsh.md)。

2. 將執行 Windows Server 2016 的新 RD 工作階段主機伺服器新增至工作階段集合。

3. 登出 RD 工作階段主機伺服器上的所有工作階段，然後從工作階段集合中移除需要移轉的伺服器。

   > [!NOTE]
   > 如果已在工作階段集合中啟用 UVHD 範本 (UVHD-template.vhdx) 且已將檔案伺服器移轉到新伺服器，請使用新路徑更新使用者設定檔磁碟：位置集合屬性。 使用者設定檔磁碟必須可以在新位置中與它們在來源伺服器上相同的相對路徑上取得。
   >
   > 如果 RD 工作階段主機伺服器具有執行 Windows Server 2012 R2 和 Windows Server 2016 的伺服器組合，則不支援這類伺服器的工作階段集合。

## <a name="migrate-virtual-desktop-collections"></a>移轉虛擬桌面集合

遵循下列步驟，將虛擬桌面集合從執行 Windows Server 2012 R2 的來源伺服器移轉到執行 Windows Server 2016 的目的地伺服器。

> [!IMPORTANT]
> 只有在成功完成前一個步驟 ([移轉 RD 連線代理人伺服器](#migrate-rdconnection-broker-servers)) 之後，才能移轉虛擬桌面集合。

1. [將虛擬桌面集合從執行 Windows Server 2012 R2 的伺服器升級至 Windows Server 2016](./upgrade-to-rdvh.md)。

2. 將新的 Windows Server 2016 RD 虛擬主機伺服器新增至虛擬桌面集合。

3. 將目前虛擬桌面集合中所有在 RD 虛擬主機伺服器上執行的虛擬機器移轉到新伺服器。

4. 在來源伺服器中，移除所有需要從虛擬桌面集合移轉的 RD 虛擬主機伺服器。

> [!NOTE]
> 如果已在工作階段集合中啟用 UVHD 範本 (UVHD-template.vhdx) 且已將檔案伺服器移轉到新伺服器，請使用新路徑來更新使用者設定檔磁碟：位置集合屬性。 使用者設定檔磁碟必須可以在新位置中與它們在來源伺服器上相同的相對路徑上取得。
>
> 如果 RD 虛擬主機伺服器具有執行 Windows Server 2012 R2 和 Windows Server 2016 的伺服器組合，則不支援這類伺服器的虛擬桌面集合。

## <a name="migrate-rdweb-access-servers"></a>移轉 RD Web 存取伺服器

遵循下列步驟來移轉 RD Web 存取伺服器：

- 將執行 Windows Server 2016 的目的地伺服器加入遠端桌面服務部署，並安裝 RD Web 角色

- 使用 [IIS Web Deploy 工具](https://www.iis.net/)，將 RD Web 網站設定從目前 RD Web 存取伺服器移轉到執行 Windows Server 2016 的目的地伺服器。

- [移轉憑證](#migrate-certificates)到執行 Windows Server 2016 的目的地伺服器。

- 從遠端桌面服務部署移除來源伺服器。

## <a name="migrate-rdgateway-servers"></a>移轉 RD 閘道伺服器

遵循下列步驟來移轉 RD 閘道伺服器：

- 將執行 Windows Server 2016 的目的地伺服器加入遠端桌面服務部署，並安裝 RD 閘道角色

- 使用 [IIS Web Deploy 工具](https://www.iis.net/)，將 RD 閘道端點設定從目前 RD 閘道伺服器移轉到執行 Windows Server 2016 的目的地伺服器。

- [移轉憑證](#migrate-certificates)到執行 Windows Server 2016 的目的地伺服器。

- 從遠端桌面服務部署移除來源伺服器。

## <a name="migrate-rdlicensing-servers"></a>移轉 RD 授權伺服器

遵循下列步驟，將 RD 授權伺服器從執行 Windows Server 2012 或 Windows Server 2012 R2 來源伺服器移轉到執行 Windows Server 2016 的目的地伺服器。

1. [將遠端桌面服務用戶端存取使用權 (RDS CAL) 從來源伺服器移轉到目的地伺服器](migrate-rds-cals.md)。

2. 在遠端桌面管理伺服器 (通常會在第一部 RD 連線代理人伺服器上執行) 的 [伺服器管理員]  中，編輯 [部署內容]  ，只包含執行 Windows Server 2016 的新 RD 授權伺服器。

3. 停用來源 RD 授權伺服器：在 [遠端桌面授權管理員]  中，以滑鼠右鍵按一下適當的伺服器，暫留在 [進階]  上以選取 [停用伺服器]  ，然後遵循精靈中的步驟進行。

4. 在遠端桌面管理伺服器的 [伺服器管理員]  中，將來源 RD 授權伺服器從部署中移除。

## <a name="migrate-certificates"></a>移轉憑證

若要成功移轉憑證，實際上需要同時移轉憑證和更新遠端桌面服務部署內容中的憑證資訊。

一般憑證移轉包括下列步驟：

- 使用私密金鑰將憑證匯出為 PFX 檔案。

- 從 PFX 檔案匯入憑證。

移轉適當的憑證之後，請在 [伺服器管理員] 或 PowerShell 中，更新遠端桌面服務部署所需的下列憑證：

- RD 連線代理人 - 單一登入

- RD 連線代理人 - RDP 檔案發佈

- RD 閘道 - HTTPS 連線

- RD Web 存取 - HTTPS 連線和 RemoteApp/桌面連線訂用帳戶
