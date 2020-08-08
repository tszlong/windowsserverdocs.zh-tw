---
title: 對 DNS 用戶端進行疑難排解
description: 本文介紹如何針對用戶端的 DNS 問題進行疑難排解。
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 8098f49c0a48004c54e4acc67522d61d4b30717c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958636"
---
# <a name="troubleshooting-dns-clients"></a>對 DNS 用戶端進行疑難排解

本文討論如何針對來自 DNS 用戶端的問題進行疑難排解。

## <a name="check-ip-configuration"></a>檢查 IP 設定

1. 在用戶端電腦上以系統管理員身分開啟 [命令提示字元] 視窗。

2. 執行下列命令：

   ```cmd
   ipconfig /all
   ```

3. 確認用戶端具有有效的 IP 位址、子網路遮罩和預設閘道，適用于其所附加且正在使用的網路。

4. 檢查輸出中列出的 DNS 伺服器，並確認列出的 IP 位址是正確的。

5. 檢查輸出中的連線特定 DNS 尾碼，並確認它是否正確。

如果用戶端沒有有效的 TCP/IP 設定，請使用下列其中一種方法：

* 若為動態設定的用戶端，請使用 `ipconfig /renew` 命令手動強制用戶端使用 DHCP 伺服器更新其 IP 位址設定。

* 針對靜態設定的用戶端，修改用戶端 TCP/IP 內容，以使用正確的設定，或完成網路的用戶端 DNS 設定。

## <a name="check-network-connection"></a>檢查網路連線

### <a name="ping-test"></a>Ping 測試

確認用戶端可以透過其 IP 位址來 ping 慣用的 DNS 伺服器，以聯繫慣用的 (或替代的) DNS 伺服器。

例如，如果用戶端使用的是慣用的 DNS 伺服器10.0.0.1，請在命令提示字元中執行此命令：

```cmd
ping 10.0.0.1
```

如果沒有設定的 DNS 伺服器回應其 IP 位址的直接 ping，這表示問題來源可能是用戶端與 DNS 伺服器之間的網路連線能力。 如果是這種情況，請遵循基本 TCP/IP 網路疑難排解步驟來修正問題。 請記住，必須允許 ICMP 流量通過防火牆，ping 命令才能正常執行。

### <a name="dns-query-tests"></a>DNS 查詢測試

如果 DNS 用戶端可以 ping DNS 伺服器電腦，請嘗試使用下列 `nslookup` 命令來測試伺服器是否可以回應 dns 用戶端。 因為 nslookup 不會使用用戶端的 DNS 快取，名稱解析將會使用用戶端設定的 DNS 伺服器。

#### <a name="test-a-client"></a>測試用戶端

```cmd
nslookup <client>
```

例如，如果用戶端電腦名稱為**client1**，請執行下列命令：

```cmd
nslookup client1
```

如果未傳回成功的回應，請嘗試執行下列命令：

```cmd
nslookup <fqdn of client>
```

例如，如果 FQDN 是**client1.corp.contoso.com**，請執行下列命令：

```cmd
nslookup client1.corp.contoso.com.
```

> [!NOTE]
> 執行此測試時，您必須包含尾端的句點。

如果 Windows 成功找到 FQDN，但找不到簡短名稱，請在 NIC 的 Advanced TCP/IP 設定的 [DNS] 索引標籤上，檢查 DNS 尾碼設定。 如需詳細資訊，請參閱設定[DNS 解析](/previous-versions/tn-archive/dd163570(v=technet.10)#configuring-dns-resolution)。

#### <a name="test-the-dns-server"></a>測試 DNS 伺服器

```cmd
nslookup <DNS Server>
```

例如，如果 DNS 伺服器名為 DC1，請執行此命令：

```cmd
nslookup dc1
```
如果先前的測試成功，此測試應該也會成功。 如果此測試不成功，請確認與 DNS 伺服器的連線能力。

#### <a name="test-the-failing-record"></a>測試失敗的記錄

```cmd
nslookup <failed internal record>
```

例如，如果失敗的記錄是**app1.corp.contoso.com**，請執行此命令：

```cmd
nslookup app1.corp.contoso.com
```

#### <a name="test-a-public-internet-address"></a>測試公用網際網路位址

```cmd
nslookup <external name>
```

例如：
```cmd
nslookup bing.com
```

如果所有這四個測試都成功，請執行 `ipconfig /displaydns` 並檢查失敗名稱的輸出。 如果您在失敗的名稱底下看到「名稱不存在」，則會從 DNS 伺服器傳回否定回應，並在用戶端上快取。

若要解決此問題，請執行以清除快取 `ipconfig /flushdns` 。

## <a name="next-step"></a>後續步驟

如果名稱解析仍然失敗，請移至[疑難排解 DNS 伺服器](troubleshoot-dns-server.md)一節。
