---
title: 針對 DHCP 伺服器上的問題進行疑難排解
description: 本 artilce 介紹如何疑難排解 DHCP 伺服器上的問題並收集資料。
ms.service: na
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: d6fc69c15c3465769232d89f70a65ca915d0584e
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989034"
---
# <a name="troubleshoot-problems-on-the-dhcp-server"></a>針對 DHCP 伺服器上的問題進行疑難排解

本文討論如何疑難排解 DHCP 伺服器上發生的問題。

## <a name="troubleshooting-checklist"></a>疑難排解檢查清單

選取下列設定：

  - DHCP 伺服器服務已啟動且正在執行。 若要檢查此設定，請執行**net start**命令，並尋找**DHCP 伺服器**。

  - DHCP 伺服器已獲授權。 請參閱已[加入網域的案例中的 WINDOWS DHCP 伺服器授權](/openspecs/windows_protocols/ms-dhcpe/56f8870b-a7c1-4db1-8a86-f69079fe5077)。

  - 確認 DHCP 用戶端所在之子網的 DHCP 伺服器範圍中有可用的 IP 位址租用。 若要執行這項操作，請參閱 DHCP 伺服器管理主控台中適當範圍的統計資料。

  - 檢查是否 \_ 可以在位址租用中找到任何錯誤的地址清單。

  - 檢查網路上的任何裝置是否有未從 DHCP 領域中排除的靜態 IP 位址。

  - 確認 DHCP 伺服器所系結的 IP 位址是在必須從中租用 IP 位址之範圍的子網內。如果沒有可用的轉送代理，就會發生這種情況。 若要這麼做，請執行**DhcpServerv4Binding**或**DhcpServerv6Binding** Cmdlet。

  - 確認只有 DHCP 伺服器接聽 UDP 埠67和68。 沒有其他進程或其他服務 (例如 WDS 或 PXE) 應該佔用這些埠。 若要這麼做，請執行 `netstat -anb` 命令。

  - 如果您要處理 IPsec 部署的環境，請確認已新增 DHCP 伺服器 IPsec 豁免。

  - 確認可以從 DHCP 伺服器 ping 轉送代理程式的 IP 位址。

  - 列舉並檢查已設定的 DHCP 原則和篩選準則。

## <a name="event-logs"></a>事件記錄檔

檢查系統和 DHCP 伺服器服務事件記錄檔 (**應用程式和服務記錄**檔 \> **Microsoft** \> **Windows** \> **DHCP 伺服器**) ，以瞭解與觀察到的問題相關的回報問題。
視問題類型而定，事件會記錄到下列其中一個事件通道： [dhcp 伺服器操作事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器管理事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器系統事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器篩選通知事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器 Audit 事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))

## <a name="data-collection"></a>資料集合

### <a name="dhcp-server-log"></a>DHCP 伺服器記錄檔

DHCP 伺服器服務的偵錯工具記錄檔提供有關 IP 位址租用指派的詳細資訊，以及 DHCP 伺服器完成的 DNS 動態更新。 這些記錄檔預設位於% windir% \\ System32 \\ Dhcp。
如需詳細資訊，請參閱[分析 DHCP 伺服器記錄](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd183591\(v=ws.10\))檔。

### <a name="network-trace"></a>網路追蹤

相互關聯網路追蹤可能會指出 DHCP 伺服器在記錄事件時所執行的作業。 若要建立這類追蹤，請遵循下列步驟：

1.  前往[GitHub](https://github.com/CSS-Windows/WindowsDiag/tree/master/ALL/TSS)，並下載[tss \_tools.zip](https://github.com/CSS-Windows/WindowsDiag/blob/master/ALL/TSS/tss_tools.zip)檔案。

2.  複製 Tss \_tools.zip 檔案，並將它展開到本機磁片上的某個位置，例如 [C： tools] \\ 資料夾。

3.  \\在提高許可權的命令提示字元視窗中，從 C： tools 執行下列命令：
    ```console
    TSS Ron Trace <Stop:Evt:>20321:<Other:>DhcpAdminEvents NoSDP NoPSR NoProcmon NoGPresult
    ```

    >[!Note]
    >在此命令中，請 \<*Stop:Evt:*\> 將和取代為 \<*Other:*\> 您要在追蹤會話中專注于的事件識別碼和事件通道。
    >\_Tsstools.zip 檔案中所包含的 Tss 讀我檔案 \_Help.docx 檔案 \_ 提供所有可用設定的詳細資訊。

4.  觸發事件之後，此工具會建立名為 C： MS DATA 的資料夾 \\ \_ 。 此資料夾將包含一些實用的輸出檔，提供有關電腦的網路和網域設定的一般資訊。
    此資料夾中最有趣的檔案是% Computername% \_ date \_ time \_ packetcapture \_ InternetClient \_ dbg. etl。
    藉由使用[網路監視器](https://www.microsoft.com/download/4865)應用程式，您可以載入檔案，並在「DHCP 或 DNS」通訊協定上設定顯示篩選器，以檢查幕後發生的狀況。