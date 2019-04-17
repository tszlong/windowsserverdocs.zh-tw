---
title: 移動及調整大小裝載快取（選擇性）
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a3ed1d6de1281575b33cdf75a5970d2ed033085
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移動及調整大小裝載快取 \(Optional\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程序將裝載快取的磁碟機和您喜歡的資料夾，並在指定裝載快取伺服器可用於裝載快取的磁碟空間量。

此程序是選擇性的。 如果預設快取位置 \(%windir%\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) 和大小 – 也就是的硬碟空間總計 5%– 適用於您的部署，您不需要變更。

您必須是系統管理員才能執行此程序群組成員。

### <a name="to-move-and-resize-the-hosted-cache"></a>以移動及調整大小裝載快取

1. 打開 Windows PowerShell 以系統管理員權限。

2. 輸入下列命令，以在本機電腦上，將裝載快取移到另一個位置，然後按 ENTER 鍵。

    > [!IMPORTANT]
    > 之前，請執行下列命令，請先取代參數值，例如 – 路徑和 – MoveTo，使用適用於您的部署的值。

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  輸入下列命令，以調整裝載快取 – 專門 datacache \-本機電腦上。 按下 ENTER。

    > [!IMPORTANT]
    > 之前，請執行下列命令，請先取代參數值，例如 \-Percentage，適用於您的部署的值。  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  若要確認裝載快取伺服器設定，輸入下列命令，然後按 ENTER。

    ``` 
    Get-BCStatus
    ``` 

    命令的結果顯示所有方面 BranchCache 安裝的狀態。 以下是一些 BranchCache 設定和正確的值為每個項目：

    -   DataCache |CacheFileDirectoryPath：顯示硬碟位置符合您所提供的 – MoveTo SetBCCache 命令的參數值。 例如，如果您提供 D:\\datacache 的值，該值會顯示命令的輸出中。

    -   DataCache |MaxCacheSizeAsPercentageOfDiskVolume：顯示符合您所提供的 SetBCCache 命令 – 百分比參數值數字。 例如，如果您提供 20 的值，該值會顯示命令的輸出中。

若要繼續使用此快速入門，請查看[Prehash 和預先載入內容裝載快取伺服器與 #40; 選用和 #41;](7-Bc-Prehash-Preload.md).