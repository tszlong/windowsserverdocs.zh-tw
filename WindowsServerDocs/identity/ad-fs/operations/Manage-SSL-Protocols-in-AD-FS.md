---
title: 管理 AD FS 的 SSL/TLS 通訊協定和加密套件
description: AD FS 2016 的常見問題
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b97a9cb50743972a85826d10aba89f9e6fffb5a6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954460"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>管理 AD FS 的 SSL/TLS 通訊協定和加密套件
下列檔提供如何停用及啟用 AD FS 所使用的特定 TLS/SSL 通訊協定和加密套件的相關資訊。

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>AD FS 中的 TLS/SSL、SChannel 和加密套件

傳輸層安全性（TLS）和安全通訊端層（SSL）是提供安全通訊的通訊協定。  Active Directory 同盟服務會使用這些通訊協定進行通訊。  這些通訊協定現在有數個版本。

安全通道是安全性支援提供者 (SSP)，可以實作 SSL、TLS 及 DTLS 網際網路標準驗證通訊協定。 安全性支援提供者介面 (SSPI) 是 Windows 系統所使用的 API，可以執行包含驗證在內的安全性相關功能。 SSPI 可以做為包含安全通道 SSP 在內的數個安全性支援提供者 (SSP) 的常用介面來運作。

加密套件是一組密碼編譯演算法。 TLS/SSL 通訊協定的安全通道 SSP 執行會使用加密套件中的演算法來建立金鑰和加密資訊。 加密套件會為下列每個工作指定一種演算法：

- 金鑰交換
- 大量加密
- 訊息驗證

AD FS 使用 Schannel.dll 來執行其安全通訊互動。  目前 AD FS 支援 Schannel.dll 支援的所有通訊協定和加密套件。

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>管理的 TLS/SSL 通訊協定和加密套件
> [!IMPORTANT]
> 本節包含的步驟會告訴您如何修改登錄。 然而，如果您不當修改登錄，可能會發生嚴重的問題。 因此，請務必小心遵循下列步驟。 
> 
> 請注意，變更 SCHANNEL 的預設安全性設定可能會中斷或防止特定用戶端與伺服器之間的通訊。  如果需要安全通訊，而且沒有可與通訊的通訊協定，就會發生這種情況。
> 
> 如果您要套用這些變更，則必須將它們套用至伺服器陣列中的所有 AD FS 伺服器。  套用這些變更之後，需要重新開機。

在今天的日期和年齡中，強化伺服器並移除舊版或弱式加密套件，對於許多組織而言都是主要的優先考慮。  提供的軟體套件將會測試您的伺服器，並提供這些通訊協定和套件的詳細資訊。  為了保持相容或達到安全的評等，移除或停用較弱的通訊協定或加密套件已成為必須。  本檔的其餘部分將提供如何啟用或停用特定通訊協定和加密套件的指引。

下列登錄機碼位於相同的位置： HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols。  使用 regedit 或 PowerShell 來啟用或停用這些通訊協定和加密套件。

![登錄位置](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>啟用和停用 SSL 2。0
使用下列登錄機碼及其值來啟用和停用 SSL 2.0。

### <a name="enable-ssl-20"></a>啟用 SSL 2。0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 伺服器]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 伺服器]"DisabledByDefault" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 用戶端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 用戶端]"DisabledByDefault" = dword：00000000 

### <a name="disable-ssl-20"></a>停用 SSL 2。0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 伺服器]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 伺服器]"DisabledByDefault" = dword：00000001 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 用戶端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 用戶端]"DisabledByDefault" = dword：00000001

### <a name="using-powershell-to-disable-ssl-20"></a>使用 PowerShell 停用 SSL 2。0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>啟用和停用 SSL 3。0
使用下列登錄機碼及其值來啟用和停用 SSL 3.0。

### <a name="enable-ssl-30"></a>啟用 SSL 3.0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 伺服器]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 伺服器]"DisabledByDefault" = dword：00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 用戶端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 用戶端]"DisabledByDefault" = dword：00000000 

### <a name="disable-ssl-30"></a>停用 SSL 3.0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 伺服器]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 伺服器]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 用戶端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 用戶端]"DisabledByDefault" = dword：00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>使用 PowerShell 停用 SSL 3。0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>啟用和停用 TLS 1。0
使用下列登錄機碼及其值來啟用和停用 TLS 1.0。

> [!IMPORTANT]
> 停用 TLS 1.0 將會中斷 WAP 以 AD FS 信任。  如果您停用 TLS 1.0，您應該為您的應用程式啟用增強式驗證。  請參閱[啟用增強式驗證](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>啟用 TLS 1。0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 伺服器]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 伺服器]"DisabledByDefault" = dword：00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 用戶端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 用戶端]"DisabledByDefault" = dword：00000000 

