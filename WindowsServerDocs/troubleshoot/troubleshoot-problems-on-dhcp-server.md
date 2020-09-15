---
title: 針對 DHCP 伺服器上的問題進行疑難排解
description: 本 artilce 介紹如何針對 DHCP 伺服器上的問題進行疑難排解，並收集資料。
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: a6b5e4128c2e07e51ab8a9c07155a8c0212fcad8
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078585"
---
# <a name="troubleshoot-problems-on-the-dhcp-server"></a>針對 DHCP 伺服器上的問題進行疑難排解

本文討論如何針對 DHCP 伺服器上發生的問題進行疑難排解。

## <a name="troubleshooting-checklist"></a>疑難排解檢查清單

選取下列設定：

  - DHCP 伺服器服務已啟動且正在執行。 若要檢查此設定，請執行 **net start** 命令，然後尋找 **DHCP 伺服器**。

  - DHCP 伺服器已獲授權。 請參閱已 [加入網域的案例中的 WINDOWS DHCP 伺服器授權](/openspecs/windows_protocols/ms-dhcpe/56f8870b-a7c1-4db1-8a86-f69079fe5077)。

  - 確認 dhcp 用戶端所在子網的 DHCP 伺服器範圍中有 IP 位址租用。 若要這樣做，請參閱 DHCP 伺服器管理主控台中適當範圍的統計資料。

  - 檢查是否 \_ 可以在位址租用中找到任何錯誤的地址清單。

  - 檢查網路上的任何裝置是否具有未從 DHCP 領域排除的靜態 IP 位址。

  - 確認 DHCP 伺服器所系結的 IP 位址位於必須租用 IP 位址之範圍的子網內。如果沒有可用的轉送代理程式，就會發生這種情況。 若要這樣做，請執行 **DhcpServerv4Binding** 或 **DhcpServerv6Binding** Cmdlet。

  - 確認只有 DHCP 伺服器正在接聽 UDP 埠67和68。 沒有其他進程或其他服務 (例如 WDS 或 PXE) 應該佔用這些埠。 若要這樣做，請執行 `netstat -anb` 命令。

  - 如果您要處理 IPsec 部署的環境，請確認已新增 DHCP 伺服器 IPsec 豁免。

  - 確認可以從 DHCP 伺服器 ping 轉送代理程式 IP 位址。

  - 列舉和檢查已設定的 DHCP 原則和篩選器。

## <a name="event-logs"></a>事件記錄檔

檢查系統和 DHCP 伺服器服務事件記錄檔 (**應用程式和服務會記錄** \> **Microsoft** \> **Windows** \> **DHCP-伺服器**) 是否有與觀察到的問題相關的回報問題。
視問題的種類而定，事件會記錄到下列其中一個事件通道： [dhcp 伺服器操作事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器系統管理事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器系統事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器篩選通知事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [dhcp 伺服器審核事件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))

## <a name="data-collection"></a>資料收集

### <a name="dhcp-server-log"></a>DHCP 伺服器記錄檔

DHCP 伺服器服務偵錯工具記錄檔提供有關 IP 位址租用指派的詳細資訊，以及 DHCP 伺服器完成的 DNS 動態更新。 這些記錄檔預設位於% windir% \\ System32 Dhcp 中 \\ 。
如需詳細資訊，請參閱 [分析 DHCP 伺服器記錄](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd183591\(v=ws.10\))檔。

### <a name="network-trace"></a>網路追蹤

相互關聯的網路追蹤可能會指出 DHCP 伺服器在事件記錄時所執行的作業。 若要建立這類追蹤，請遵循下列步驟：

1.  移至 [GitHub](https://github.com/CSS-Windows/WindowsDiag/tree/master/ALL/TSS)，然後下載 [tss \_tools.zip](https://github.com/CSS-Windows/WindowsDiag/blob/master/ALL/TSS/tss_tools.zip) 檔。

2.  複製 Tss \_tools.zip 檔，並將它展開到本機磁片上的位置，例如 [C： \\ tools] 資料夾。

3.  \\在提高許可權的命令提示字元視窗中，從 C： tools 執行下列命令：
    ```console
    TSS Ron Trace <Stop:Evt:>20321:<Other:>DhcpAdminEvents NoSDP NoPSR NoProcmon NoGPresult
    ```

    >[!Note]
    >在此命令中，將和取代為 \<*Stop:Evt:*\> \<*Other:*\> 您將著重在追蹤會話中的事件識別碼和事件通道。
    >\_Tsstools.zip 檔案中包含的 Tss .Cmd 自述 \_ 檔Help.docx 檔案 \_ 提供所有可用設定的詳細資訊。

4.  觸發事件之後，此工具會建立名為 C： MS DATA 的資料夾 \\ \_ 。 此資料夾會包含一些實用的輸出檔案，以提供有關電腦網路和網域設定的一般資訊。
    此資料夾中最感興趣的檔案是% Computername% \_ date \_ time \_ packetcapture \_ InternetClient dbg. \_ etl。
    藉由使用 [網路監視器](https://www.microsoft.com/download/4865) 應用程式，您可以載入檔案，並設定「DHCP 或 DNS」通訊協定上的顯示篩選器，以檢查幕後背後的狀況。