---
title: 修補伺服器核心
description: 了解如何更新 Windows Server 的伺服器核心安裝
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1447980"
---
# <a name="patch-a-server-core-installation"></a>修補程式的伺服器核心安裝

> 適用於： Windows Server （分號每年次通道） 和 Windows Server 2016

您可以透過下列方式執行伺服器核心 」 安裝伺服器修補程式：

- **使用 Windows Update 自動] 或 [使用 Windows Server Update Services (WSUS)**。 藉由使用 Windows Update，[自動或使用命令列工具或 Windows Server Update Services (WSUS)，您可以服務執行的伺服器核心安裝的伺服器。

- **以手動方式**。 即使在不使用 Windows update 或 WSUS 的組織，您可以手動套用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>檢視在 Server Core 伺服器上安裝更新
伺服器核心中加入新的更新之前，它會是不錯的選項以查看哪些更新已安裝。

若要使用 Windows PowerShell 檢視更新，請執行**Get Hotfix**。

若要檢視執行命令的更新，請執行**systeminfo.exe**。 可能會有短暫的延遲而此工具會檢查您的系統。

您也可以從命令列執行**wmic qfe 清單**。 

## <a name="patch-server-core-automatically-with-windows-update"></a>修補程式自動使用 Windows Update 的伺服器核心

修補程式伺服器與 Windows Update 自動使用下列步驟：

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

如果網域的成員伺服器，您也可以設定 Windows Update 使用群組原則。 如需詳細資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=192470。 不過，當您使用此方法，唯一的選擇 4 （「 自動下載 （英文） 和排程安裝 」） 是伺服器核心安裝相關因缺乏圖形化介面。 更新已安裝的更多控制能力這與時，您可以使用其提供相當於 Windows Update 圖形化介面的大部分命令列指令碼。 指令碼的相關資訊，請參閱https://go.microsoft.com/fwlink/?LinkId=192471。

若要強制 Windows Update 立即偵測並安裝任何可用的更新，請執行下列命令：

```
Wuauclt /detectnow 
```

依據所安裝的更新，您可能需要重新啟動電腦，雖然系統將不會通知您。 若要判斷是否已完成安裝程序，請使用工作管理員確認**Wuauclt**或**信任的安裝程式**的程序不主動執行。 您也可以使用[更新安裝在您的伺服器核心伺服器上的檢視](#view-the-updates-installed-on-your-Server-Core-server)中的方法來檢查已安裝的更新清單。

## <a name="patch-the-server-with-wsus"></a>修補程式具有 WSUS 伺服器 

網域的成員的 Server Core 伺服器時，您可以將它 WSUS 伺服器使用群組原則設定。 如需詳細資訊，下載[群組原則的參考資訊](https://www.microsoft.com/download/details.aspx?id=25250)。 您也可以檢閱[設定群組原則設定為自動更新](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>手動修補程式伺服器

下載更新並提供給伺服器核心安裝。
在命令提示字元中執行下列命令：

```
Wusa <update>.msu /quiet 
```

依據所安裝的更新，您可能需要重新啟動電腦，雖然系統將不會通知您。

若要手動解除安裝更新，請執行下列命令：

```
Wusa /uninstall <update>.msu /quiet 
```

