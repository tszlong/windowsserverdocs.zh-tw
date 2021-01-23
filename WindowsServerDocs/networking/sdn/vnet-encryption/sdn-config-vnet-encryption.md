---
title: 設定虛擬網路的加密
description: 瞭解如何建立加密憑證、建立憑證認證，以及設定虛擬網路以進行加密。
manager: grcusanz
ms.topic: how-to
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 15f47d48ca0e3873433fcaa3e6dd7160bc0f9126
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716604"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>設定虛擬子網的加密

>適用於：Windows Server 2019、Windows Server 2016

虛擬網路加密可加密 Vm 之間的虛擬網路流量，這些 Vm 會在標示為「啟用加密」的子網內彼此通訊。 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。

虛擬網路加密需要：
- 在每個啟用 SDN 的 Hyper-v 主機上安裝的加密憑證。
- 網路控制卡中的認證物件，其參考該憑證的指紋。
- 每個虛擬網路上的設定都包含需要加密的子網。

一旦在子網上啟用加密，該子網內的所有網路流量都會自動加密，除了可能也會發生的任何應用層級加密之外。  子網之間的流量如果標示為已加密，則會自動以未加密的方式傳送。 任何跨越虛擬網路界限的流量也會以未加密的形式傳送。

>[!NOTE]
>當與相同子網上的其他 VM 通訊時（不論其目前是否已連線或已連線），流量會自動加密。

>[!TIP]
>如果您必須將應用程式限制為只在加密子網上進行通訊，您可以使用 (Acl) 的存取控制清單，以允許目前子網內的通訊。 如需詳細資訊，請參閱 [使用 (acl 的存取控制清單) 管理資料中心網路流量流程](../manage/use-acls-for-traffic-flow.md)。


## <a name="step-1-create-the-encryption-certificate"></a>步驟 1： 建立加密憑證
每部主機必須安裝加密憑證。 您可以針對所有租使用者使用相同的憑證，或為每個租使用者各產生一個唯一的憑證。

1.  產生憑證

    ```
    $subjectName = "EncryptedVirtualNetworks"
    $cryptographicProviderName = "Microsoft Base Cryptographic Provider v1.0";
    [int] $privateKeyLength = 1024;
    $sslServerOidString = "1.3.6.1.5.5.7.3.1";
    $sslClientOidString = "1.3.6.1.5.5.7.3.2";
    [int] $validityPeriodInYear = 5;

    $name = new-object -com "X509Enrollment.CX500DistinguishedName.1"
    $name.Encode("CN=" + $SubjectName, 0)

    #Generate Key
    $key = new-object -com "X509Enrollment.CX509PrivateKey.1"
    $key.ProviderName = $cryptographicProviderName
    $key.KeySpec = 1 #X509KeySpec.XCN_AT_KEYEXCHANGE
    $key.Length = $privateKeyLength
    $key.MachineContext = 1
    $key.ExportPolicy = 0x2 #X509PrivateKeyExportFlags.XCN_NCRYPT_ALLOW_EXPORT_FLAG
    $key.Create()

    #Configure Eku
    $serverauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $serverauthoid.InitializeFromValue($sslServerOidString)
    $clientauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $clientauthoid.InitializeFromValue($sslClientOidString)
    $ekuoids = new-object -com "X509Enrollment.CObjectIds.1"
    $ekuoids.add($serverauthoid)
    $ekuoids.add($clientauthoid)
    $ekuext = new-object -com "X509Enrollment.CX509ExtensionEnhancedKeyUsage.1"
    $ekuext.InitializeEncode($ekuoids)

    # Set the hash algorithm to sha512 instead of the default sha1
    $hashAlgorithmObject = New-Object -ComObject X509Enrollment.CObjectId
    $hashAlgorithmObject.InitializeFromAlgorithmName( $ObjectIdGroupId.XCN_CRYPT_HASH_ALG_OID_GROUP_ID, $ObjectIdPublicKeyFlags.XCN_CRYPT_OID_INFO_PUBKEY_ANY, $AlgorithmFlags.AlgorithmFlagsNone, "SHA512")


    #Request Certificate
    $cert = new-object -com "X509Enrollment.CX509CertificateRequestCertificate.1"

    $cert.InitializeFromPrivateKey(2, $key, "")
    $cert.Subject = $name
    $cert.Issuer = $cert.Subject
    $cert.NotBefore = (get-date).ToUniversalTime()
    $cert.NotAfter = $cert.NotBefore.AddYears($validityPeriodInYear);
    $cert.X509Extensions.Add($ekuext)
    $cert.HashAlgorithm = $hashAlgorithmObject
    $cert.Encode()

    $enrollment = new-object -com "X509Enrollment.CX509Enrollment.1"
    $enrollment.InitializeFromRequest($cert)
    $certdata = $enrollment.CreateRequest(0)
    $enrollment.InstallResponse(2, $certdata, 0, "")
    ```

    執行腳本後，[我的存放區] 中會出現新的憑證：

    ```
    PS D:\> dir cert:\\localmachine\my
    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks
    ```