### <a name="disable-tls-10"></a>停用 TLS 1.0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 伺服器]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 伺服器]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 用戶端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 用戶端]"DisabledByDefault" = dword：00000001 

### <a name="using-powershell-to-disable-tls-10"></a>使用 PowerShell 來停用 TLS 1。0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>啟用和停用 TLS 1。1
使用下列登錄機碼及其值來啟用和停用 TLS 1.1。

### <a name="enable-tls-11"></a>啟用 TLS 1。1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 伺服器]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 伺服器]"DisabledByDefault" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 用戶端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 用戶端]"DisabledByDefault" = dword：00000000

### <a name="disable-tls-11"></a>停用 TLS 1。1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 伺服器]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 伺服器]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 用戶端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 用戶端]"DisabledByDefault" = dword：00000001 

### <a name="using-powershell-to-disable-tls-11"></a>使用 PowerShell 來停用 TLS 1。1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>啟用和停用 TLS 1。2

使用下列登錄機碼及其值來啟用和停用 TLS 1.2。

### <a name="enable-tls-12"></a>啟用 TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000

### <a name="disable-tls-12"></a>停用 TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000001

### <a name="using-powershell-to-disable-tls-12"></a>使用 PowerShell 來停用 TLS 1。2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>啟用和停用 RC4 

使用下列登錄機碼及其值來啟用和停用 RC4。  此加密套件的登錄機碼位於此處：

- HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![登錄位置](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>啟用 RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]"Enabled" = dword：00000001 

### <a name="disable-rc4"></a>停用 RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]"Enabled" = dword：00000000 

### <a name="using-powershell"></a>使用 PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>啟用或停用其他加密套件

您可以從 HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 中移除特定的加密，藉以停用這些密碼 

![登錄位置](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

若要啟用加密套件，請將其字串值新增至函數多字串值索引鍵。  例如，如果我們想要啟用 TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521，則會將它新增至字串。

如需支援的加密套件的完整清單，請參閱[TLS/SSL （安全通道 SSP）中的加密套件](/windows/win32/secauthn/cipher-suites-in-schannel)。  本檔提供預設啟用的套件清單，以及預設支援但未啟用的套件。  若要設定加密套件的優先順序，請參閱設定[Schannel 加密套件的優先順序](/windows/win32/secauthn/prioritizing-schannel-cipher-suites)。

## <a name="enabling-strong-authentication-for-net-applications"></a>為 .NET 應用程式啟用增強式驗證
.NET Framework 3.5/4.0/4.5. x 應用程式可以藉由啟用 SchUseStrongCrypto 登錄機碼，將預設通訊協定切換成 TLS 1.2。  此登錄機碼會強制 .NET 應用程式使用 TLS 1.2。

> [!IMPORTANT]
> 針對 Windows Server 2016 和 Windows Server 2012 R2 上的 AD FS，您必須使用 .NET Framework 4.0/4.5. x 金鑰： HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ 。NETFramework\v4.0.30319


針對 .NET Framework 3.5，請使用下列登錄機碼：

[HKEY_LOCAL_MACHINE \SOFTWARE\Wow6432Node\Microsoft \\ 。NETFramework\v2.0.50727] "SchUseStrongCrypto" = dword：00000001

針對 .NET Framework 4.0/4.5. x 使用下列登錄機碼： HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ 。NETFramework\v4.0.30319 "SchUseStrongCrypto" = dword：00000001

![增強式驗證](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>其他資訊

- [TLS/SSL （安全通道 SSP）中的加密套件](/windows/win32/secauthn/cipher-suites-in-schannel)
- [Windows 8.1 中的 TLS 加密套件](/windows/win32/secauthn/tls-cipher-suites-in-windows-8-1)
- [排序 Schannel 加密套件](/windows/win32/secauthn/prioritizing-schannel-cipher-suites)
- [在編碼器和其他謎樣 tongues 中說話](/archive/blogs/askds/speaking-in-ciphers-and-other-enigmatic-tonguesupdate)
