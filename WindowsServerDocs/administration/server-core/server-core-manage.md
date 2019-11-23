---
title: 管理 Server Core
description: 瞭解如何管理 Windows Server 的 Server Core 安裝
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 07/23/2019
ms.openlocfilehash: bd96dbfc93f3999d8fb3ddf7ec94cc11025bba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383404"
---
# <a name="manage-a-server-core-server"></a>管理 Server Core 伺服器
 
> 適用于： Windows Server 2019、Windows Server 2016 和 Windows Server （半年通道）

您可以利用下列方式來管理 Server Core 伺服器：
- 使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)
- 使用在 Windows 10 上執行的[遠端伺服器管理工具](../../remote/remote-server-administration-tools.md)
- 使用 Windows PowerShell 在本機或遠端管理
- 從遠端使用[伺服器管理員](../server-manager/server-manager.md)
- 使用 MMC 嵌入式[管理](#managing-with-microsoft-management-console)單元從遠端
- 從遠端使用[遠端桌面服務](#managing-with-remote-desktop-services)

您也可以在本機新增硬體和管理驅動程式，只要從命令列執行該動作即可。

當您使用 Server Core 時，有一些重要的限制和秘訣必須牢記在心：

- 如果您關閉所有命令提示字元視窗，而且想要開啟新的 [命令提示字元] 視窗，您可以從 [工作管理員] 執行此動作。 按**CTRL\+ALT\+刪除**，按一下 **啟動工作管理員**，按一下 **更多詳細資料 >** 檔案 > 執行，然後輸入**cmd.exe**。 （輸入**powershell**以開啟 powershell 命令視窗。）或者，您可以登出後再重新登入。
- 嘗試啟動 Windows 檔案總管的任何命令或工具都將無法運作。 例如，執行 [**啟動]。** 從命令提示字元無法使用。
- 在 Server Core 中不支援 HTML 轉譯或 HTML 說明。
- Server Core 支援在無訊息模式下 Windows Installer，讓您可以從 Windows Installer 檔案安裝工具和公用程式。 在 Server Core 上安裝 Windows Installer 套件時，請使用 **/qb**選項來顯示基本使用者介面。
- 若要變更時區，請執行**設定-Date**。
- 若要變更國際設定，請執行**控制 [國際] cpl**。
- **Control .exe**不會自行執行。 您必須使用**timedate 時間**或**國際 cpl**來執行它。
- 在 Server Core 中無法使用**Winver。** 若要取得版本資訊，請使用**Systeminfo**。

## <a name="managing-server-core-with-windows-admin-center"></a>使用 Windows 管理中心管理 Server Core
[Windows Admin Center](../../manage/windows-admin-center/overview.md) 是一個瀏覽器型管理應用程式，可在沒有任何 Azure 或雲端相依性的情況下用來進行 Windows Server 的內部部署管理。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，對於管理未連線至網際網路的私人網路特別有用。 您可以在 Windows 10、閘道伺服器或 Windows Server 含桌面體驗的安裝上安裝 Windows 系統管理中心，然後連線到您要管理的 Server Core 系統。

## <a name="managing-server-core-remotely-with-server-manager"></a>使用伺服器管理員從遠端系統管理 Server Core

伺服器管理員是 Windows Server 中的管理主控台，可協助您從桌面布建和管理本機與遠端 Windows 伺服器，而不需要實際存取伺服器，或需要啟用遠端桌面通訊協定（RDP）每一部伺服器的連接。 伺服器管理員支援遠端、多伺服器管理。

若要讓您的本機伺服器受伺服器管理員在遠端伺服器上執行，請執行 Windows PowerShell Cmdlet **configure-smremoting.exe – enable**。

## <a name="managing-with-microsoft-management-console"></a>使用 Microsoft Management Console 管理

您可以使用許多適用于 Microsoft Management Console （MMC）的嵌入式管理單元，來管理您的 Server Core 伺服器。

若要使用 MMC 嵌入式管理單元來管理屬於網域成員的伺服器核心伺服器： 

1. 啟動 MMC 嵌入式管理單元，例如 [電腦管理]。
2. 在嵌入式管理單元上按一下滑鼠右鍵，然後按一下 [連線**到另一台電腦]** 。
2. 輸入 Server Core 伺服器的電腦名稱稱，然後按一下 **[確定]** 。 您現在可以使用 MMC 嵌入式管理單元來管理 Server Core 伺服器，就像任何其他電腦或伺服器一樣。

若要使用 MMC 嵌入式管理單元來管理*不*是網域成員的 server Core 伺服器： 

1. 在遠端電腦的命令提示字元中輸入下列命令，建立用來連接到 Server Core 電腦的替代認證：

   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   如果您想要收到輸入密碼的提示，請省略 **/pass**選項。

2. 出現提示時，輸入您所指定使用者名稱的密碼。
   如果 Server Core 伺服器上的防火牆尚未設定為允許 MMC 嵌入式管理單元連線，請遵循下列步驟來設定 Windows 防火牆，以允許 MMC 嵌入式管理單元。 然後繼續進行步驟3。
3. 在不同的電腦上，啟動 MMC 嵌入式管理單元，例如 [**電腦管理**]。
4. 在左窗格中，以滑鼠右鍵按一下嵌入式管理單元，然後按一下 [連線**到另一台電腦]** 。 （例如，在電腦管理範例中，您可以用滑鼠右鍵按一下 [**電腦管理（本機）** ]）。
5. 在 [**另一台電腦**] 中，輸入 server Core 伺服器的電腦名稱稱，然後按一下 **[確定]** 。 您現在可以使用 MMC 嵌入式管理單元來管理 Server Core 伺服器，如同管理執行 Windows Server 作業系統的其他任何電腦一樣。

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>設定 Windows 防火牆允許 MMC 嵌入式管理單元連線
若要允許所有 MMC 嵌入式管理單元連線，請執行下列命令：

```PowerShell
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

若只要允許特定 MMC 嵌入式管理單元連線，請執行下列動作：

```PowerShell
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

其中， *rulegroup*是下列其中一項，視您想要連接的嵌入式管理單元而定：

| MMC 嵌入式管理單元                            | 規則群組                                            |
| ---------------------------------------- | ------------------------------------------------------- |
| 事件檢視器                           | 遠端事件記錄檔管理                           |
| 服務                               | 遠端服務管理                             |
| 共用資料夾                         | 檔案與印表機共用                              |
| 工作排程器                         | 效能記錄檔及警示、檔案及印表機共用 |
| 磁碟管理                        | 遠端磁碟區管理                              |
| Windows 防火牆和 Advanced Security | Windows 防火牆遠端管理                    |


> [!NOTE] 
> 某些 MMC 嵌入式管理單元沒有對應的規則群組，可讓他們透過防火牆進行連接。 不過，啟用事件檢視器、服務或共用資料夾的規則群組將允許其他大部分的嵌入式管理單元連線。 
>
> 此外，某些嵌入式管理單元在可以透過 Windows 防火牆連線之前，需要進一步的設定：
>
> - 磁碟管理。 您必須先在 Server Core 電腦上啟動虛擬磁碟服務 (VDS)。 您也必須在執行 MMC 嵌入式管理單元的電腦上，適當地設定磁碟管理規則。
> - IP 安全性監視器。 您必須先啟用此嵌入式管理單元的遠端管理。 若要這麼做，請在命令提示字元中輸入**Cscript \windows\system32\scregedit.wsf/im 1**
> - 可靠性和效能。 此嵌入式管理單元不需要其他設定，但是當您使用它來監視 Server Core 電腦時，僅能監視效能資料。 可靠性資料無法使用。

## <a name="managing-with-remote-desktop-services"></a>使用遠端桌面服務管理

您可以使用[遠端桌面](../../remote/remote-desktop-services/welcome-to-rds.md)，從遠端電腦管理 server Core 伺服器。

您必須先執行下列命令，才可以存取 Server Core： 

```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```

這可讓「系統管理遠端桌面」模式接受連線。

## <a name="add-hardware-and-manage-drivers-locally"></a>在本機新增硬體和管理驅動程式

若要將硬體新增到 Server Core 伺服器，請依照硬體廠商提供的指示來安裝新硬體。 

如果硬體不是隨插即用，您將需要手動安裝驅動程式。 若要這麼做，請將驅動程式檔案複製到伺服器上的暫存位置，然後執行下列命令：

```
pnputil –i –a <driverinf>
```

其中*driverinf*是驅動程式之 .inf 檔案的檔案名。

顯示提示時，請重新啟動電腦。

若要查看已安裝的驅動程式，請執行下列命令： 

```
sc query type= driver
```

> [!NOTE] 
> 您必須在命令的等號後面加入空格，才能成功完成命令。

若要停用設備磁碟機，請執行下列程式：

```
sc delete <service_name>
```

其中*service_name*是您執行**sc query type = driver**時所得到的服務名稱。