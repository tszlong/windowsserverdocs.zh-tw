---
title: 設定 DNS 轉送和網域信任
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ebc9c2a3cac85ab998075d784111808b3d590d46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854139"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>HGS 網域和單向信任與網狀架構之網域中設定 DNS 轉送

>適用於：Windows Server （半年通道），Windows Server 2016

>[!IMPORTANT]
>AD 模式開頭為 Windows Server 2019 已被取代。 TPM 證明不可能的環境下，設定[裝載金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似的保證，能 AD 模式，並已設定的工作變得更容易。 

使用下列步驟來設定 DNS 轉送，並建立與網狀架構之網域的單向信任。 這些步驟允許 HGS 找出網狀架構的網域控制站，並驗證群組成員資格的 HYPER-V 主機。

1.  執行下列命令，在提升權限的 PowerShell 工作階段設定 DNS 轉送。 Fabrikam.com 取代 fabric 網域的名稱，並輸入 fabric 網域中的 DNS 伺服器的 IP 位址。 如需更高的可用性，指向多部 DNS 伺服器。

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  若要建立單向樹系信任，請在提升權限的命令提示字元執行下列命令：

    取代`bastion.local`HGS 網域的名稱和`fabrikam.com`fabric 網域的名稱。 網狀架構網域的系統管理員提供的密碼。

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>後續步驟 

>[!div class="nextstepaction"]
[設定 HTTPS](guarded-fabric-configure-hgs-https.md)
