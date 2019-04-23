---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH、 SSH、 SSHD，安裝，安裝程式
contributor: maertendMSFT
author: maertendMSFT
title: 安裝適用於 Windows 的 OpenSSH
ms.openlocfilehash: f617b01ee7dabd4897f99e374420f673e209e145
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859559"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>安裝適用於 Windows Server 2019 和 Windows 10 的 OpenSSH #

OpenSSH 用戶端和 OpenSSH 伺服器是在 Windows Server 2019 和 Windows 10 1809年可個別安裝的元件。
使用這些 Windows 版本的使用者應該使用以安裝和設定 OpenSSH 的指示。 

> [!NOTE] 
> 從 PowerShell Github 存放庫取得 OpenSSH 的使用者 (https://github.com/PowerShell/OpenSSH-Portable)應該使用的指示，從該處，並__不應該__使用這些指示。 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>從 設定在 Windows Server 2019 或 Windows 10 1809 UI 安裝 OpenSSH

OpenSSH 用戶端和伺服器位於 Windows 10 1809年可安裝功能。 

若要安裝 OpenSSH，開始設定，然後移至應用程式 > 應用程式和功能 > 管理選擇性功能。 

掃描是否已安裝 OpenSSH 用戶端，這份清單。 如果沒有，然後在頁面頂端選取 [新增功能]，然後： 

* 若要安裝 OpenSSH 用戶端，找出 「 OpenSSH 用戶端 」，然後按一下 [安裝]。 
* 若要安裝 OpenSSH 伺服器時，找出 「 OpenSSH 伺服器 」，然後按一下 [安裝]。 

安裝完成後，返回應用程式 > 應用程式和功能 > 管理選擇性功能，您應該會看到列出的 OpenSSH 元件。

> [!NOTE]
> 安裝 OpenSSH 伺服器後，會建立，並啟用名為"OpenSSH 伺服器-在-TCP 」 的防火牆規則。 這可讓輸入連接埠 22 上的 SSH 流量。 

## <a name="installing-openssh-with-powershell"></a>使用 PowerShell 來安裝 OpenSSH 

若要安裝 OpenSSH 使用 PowerShell，先以系統管理員身分啟動 PowerShell。
若要確定 OpenSSH 功能可供安裝：

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

然後，安裝的伺服器和/或用戶端功能：

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Both of these should return the following output:

Path          :
Online        : True
RestartNeeded : False
```

## <a name="uninstalling-openssh"></a>解除安裝 OpenSSH

若要解除安裝使用 Windows 設定的 OpenSSH、 啟動設定，然後移至應用程式 > 應用程式和功能 > 管理選擇性功能。 在已安裝的功能清單中，選取 OpenSSH 用戶端或 OpenSSH 伺服器 元件，然後選取 解除安裝。

若要解除安裝 OpenSSH 使用 PowerShell，請使用其中一個下列的命令：

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

移除 OpenSSH，如果服務正在使用中時它已解除安裝之後，可能需要的 Windows 重新啟動。


## <a name="initial-configuration-of-ssh-server"></a>SSH 伺服器的初始設定

若要設定為初次使用 OpenSSH 伺服器在 Windows 上，啟動 PowerShell，身為管理員，然後執行下列命令來啟動 SSHD 服務：

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>初次使用 SSH

當在 Windows 上安裝 OpenSSH 伺服器之後時，您可以快速測試使用從任何 Windows 裝置的 PowerShell，使用 SSH 用戶端安裝。 在 PowerShell 中輸入下列命令： 

```powershell
Ssh username@servername
```

第一個連接到任何伺服器會產生類似下面的訊息：

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

答案必須是"yes"no"。 回答 [是] 會將該伺服器新增到本機系統的清單中的已知的 ssh 主機。

您將會提示輸入密碼此時。 為了安全起見，當您輸入系統不會顯示您的密碼。 

連線之後，您會看到類似下列的命令殼層提示字元：

```
domain\username@SERVERNAME C:\Users\username>
```

Windows OpenSSH 伺服器所使用的預設殼層是 Windows 命令殼層。 

