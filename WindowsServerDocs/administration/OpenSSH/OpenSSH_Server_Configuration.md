---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, 安裝, 安裝程式
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: 適用于 Windows 的 OpenSSH 伺服器設定
ms.openlocfilehash: ed424c33c4cd2c19a9b5e985ab6083bcbcb9fbdc
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546258"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>適用于 Windows 10 1809 和伺服器2019的 OpenSSH 伺服器設定

本主題涵蓋適用于 OpenSSH 伺服器的 Windows 專屬設定 (sshd)。 

OpenSSH 會在[OpenSSH.com](https://www.openssh.com/manual.html)上維護線上設定選項的詳細檔, 這在此檔集中不會重複。 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>在 Windows 中設定 OpenSSH 的預設 shell

預設的命令 shell 提供使用者使用 SSH 連接到伺服器時所看到的體驗。 初始預設視窗是 Windows 命令 shell (cmd.exe)。 Windows 也包含 PowerShell 和 Bash, 而協力廠商命令 shell 也適用于 Windows, 而且可以設定為伺服器的預設 shell。

若要設定預設的命令 shell, 請先確認 OpenSSH 安裝資料夾位於系統路徑上。 對於 Windows, 預設安裝資料夾是 SystemDrive: WindowsDirectory\System32\openssh。 下列命令會顯示目前的路徑設定, 並在其中新增預設的 OpenSSH 安裝資料夾。 

命令 shell | 要使用的命令
------------- | -------------- 
命令 | path
PowerShell | $env:p 路徑 a)

在 Windows 登錄中設定預設的 ssh 命令介面, 方法是在字串值 DefaultShell 中, 將 shell 可執行檔的完整路徑新增至 Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH。 

例如, 下列 Powershell 命令會將預設 shell 設定為 PowerShell .exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Sshd_config 中的 Windows 設定 

在 Windows 中, sshd 預設會從%programdata%\ssh\sshd_config 讀取設定資料, 或藉由使用-f 參數啟動 sshd 來指定不同的設定檔。
如果檔案不存在, sshd 會在服務啟動時, 以預設設定產生一個檔案。

下面所列的元素可透過 sshd_config 中的專案, 提供 Windows 特有的設定。 這裡有其他可能沒有列出的設定, 因為它們在線上的[Win32 OpenSSH 檔](https://github.com/powershell/win32-openssh/wiki)中有詳細的說明。 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

使用 AllowGroups、AllowUsers、DenyGroups 和 DenyUsers 指示詞來控制哪些使用者和群組可以連接到伺服器。 允許/拒絕指示詞會依照下列連續處理:DenyUsers、AllowUsers、DenyGroups 和 finally AllowGroups。 所有的帳戶名稱都必須以小寫來指定。 如需萬用字元模式的詳細資訊, 請參閱 ssh_config 中的模式。

以網域使用者或群組設定使用者/群組型規則時, 請使用下列格式: ``` user?domain* ```。
Windows 允許多個格式來指定網域主體, 但與標準 Linux 模式有許多衝突。 基於這個理由, 會新增 * 來涵蓋 Fqdn。 此外, 此方法會使用 "？", 而不是 @, 以避免與username@host格式發生衝突。 

工作群組使用者/群組和網際網路線上帳戶一律會解析為其本機帳戶名稱 (沒有網域部分, 類似于標準 Unix 名稱)。 網域使用者和群組嚴格解析為[NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format)格式-domain_short_name\user_name。 所有以使用者/群組為基礎的設定規則都必須遵守此格式。

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

對於 Windows OpenSSH, 唯一可用的驗證方法為 "password" 和 "publickey"。

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

預設值為 ". ssh/authorized_keys/authorized_keys2"。 如果路徑不是絕對路徑, 則會相對於使用者的主目錄 (或設定檔映射路徑) 來取得。 Ex. c:\users\user.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (在 v 7.7.0.0 中新增支援)

只有 sftp 會話才支援這個指示詞。 Cmd.exe 的遠端會話不會接受這種情況。 若要設定僅限 sftp 的 chroot 伺服器, 請將 ForceCommand 設為 internal-sftp。 您也可以藉由執行只允許 scp 和 sftp 的自訂 shell, 以使用 chroot 設定 scp。

### <a name="hostkey"></a>Hostkey.cer

預設值為% programdata%/ssh/ssh_host_ecdsa_key、% programdata%/ssh/ssh_host_ed25519_key 和% programdata%/ssh/ssh_host_rsa_key。 如果預設值不存在, sshd 會在服務啟動時自動產生它們。

### <a name="match"></a>比對

請注意, 本節中的模式規則。 使用者和組名的大小寫應該是小寫。

### <a name="permitrootlogin"></a>PermitRootLogin

不適用於 Windows。 若要防止系統管理員登入, 請使用具有 DenyGroups 指示詞的管理員。

### <a name="syslogfacility"></a>SyslogFacility

如果您需要以檔案為基礎的記錄, 請使用 LOCAL0。 記錄檔會在%programdata%\ssh\logs. 下產生
任何其他值 (包括預設值 AUTH) 都會將記錄導向至 ETW。 如需詳細資訊, 請參閱 Windows 中的記錄功能。

### <a name="not-supported"></a>不支援 

在 Windows Server 2019 和 Windows 10 1809 隨附的 OpenSSH 版本中, 無法使用下列設定選項:

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

