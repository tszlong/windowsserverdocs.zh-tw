---
title: 部署具有高可用性的 Windows Admin Center
description: 部署具有高可用性的 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "71406945"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>部署具有高可用性的 Windows Admin Center

>適用於：Windows Admin Center、Windows Admin Center 預覽版

您可以在容錯移轉叢集中部署 Windows Admin Center，為您的 Windows Admin Center 閘道服務提供高可用性。 提供的解決方案是主動-被動解決方案，其中只有一個作用中的 Windows Admin Center 執行個體。 如果叢集中的其中一個節點失敗，則 Windows Admin Center 會適當地容錯移轉到另一個節點，讓您繼續順暢地管理環境中的伺服器。 

[了解其他 Windows Admin Center 部署選項。](../plan/installation-options.md)

## <a name="prerequisites"></a>必要條件

- Windows Server 2016 或 2019 上 2 個以上節點的容錯移轉叢集。 [深入了解如何部署容錯移轉叢集](../../../failover-clustering/failover-clustering-overview.md)。
- 適用於 Windows Admin Center 的叢集共用磁碟區 (CSV)，用於儲存可供叢集中所有節點存取的持續性資料。 10 GB 就足夠您的 CSV 使用。
- [Windows Admin Center HA 指令碼 zip 檔案](https://aka.ms/WACHAScript)中的高可用性部署指令碼。 將包含指令碼的 .zip 檔案下載至您的本機電腦，然後視需要根據以下指導方針複製指令碼。
- 建議事項，但為選擇性：已簽署的憑證 .pfx 和密碼。 您不必已經在叢集節點上安裝憑證，指令碼會為您執行此作業。 如果您未提供憑證，安裝指令碼會產生自我簽署的憑證，其會在 60 天後過期。

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>在容錯移轉叢集上安裝 Windows Admin Center

1. 將 ```Install-WindowsAdminCenterHA.ps1``` 指令碼複製到叢集中的節點。 將 Windows Admin Center .msi 下載或複製到相同的節點。
2. 透過 RDP 連線到節點，並使用下列參數從該節點執行 ```Install-WindowsAdminCenterHA.ps1``` 指令碼：
    - `-clusterStorage`：叢集共用磁碟區的本機路徑，其用於儲存 Windows Admin Center 資料。
    - `-clientAccessPoint`：選擇您將用來存取 Windows Admin Center 的名稱。 例如，如果您使用 `-clientAccessPoint contosoWindowsAdminCenter` 參數執行指令碼，您將藉由造訪 `https://contosoWindowsAdminCenter.<domain>.com` 來存取 Windows Admin Center 服務
    - `-staticAddress`:選擇性。 叢集一般服務的一或多個靜態位址。 
    - `-msiPath`:Windows Admin Center .msi 檔案的路徑。
    - `-certPath`:選擇性。 憑證 .pfx 檔案的路徑。
    - `-certPassword`:選擇性。 在 `-certPath` 中所提供憑證 .pfx 的 SecureString 密碼
    - `-generateSslCert`:選擇性。 如果您不想提供已簽署的憑證，請包含此參數旗標以產生自我簽署的憑證。 請注意，自我簽署的憑證將於 60 天後過期。
    - `-portNumber`:選擇性。 如果您未指定連接埠，閘道服務會部署於連接埠 443 (HTTPS)。 若要使用不同的連接埠，請在此參數中指定。 請注意，如果您使用自訂連接埠 (443 以外的連接埠)，您將前往 https://\<clientAccessPoint\>:\<port\> 來存取 Windows Admin Center。

> [!NOTE]
> ```Install-WindowsAdminCenterHA.ps1``` 指令碼支援 ```-WhatIf ``` 和 ```-Verbose``` 參數

### <a name="examples"></a>範例

#### <a name="install-with-a-signed-certificate"></a>使用簽署的憑證安裝：

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>使用自我簽署的憑證安裝：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>更新現有的高可用性安裝

使用相同的 ```Install-WindowsAdminCenterHA.ps1``` 指令碼來更新 HA 部署，而不會遺失您的連線資料。

### <a name="update-to-a-new-version-of-windows-admin-center"></a>更新至新版本的 Windows Admin Center

在新版 Windows Admin Center 發行後，只需使用 ```msiPath``` 參數再次執行 ```Install-WindowsAdminCenterHA.ps1``` 指令碼：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>更新 Windows Admin Center 所使用的憑證

您可以提供新憑證的 .pfx 檔案和密碼，隨時更新 Windows Admin Center 的 HA 部署所使用的憑證。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

您也可以在使用新的 .msi 檔案更新 Windows Admin Center 平台時，同時更新此憑證。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>解除安裝

若要從容錯移轉叢集解除安裝 Windows Admin Center 的 HA 部署，請將 ```-Uninstall``` 參數傳遞至 ```Install-WindowsAdminCenterHA.ps1``` 指令碼。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>疑難排解

記錄會儲存在 CSV 的 temp 資料夾中 (例如 C:\ClusterStorage\Volume1\temp)。