---
title: IP 位址設定 Kerberos
description: 以 IP 為主的 spn 的 Kerberos 支援
ms.openlocfilehash: aa2685fcff2fdf231e5e5884d25885585f0bd6c9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67279973"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 用戶端在服務主體名稱 (Spn) 允許 IPv4 和 IPv6 位址的主機名稱

>適用於：Windows Server （半年通道），Windows Server 2016

從 Windows 10 1507年版和 Windows Server 2016 開始，Kerberos 用戶端可以設定為在 Spn 中支援 IPv4 和 IPv6 的主機名稱。

根據預設 Windows 將不會嘗試 Kerberos 驗證的主機，如果主機名稱是 IP 位址。 它會切換回 NTLM 等其他已啟用的驗證通訊協定。 不過，應用程式有時會硬式編碼以使用 IP 位址，這表示應用程式會回到 NTLM，並不會使用 Kerberos。 當環境移至停用 NTLM，這會造成相容性問題。

為了降低停用 NTLM 的新功能的影響，我們引進，可讓系統管理員使用 IP 位址作為服務主體名稱中的主機名稱。 這項功能會啟用用戶端透過登錄機碼值。

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

若要設定 Spn 的 IP 位址的主機名稱的支援，建立 TryIPSPN 項目。 此項目依預設不存在於登錄中。 您已建立的項目之後，請將 DWORD 值變更為 1。 此登錄值必須設定每個需要存取受 Kerberos 保護資源的用戶端電腦上，依 IP 位址。

## <a name="configuring-a-service-principal-name-as-ip-address"></a>設定服務主體名稱為 IP 位址

服務主體名稱是在 Kerberos 驗證期間用來識別網路上的服務的唯一識別碼。 SPN 組成服務、 主機名稱，以及選擇性的表單中的連接埠`service/hostname[:port]`這類`host/fs.contoso.com`。 當電腦已加入 Active Directory 時，Windows 會註冊電腦物件的多個 Spn。

IP 位址不會正常使用取代主機名稱上，因為通常是暫時性的 IP 位址。 這可能會導致衝突和驗證失敗為位址租用到期和更新。 因此註冊與 IP 位址為基礎的 SPN 是手動程序，應該只用於時就無法切換到 DNS 主機名稱。

建議的方法是使用[Setspn.exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11))工具。 請注意，SPN 只能註冊在 Active Directory 中的單一帳戶一次因此建議您使用 IP 位址會有靜態的租用期，如果使用 DHCP。

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

範例：

```
Setspn -s host/192.168.1.1 server01
```
