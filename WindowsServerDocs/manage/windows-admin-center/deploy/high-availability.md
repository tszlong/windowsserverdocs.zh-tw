---
title: 部署具高可用性的 Windows Admin Center
description: 部署高可用性 （專案檀香山） 的 Windows Admin Center
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861059"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>部署具高可用性的 Windows Admin Center

>適用於：Windows Admin Center，Windows Admin Center 預覽

您可以部署 Windows Admin Center，在容錯移轉叢集中以提供高可用性，您的 Windows Admin Center 閘道服務。 提供的解決方案是主動-被動解決方案，其中只有一個執行個體的 Windows Admin Center 是作用中。 如果其中一個叢集中的節點失敗，Windows Admin Center 依正常程序會容錯移轉至另一個節點，可讓您繼續順暢地管理您的環境中的伺服器。 

[深入了解其他 Windows Admin Center 部署選項。](../plan/installation-options.md)

## <a name="prerequisites"></a>必要條件

- 在 Windows Server 2016 或 2019年 2 個或多個節點的容錯移轉叢集。 [深入了解部署容錯移轉叢集](../../../failover-clustering/failover-clustering-overview.md)。
- 叢集共用磁碟區 (CSV) 的 Windows Admin Center，來儲存可在叢集中的所有節點存取的永續性資料。 10 GB 足以符合您的 CSV。
- 高可用性部署指令碼，從[Windows Admin Center HA 指令碼 zip 檔案](https://aka.ms/WACHAScript)。 下載.zip 檔案包含到本機電腦的指令碼，然後複製指令碼視需要根據下列指導方針。
- 建議，但非必要： 已簽署的憑證.pfx 和密碼。 您不需要在叢集節點上已安裝憑證-指令碼會為您做的。 如果您未提供，安裝指令碼會產生自我簽署的憑證，會在 60 天後到期。

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>容錯移轉叢集上安裝 Windows Admin Center

1. 複製```Install-WindowsAdminCenterHA.ps1```指令碼，以您的叢集中的節點。 下載或將 Windows Admin Center.msi 複製到相同的節點。
2. 連線到透過 RDP 和執行節點```Install-WindowsAdminCenterHA.ps1```指令碼，從該節點，使用下列參數：
    - `-clusterStorage`： 在叢集共用磁碟區來儲存 Windows Admin Center 資料的本機路徑。
    - `-clientAccessPoint`： 選擇您要用以存取 Windows Admin Center 的名稱。 例如，如果您執行指令碼參數`-clientAccessPoint contosoWindowsAdminCenter`，您將會存取 Windows Admin Center 服務，請造訪 `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`:選擇性。 叢集中的泛型服務的一或多個靜態位址。 
    - `-msiPath`:Windows Admin Center.msi 檔案的路徑。
    - `-certPath`:選擇性。 憑證的.pfx 檔案的路徑。
    - `-certPassword`:選擇性。 SecureString 中提供的憑證.pfx 的密碼 `-certPath`
    - `-generateSslCert`:選擇性。 如果您不想提供的簽署的憑證，包含此參數旗標來產生自我簽署的憑證。 請注意的自我簽署的憑證，將在 60 天後到期。
    - `-portNumber`:選擇性。 如果您未指定連接埠，連接埠 443 (HTTPS) 上部署閘道服務。 若要使用不同的連接埠指定此參數中。 請注意，如果您使用自訂連接埠 （如果要保護除了 443)，您將會存取 Windows Admin Center 前往 https://\<clientAccessPoint\>:\<連接埠\>。

> [!NOTE]
> ```Install-WindowsAdminCenterHA.ps1```指令碼支援```-WhatIf ```和```-Verbose```參數

### <a name="examples"></a>範例

#### <a name="install-with-a-signed-certificate"></a>使用簽署的憑證安裝：

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>使用自我簽署憑證安裝：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>更新現有的高可用性安裝

使用相同```Install-WindowsAdminCenterHA.ps1```指令碼來更新您的 HA 部署，而不會遺失您連接的資料。

### <a name="update-to-a-new-version-of-windows-admin-center"></a>更新為新版本的 Windows Admin Center

新版本的 Windows Admin Center 發行時，只要執行```Install-WindowsAdminCenterHA.ps1```指令碼一次只含```msiPath```參數：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>更新 Windows Admin Center 所使用的憑證

您可以更新提供新的憑證.pfx 檔案的 Windows Admin Center HA 部署在任何時間使用的憑證和與密碼。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

您也可以在相同的時間，您使用新的.msi 檔案來更新 Windows Admin Center 平台更新的憑證。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>解除安裝

若要從您的容錯移轉叢集的解除安裝的 Windows Admin Center HA 部署，傳遞```-Uninstall```參數來```Install-WindowsAdminCenterHA.ps1```指令碼。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>疑難排解

記錄檔會儲存在 CSV (比方說，C:\ClusterStorage\Volume1\temp) 的暫存資料夾。