---
title: 設定虛擬網路的加密
description: 虛擬網路加密可加密標示為 '啟用加密。' 的子網路內彼此通訊的虛擬機器之間的虛擬網路流量
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 90fb33eb4c4b63fdd5c84bf3ffc2447fd52a809b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845489"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>設定加密的虛擬子網路

>適用於：Windows Server

虛擬網路加密是用來彼此通訊，標示為 '啟用加密。' 的子網路內的 Vm 之間的虛擬網路流量的加密 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。

虛擬網路的加密需要：
- 安裝在每個已啟用 SDN 的 HYPER-V 主機上的加密憑證。
- 參考憑證的指紋網路控制卡中的認證物件。
- 每個虛擬網路上的組態包含需要加密的子網路。

一旦您啟用的子網路上的加密，該子網路內的所有網路流量會自動都加密除了可能也需要進行任何應用程式層級加密。  自動即使標示為已加密，跨越子網路的流量會傳送未加密。 跨越網路界限的任何流量也取得傳送未加密。

>[!NOTE]
>當通訊的另一個 VM 位於相同的子網路是否其目前已連線或連線在稍後的時間，流量會自動加密。

>[!TIP]
>如果您必須限制只有已加密的子網路上通訊的應用程式，您可以使用存取控制清單 (Acl) 只是為了讓目前的子網路內的通訊。 如需詳細資訊，請參閱 <<c0> [ 使用存取控制清單 (Acl) 來管理資料中心網路流量流動](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。


## <a name="step-1-create-the-encryption-certificate"></a>步驟 1. 建立加密憑證
每一部主機必須已安裝的加密憑證。 您可以為所有租用戶中使用相同的憑證，或產生每個租用戶的唯一權杖。 

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

之後執行指令碼，新的憑證會出現在 My 存放區：

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2.  將憑證匯出至檔案。<p>您需要兩份憑證，一個具有私用的索引鍵，另一個則沒有。

    $subjectName = "EncryptedVirtualNetworks" $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"} [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret")) Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

3.  在每部 hyper-v 主機上安裝憑證 

    PS c:\> dir c:\$subjectname.*


        Directory: C:\


    模式 LastWriteTime 長度的名稱
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx

4.  在 HYPER-V 主機上安裝

    $server = "Server01"

    $subjectname ="EncryptedVirtualNetworks 」 複製 c:\$SubjectName.* \\$server\c$ 叫用命令-computername $server-ArgumentList $subjectname，「 密碼 」 {param ([string] $SubjectName，[string] $Secret) $certFullPath ="c:\$SubjectName.cer"

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

5.  針對您的環境中的每部伺服器重複。<p>之後對每部伺服器重複執行，您應該安裝在根與我的存放區的每一部 HYPER-V 主機的憑證。 

6.  確認安裝憑證。<p>藉由檢查的內容驗證的憑證我和根憑證存放區：

    PS C:\>輸入 pssession Server1

    [Server1]:PS C:\> -get-childitem cert://localmachine/my、 cert://localmachine/root |？ {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath:Microsoft.PowerShell.Security\Certificate::localmachine\my

    憑證指紋的主體
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


    PSParentPath:Microsoft.PowerShell.Security\Certificate::localmachine\root

    憑證指紋的主體
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

7.  記下憑證指紋。<p>因為您需要在網路控制站中建立憑證認證物件，您必須記下憑證指紋。

## <a name="step-2-create-the-certificate-credential"></a>步驟 2. 建立憑證認證

在每個連接至網路控制卡的 HYPER-V 主機上安裝憑證之後，您現在必須設定網路控制站來使用它。  若要這樣做，您必須建立認證物件，包含已安裝的網路控制站的 PowerShell 模組從電腦的憑證指紋。 


    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

>[!TIP]
>您可以重複使用此認證的每個加密的虛擬網路，或您可以部署並使用每個租用戶的唯一的憑證。


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>步驟 3。 設定加密的虛擬網路

這個步驟假設您已經建立虛擬網路名稱 「 我的網路 」，其中包含至少一個虛擬子網路。  如需建立虛擬網路的詳細資訊，請參閱[建立、 刪除或更新租用戶虛擬網路](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md)。

>[!NOTE]
>當通訊的另一個 VM 位於相同的子網路是否其目前已連線或連線在稍後的時間，流量會自動加密。

1.  從網路控制站擷取的虛擬網路及認證物件

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork" $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

2.  將參考加入憑證認證，並在個別的子網路上啟用加密

    $vnet.properties.EncryptionCredential = $certcred

    # <a name="replace-the-subnets-index-with-the-value-corresponding-to-the-subnet-you-want-encrypted"></a>將子網路索引替換為您想要加密與子的網路對應的值。  
    # <a name="repeat-for-each-subnet-where-encryption-is-needed"></a>針對需要加密每個子網路重複
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

3.  將更新的虛擬網路物件放入網路控制站

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force


_**恭喜您 ！**_ 完成這些步驟後，您已完成。 


## <a name="next-steps"></a>後續步驟



