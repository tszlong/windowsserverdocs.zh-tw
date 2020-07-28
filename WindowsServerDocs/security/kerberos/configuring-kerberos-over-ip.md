---
title: 正在設定 IP 位址的 Kerberos
description: 以 IP 為基礎的 Spn 的 Kerberos 支援
author: daveba
ms.author: daveba
ms.openlocfilehash: 16feb7045508a854657834fcbb4ab850f9b8ac3b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181914"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 用戶端允許服務主體名稱（Spn）中的 IPv4 和 IPv6 位址主機名稱

>適用於：Windows Server (半年度管道)、Windows Server 2016

從 Windows 10 版本1507和 Windows Server 2016 開始，可以將 Kerberos 用戶端設定為支援 Spn 中的 IPv4 和 IPv6 主機名稱。

根據預設，如果主機名稱為 IP 位址，Windows 將不會嘗試 Kerberos 驗證。 它會切換回其他啟用的驗證通訊協定，例如 NTLM。 不過，應用程式有時會以硬式編碼方式使用 IP 位址，這表示應用程式會切換回 NTLM，而不會使用 Kerberos。 這可能會在環境移至停用 NTLM 時造成相容性問題。

為了降低停用 NTLM 的影響，引進了新功能，可讓系統管理員使用 IP 位址做為服務主體名稱中的主機名稱。 此功能會透過登錄機碼值在用戶端上啟用。

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

若要設定 Spn 中 IP 位址主機名稱的支援，請建立 TryIPSPN 專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 必須在每一部用戶端電腦上設定此登錄值，而這些機器需要依 IP 位址存取受 Kerberos 保護的資源。

## <a name="configuring-a-service-principal-name-as-ip-address"></a>將服務主體名稱設定為 IP 位址

服務主體名稱是在 Kerberos 驗證期間用來識別網路上服務的唯一識別碼。 SPN 是由服務、主機名稱和選擇性的埠（例如）所組成 `service/hostname[:port]` `host/fs.contoso.com` 。 當電腦加入 Active Directory 時，Windows 會向電腦物件註冊多個 Spn。

IP 位址通常不會用來取代主機名稱，因為 IP 位址通常是暫時性的。 這可能會導致衝突和驗證失敗，因為位址租用過期和續約。 因此，註冊以 IP 位址為基礎的 SPN 是手動程式，只有在無法切換到 DNS 型主機名稱時才使用。

建議的方法是使用[Setspn.exe](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11))工具。 請注意，SPN 一次只能註冊到 Active Directory 中的單一帳戶，因此，如果使用 DHCP，建議 IP 位址具有靜態租用。

```
Setspn -s <service>/ip.address> <domain-user-account>
```

範例：

```
Setspn -s host/192.168.1.1 server01
```
