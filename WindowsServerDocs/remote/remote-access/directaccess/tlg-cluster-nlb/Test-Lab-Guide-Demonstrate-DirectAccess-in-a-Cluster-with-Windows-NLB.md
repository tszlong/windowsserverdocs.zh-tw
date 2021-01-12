---
title: 測試實驗室指南-在具有 Windows NLB 的叢集中示範 DirectAccess
description: 瞭解如何使用混合的 IPv4 和 IPv6 測試實驗室指南，擴充示範 directaccess 單一伺服器設定，以示範 DirectAccess 網路負載平衡與叢集設定。
manager: brianlic
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9412711a2b4f394908547bb3381a1319955046b1
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112844"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>測試實驗室指南：示範在具備 Windows NLB 功能的叢集中使用 DirectAccess

>適用於：Windows Server (半年度管道)、Windows Server 2016

遠端存取是 Windows Server 2016、Windows Server 2012 R2 andWindows Server 2012 作業系統中的伺服器角色，可讓遠端使用者使用 DirectAccess 或 RRAS VPN 安全地存取內部網路資源。 本指南包含延伸 [Test Lab Guide: Demonstrate DirectAccess Single Server Setup with Mixed IPv4 and IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) (測試實驗室指南：使用混合的 IPv4 與 IPv6 示範 DirectAccess 單一伺服器安裝) 的逐步指示，以示範 DirectAccess 網路負載平衡與叢集設定。

## <a name="about-this-guide"></a>關於本指南
本指南使用六部伺服器與兩部用戶端電腦，說明設定和示範遠端存取的方法。 完整的遠端存取測試實驗室包含模擬內部網路、網際網路及家用網路的 NLB，以及示範不同網際網路連線案例的遠端存取功能。

> [!IMPORTANT]
> 這個實驗室是使用最少部電腦的概念性驗證。 本指南詳述的設定僅供測試實驗室使用，不得用於實際執行環境。

## <a name="known-issues"></a><a name="KnownIssues"></a>已知問題
以下是設定叢集案例的已知問題：

-   在具有單一網路介面卡的純 IPv4 部署中設定 DirectAccess 之後，並且在預設 DNS64 (包含 ":3333::" 的 IPv6 位址) 自動於網路介面卡上設定之後，透過遠端存取管理主控台嘗試啟用負載平衡會導致提示使用者提供 IPv6 DIP。 如果提供 IPv6 DIP，設定會在按一下 [認可] 之後失敗，並發生錯誤：參數不正確。

    若要解決此問題：

    1.  從[備份和還原遠端存取設定](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)下載備份和還原指令碼。

    2.  使用下載的指令碼 Backup-RemoteAccess.ps1 備份遠端存取 GPO

    3.  嘗試啟用負載平衡，直到它失敗所在的步驟。 在 [啟用負載平衡] 對話方塊中，展開詳細資料區域、在詳細資料區域以滑鼠右鍵按一下，然後按一下 [複製指令碼]。

    4.  開啟 [記事本]，並貼上剪貼簿的內容。 例如：

        ```
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose
        ```

    5.  關閉任何開啟的 [遠端存取] 對話方塊，並關閉遠端存取管理主控台。

    6.  編輯貼上的文字，並移除 IPv6 位址。 例如：

        ```
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose
        ```

    7.  在提升權限的 PowerShell 視窗中，執行前一步驟的命令。

    8.  如果 Cmdlet 執行時失敗 (並非因為不正確的輸入值所致)，請執行命令 Restore-RemoteAccess.ps1 並遵循指示，以確定維護原始設定的完整性。

    9. 您現在可以重新開啟遠端存取管理主控台。



