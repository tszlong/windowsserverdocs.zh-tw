---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH、 SSH、 SSHD，安裝，安裝程式
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Windows 的 OpenSSH 伺服器組態
ms.openlocfilehash: 7eff3d3e1af67c9daf7a68c67c3609c0ee89fc93
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280030"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>適用於 Windows 10 1809年和 Server 2019 # OpenSSH 伺服器組態

本主題涵蓋 OpenSSH 伺服器 (sshd) 之 Windows 特定的組態。 

OpenSSH 維護詳細文件以設定選項線上[OpenSSH.com](https://www.openssh.com/manual.html)，也就不重複本文件集中。 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>在 Windows 中的 OpenSSH 為設定的預設殼層

預設命令殼層會提供連接到使用 SSH 伺服器時，會看到使用者的體驗。 Windows 的初始預設值是 Windows 命令殼層 (cmd.exe)。 Windows 也會包含 PowerShell 和 Bash，以及協力廠商命令殼層也適用於 Windows，可能會設定為伺服器的預設殼層。

若要設定預設值命令殼層，請先確認 OpenSSH 安裝資料夾位於系統路徑。 對於 Windows，預設的安裝資料夾會是 SystemDrive:WindowsDirectory\System32\openssh。 下列命令會顯示目前的路徑設定，並加入預設 OpenSSH 的安裝資料夾。 

命令殼層 | 若要使用的命令
------------- | -------------- 
命令 | path
PowerShell | $env： 路徑

設定預設的 ssh 命令介面，即可在 Windows 登錄中的完整路徑加入殼層可執行檔 Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH DefaultShell 字串值中。 

例如，下列 Powershell 命令會設定為 PowerShell.exe 的預設殼層：

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshdconfig"></a>在 sshd_config Windows 設定 

在 Windows，sshd %programdata%\ssh\sshd_config 從讀取組態資料，根據預設，或可由啟動 sshd.exe-f 參數指定不同的組態檔。
如果檔案不存在，sshd 服務啟動時產生其中一個預設組態。

下面所列的項目會提供 Windows 特定組態可能透過 sshd_config 中的項目。 有其他組態設定中可能未在此處，列出如所述的線上詳細[Win32 OpenSSH 文件](https://github.com/powershell/win32-openssh/wiki)。 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups，AllowUsers，DenyGroups DenyUsers 

控制哪些使用者及群組可以連線到伺服器是使用 AllowGroups、 AllowUsers、 DenyGroups 和 DenyUsers 指示詞。 允許/拒絕指示詞會依下列順序處理：DenyUsers、 AllowUsers、 DenyGroups，和最後 AllowGroups。 在較低的情況下，必須指定所有的帳戶名稱。 如需有關模式的萬用字元 ssh_config 中看到模式。

當設定 使用者/群組為基礎的網域使用者或群組的規則時，請使用下列格式： ``` user?domain* ```。
Windows 提供的格式的多個指定網域主體，但許多與標準 Linux 模式衝突。 基於這個理由，* 加入涵蓋 Fqdn。 此外，這個方法會使用"？"，而不是 @，以避免 je v konfliktuusername@host格式。 

工作群組使用者/群組和網際網路連線的帳戶一律會解析為本機帳戶名稱 （沒有網域部分，類似於標準的 Unix 名稱） 中。 網域使用者和群組會嚴格地解析為[NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format)格式-domain_short_name\user_name。 所有的使用者/群組基礎的規則必須遵守這種格式的組態。

網域使用者和群組的範例 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

本機使用者和群組的範例 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

Windows OpenSSH 的唯一可用的驗證方法為 「 密碼 」 和 「 公開金鑰 」。

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

預設值為".ssh/authorized_keys.ssh/authorized_keys2"。 如果路徑不是絕對的它會採取相對於使用者的主目錄 （或設定檔影像路徑）。 Ex. c:\users\user。

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory （v7.7.0.0 中所新增的支援）

使用 sftp 工作階段才支援這個指示詞。 Cmd.exe 將遠端工作階段不會接受這。 若要設定的僅限 sftp chroot 伺服器，請將 ForceCommand 設內部 sftp。 您可能也設有 scp chroot，藉由實作自訂殼層，只允許 scp 和 sftp。

### <a name="hostkey"></a>HostKey

預設值是 %programdata%/ssh/ssh_host_ecdsa_key、 %programdata%/ssh/ssh_host_ed25519_key 和 %programdata%/ssh/ssh_host_rsa_key。 如果預設值不存在，sshd 會自動產生這些服務啟動。

### <a name="match"></a>比對

請注意，模式這一節的規則。 使用者和群組的名稱必須是小寫。

### <a name="permitrootlogin"></a>PermitRootLogin

在 Windows 中不適用。 若要避免系統管理員登入，請先 DenyGroups 指示詞中使用系統管理員。

### <a name="syslogfacility"></a>SyslogFacility

如果您需要檔案型的記錄，請使用 LOCAL0。 %Programdata%\ssh\logs 情況下，所產生記錄檔。
任何其他值，包括預設值驗證指示記錄到 ETW。 如需詳細資訊，請參閱 Windows 中的記錄功能。

### <a name="not-supported"></a>不支援 

下列組態選項不適用於隨附於 Windows Server 2019 和 Windows 10 1809年 OpenSSH 版本：

* AcceptEnv
* AllowStreamLocalForwarding
* AuthorizedKeysCommand
* AuthorizedKeysCommandUser
* AuthorizedPrincipalsCommand
* AuthorizedPrincipalsCommandUser
* 壓縮
* ExposeAuthInfo
* GSSAPIAuthentication
* GSSAPICleanupCredentials
* GSSAPIStrictAcceptorCheck
* HostbasedAcceptedKeyTypes
* HostbasedAuthentication
* HostbasedUsesNameFromPacketOnly
* IgnoreRhosts
* IgnoreUserKnownHosts
* KbdInteractiveAuthentication
* KerberosAuthentication
* KerberosGetAFSToken
* KerberosOrLocalPasswd
* KerberosTicketCleanup
* PermitTunnel
* PermitUserEnvironment
* PermitUserRC
* PidFile
* PrintLastLog
* RDomain
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictModes
* X11DisplayOffset
* X11Forwarding
* X11UseLocalhost
* XAuthLocation

