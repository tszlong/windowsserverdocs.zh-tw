---
title: 管理伺服器核心
description: 了解如何管理 Windows Server 的伺服器核心安裝
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718708"
---
# <a name="manage-a-server-core-server"></a>管理伺服器核心伺服器
 
> 適用於： Windows Server （分號每年次通道） 和 Windows Server 2016

您可以透過下列方式來管理的伺服器核心伺服器：
- 使用[Windows 系統管理中心](../../manage/windows-admin-center/overview.md)
- 使用 Windows 10 上執行[遠端伺服器管理工具](../../remote/remote-server-administration-tools.md)
- 在本機及遠端使用 Windows PowerShell
- 從遠端使用 [[伺服器管理員](../server-manager/server-manager.md)
- 從遠端使用[MMC 嵌入式管理單元](#managing-with-microsoft-management-console)
- 從遠端與[遠端桌面服務](#managing-with-remote-desktop-services)

您也可以新增硬體和只要您這麼做從命令列管理在本機的驅動程式。

有一些重要限制和當您使用 [伺服器核心時應該記住的秘訣：

- 如果您關閉所有的命令提示字元視窗並開啟新的命令提示字元視窗，您可以執行的工作管理員從。 按**CTRL\ + ALT\ + DELETE**、 按一下 [**啟動工作管理員**]，按一下 [**其他詳細資料] > 檔案 > 執行**，然後輸入**cmd.exe**。 （輸入**Powershell.exe**若要開啟 [windows PowerShell 命令）。或者，您可以登出] 然後再次登入。
- 任何命令或嘗試啟動 [Windows 檔案總管的工具將無法運作。 例如，執行**啟動。** 在命令提示字元中將無法運作。
- 有不支援 HTML 轉譯或在 [伺服器核心的 HTML 說明。
- 伺服器核心支援 Windows Installer 安靜模式，讓您可以從 Windows Installer 檔案安裝工具和公用程式。 在 Server Core 安裝 Windows Installer 套件、 時使用 **/qb**選項以顯示基本的使用者介面。
- 若要變更時區中執行**設定日期**。
- 若要變更國際設定，請執行**控制 intl.cpl**。
- **Control.exe**不會執行其本身。 您必須使用**Timedate.cpl**或**Intl.cpl**執行它。
- **Winver.exe**並未提供 Server Core。 若要取得版本資訊，請使用**Systeminfo.exe**。

## <a name="managing-server-core-with-windows-admin-center"></a>管理與 Windows 系統管理中心的伺服器核心
[Windows 系統管理中心](../../manage/windows-admin-center/overview.md)是可讓沒有 Azure 或雲端的相依性的 Windows Server 的內部管理的瀏覽器為基礎的管理應用程式。 Windows 系統管理中心可讓您將 server 基礎結構的各方面的完全控制和特別適用於在未連線至網際網路的私人網路上的管理。 您可以 Windows 10、 閘道伺服器，或安裝的 Windows Server 桌面體驗，請安裝 Windows 系統管理中心，然後連線至您想要管理的伺服器核心系統。

## <a name="managing-server-core-remotely-with-server-manager"></a>管理伺服器核心遠端使用伺服器管理員

伺服器管理員會在可協助您佈建並從您的桌面，管理本機和遠端 Windows 型伺服器而不需要是實體的存取權的伺服器或需要啟用遠端桌面通訊協定 (RDP) 的 Windows Server 管理主控台每一部伺服器的連線。 伺服器管理員支援遠端、 多伺服器管理。

若要啟用您要管理的 [伺服器管理員在遠端伺服器上執行的本機伺服器，請執行 Windows PowerShell cmdlet**設定 SMRemoting.exe – 啟用**。

## <a name="managing-with-microsoft-management-console"></a>利用 Microsoft Management Console 管理

您可以從遠端管理您的伺服器核心伺服器使用許多嵌入式管理單元的 Microsoft Management Console (MMC)。

若要使用 MMC 嵌入式管理單元來管理網域成員的 Server Core 伺服器： 

1. 啟動 MMC 嵌入式管理單元，例如 [電腦管理]。
2. 以滑鼠右鍵按一下嵌入式管理單元中，並再按一下 [**連線到另一部電腦**。
2. 輸入 Server Core 伺服器的電腦名稱，然後按一下 [**確定]**。 您現在可以按照您電腦或伺服器管理的伺服器核心伺服器使用 MMC 嵌入式管理單元。

若要使用 MMC 嵌入式管理單元來管理的伺服器核心伺服器也就是** 是網域成員： 

1. 建立用來連線至伺服器核心電腦在遠端電腦上的命令提示字元處輸入下列命令以替代認證：
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   如果您想要提示輸入密碼，請**省略/傳遞**] 選項。

2. 出現提示時，輸入您所指定的使用者名稱的密碼。
   如果不允許 MMC 嵌入式管理單元來連線已設定的 Server Core 伺服器上的防火牆，請遵循以下步驟來設定 Windows 防火牆以允許 MMC 嵌入式管理單元中。 再繼續執行步驟 3。
3. 在不同的電腦上啟動 MMC 嵌入式管理單元，例如 [**電腦管理]**。
4. 在左窗格中，以滑鼠右鍵按一下嵌入式管理單元，和 [**連接至另一部電腦**。 （例如，在 [電腦管理範例中，您會以滑鼠右鍵按一下**電腦管理 （本機）**。）
5. 在**另一部電腦**上，輸入伺服器核心伺服器的電腦名稱，然後按一下 [**確定]**。 您現在可以管理的伺服器核心伺服器按照您其他執行 Windows Server 作業系統的電腦使用 MMC 嵌入式管理單元。

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>若要設定 Windows 防火牆以允許 MMC 嵌入式管理單元 (s) 連線
若要允許所有 MMC 嵌入式管理單元來連線，請執行下列命令：

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

若要允許只有特定 MMC 嵌入式管理單元來連線，請執行下列命令：
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

其中*rulegroup*是其中一項，視哪些嵌入式管理單元中您要連線：

| MMC 嵌入式管理單元                            | 規則群組                                            |
|----------------------------------------|-------------------------------------------------------|
| 事件檢視器                           | 遠端事件記錄檔管理                           |
| 服務                               | 遠端服務管理                             |
| 共用的資料夾                         | 檔案及印表機共用                              |
| 工作排程器                         | 效能記錄檔及提醒、 檔案及印表機共用 |
| 磁碟管理                        | 遠端磁碟區管理                              |
| 將 Windows 防火牆與進階的安全性 | Windows 防火牆遠端管理                    |


> [!NOTE] 
> 某些 MMC 嵌入式管理單元沒有對應的規則群組，讓它們可以透過防火牆連線。 不過，啟用事件檢視器、 服務或共用資料夾的 「 規則群組會允許連線大部分其他嵌入式管理單元。 
>
> 此外，某些嵌入式管理單元需要進一步設定他們才能連線通過 Windows 防火牆之前：
>
> - 磁碟管理]。 您必須先啟動虛擬磁碟服務 (VDS) 的伺服器核心電腦上。 您也必須在 MMC 嵌入式管理單元中執行之電腦的適當地設定磁碟管理規則。
> - IP 安全性監視器。 您必須先啟用遠端管理嵌入式管理單元。 若要這樣做，在命令提示字元中輸入**Cscript \windows\system32\scregedit.wsf /im 1**
> - 效能和可靠性。 嵌入式管理單元不需要任何進一步設定，但是當您使用監控伺服器核心電腦，您只可以監視效能資料。 無法使用可靠性的資料。

## <a name="managing-with-remote-desktop-services"></a>與遠端桌面服務管理

您可以使用[遠端桌面](../../remote/remote-desktop-services/welcome-to-rds.md)管理從遠端電腦的伺服器核心伺服器。

您可以存取伺服器核心之前，您需要執行下列命令： 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
這可讓管理模式的遠端桌面接受連線。

## <a name="add-hardware-and-manage-drivers-locally"></a>新增硬體及管理在本機上的驅動程式

若要新增的 Server Core 伺服器硬體，遵循指示安裝新的硬體硬體廠商所提供。 

如果硬體不隨插即用，您需要以手動方式安裝驅動程式。 若要這樣做，將驅動程式檔案複製到伺服器上的暫存位置並再執行下列命令：
```
pnputil –i –a <driverinf>
```
*Driverinf*所在的驅動程式.inf 檔的檔案名稱。

如果出現提示，請重新啟動電腦。

若要查看哪些驅動程式會安裝，請執行下列命令： 
```
sc query type= driver
```

> [!NOTE] 
> 您必須包含的空間之後，命令才會成功完成號。

若要停用的裝置驅動程式，請執行下列命令： 
```
sc delete <service_name>
```

其中*服務名稱*是您取得當您執行的服務名稱**sc 查詢類型 = 驅動程式**。