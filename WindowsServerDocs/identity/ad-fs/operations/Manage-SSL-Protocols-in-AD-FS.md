---
title: "AD fs 管理 SSL 日 TLS 通訊協定，以配合密碼"
description: "Ad FS 2016 常見問題集"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 23055a1b727e4f1a6b1ccafdea9a410c6021c432
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2018
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>AD fs 管理 SSL 日 TLS 通訊協定，以配合密碼
下列文件會提供資訊停用，可讓某些 TLS 日 SSL 通訊協定及密碼套件 AD FS 使用的方式

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS 日 SSL SChannel，AD FS 中的密碼套件

安全通訊端層 (SSL) 與傳輸層級的安全性 (TLS) 的通訊協定，以提供安全通訊。  Active Directory 同盟服務會使用這些通訊協定進行通訊。  今天有數個版本的這些通訊協定。

Schannel 是安全性支援提供者 (SSP) 實作 SSL、 TLS 和 DTLS 網際網路標準驗證通訊協定。 安全性支援提供者介面 (SSPI) 是使用 windows 來執行的安全性相關的功能包括驗證 API。 常見介面數個安全性支援提供者 （層），包括 Schannel SSP.SSPI 功能

密碼套件是一組密碼編譯演算法。 TLS 日 SSL 通訊協定的 schannel SSP 實作使用密碼套件的演算法建立金鑰，並將資訊加密。 密碼套件指定演算法每個下列工作：

- 金鑰交換
- 大量加密
- 訊息驗證

AD FS 使用 Schannel.dll 執行安全通訊互動。  目前 AD FS 可支援的所有通訊協定與支援 Schannel.dll 密碼套件。

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>管理 TLS 日 SSL 通訊協定，以配合密碼
> [!IMPORTANT]
> 本節所述之告訴您如何修改登錄的步驟。 不過，如果您不正確修改登錄可能會發生嚴重問題。 因此，請務必小心執行這些步驟。 
> 
> 請注意變更預設的 SCHANNEL 安全性設定，可能會 break 或禁止特定戶端之間伺服器通訊。  這將會發生在必要安全通訊，不需要交涉通訊的通訊協定。
> 
> 如果您要套用的這些變更，他們必須套用到所有伺服器 AD FS 陣列中。  這些變更以後, 是需要重新開機。

在當今天和年齡、 強化伺服器及移除較舊或弱密碼套件變得許多組織的主要優先順序。  軟體套件可供使用，將測試您的伺服器，並提供這些通訊協定，以配合的詳細的資訊。  為了維持相容或達到安全評分、 移除或停用弱通訊協定或加密套件已成為必須。  如何讓或停用特定通訊協定與密碼套件本文件中的其餘部分將會提供指引。

下列登錄位於在同一個位置： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols。  使用 regedit 或 PowerShell 來支援這些通訊協定停用或密碼套件。

![登錄位置](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>讓和停用 SSL 2.0
使用下列登錄金鑰和他們值讓和 SSL 2.0 停用。

### <a name="enable-ssl-20"></a>讓 SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]「 DisabledByDefault 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]「 DisabledByDefault 「 = dword:00000000 

### <a name="disable-ssl-20"></a>停用 SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]「 DisabledByDefault 「 = dword:00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]「 DisabledByDefault 「 = dword:00000001

### <a name="using-powershell-to-disable-ssl-20"></a>若要停用 SSL 2.0 使用 PowerShell

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>讓和停用 SSL 3.0
使用下列登錄金鑰和他們值讓和 SSL 3.0 停用。

### <a name="enable-ssl-30"></a>讓 SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]「 DisabledByDefault 「 = dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]「 DisabledByDefault 「 = dword:00000000 

### <a name="disable-ssl-30"></a>停用 SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]「 DisabledByDefault 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]「 DisabledByDefault 「 = dword:00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>若要停用 SSL 3.0 使用 PowerShell

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>讓和停用 TLS 1.0
使用下列登錄金鑰和他們值讓和 TLS 1.0 停用。

> [!IMPORTANT]
> 停用 TLS 1.0 會中斷 WAP AD FS 信任。  如果您要停用 TLS 1.0 您應該讓穩固驗證您的應用程式。  查看[讓穩固驗證](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>讓 TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]「 DisabledByDefault 「 = dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]「 DisabledByDefault 「 = dword:00000000 

