---
title: 修補伺服器核心
description: 瞭解如何更新 Windows Server 的 Server Core 安裝
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: d670add6e4b4fc7369c48905bb297642ae07ff20
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588070"
---
# <a name="patch-a-server-core-installation"></a>修補 Server Core 安裝

> 適用于： Windows Server 2019、Windows Server 2016 和 Windows Server （半年通道）

您可以透過下列方式修補執行 Server Core 安裝的伺服器：

- **使用 Windows Update 自動或搭配 Windows Server Update Services （WSUS）** 。 藉由使用 Windows Update （自動或透過命令列工具或 Windows Server Update Services （WSUS）），您可以為執行 Server Core 安裝的伺服器提供服務。

- **手動**。 即使在不使用 Windows update 或 WSUS 的組織中，您也可以手動套用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>查看安裝在 Server Core 伺服器上的更新
在您將新的更新新增到 Server Core 之前，最好先查看已安裝的更新。

若要使用 Windows PowerShell 來查看更新，請執行 [**取得-修復程式**]。

若要藉由執行命令來查看更新，請執行**systeminfo**。 當工具檢查您的系統時，可能會有短暫的延遲。

您也可以從命令列執行**wmic qfe 清單**。 

## <a name="patch-server-core-automatically-with-windows-update"></a>使用 Windows Update 自動修補伺服器核心

使用下列步驟，利用 Windows Update 自動修補伺服器：

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

如果伺服器是某個網域的成員，您也可以使用群組原則設定 Windows Update。 如需詳細資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=192470 。 不過，當您使用這個方法時，只有選項4（「自動下載和排程安裝」）與 Server Core 安裝相關，因為缺乏圖形化介面。 為了更能掌握要安裝的更新以及安裝時間，您可以使用指令碼來提供命令列，這個命令列會提供相當於大部分 Windows Update 圖形化介面的功能。 如需腳本的相關資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=192471 。

若要強制讓 Windows Update 立即偵測並安裝任何可用的更新，請執行下列命令：

```
Wuauclt /detectnow 
```

根據所安裝的更新，您可能需要重新啟動電腦，但系統將不會通知您。 若要判斷安裝程式是否已完成，請使用 [工作管理員] 來確認**wuauclt.exe**或**信任的安裝程式**進程並未主動執行。 您也可以使用中的方法，來[查看安裝在 Server Core 伺服器上的更新](#view-the-updates-installed-on-your-server-core-server)，以檢查已安裝的更新清單。

## <a name="patch-the-server-with-wsus"></a>使用 WSUS 修補伺服器 

如果 Server Core 伺服器是某個網域的成員，您可以將它設定為搭配群組原則使用 WSUS 伺服器。 如需詳細資訊，請下載[群組原則參考資訊](https://www.microsoft.com/download/details.aspx?id=25250)。 您也可以查看[設定群組原則設定自動更新](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

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

