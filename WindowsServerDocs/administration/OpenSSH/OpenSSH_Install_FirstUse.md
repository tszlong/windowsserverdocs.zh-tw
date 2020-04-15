---
ms.date: 09/27/2019
ms.topic: conceptual
contributor: maertendMSFT
author: maertendmsft
title: 安裝適用於 Windows 的 OpenSSH
ms.openlocfilehash: b9889a9057a1ddd5181f4ea4aab35680d524eabf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852051"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>安裝適用於 Windows Server 2019 和 Windows 10 的 OpenSSH #

OpenSSH 用戶端和 OpenSSH 伺服器是 Windows Server 2019 和 Windows 10 1809 中可獨立安裝的元件。
具有這些 Windows 版本的使用者應該使用後續的指示來安裝和設定 OpenSSH。 

> [!NOTE] 
> 從 PowerShell GitHub 存放庫 (https://github.com/PowerShell/OpenSSH-Portable) ) 取得 OpenSSH 的使用者應該使用該處的指示，__不應該__使用這些指示。 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>從 Windows Server 2019 或 Windows 10 1809 上的設定 UI 安裝 OpenSSH

OpenSSH 用戶端和伺服器是 Windows 10 1809 的可安裝功能。 

若要安裝 OpenSSH，請啟動 [設定]，然後移至 [應用程式] > [應用程式和功能] > [管理選用功能]。 

掃描這份清單，查看是否已安裝 OpenSSH 用戶端。 如果沒有，請在頁面頂端選取 [新增功能]，然後： 

* 若要安裝 OpenSSH 用戶端，請找出 [OpenSSH 用戶端]，然後按一下 [安裝]。 
* 若要安裝 OpenSSH 伺服器，請找出 [OpenSSH 伺服器]，然後按一下 [安裝]。 

安裝完成之後，請回到 [應用程式] > [應用程式和功能] > [管理選用功能]，您應該會看到 OpenSSH 元件列出來。

> [!NOTE]
> 安裝 OpenSSH 伺服器將會建立並啟用名為 "OpenSSH-Server-In-TCP" 的防火牆規則。 這個規則會允許連接埠 22 上的輸入 SSH 流量。 

## <a name="installing-openssh-with-powershell"></a>使用 PowerShell 安裝 OpenSSH 

若要使用 PowerShell 安裝 OpenSSH，請先以系統管理員身分啟動 PowerShell。
若要確認 OpenSSH 功能可供安裝：

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

然後，安裝伺服器和/或用戶端功能：

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

若要解除安裝 OpenSSH，請使用 Windows 設定，啟動 [設定]，然後移至 [應用程式] > [應用程式和功能] > [管理選用功能]。 在已安裝的功能清單中，選取 [OpenSSH 用戶端] 或 [OpenSSH 伺服器] 元件，然後選取 [解除安裝]。

若要使用 PowerShell 解除安裝 OpenSSH，請使用下列其中一個命令：

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

如果服務在解除安裝時仍在使用中，則在移除 OpenSSH 之後可能需要重新啟動 Windows。


## <a name="initial-configuration-of-ssh-server"></a>SSH 伺服器的初始設定

若要設定 OpenSSH 伺服器以在 Windows 上首次使用，請以系統管理員身分啟動 PowerShell，然後執行下列命令以啟動 SSHD 服務：

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled
# If the firewall does not exist, create one
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

## <a name="initial-use-of-ssh"></a>初始使用 SSH

在 Windows 上安裝 OpenSSH 伺服器之後，您可以使用 PowerShell，從已安裝 SSH 用戶端的任何 Windows 裝置快速測試它。 在 PowerShell 中輸入下列命令： 

```powershell
Ssh username@servername
```

第一次連線到任何伺服器時，將會產生類似下列的訊息：

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

答案必須是「是」或「否」。 回答「是」會將該伺服器新增到本機系統的已知 ssh 主機清單中。

此時，系統會提示您輸入密碼。 作為安全性預防措施，您的密碼在您輸入時不會顯示。 

連線之後，您會看到類似下面的命令殼層提示字元：

```
domain\username@SERVERNAME C:\Users\username>
```

Windows OpenSSH 伺服器使用的預設殼層是 Windows 命令殼層。 