1. 將憑證匯出至檔案。<p>您需要憑證的兩個複本，一個具有私密金鑰，另一個則沒有。

    ```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
    ```

3. 在每部 hyper-v 主機上安裝憑證

    ```
    PS C:\> dir c:\$subjectname.*

    Directory: C:\

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
    -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx
    ```

1. 在 Hyper-v 主機上安裝

    ```
    $server = "Server01"

    $subjectname = "EncryptedVirtualNetworks"
    copy c:\$SubjectName.* \\$server\c$
    invoke-command -computername $server -ArgumentList $subjectname,"secret" {
        param (
            [string] $SubjectName,
            [string] $Secret
        )
        $certFullPath = "c:\$SubjectName.cer"

        # create a representation of the certificate file
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath)

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        $certFullPath = "c:\$SubjectName.pfx"
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath, $Secret, "MachineKeySet,PersistKeySet")

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("My", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        # Important: Remove the certificate files when finished
        remove-item C:\$SubjectName.cer
        remove-item C:\$SubjectName.pfx
    }
    ```

5. 針對您環境中的每部伺服器重複執行。<p>針對每部伺服器重複之後，您應該在每個 Hyper-v 主機的根目錄和我的存放區中安裝憑證。

6. 確認憑證的安裝。<p>藉由檢查 My 和根憑證存放區的內容來確認憑證：

    ```
    PS C:\> enter-pssession Server1

    [Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks
    ```

7. 記下憑證指紋。<p>您必須記下憑證指紋，因為您需要它才能在網路控制站中建立憑證認證物件。

## <a name="step-2-create-the-certificate-credential"></a>步驟 2： 建立憑證認證

在連線到網路控制站的每個 Hyper-v 主機上安裝憑證之後，您現在必須設定網路控制站來使用它。  若要這樣做，您必須從已安裝網路控制卡 PowerShell 模組的電腦建立包含憑證指紋的認證物件。

```
///Replace with the thumbprint from your certificate
$thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"

$uri = "https://nc.contoso.com"

///Replace with your Network Controller URI
Import-module networkcontroller

$credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
$credproperties.Type = "X509Certificate"
$credproperties.Value = $thumbprint
New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force
```

> [!TIP]
> 您可以針對每個加密的虛擬網路重複使用此認證，也可以為每個租使用者部署和使用唯一的憑證。

## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>步驟 3： 設定虛擬網路以進行加密

此步驟假設您已建立虛擬網路名稱「我的網路」，且其中至少包含一個虛擬子網。  如需建立虛擬網路的相關資訊，請參閱 [建立、刪除或更新租使用者虛擬網路](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md)。

>[!NOTE]
>當與相同子網上的其他 VM 通訊時（不論其目前是否已連線或已連線），流量會自動加密。

1.  從網路控制站取出虛擬網路和認證物件：

    ```
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"
    ```

2.  新增憑證認證的參考，並在個別子網上啟用加密：

    ```
    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true
    ```

3.  將更新的虛擬網路物件放入網路控制站：

    ```
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force
    ```

*恭喜！** 完成這些步驟之後，您就完成了。