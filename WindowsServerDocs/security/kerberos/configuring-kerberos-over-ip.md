---
title: 設定 IP 位址的 Kerberos
description: 以 IP 為基礎的 Spn 的 Kerberos 支援
author: daveba
ms.author: daveba
ms.date: 07/27/2020
ms.topic: article
ms.openlocfilehash: d7203711e1d1eb42c7e9b06fe2b8a4cc66e7d74b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949714"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 用戶端允許服務主體名稱中的 IPv4 和 IPv6 位址主機名稱 (Spn) 

>適用於：Windows Server (半年度管道)、Windows Server 2016

從 Windows 10 1507 版和 Windows Server 2016 開始，可以將 Kerberos 用戶端設定為支援 Spn 中的 IPv4 和 IPv6 主機名稱。

如果主機名稱是 IP 位址，則 Windows 預設不會嘗試對主機進行 Kerberos 驗證。 它會切換回其他已啟用的驗證通訊協定，例如 NTLM。 不過，應用程式有時會硬式編碼為使用 IP 位址，這表示應用程式會切換回 NTLM，而不會使用 Kerberos。 當環境移至停用 NTLM 時，這可能會導致相容性問題。

為了降低停用 NTLM 的影響，引進了新功能，讓系統管理員可以使用 IP 位址做為服務主體名稱中的主機名稱。 用戶端會透過登錄機碼值來啟用這項功能。

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

若要設定 Spn 中 IP 位址主機名稱的支援，請建立 TryIPSPN 專案。 此項目依預設不存在於登錄中。 在您建立專案之後，請將 DWORD 值變更為1。 需要依 IP 位址存取受 Kerberos 保護之資源的每部用戶端電腦上，都必須設定此登錄值。

## <a name="configuring-a-service-principal-name-as-ip-address"></a>將服務主體名稱設定為 IP 位址

服務主體名稱是在 Kerberos 驗證期間用來識別網路上服務的唯一識別碼。 SPN 是由服務、主機名稱及（選擇性）形式的埠 `service/hostname[:port]` （例如）所組成 `host/fs.contoso.com` 。 當電腦加入 Active Directory 時，Windows 會將多個 Spn 註冊至電腦物件。

IP 位址通常不會用來取代主機名稱，因為 IP 位址通常是暫時性的。 這可能會導致衝突和驗證失敗，因為位址租用到期和更新。 因此，登錄以 IP 位址為基礎的 SPN 是手動程式，只在無法切換至以 DNS 為基礎的主機名稱時使用。

建議的方法是使用 [Setspn.exe](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) 工具。 請注意，SPN 一次只能註冊到 Active Directory 中的單一帳戶，因此如果使用 DHCP，建議 IP 位址具有靜態租用。

```
Setspn -s <service>/ip.address> <domain-user-account>
```

範例：

```
Setspn -s host/192.168.1.1 server01
```
