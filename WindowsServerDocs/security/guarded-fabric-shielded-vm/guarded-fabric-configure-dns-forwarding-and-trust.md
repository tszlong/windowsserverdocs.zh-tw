---
title: 設定 DNS 轉送和網域信任
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5d8ffe82065caeee27c5d13f5243f13addc6c325
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386745"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>設定 HGS 網域中的 DNS 轉送和網狀架構網域的單向信任

>適用於：Windows Server (半年度管道)、Windows Server 2016

>[!IMPORTANT]
>從 Windows Server 2019 開始，AD 模式已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。 

使用下列步驟來設定 DNS 轉送，並建立與網狀架構網域的單向信任。 這些步驟可讓 HGS 找出網狀架構網域控制站，並驗證 Hyper-v 主機的群組成員資格。

1.  在提高許可權的 PowerShell 會話中執行下列命令，以設定 DNS 轉送。 以網狀架構網域的名稱取代 fabrikam.com，然後輸入網狀架構網域中 DNS 伺服器的 IP 位址。 如需更高的可用性，請指向一部以上的 DNS 伺服器。

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  若要建立單向樹系信任，請在提升許可權的命令提示字元中執行下列命令：

    以 HGS 網域的名稱取代 `bastion.local`，並使用網狀架構網域的名稱 `fabrikam.com`。 提供網狀架構網域的系統管理員密碼。

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>後續步驟 

> [!div class="nextstepaction"]
> [設定 HTTPS](guarded-fabric-configure-hgs-https.md)
