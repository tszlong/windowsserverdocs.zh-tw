---
title: Windows Server 疑難排解
description: 取得 Windows Server 問題疑難排解文章的連結
ms.prod: windows-server
ms.technology: server-general
ms.date: 1/24/2020
author: kaushika-msft
ms.author: kaushika
ms.openlocfilehash: f3012f499e67f73ec9e8ab20b24df122492ea0ea
ms.sourcegitcommit: fa9a8badf4eb366aeeca7d2905e2cad711ee8dae
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2020
ms.locfileid: "84714900"
---
# <a name="troubleshooting-windows-server-components"></a>Windows Server 元件疑難排解

> [!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。  
>   
> 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

Microsoft 會定期發行 Windows Server 的這兩項更新。 為了確保您的伺服器可以收到日後的更新 (包括安全性更新)，請務必持續更新這些伺服器。 如需已發行更新的完整清單，請查看 [Windows 10 及 Windows Server 2016 更新記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。

本節包含先進的疑難排解主題和連結，可協助您解決 Windows Server 的問題。 其他主題將會在可用時新增。

## <a name="troubleshoot-activation"></a>啟用疑難排解

- [針對 Windows 大量啟用進行疑難排解](https://docs.microsoft.com/windows-server/get-started/activation-troubleshooting-guide)
- [針對 KMS 進行疑難排解的指導方針](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-kms-general)
- [用於取得大量啟用資訊的 Slmgr.vbs 選項](https://docs.microsoft.com/windows-server/get-started/activation-slmgr-vbs-options)
- [解析 Windows 啟用錯誤碼](https://docs.microsoft.com/windows-server/get-started/activation-error-codes)
- [KMS 啟用的已知問題](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-kms-issues)
- [MAK 啟用的已知問題](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-mak-issues)
- [針對 DNS 相關啟用問題進行疑難排解的指導方針](https://docs.microsoft.com/windows-server/get-started/common-troubleshooting-procedures-kms-dns)
- [重建 Tokens.dat 檔案](https://docs.microsoft.com/windows-server/get-started/activation-rebuild-tokens-dat-file)
- [ADBA 用戶端疑難排解](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-adba-clients)

## <a name="troubleshoot-startup-and-restart"></a>疑難排解啟動和重新開機

- [Windows 啟動進階疑難排解](https://docs.microsoft.com/windows/client-management/troubleshoot-windows-startup)
- [如何判斷適用於 64 位元版本 Windows 的適當分頁檔大小](https://docs.microsoft.com/windows/client-management/determine-appropriate-page-file-size)
- [產生核心或完成損毀傾印](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump)
- [分頁檔簡介](https://docs.microsoft.com/windows/client-management/introduction-page-file)
- [在 Windows 中設定系統失敗和修復選項](https://docs.microsoft.com/windows/client-management/system-failure-recovery-options)
- [Windows 開機問題進階疑難排解](https://docs.microsoft.com/windows/client-management/advanced-troubleshooting-boot-problems)
- [Windows 電腦凍結進階疑難排解](https://docs.microsoft.com/windows/client-management/troubleshoot-windows-freeze)
- [停止錯誤或藍色當機畫面錯誤進階疑難排解](https://docs.microsoft.com/windows/client-management/troubleshoot-stop-errors)
- [停止錯誤 7B 或 Inaccessible_Boot_Device 進階疑難排解](https://docs.microsoft.com/windows/client-management/troubleshoot-inaccessible-boot-device)
- [事件識別碼41的 Advanced 疑難排解「系統已重新開機，但未完全關閉」](https://docs.microsoft.com/windows/client-management/troubleshoot-event-id-41-restart)
- [當您更新內建的 Broadcom 網路介面卡驅動程式時，發生停止錯誤](https://docs.microsoft.com/windows/client-management/troubleshoot-stop-error-on-broadcom-driver-update)

## <a name="troubleshoot-ad-forest-recovery"></a>針對 AD 樹系復原進行疑難排解

- [AD 樹系復原 - 常見問題](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-faq)

## <a name="troubleshoot-ad-replication"></a>針對 AD 複寫進行疑難排解

- [進行 Active Directory 複寫問題疑難排解](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems)
- [虛擬網域控制站的疑難排解](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/virtual-dc/virtualized-domain-controller-troubleshooting)
- [疑難排解網域控制站部署](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/troubleshooting-domain-controller-deployment)
- [設定電腦進行疑難排解](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/troubleshoot/configuring-a-computer-for-troubleshooting)

## <a name="troubleshoot-ad-fs"></a>疑難排解 AD FS

- [疑難排解 AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-overview)
- [AD FS 疑難排解-審核事件和記錄](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging)
- [AD FS 疑難排解-SQL 連線能力](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-sql)
- [AD FS 疑難排解-宣告發行](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-issuance)
- [AD FS 疑難排解-迴圈偵測](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-loop)
- [AD FS 疑難排解-憑證](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-certs)
- [AD FS 疑難排解-Fiddler](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler)
- [AD FS 疑難排解-Fiddler-WS-FEDERATION](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler-ws-fed)
- [AD FS 疑難排解-宣告規則](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-rules)
- [AD FS 疑難排解-整合式 Windows 驗證](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-iwa)
- [AD FS 疑難排解-Azure AD](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-azure)
- [AD FS 常見問題集](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-faq)
- [AD FS 說明診斷分析器](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-diagnostics-analyzer)

## <a name="troubleshoot-aovpn"></a>針對 AoVPN 進行疑難排解

- [Always On VPN 疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy-troubleshooting)

## <a name="troubleshoot-converged-nic"></a>針對聚合式 NIC 進行疑難排解

- [針對聚合式 NIC 設定進行疑難排解](https://docs.microsoft.com/windows-server/networking/technologies/conv-nic/cnic-app-troubleshoot)

## <a name="troubleshoot-dfsr"></a>針對 DFSR 進行疑難排解

- [DFS 複寫：常見問題集 (FAQ)](https://docs.microsoft.com/windows-server/storage/dfs-replication/dfsr-faq)

## <a name="troubleshoot-directaccess"></a>針對 DirectAccess 進行疑難排解

- [對 DirectAccess 進行疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/troubleshooting-directaccess)

## <a name="troubleshoot-disk-management"></a>針對磁碟管理問題進行疑難排解

- [針對磁碟管理問題進行疑難排解](https://docs.microsoft.com/windows-server/storage/disk-management/troubleshooting-disk-management)

## <a name="troubleshoot-dns"></a>DNS 疑難排解

- [疑難排解網域名稱系統（DNS）問題](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/troubleshoot-dns-data-collection)
- [對 DNS 用戶端進行疑難排解](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/troubleshoot-dns-client)
- [停用 DNS 用戶端上的 DNS 用戶端快取](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/disable-dns-client-side-caching)
- [疑難排解 DNS 伺服器](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/troubleshoot-dns-server)

## <a name="troubleshoot-failover-cluster"></a>疑難排解容錯移轉叢集

- [使用 Windows 錯誤報告針對容錯移轉叢集進行疑難排解](https://docs.microsoft.com/windows-server/failover-clustering/troubleshooting-using-wer-reports)
- [叢集感知更新-常見問題](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating-faq)
- [針對事件識別碼1135的叢集問題進行疑難排解](https://docs.microsoft.com/windows-server/troubleshoot/troubleshooting-cluster-event-id-1135)
- [從作用中的容錯移轉叢集成員資格移除節點時發生問題](https://docs.microsoft.com/windows-server/troubleshoot/problem-nodes-failover-cluster)
- [從 VMWare ESX 上的容錯移轉叢集成員資格移除節點](https://docs.microsoft.com/windows-server/troubleshoot/nodes-failover-cluster-vmware)
- [IaaS 與 SQL AlwaysOn - 微調容錯移轉叢集網路閾值](https://docs.microsoft.com/windows-server/troubleshoot/iaas-sql-failover-cluster)

## <a name="troubleshoot-dhcp"></a>疑難排解 DHCP

- [動態主機設定通訊協定（DHCP）的疑難排解指南](https://docs.microsoft.com/windows-server/troubleshoot/troubleshoot-dhcp-issue)
- [DHCP （動態主機設定通訊協定）基本概念](https://docs.microsoft.com/windows-server/troubleshoot/dynamic-host-configuration-protocol-basics)
- [針對 DHCP 進行疑難排解的一般指導方針](https://docs.microsoft.com/windows-server/troubleshoot/general-guidance-to-troubleshoot-dhcp)
- [如何在沒有 DHCP 伺服器的情況下使用自動 TCP/IP 定址](https://docs.microsoft.com/windows-server/troubleshoot/how-to-use-automatic-tcpip-addressing-without-a-dh)
- [針對 DHCP 用戶端上的問題進行疑難排解](https://docs.microsoft.com/windows-server/troubleshoot/troubleshoot-problems-on-dhcp-client)
- [針對 DHCP 伺服器上的問題進行疑難排解](https://docs.microsoft.com/windows-server/troubleshoot/troubleshoot-problems-on-dhcp-server)

## <a name="troubleshoot-fsrm"></a>針對 FSRM 進行疑難排解

- [檔案伺服器資源管理員疑難排解](https://docs.microsoft.com/windows-server/storage/fsrm/troubleshooting-file-server-resource-manager)

## <a name="troubleshoot-guarded-fabric"></a>針對受防護網狀架構進行疑難排解

- [使用受防護的網狀架構診斷工具進行疑難排解](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-diagnostics)
- [針對主機守護者服務進行疑難排解](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hgs)
- [針對主機守護者服務進行疑難排解](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts)

## <a name="troubleshoot-multi-site-ras"></a>針對多網站 RAS 進行疑難排解

- [啟用多站台的疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-enabling-multisite)
- [新增進入點的疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-adding-entry-points)
- [設定進入點網域控制站的疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-setting-the-entry-point-domain-controller)
- [Web 探查 URL 的疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-web-probe-urls)

## <a name="troubleshoot-nano-server"></a>針對 Nano 伺服器進行疑難排解

- [針對 Nano 伺服器進行疑難排解](https://docs.microsoft.com/windows-server/get-started/troubleshooting-nano-server)

## <a name="troubleshoot-nic-teaming"></a>針對 NIC 小組進行疑難排解

- [對 NIC 小組進行疑難排解](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/troubleshooting-nic-teaming)

## <a name="troubleshoot-otp-authentication"></a>針對 OTP 驗證進行疑難排解

- [驗證問題的疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/troubleshoot/troubleshooting-authentication-issues)
- [啟用 OTP 的疑難排解](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/troubleshoot/troubleshooting-enabling-otp)

## <a name="troubleshoot-qos"></a>疑難排解 QoS

- [QoS 常見問題](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-faq)

## <a name="troubleshoot-s2d"></a>針對 S2D 進行疑難排解

- [儲存空間直接存取疑難排解](https://docs.microsoft.com/windows-server/storage/storage-spaces/troubleshooting-storage-spaces)
- [儲存空間直接存取的常見問題](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-faq)
- [儲存空間直接存取健全狀況和操作狀態](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-states)
- [使用儲存空間直接存取收集診斷資料](https://docs.microsoft.com/windows-server/storage/storage-spaces/data-collection)
- [Windows 中的存放裝置類別記憶體 (NVDIMM-N) 健全狀況管理](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-class-memory-health)

## <a name="troubleshoot-sdn"></a>疑難排解 SDN

- [疑難排解 SDN](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-software-defined-networking)
- [對 Windows Server 軟體定義網路堆疊進行疑難排解](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)

## <a name="troubleshoot-rds-session-connectivity"></a>針對 RDS 會話連接進行疑難排解

- [針對一般遠端桌面連線進行疑難排解](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-error-general-troubleshooting)
- [用戶端無法連線並取得類別未註冊的錯誤](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-error-class-not-registered)
- [用戶端無法連線，而且看不到可用的授權錯誤](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-error-no-licenses-available)
- [使用者無法驗證，或必須驗證兩次](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/cannot-authenticate-or-must-authenticate-twice)
- [連線時，使用者接收遠端桌面服務目前忙碌中的訊息](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/remote-desktop-service-currently-busy)
- [遠端桌面用戶端連線中斷，且無法重新連線至同一工作階段](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-client-disconnects-cannot-reconnect-same-session)
- [遠端膝上型電腦從無線網路中斷連線](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/remote-laptop-disconnects-wireless-network)
- [遠端桌面連線期間效能不佳或應用程式發生問題](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/poor-performance-or-application-problems)

## <a name="troubleshoot-shielded-vm"></a>針對受防護的 VM 進行疑難排解

- [針對受防護的 Vm 進行疑難排解](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)

## <a name="troubleshoot-software-restriction-policies"></a>針對軟體限制原則進行疑難排解

- [為軟體限制原則進行疑難排解](https://docs.microsoft.com/windows-server/identity/software-restriction-policies/troubleshoot-software-restriction-policies)

## <a name="troubleshoot-storage-migration"></a>針對儲存體遷移進行疑難排解

- [儲存體遷移服務的已知問題](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues)
- [儲存體遷移服務常見問題（FAQ）](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq)

## <a name="troubleshoot-storage-replica"></a>針對儲存體複本進行疑難排解

- [儲存體複本的已知問題](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-known-issues)
- [儲存體複本的常見問題集](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-frequently-asked-questions)

## <a name="troubleshoot-user-profiles"></a>疑難排解使用者設定檔

- [透過有事件對使用者設定檔進行疑難排解](https://docs.microsoft.com/windows-server/storage/folder-redirection/troubleshoot-user-profiles-events)

## <a name="troubleshoot-vrss"></a>針對 vRSS 進行疑難排解

- [vRSS 常見問題](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-faq)

## <a name="troubleshoot-webproxy"></a>針對 WebProxy 進行疑難排解

- [對 Web 應用程式 Proxy 進行疑難](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/troubleshooting-web-application-proxy)排解

## <a name="troubleshoot-windows-admin-center"></a>針對 Windows Admin Center 問題進行疑難排解

- [Windows Admin Center 一般疑難排解步驟](https://docs.microsoft.com/windows-server/manage/windows-admin-center/support/troubleshooting)
- [Windows Admin Center 已知問題](https://docs.microsoft.com/windows-server/manage/windows-admin-center/support/known-issues)
- [Windows Admin Center 常見問題集](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/faq)
