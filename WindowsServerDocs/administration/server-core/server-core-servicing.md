---
title: 修補 Server Core
description: 瞭解如何更新 Windows Server 的 Server Core 安裝
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.topic: how-to
ms.openlocfilehash: f02db48083cdc8d63c834347a3b80321c0935fa2
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97943714"
---
# <a name="patch-a-server-core-installation"></a>修補 Server Core 安裝

> 適用于： Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道) 

您可以透過下列方式修補執行 Server Core 安裝的伺服器：

- **使用 Windows Update 自動或透過 Windows Server Update Services (WSUS)**。 藉由使用 Windows Update （自動或透過命令列工具）或 Windows Server Update Services (WSUS) ，您可以為執行 Server Core 安裝的伺服器提供服務。

- **手動**。 即使在不使用 Windows update 或 WSUS 的組織中，您也可以手動套用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>查看安裝在 Server Core 伺服器上的更新
在您將新的更新新增至 Server Core 之前，最好先查看已安裝哪些更新。

若要使用 Windows PowerShell 來查看更新，請執行 **Get-修復**。

若要藉由執行命令來查看更新，請執行 **systeminfo.exe**。 工具檢查您的系統時，可能會有短暫的延遲。

您也可以從命令列執行 **wmic qfe 清單** 。

## <a name="patch-server-core-automatically-with-windows-update"></a>使用 Windows Update 自動修補 Server Core

使用下列步驟，透過 Windows Update 自動修補伺服器：

1. 確認目前的 Windows Update 設定：
   ```
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU /v
   ```

2. 若要啟用自動更新：

   ```
   Net stop wuauserv
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU 4
   Net start wuauserv
   ```

3. 若要停用自動更新，請執行：

   ```
   Net stop wuauserv
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU 1
   Net start wuauserv
   ```

如果伺服器是某個網域的成員，您也可以使用群組原則設定 Windows Update。 如需詳細資訊，請參閱https://go.microsoft.com/fwlink/?LinkId=192470。 不過，當您使用這個方法時，只有選項 4 ( [自動下載和排程安裝] ) 與 Server Core 安裝有關，因為缺乏圖形化介面。 為了更能掌握要安裝的更新以及安裝時間，您可以使用指令碼來提供命令列，這個命令列會提供相當於大部分 Windows Update 圖形化介面的功能。 如需腳本的詳細資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=192471 。

若要強制讓 Windows Update 立即偵測並安裝任何可用的更新，請執行下列命令：

```
Wuauclt /detectnow
```

根據所安裝的更新，您可能需要重新啟動電腦，但系統將不會通知您。 若要判斷安裝程式是否已完成，請使用工作管理員來確認 **wuauclt.exe** 或 **受信任的安裝程式處理常式** 並未主動執行。 您也可以使用中的方法來 [查看安裝在 Server Core 伺服器上的更新](#view-the-updates-installed-on-your-server-core-server) ，以檢查已安裝的更新清單。

## <a name="patch-the-server-with-wsus"></a>使用 WSUS 修補伺服器

如果 Server Core 伺服器是某個網域的成員，您可以將它設定為搭配群組原則使用 WSUS 伺服器。 如需詳細資訊，請下載 [群組原則參考資訊](https://www.microsoft.com/download/details.aspx?id=25250)。 您也可以查看 [設定群組原則設定自動更新](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>手動修補伺服器

下載更新並將它提供給 Server Core 安裝。
在命令提示字元中，執行下列命令：

```
Wusa <update>.msu /quiet
```

根據所安裝的更新，您可能需要重新啟動電腦，但系統將不會通知您。

若要手動卸載更新，請執行下列命令：

```
Wusa /uninstall <update>.msu /quiet
```

