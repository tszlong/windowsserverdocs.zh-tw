---
title: 適用於 Windows 的 OpenSSH 伺服器設定
description: 與適用於 Windows 10 1809 和 Server 2019 的 OpenSSH 伺服器有關的設定。
ms.date: 09/27/2018
ms.topic: conceptual
ms.author: damaerte
author: maertendmsft
ms.openlocfilehash: 99947390b698da22d4ab2ba497ae02222b523eb7
ms.sourcegitcommit: 8c0a419ae5483159548eb0bc159f4b774d4c3d85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235795"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>適用於 Windows 10 1809 和 Server 2019 的 OpenSSH 伺服器設定

本主題涵蓋適用於 OpenSSH 伺服器 (sshd) 的 Windows 專屬設定。

OpenSSH 會在線上 ([OpenSSH.com](https://www.openssh.com/manual.html)) 維護設定選項的詳細文件，在本文件集當中不會重複。

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>在 Windows 中設定 OpenSSH 的預設殼層

預設的命令殼層提供使用者使用 SSH 連線到伺服器時所看到的體驗。
初始預設視窗是 Windows 命令殼層 (cmd.exe)。
Windows 也包含 PowerShell 和 Bash，而第三方命令殼層也適用於 Windows，而且可以設定為伺服器的預設殼層。

若要設定預設的命令殼層，請先確認 OpenSSH 安裝資料夾位於系統路徑上。
若為 Windows，預設安裝資料夾是 SystemDrive:WindowsDirectory\System32\openssh。
下列命令會顯示目前的路徑設定，並在其中新增預設的 OpenSSH 安裝資料夾。

命令殼層 | 要使用的命令
------------- | --------------
命令 | 路徑
PowerShell | $env:path

在 Windows 中設定預設 ssh 殼層，是藉由將殼層可執行檔的完整路徑新增至 DefaultShell 字串值的 Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH 中來完成。

例如，下列 Powershell 命令會將預設殼層設定為 PowerShell.exe：

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>sshd_config 中的 Windows 設定

在 Windows 中，sshd 預設會從 %programdata%\ssh\sshd_config 讀取設定資料，或者藉由使用 -f 參數啟動 sshd.exe 來指定不同的設定檔。
如果檔案不存在，sshd 會在服務啟動時，以預設設定產生一個檔案。

以下列出的元素可透過 sshd_config 中的項目，提供 Windows 特有的設定。
還有其他可行的組態設定並未在這裡列出，它們在線上 [Win32 OpenSSH 文件](https://github.com/powershell/win32-openssh/wiki)中有詳細說明。


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups，AllowUsers，DenyGroups，DenyUsers

使用 AllowGroups、AllowUsers、DenyGroups 和 DenyUsers 指示詞來控制哪些使用者和群組可以連線到伺服器。
允許/拒絕指示詞會依照下列順序處理：DenyUsers、AllowUsers、DenyGroups，最後是 AllowGroups。
所有帳戶名稱都必須以小寫來指定。
如需萬用字元模式的詳細資訊，請參閱 ssh_config 中的「模式」。

以網域使用者或群組設定使用者/群組型規則時，請使用下列格式：``` user?domain* ```。
Windows 允許使用多種格式來指定網域主體，但是許多格式與標準 Linux 模式有衝突。
基於這個理由，會新增 * 來涵蓋 FQDN。
此外，這種方法會使用 "?"，而不是 @，以避免與 username@host 格式發生衝突。

工作群組使用者/群組和網際網路連線帳戶一律會解析為其本機帳戶名稱 (沒有網域部分，類似於標準 Unix 名稱)。
網域使用者和群組會嚴格地解析為 [NameSamCompatible](/windows/desktop/api/secext/ne-secext-extended_name_format) 格式 - domain_short_name\user_name。
所有以使用者/群組為基礎的設定規則都必須遵守此格式。

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

對於 Windows OpenSSH，唯一可用的驗證方法為 "password" 和 "publickey"。

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile

預設值為 ".ssh/authorized_keys .ssh/authorized_keys2"。 如果路徑不是絕對路徑，則會相對於使用者的主目錄 (或設定檔映像路徑) 來取得。 例如： c:\users\user。 請注意，如果使用者屬於系統管理員群組，則會改用 %programdata%/ssh/administrators_authorized_keys。

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (在 v7.7.0.0 中新增支援)

只有 sftp 工作階段才支援這個指示詞。 cmd.exe 的遠端工作階段不接受這個指示詞。 若要設定僅限 sftp 的 chroot 伺服器，請將 ForceCommand 設為 internal-sftp。 您也可以藉由實作只允許 scp 和 sftp 的自訂殼層，以使用 chroot 設定 scp。

### <a name="hostkey"></a>HostKey

預設值為 %programdata%/ssh/ssh_host_ecdsa_key、%programdata%/ssh/ssh_host_ed25519_key、%programdata%/ssh/ssh_host_dsa_key 和 %programdata%/ssh/ssh_host_rsa_key。 如果預設值不存在，sshd 會在服務啟動時自動產生。

### <a name="match"></a>比對

請注意，在本節中以該模式為準。 使用者和群組名稱應該是小寫。

### <a name="permitrootlogin"></a>PermitRootLogin

在 Windows 中不適用。 若要防止系統管理員登入，請使用具有 DenyGroups 指示詞的系統管理員。

### <a name="syslogfacility"></a>SyslogFacility

如果您需要以檔案為基礎的記錄，請使用 LOCAL0。 記錄檔會在 %programdata%\ssh\logs 下產生
其他任何值 (包括預設值 AUTH) 都會將記錄導向至 ETW。 如需詳細資訊，請參閱 [Windows 中的「記錄設施」](https://github.com/PowerShell/Win32-OpenSSH/wiki/Logging-Facilities)。

### <a name="not-supported"></a>不受支援

在 Windows Server 2019 和 Windows 10 1809 隨附的 OpenSSH 版本中，無法使用下列設定選項：

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
