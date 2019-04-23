---
title: 移動並調整託管快取的大小 (選用)
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cb75e06b5da8ff95fcf763b22c5160ea200035f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853529"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移動和調整託管快取\(選擇性\)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

移動託管快取到磁碟機和您喜歡的資料夾，以及指定的託管快取伺服器可以使用託管快取的磁碟空間量，您可以使用此程序。

此程序是選用的。 如果預設快取位置\(%windir%\\ServiceProfiles\\NetworkService\\AppData\\本機\\PeerDistPub\)和大小 – 也就是 5%的總硬碟空間 – 是適當的部署，您不需要加以變更。

您必須是 Administrators 群組的成員才能執行此程序。

### <a name="to-move-and-resize-the-hosted-cache"></a>若要移動和調整託管快取

1. 使用系統管理員權限開啟 Windows PowerShell。

2. 輸入下列命令，以在本機電腦上，移至另一個位置的託管快取，然後按 ENTER 鍵。

    > [!IMPORTANT]
    > 執行下列命令之前，請以適用於您部署的值取代參數的值，例如-路徑和 – MoveTo。

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  輸入下列命令來調整託管快取 – 特別是 datacache\-本機電腦上。 按 ENTER 鍵。

    > [!IMPORTANT]
    > 執行下列命令之前，取代參數值，例如\-百分比，以適合您的部署的值。  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  若要確認託管快取伺服器組態，請輸入下列命令，然後按 ENTER。

    ``` 
    Get-BCStatus
    ``` 

    命令的結果會顯示您 BranchCache 安裝的各個層面的狀態。 以下是幾個 BranchCache 設定和正確的值，每個項目：

    -   DataCache |CacheFileDirectoryPath:顯示您提供 SetBCCache 命令的 – MoveTo 參數的值相符的硬碟位置。 例如，如果您提供的值為 d:\\datacache，值會顯示在命令輸出。

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume:顯示符合您提供與 SetBCCache 命令的 – 百分比參數的值數目。 例如，如果您提供的值為 20，該值會顯示命令輸出中。

若要繼續進行本指南，請參閱[Prehash 和預先載入內容在託管快取伺服器上&#40;選擇性&#41;](7-Bc-Prehash-Preload.md)。