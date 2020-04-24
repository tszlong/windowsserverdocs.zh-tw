---
title: 使用 Sconfig.cmd 設定 Windows Server 的 Server Core 安裝
description: 說明如何使用 Sconfig.cmd
ms.prod: windows-server
ms.date: 10/17/2017
ms.technology: server-general
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: e6c218b08cc39edd9b3d93ae78b0b5c7aa293858
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826671"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>使用 Sconfig.cmd 設定 Windows Server 2016 或 Windows Server 1709 版的 Server Core 安裝

> 適用於：Windows Server (半年通道) 和 Windows Server 2016

在 Windows Server 2016 和 Windows Server 1709 版中，您可以使用伺服器設定工具 (Sconfig.cmd) 來設定和管理 Server Core 安裝的數個常用部分。 您必須是 Administrators 群組的成員，才能使用此工具。

您可以在 Server Core 安裝以及含有桌面體驗的伺服器安裝 (僅限 Windows Server 2016) 中使用 Sconfig.cmd。

## <a name="start-the-server-configuration-tool"></a>啟動伺服器設定工具

1. 變更到系統磁碟機。

2. 輸入 `Sconfig.cmd`，然後按 ENTER 鍵。 伺服器設定工具介面便會開啟：

    ![Sconfig.cmd 使用者介面的螢幕擷取畫面](media/mainsconfigpage.png)

## <a name="domainworkgroup-settings"></a>網域/工作群組設定

目前的 [網域/工作群組] 設定會顯示在預設的伺服器設定工具畫面。 若要加入網域或工作群組，您可以從主功能表存取 [網域/工作群組]  設定頁面，然後遵循指示提供所有必要資訊。

如果尚未將網域使用者新增到本機 Administrator 群組，您將無法使用網域使用者進行系統變更 (例如變更電腦名稱)。 若要將網域使用者新增到本機 Administrators 群組，請允許電腦重新啟動。 接著，以本機系統管理員的身分登入電腦，然後依照本文稍後[本機系統管理員設定](#local-administrator-settings)一節中的步驟進行。

> [!NOTE]
> 您必須重新啟動伺服器，才能將所有變更套用到網域或工作群組成員資格。 不過，您可以在所有變更之後進行額外的變更，然後再重新啟動伺服器，避免需要重新啟動伺服器數次。 根據預設，重新啟動 Hyper-V 伺服器之前，會自動儲存執行中的虛擬機器。

## <a name="computer-name-settings"></a>電腦名稱設定

目前的電腦名稱會顯示在預設的伺服器設定工具畫面。 您可以從主功能表存取 [電腦名稱]  設定頁面並遵循指示進行，以變更電腦名稱。

> [!NOTE]
> 您必須重新啟動伺服器，才能將所有變更套用到網域或工作群組成員資格。 不過，您可以在所有變更之後進行額外的變更，然後再重新啟動伺服器，避免需要重新啟動伺服器數次。 根據預設，重新啟動 Hyper-V 伺服器之前，會自動儲存執行中的虛擬機器。

## <a name="local-administrator-settings"></a>本機系統管理員設定

若要將其他使用者新增到本機 Administrators 群組，請使用主功能表上的 [新增本機系統管理員]  選項。 在加入網域的電腦上，以下列格式輸入使用者：網域\使用者名稱。 在未加入網域的電腦 (工作群組機器) 上，僅輸入使用者名稱。 這些變更會立即生效。

## <a name="network-settings"></a>網路設定

您可以設定由 DHCP 伺服器自動指派 IP 位址，或者您可以手動指派靜態 IP 位址。 這個選項也可讓您設定伺服器的 DNS 伺服器設定。

> [!NOTE]
> 這些選項及其他更多選項現在可以使用網路 Windows PowerShell Cmdlet 取得。 如需詳細資訊，請參閱 Windows Server 文件庫中的 [網路介面卡 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps) 。

## <a name="windows-update-settings"></a>Windows Update 設定

目前的 Windows Update 設定會顯示在預設的伺服器設定工具畫面。 您可以在主功能表的 [Windows Update 設定]  設定選項上，將伺服器設定為使用自動或手動更新。

選取 [自動更新]  時，系統會在每天凌晨三點檢查並安裝更新。 這些設定會立即生效。 選取 [手動]  更新時，系統將不會自動檢查更新。

您可以從主功能表的 [下載並安裝更新]  選項，隨時下載並安裝適用的更新。

**\[僅下載\]** 選項將會掃描並下載任何可用的更新，然後在準備好開始安裝時，透過控制中心通知您。 此為預設選項。

## <a name="remote-desktop-settings"></a>遠端桌面設定

目前的遠端桌面設定狀態會顯示在預設的伺服器設定工具畫面。 若要設定下列遠端桌面設定，您可以存取 [遠端桌面]  主功能表選項，並遵循畫面上的指示進行。

- 使用網路層級驗證，針對執行遠端桌面的用戶端啟用遠端桌面

- 針對執行任何版本遠端桌面的用戶端啟用遠端桌面

- 停用遠端

## <a name="date-and-time-settings"></a>日期和時間設定

您可以存取 [日期和時間]  主功能表選項，以存取及變更日期和時間設定。

## <a name="telemetry-settings"></a>遙測設定

此選項可讓您設定要傳送哪些資料給 Microsoft。

## <a name="windows-activation-settings"></a>Windows 啟用設定

此選項可讓您設定 Windows 啟用。

## <a name="to-enable-remote-management"></a>啟用遠端管理

您可以從 [設定遠端管理]  主功能表選項，啟用各種遠端管理狀況：

- Microsoft Management Console 遠端管理

- Windows PowerShell

- 伺服器管理員  

## <a name="to-log-off-restart-or-shut-down-the-server"></a>登出、重新啟動或關閉伺服器

若要登出、重新啟動或關閉伺服器，請從主功能表存取對應的功能表項目。 這些選項也可從 [Windows 安全性]  功能表取得，隨時按下 CTRL+ALT+DEL，就可以從任何應用程式存取該功能表。  

## <a name="to-exit-to-the-command-line"></a>結束並返回命令列
  
選取 [結束並返回命令列]  選項，然後按 ENTER 以結束並返回命令列。 若要返回伺服器設定工具，請輸入 **Sconfig.cmd**，然後按 ENTER。
