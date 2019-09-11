---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH，SSH，SSHD，安裝，安裝程式
contributor: maertendMSFT
author: maertendMSFT
title: 安裝適用于 Windows 的 OpenSSH
ms.openlocfilehash: 6a5d4d47fbb3f962c2a19582eb0a72810145a28c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866880"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>安裝適用于 Windows Server 2019 和 Windows 10 的 OpenSSH #

OpenSSH 用戶端和 OpenSSH 伺服器是 Windows Server 2019 和 Windows 10 1809 中可獨立安裝的元件。
具有這些 Windows 版本的使用者應該使用遵循的指示來安裝和設定 OpenSSH。 

> [!NOTE] 
> 從 PowerShell Github 存放庫取得 OpenSSH 的使用者（ https://github.com/PowerShell/OpenSSH-Portable) 應使用該處的指示，且__不應__使用這些指示。 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>從 Windows Server 2019 或 Windows 10 1809 上的設定 UI 安裝 OpenSSH

OpenSSH 用戶端和伺服器是 Windows 10 1809 的可安裝功能。 

若要安裝 OpenSSH，請啟動設定，然後移至 [應用程式] > [應用程式和功能] > 管理選擇性功能。 

掃描這份清單，查看是否已安裝 OpenSSH 用戶端。 如果沒有，請在頁面頂端選取 [新增功能]，然後： 

* 若要安裝 OpenSSH 用戶端，請找出 [OpenSSH 用戶端]，然後按一下 [安裝]。 
* 若要安裝 OpenSSH 伺服器，請找出「OpenSSH 伺服器」，然後按一下 [安裝]。 

安裝完成後，請回到 應用程式 > 應用程式和功能 > 管理選用功能，您應該會看到列出的 OpenSSH 元件。

> [!NOTE]
> 安裝 OpenSSH 伺服器將會建立並啟用名為「OpenSSH-伺服器-TCP」的防火牆規則。 這允許埠22上的輸入 SSH 流量。 

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

## <a name="uninstalling-openssh"></a>卸載 OpenSSH

若要使用 Windows 設定來卸載 OpenSSH，請啟動設定，然後移至 應用程式 > 應用程式和功能 > 管理選擇性功能。 在已安裝的功能清單中，選取 [OpenSSH 用戶端] 或 [OpenSSH 伺服器] 元件，然後選取 [卸載]。

若要使用 PowerShell 卸載 OpenSSH，請使用下列其中一個命令：

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

移除 OpenSSH 之後可能需要重新開機 Windows，如果服務在卸載時仍在使用中。


## <a name="initial-configuration-of-ssh-server"></a>SSH 伺服器的初始設定

若要設定 OpenSSH 伺服器以在 Windows 上首次使用，請以系統管理員身分啟動 PowerShell，然後執行下列命令以啟動 SSHD 服務：

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>第一次使用 SSH

在 Windows 上安裝 OpenSSH 伺服器後，您可以使用 PowerShell，從已安裝 SSH 用戶端的任何 Windows 裝置快速測試它。 在 PowerShell 中輸入下列命令： 

```powershell
Ssh username@servername
```

第一次連接到任何伺服器時，將會產生類似下列的訊息：

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

答案必須是「是」或「否」。 回答 [是] 會將該伺服器新增到本機系統的已知 ssh 主機清單中。

此時，系統會提示您輸入密碼。 作為安全性預防措施，您的密碼不會在您輸入時顯示。 

連接之後，您會看到類似下面的命令 shell 提示字元：

```
domain\username@SERVERNAME C:\Users\username>
```

Windows OpenSSH 伺服器使用的預設 shell 是 Windows 命令 shell。 

