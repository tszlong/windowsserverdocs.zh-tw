---
title: SMB 多重通道疑難排解
description: 介紹 SMB 多重通道的疑難排解方法。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 662a58fdeb3cda14a0e54c8d0ab7bd0b85387fd7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960130"
---
# <a name="smb-multichannel-troubleshooting"></a>SMB 多重通道疑難排解

本文說明如何針對與 SMB 多重通道相關的問題進行疑難排解。

## <a name="check-the-network-interface-status"></a>檢查網路介面狀態

請確定在 SMB 用戶端（MS **True** \_ 用戶端）和 SMB 伺服器（ms 伺服器）上，網路介面的系結已設定為 True \_ 。 當您執行下列命令時，輸出應該會在 [**已啟用**] 的兩個網路介面底下顯示 [ **True** ]：

```PowerShell
Get-NetAdapterBinding -ComponentID ms_server,ms_msclient
```

之後，請確定網路介面列在下列命令的輸出中：

```PowerShell
Get-SmbServerNetworkInterface
```

```PowerShell
Get-SmbClientNetworkInterface
```

您也可以執行**get-netadapter**命令來查看介面索引，以驗證結果。 介面索引會顯示主動系結至適當介面的所有作用中 SMB 介面卡。

## <a name="check-the-firewall"></a>檢查防火牆

如果只有一個連結-本機 IP 位址，而且沒有可公開路由傳送的位址，則網路設定檔可能會設定為 [**公用**]。 這表示 SMB 預設會在防火牆封鎖。

下列命令會顯示正在使用的連線設定檔。 您也可以使用 [網路和共用中心] 來取得此資訊。

**NetConnectionProfile**

在 [檔案**及印表機共用**] 群組底下，檢查防火牆輸入規則，以確定已針對正確的設定檔啟用 [SMB 傳入]。

![SMB-in 規則](media/smb-multichannel-troubleshooting-1.png)

您也可以在 [**網路和共用中心**] 視窗中啟用 [檔案**及印表機共用**]。 若要這麼做，請選取左側功能表中的 [**變更 advanced 共用設定**]，然後選取 [開啟設定檔的檔案**及印表機共用**]。 此選項會啟用檔案和印表機共用防火牆規則。

![變更 advanced 共用設定](media/smb-multichannel-troubleshooting-2.png)

## <a name="capture-client-and-server-sided-traffic-for-troubleshooting"></a>捕捉用戶端和伺服器側邊流量以進行疑難排解

您需要從 TCP 三向交握開始的 SMB 連接追蹤資訊。 我們建議您先關閉所有應用程式（尤其是 Windows Explorer），然後再開始捕獲。 重新開機 SMB 用戶端上的**工作站**服務，啟動封包捕獲，然後重現問題。

請確定 SMBv3 連線正在進行*x*協商，而且伺服器與用戶端之間的任何內容都不會影響方言的協商。 SMBv2 和較早的版本不支援多重通道。

尋找網路 \_ 介面資訊封 \_ 包。 這是 SMB 用戶端向 SMB 伺服器要求介面卡清單的位置。 如果未交換這些封包，則多重通道無法使用。

伺服器會傳回有效的網路介面清單來回應。 然後，SMB 用戶端會將它們新增至多重通道的可用介面卡清單。 此時，多重通道應該會啟動，而且至少會嘗試啟動連接。

如需詳細資訊，請參閱下列文章：

- [3.2.4.20.10 應用程式要求查詢伺服器的網路介面](/openspecs/windows_protocols/ms-smb2/147adde4-d936-4597-924a-8caa3429c6b0)

- [2.2.32.5 網路 \_ 介面 \_ 資訊回應](/openspecs/windows_protocols/ms-smb2/fcd862d1-1b85-42df-92b1-e103199f531f)

- [3.2.5.14.11 處理網路介面回應](/openspecs/windows_protocols/ms-smb2/5459722b-1eaa-4ead-b465-284363264cad)

在下列案例中，無法使用介面卡：

- 用戶端上有路由問題。 這通常是由不正確的路由表所造成，該資料表會強制流量超過錯誤的介面。

- 已設定多重通道條件約束。 如需詳細資訊，請參閱[SmbMultichannelConstraint](/powershell/module/smbshare/new-smbmultichannelconstraint)。

- 封鎖了網路介面要求和回應封包。

- 用戶端和伺服器無法透過額外的網路介面進行通訊。 例如，TCP 三向交握失敗、防火牆封鎖連接、會話設定失敗等等。

如果介面卡及其 IPv6 位址是在伺服器所傳送的清單上，下一個步驟就是查看是否嘗試透過該介面進行通訊。 依連結-本機位址和 SMB 流量篩選追蹤，並尋找連線嘗試。 如果這是 Test-netconnection 追蹤，您也可以檢查 Windows 篩選平台（WFP）事件，以查看是否已封鎖連接。
