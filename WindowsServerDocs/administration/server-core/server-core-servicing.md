---
title: 修補的伺服器核心
description: 了解如何更新 Windows Server 的 Server Core 安裝
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817349"
---
# <a name="patch-a-server-core-installation"></a>修補程式的 Server Core 安裝

> 適用於：Windows Server （半年通道） 和 Windows Server 2016

您可以修補伺服器，以下列方式執行 Server Core 安裝：

- **使用 Windows Update，自動或使用 Windows Server Update Services (WSUS)**。 使用 Windows Update，自動或透過命令列工具或 Windows Server Update Services (WSUS)，您可以服務執行 Server Core 安裝的伺服器。

- **手動**。 即使在不使用 Windows update 或 WSUS 的組織中，您可以手動套用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>檢視您的 Server Core 伺服器上安裝的更新
Server Core 中加入新的更新之前，它會是個不錯的主意，請參閱已安裝哪些更新。

若要使用 Windows PowerShell 中檢視更新，請執行**Get-hotfix**。

若要執行的命令，以檢視更新，執行**systeminfo.exe**。 雖然此工具會檢查您的系統時，則可能是短暫的延遲。

您也可以執行**wmic qfe 清單**從命令列。 

## <a name="patch-server-core-automatically-with-windows-update"></a>使用 Windows Update 自動的修補程式 Server Core

若要修補的伺服器使用 Windows Update 自動使用下列步驟：

1. 確認目前的 Windows Update 設定：
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. 若要啟用自動更新：

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. 若要停用自動更新，請執行：

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

如果伺服器是某個網域的成員，您也可以使用群組原則設定 Windows Update。 如需詳細資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=192470 。 不過，當您使用這個方法時，唯一的選項 4 （「 自動下載和排程安裝 」） 是與 Server Core 安裝因為缺乏圖形化介面。 為了更能掌握要安裝的更新以及安裝時間，您可以使用指令碼來提供命令列，這個命令列會提供相當於大部分 Windows Update 圖形化介面的功能。 指令碼的相關資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=192471。

若要強制讓 Windows Update 立即偵測並安裝任何可用的更新，請執行下列命令：

```
Wuauclt /detectnow 
```

根據所安裝的更新，您可能需要重新啟動電腦，但系統將不會通知您。 若要判斷是否已完成安裝程序，請使用工作管理員來確認**Wuauclt**或是**Trusted Installer**程序目前並未執行。 您也可以使用中的方法[檢視您的 Server Core 伺服器上安裝的更新](#view-the-updates-installed-on-your-Server-Core-server)檢查已安裝的更新清單。

## <a name="patch-the-server-with-wsus"></a>修補與 WSUS 伺服器 

如果 Server Core 伺服器是某個網域的成員，您可以將它設定為搭配群組原則使用 WSUS 伺服器。 如需詳細資訊，請下載[群組原則的參考資訊](https://www.microsoft.com/download/details.aspx?id=25250)。 您也可以檢閱[自動更新的設定群組原則設定](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>手動修補的伺服器

下載更新，並使其可供的 Server Core 安裝。
在命令提示字元中，執行下列命令：

```
Wusa <update>.msu /quiet 
```

根據所安裝的更新，您可能需要重新啟動電腦，但系統將不會通知您。

若要手動解除安裝更新，請執行下列命令：

```
Wusa /uninstall <update>.msu /quiet 
```

