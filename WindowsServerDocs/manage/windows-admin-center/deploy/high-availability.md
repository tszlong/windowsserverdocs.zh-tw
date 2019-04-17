---
title: 部署 Windows Admin Center 與高可用性
description: 部署 Windows Admin Center 與高可用性 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 802a7bd537cab22893abb7e6657c4be90346ef88
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2019
ms.locfileid: "9025030"
---
# 部署 Windows Admin Center 與高可用性

>適用於：Windows Admin Center、Windows Admin Center 預覽版

您可以為您的 Windows Admin Center 閘道服務提供高可用性容錯移轉叢集中部署 Windows Admin Center。 提供的解決方案是主動被動的解決方案，其中只有一個執行個體的 Windows Admin Center 是作用中。 如果其中一個叢集節點失敗時，Windows Admin Center 妥善容錯移轉至另一個節點，讓您繼續順暢地管理您的環境中的伺服器。 

[深入了解其他 Windows Admin Center 部署選項。](../plan/installation-options.md)

## 必要條件

- 在 Windows Server 2016 或 2019年容錯移轉叢集的 2 個或多個節點。 [深入了解有關部署容錯移轉叢集](../../../failover-clustering/failover-clustering-overview.md)。
- 叢集共用磁碟區 (CSV) 來儲存永續性資料可以由叢集中的所有節點存取 Windows Admin center。 會不足以供您 CSV 10 GB。
- 從[Windows Admin Center HA 指令碼 zip 檔案](https://aka.ms/WACHAScript)的高可用性部署指令碼。 下載包含指令碼以您的本機電腦的.zip 檔案，然後複製指令碼視根據下列指導方針。
- 建議，但選用： 已簽署的憑證.pfx & 密碼。 您不需要已經在叢集節點上安裝憑證-指令碼將可為您做到的。 如果您不提供其中一個，安裝指令碼會產生自我簽署的憑證，在 60 天後到期。

## 容錯移轉叢集上安裝 Windows Admin Center

1. 複製```Install-WindowsAdminCenterHA.ps1```指令碼中您的叢集的節點。 下載，或將 Windows Admin Center.msi 複製到相同的節點。
2. 透過 RDP 和執行的節點連線```Install-WindowsAdminCenterHA.ps1```指令碼從該節點，使用以下參數：
    - `-clusterStorage`: 「 叢集共用磁碟區將 Windows Admin Center 資料儲存的本機路徑。
    - `-clientAccessPoint`： 選擇的名稱，您將用來存取 Windows Admin Center。 例如，如果您執行指令碼與參數`-clientAccessPoint contosoWindowsAdminCenter`，您將會瀏覽來存取 Windows Admin Center 服務 `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`： 選擇性的。 一或多個叢集泛型服務的靜態位址。 
    - `-msiPath`： Windows Admin Center.msi 檔案的路徑。
    - `-certPath`： 選擇性的。 憑證.pfx 檔案的路徑。
    - `-certPassword`： 選擇性的。 中所提供憑證.pfx SecureString 密碼 `-certPath`
    - `-generateSslCert`： 選擇性的。 如果您不想要提供的簽署的憑證，包括此參數旗標，以產生自我簽署的憑證。 請注意自我簽署的憑證將會在 60 天後到期。
    - `-portNumber`： 選擇性的。 如果您沒有指定連接埠，連接埠 443 (HTTPS) 上部署閘道服務。 若要使用不同的連接埠此參數中指定。 請注意，是否您使用自訂的連接埠 （任何項目除了 443），您將會存取 Windows Admin Center 移至 https://\<clientAccessPoint\>:\<port\>。

> [!NOTE]
> ```Install-WindowsAdminCenterHA.ps1```支援的指令碼```-WhatIf ```和```-Verbose```參數

### 範例

#### 安裝已簽署的憑證：

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### 安裝使用自我簽署憑證：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## 更新現有的高可用性安裝

使用相同```Install-WindowsAdminCenterHA.ps1```指令碼來更新您的 HA 部署，而不會遺失您連線的資料。

### 更新至新版本的 Windows Admin Center

當 Windows Admin Center 的新版本發行時，只需執行```Install-WindowsAdminCenterHA.ps1```一次只包含指令碼```msiPath```參數：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### 更新使用 Windows Admin Center 的憑證

您可以更新使用 Windows Admin Center 的 HA 部署在任何時候藉由提供新的憑證.pfx 檔案的憑證和密碼。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

您也可能會在您的新.msi 檔案來更新 Windows Admin Center 平台的同時更新憑證。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## Uninstall

若要從您的容錯移轉叢集，解除安裝 Windows Admin Center 的 HA 部署，傳遞```-Uninstall```參數，以```Install-WindowsAdminCenterHA.ps1```指令碼。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## 疑難排解

在 CSV (例如，C:\ClusterStorage\Volume1\temp) 暫存資料夾中儲存記錄檔。