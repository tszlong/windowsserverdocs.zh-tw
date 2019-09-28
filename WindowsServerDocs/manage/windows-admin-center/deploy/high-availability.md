---
title: 部署具有高可用性的 Windows 管理中心
description: 部署具有高可用性的 Windows 管理中心（Project 檀香山）
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406945"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>部署具有高可用性的 Windows 管理中心

>適用於：Windows Admin Center、Windows Admin Center 預覽版

您可以在容錯移轉叢集中部署 Windows 系統管理中心，為您的 Windows 管理中心閘道服務提供高可用性。 提供的解決方案是主動-被動解決方案，其中只有一個 Windows 管理中心的實例為作用中。 如果叢集中的其中一個節點失敗，Windows 系統管理中心會適當地故障轉到另一個節點，讓您在環境中繼續以順暢的方式管理伺服器。 

[瞭解其他 Windows 管理中心部署選項。](../plan/installation-options.md)

## <a name="prerequisites"></a>必要條件

- Windows Server 2016 或2019上2個或多個節點的容錯移轉叢集。 [深入瞭解如何部署容錯移轉](../../../failover-clustering/failover-clustering-overview.md)叢集。
- 適用于 Windows 管理中心的叢集共用磁片區（CSV），用來儲存可供叢集中所有節點存取的持續性資料。 10 GB 就足以用於您的 CSV。
- 來自[Windows 管理中心 HA 腳本 zip](https://aka.ms/WACHAScript)檔案的高可用性部署腳本。 將包含腳本的 .zip 檔案下載至您的本機電腦，然後視需要根據下列指引複製腳本。
- 建議使用，但選擇性：已簽署的憑證 .pfx & 密碼。 您不需要在叢集節點上安裝憑證，腳本會為您執行此動作。 如果您沒有提供，安裝腳本會產生自我簽署的憑證，這會在60天后過期。

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>在容錯移轉叢集上安裝 Windows 系統管理中心

1. 將 ```Install-WindowsAdminCenterHA.ps1``` 腳本複製到叢集中的節點。 將 Windows Admin Center .msi 下載或複製到相同的節點。
2. 透過 RDP 連接到節點，並使用下列參數從該節點執行 @no__t 0 腳本：
    - `-clusterStorage`：用來儲存 Windows 管理中心資料之叢集共用磁碟區的本機路徑。
    - `-clientAccessPoint`：選擇您要用來存取 Windows 系統管理中心的名稱。 例如，如果您使用參數執行腳本 `-clientAccessPoint contosoWindowsAdminCenter`，您將會造訪 `https://contosoWindowsAdminCenter.<domain>.com` 來存取 Windows Admin Center 服務
    - `-staticAddress`:選擇性。 叢集一般服務的一或多個靜態位址。 
    - `-msiPath`:Windows Admin Center .msi 檔案的路徑。
    - `-certPath`:選擇性。 憑證 .pfx 檔案的路徑。
    - `-certPassword`:選擇性。 @No__t 中提供的 .pfx 憑證 SecureString 密碼-0
    - `-generateSslCert`:選擇性。 如果您不想要提供已簽署的憑證，請包含此參數旗標來產生自我簽署憑證。 請注意，自我簽署憑證將于60天后過期。
    - `-portNumber`:選擇性。 如果您未指定埠，閘道服務會部署在埠443（HTTPS）。 若要使用不同的埠，請在此參數中指定。 請注意，如果您使用自訂埠（443以外的任何專案），您將會前往 HTTPs://\<clientAccessPoint @ no__t-1： \<port @ no__t-3 來存取 Windows 系統管理中心。

> [!NOTE]
> @No__t-0 腳本支援 ```-WhatIf ``` 和 @no__t 2 參數

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

使用相同的 ```Install-WindowsAdminCenterHA.ps1``` 腳本來更新 HA 部署，而不會遺失您的連接資料。

### <a name="update-to-a-new-version-of-windows-admin-center"></a>更新至新版本的 Windows 管理中心

發行新版本的 Windows 管理中心時，只要使用 ```msiPath``` 參數，就能再次執行 ```Install-WindowsAdminCenterHA.ps1``` 腳本：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>更新 Windows 管理中心所使用的憑證

您可以提供新憑證的 .pfx 檔案和密碼，隨時更新 Windows 管理中心的 HA 部署所使用的憑證。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

您也可以在使用新的 .msi 檔案更新 Windows Admin Center 平臺時，同時更新憑證。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>解除安裝

若要從容錯移轉叢集卸載 Windows 系統管理中心的 HA 部署，請將 ```-Uninstall``` 參數傳遞給 ```Install-WindowsAdminCenterHA.ps1``` 腳本。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>疑難排解

記錄檔會儲存在 CSV 的 temp 資料夾中（例如，C:\ClusterStorage\Volume1\temp）。