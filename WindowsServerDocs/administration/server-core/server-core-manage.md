---
title: 管理 Server Core
description: 了解如何管理 Windows Server 的 Server Core 安裝
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869299"
---
# <a name="manage-a-server-core-server"></a>管理 Server Core 伺服器
 
> 適用於：Windows Server （半年通道） 和 Windows Server 2016

您可以透過下列方式來管理 Server Core 伺服器：
- 使用[Windows Admin Center](../../manage/windows-admin-center/overview.md)
- 使用[遠端伺服器管理工具](../../remote/remote-server-administration-tools.md)Windows 10 上執行
- 使用 Windows PowerShell 在本機或遠端管理
- 從遠端使用[伺服器管理員](../server-manager/server-manager.md)
- 從遠端使用[MMC 嵌入式管理單元](#managing-with-microsoft-management-console)
- 從遠端使用[遠端桌面服務](#managing-with-remote-desktop-services)

您也可以新增硬體與管理驅動程式在本機，只要您從命令列執行。

有一些重要的限制和秘訣，請記住，當您使用 Server Core:

- 如果您關閉所有命令提示字元視窗，並想要開啟新的 命令提示字元 視窗，您可以執行，從 工作管理員。 按下**CTRL\+ALT\+刪除**，按一下 **啟動工作管理員**，按一下 **更多詳細資料 > 檔案 > 執行**，然後輸入**cmd.exe**。 (型別**Powershell.exe**開啟 PowerShell 命令視窗。)或者，您可以登入並登出再登入。
- 嘗試啟動 Windows 檔案總管的任何命令或工具都將無法運作。 例如，執行**開始。** 從命令提示字元將無法運作。
- 沒有支援 HTML 轉譯或 HTML 說明在 Server Core。
- Server Core 會支援 Windows Installer 以安靜模式，讓您可以從 Windows Installer 檔案安裝工具和公用程式。 當在 Server Core 上安裝 Windows Installer 封裝，使用 **/qb**選項顯示基本使用者介面。
- 若要變更時區，請執行**Set-date**。
- 若要變更國際設定，請執行**control intl.cpl**。
- **Control.exe**不會自行執行。 您必須使用執行**Timedate.cpl**或是**Intl.cpl**。
- **Winver.exe**不適用於 Server Core。 若要取得版本資訊，請使用**Systeminfo.exe**。

## <a name="managing-server-core-with-windows-admin-center"></a>管理 Server Core 與 Windows Admin Center
[Windows Admin Center](../../manage/windows-admin-center/overview.md) 是一個瀏覽器型管理應用程式，可在沒有任何 Azure 或雲端相依性的情況下用來進行 Windows Server 的內部部署管理。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，對於管理未連線至網際網路的私人網路特別有用。 您可以安裝 Windows 10、 閘道伺服器，或含桌面體驗的 Windows Server 安裝的 Windows Admin Center，並接著連接到您想要管理的 Server Core 系統。

## <a name="managing-server-core-remotely-with-server-manager"></a>管理 Server Core 從遠端使用伺服器管理員

伺服器管理員是可協助您佈建及管理本機和遠端 Windows 伺服器從您的桌面，而不需要實際操作伺服器或啟用遠端桌面通訊協定 (RDP) 的 Windows Server 管理主控台每一部伺服器的連接。 伺服器管理員支援遠端多伺服器管理。

若要啟用您的本機伺服器來管理遠端伺服器上執行的伺服器管理員，請執行 Windows PowerShell cmdlet **SMRemoting.exe – 啟用**。

## <a name="managing-with-microsoft-management-console"></a>使用 Microsoft Management Console 管理

您可以從遠端使用許多嵌入式管理單元的 Microsoft Management Console (MMC)，來管理 Server Core 伺服器。

若要使用 MMC 嵌入式管理單元來管理 Server Core 伺服器是網域成員： 

1. 啟動 MMC 嵌入式管理單元，例如 電腦管理。
2. 嵌入式管理單元，以滑鼠右鍵按一下，然後按一下**連接到另一部電腦**。
2. 輸入在 Server Core 伺服器的電腦名稱，然後按一下**確定**。 您可以現在使用 MMC 嵌入式管理單元來管理 Server Core 伺服器，如同任何其他電腦或伺服器。

若要使用 MMC 嵌入式管理單元來管理 Server Core 伺服器所*不*網域成員： 

1. 建立替代的認證，要用來連線的 Server Core 電腦的遠端電腦上的命令提示字元中輸入下列命令：
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   如果您想要提示輸入密碼，請省略**傳遞/** 選項。

2. 出現提示時，輸入您所指定的使用者名稱的密碼。
   如果 Server Core 伺服器上的防火牆不已設定為允許 MMC 嵌入式管理單元連線，請遵循下列步驟來設定 Windows 防火牆允許 MMC 嵌入式管理單元。 然後繼續進行步驟 3。
3. 在不同的電腦上啟動 MMC 嵌入式管理單元，例如**電腦管理**。
4. 在左窗格中，請在嵌入式管理單元，以滑鼠右鍵按一下，，然後按一下**連接到另一部電腦**。 (例如，在電腦管理範例中，您可以按一下滑鼠右鍵**電腦管理 （本機）**。)
5. 在 **另一部電腦**，輸入在 Server Core 伺服器的電腦名稱，然後按一下**確定**。 您現在可以使用 MMC 嵌入式管理單元來管理 Server Core 伺服器，如同管理執行 Windows Server 作業系統的其他任何電腦一樣。

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>設定 Windows 防火牆允許 MMC 嵌入式管理單元連線
若要允許所有 MMC 嵌入式管理單元連線，請執行下列命令：

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

若要允許只有特定 MMC 嵌入式管理單元連線，執行下列命令：
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

何處*rulegroup*是的下列項目，取決於哪一個嵌入式管理單元，您想要連接的其中一個：

| MMC 嵌入式管理單元                            | 規則群組                                            |
|----------------------------------------|-------------------------------------------------------|
| 事件檢視器                           | 遠端事件記錄檔管理                           |
| 服務                               | 遠端服務管理                             |
| 共用資料夾                         | 檔案與印表機共用                              |
| 工作排程器                         | 效能記錄及警示、 檔案及印表機共用 |
| 磁碟管理                        | 遠端磁碟區管理                              |
| Windows 防火牆和進階的安全性 | Windows 防火牆遠端管理                    |


> [!NOTE] 
> 某些 MMC 嵌入式管理單元沒有對應的規則群組可讓它們透過防火牆連線。 不過，啟用事件檢視器、服務或共用資料夾的規則群組將允許其他大部分的嵌入式管理單元連線。 
>
> 此外，某些嵌入式管理單元在可以透過 Windows 防火牆連線之前，需要進一步的設定：
>
> - 磁碟管理。 您必須先在 Server Core 電腦上啟動虛擬磁碟服務 (VDS)。 您也必須在執行 MMC 嵌入式管理單元的電腦上，適當地設定磁碟管理規則。
> - IP 安全性監視器。 您必須先啟用此嵌入式管理單元的遠端管理。 若要這樣做，請在命令提示字元中，輸入**Cscript \windows\system32\scregedit.wsf /im 1**
> - 可靠性和效能。 此嵌入式管理單元不需要其他設定，但是當您使用它來監視 Server Core 電腦時，僅能監視效能資料。 可靠性資料無法使用。

## <a name="managing-with-remote-desktop-services"></a>使用遠端桌面服務管理

您可以使用[遠端桌面](../../remote/remote-desktop-services/welcome-to-rds.md)來管理 Server Core 伺服器從遠端電腦。

您可以存取 Server Core 之前，您必須執行下列命令： 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
這可讓「系統管理遠端桌面」模式接受連線。

## <a name="add-hardware-and-manage-drivers-locally"></a>新增硬體與管理驅動程式在本機

若要將硬體新增至 Server Core 伺服器，請遵循安裝新硬體的硬體廠商所提供的指示。 

如果硬體不是隨插即用，您必須手動安裝驅動程式。 若要這樣做，請將驅動程式檔案複製到暫存位置的伺服器上，並接著執行下列命令：
```
pnputil –i –a <driverinf>
```
何處*driverinf*是驅動程式.inf 檔案的檔案名稱。

顯示提示時，請重新啟動電腦。

若要查看已安裝哪些驅動程式，請執行下列命令： 
```
sc query type= driver
```

> [!NOTE] 
> 您必須在命令的等號後面加入空格，才能成功完成命令。

若要停用裝置驅動程式，請執行下列命令： 
```
sc delete <service_name>
```

何處*service_name*是您在執行時取得的服務名稱**sc 查詢類型 = 驅動程式**。