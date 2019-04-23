有許多方式來設定網狀架構網域的名稱解析。 一個簡單方式是設定條件式轉寄站 DNS 中區域的網狀架構。 Fabric DNS 伺服器上設定此區域，請在提升權限的 Windows PowerShell 主控台中執行下列命令。 以下 Windows PowerShell 語法，視您的環境中的位址和名稱取代。 新增其他 HGS 節點的主要伺服器。

```
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers <IP addresses of HGS server>
```

<!-- Appears in guarded-fabric-configuring-fabric-dns-ad.md and guarded-fabric-configuring-fabric-dns.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->    