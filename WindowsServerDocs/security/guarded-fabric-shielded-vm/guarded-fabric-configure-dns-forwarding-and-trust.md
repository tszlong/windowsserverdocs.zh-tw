---
title: 設定 DNS 轉送和網域信任
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 102d1267fe2b15e50ab2d078647ff86f7c937a6d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966165"
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

    `bastion.local`以 HGS 網域的名稱取代，並 `fabrikam.com` 以網狀架構網域的名稱取代。 提供網狀架構網域的系統管理員密碼。

    ```powershell
    netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add
    ```

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定 HTTPS](guarded-fabric-configure-hgs-https.md)