### <a name="disable-tls-10"></a>停用 TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]「 DisabledByDefault 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]「 DisabledByDefault 「 = dword:00000001 

### <a name="using-powershell-to-disable-tls-10"></a>若要停用 TLS 1.0 使用 PowerShell

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>讓和 TLS 1.1 停用
使用下列登錄金鑰和他們值讓和 TLS 1.1 停用。

### <a name="enable-tls-11"></a>讓 TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]「 DisabledByDefault 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]「 DisabledByDefault 「 = dword:00000000

### <a name="disable-tls-11"></a>停用 TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]「 DisabledByDefault 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]「 DisabledByDefault 「 = dword:00000001 

### <a name="using-powershell-to-disable-tls-11"></a>若要停用 TLS 1.1 使用 PowerShell

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>讓和停用 TLS 1.2

使用下列登錄金鑰和他們值讓和 TLS 1.2 停用。

### <a name="enable-tls-12"></a>讓 TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]「 DisabledByDefault 「 = dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]「 DisabledByDefault 「 = dword:00000000

### <a name="disable-tls-12"></a>停用 TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]「 DisabledByDefault 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]「 DisabledByDefault 「 = dword:00000001

### <a name="using-powershell-to-disable-tls-12"></a>若要停用 TLS 1.2 使用 PowerShell

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>讓和停用 RC4 

使用下列登錄金鑰和他們值讓和 RC4 停用。  以下位於這個密碼套件的登錄鍵：

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![登錄位置](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>讓 RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128 日 128]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40 日 128]「 支援 「 = dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56 日 128]「 支援 「 = dword:00000001 

### <a name="disable-rc4"></a>停用 RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128 日 128]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40 日 128]「 支援 「 = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56 日 128]「 支援 「 = dword:00000000 

### <a name="using-powershell"></a>使用 PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>讓或停用加密其他套件

您可以將某些特定加密停用來將它們從 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\0010002 移除 

![登錄位置](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

若要讓密碼套件，，它是字串值多字串值功能鍵。  例如，如果我們想要讓 TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521 然後我們會將它新增字串。

支援的密碼的完整清單套件看到[在 TLS SSL (Schannel SSP) 的編碼器套件](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)。  本文件會提供的套件，依預設，以及支援但預設不支援的支援。  優先順序密碼套件查看[設定優先順序 Schannel 密碼套件](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)。

## <a name="enabling-strong-authentication-for-net-applications"></a>讓強驗證.NET 應用程式
.NET Framework 3.5/4.0/4.5.x 應用程式可以切換成 TLS 1.2 預設的通訊協定，進而 SchUseStrongCrypto 登錄金鑰。  這個登錄機碼會強制.NET 應用程式使用 TLS 1.2。

> [!IMPORTANT]
> 您需要針對 Windows Server 2016 和 Windows Server 2012 R2 上 AD FS 使用的.NET Framework 4.5.x 4.0 日金鑰： HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\。NETFramework\v4.0.30319


.NET Framework 3.5 使用下列機碼：

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\。NETFramework\v2.0.50727] 」 SchUseStrongCrypto 」 = dword:00000001

.NET Framework 4.5.x 4.0 日使用下列機碼： HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\。「 SchUseStrongCrypto 「 NETFramework\v4.0.30319 = dword:00000001

![穩固驗證](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>其他資訊

- [在 TLS SSL (Schannel SSP) 的編碼器套件](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [在 Windows 8.1 中 TLS 密碼套件](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [設定優先順序 Schannel 密碼套件](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [談到加密和其他 Enigmatic tongues](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
