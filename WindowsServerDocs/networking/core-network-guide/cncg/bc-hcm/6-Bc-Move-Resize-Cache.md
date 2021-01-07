---
title: 移動並調整託管快取的大小 (選用)
description: 瞭解如何將託管快取移至您想要的磁片磁碟機和資料夾，以及指定託管快取伺服器可用於託管快取的磁碟空間量。
manager: brianlic
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 426a38b0dfb37d6898f8ede9337f912998ec2158
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965550"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移動託管快取並調整其大小（ \( 選擇性）\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式將託管快取移至您想要的磁片磁碟機和資料夾，以及指定託管快取伺服器可用於託管快取的磁碟空間量。

此程序是選用的。 如果預設快取位置 \( % windir% \\ ServiceProfiles \\ NetworkService \\ AppData \\ 本機 \\ PeerDistPub \) 和大小（即總硬碟空間的5%）適用于您的部署，您就不需要變更它們。

您必須是 Administrators 群組的成員，才能執行此程式。

### <a name="to-move-and-resize-the-hosted-cache"></a>移動託管快取並調整其大小

1. 以系統管理員許可權開啟 Windows PowerShell。

2. 輸入下列命令，將託管快取移至本機電腦上的其他位置，然後按 ENTER。

    > [!IMPORTANT]
    > 執行下列命令之前，請將參數值（例如– Path 和-MoveTo）取代為適用于您部署的值。

    ```
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ```

3.  輸入下列命令來調整託管快取的大小，特別是本機電腦上的資料快取 \- 。 按 ENTER 鍵。

    > [!IMPORTANT]
    > 執行下列命令之前，請 \- 使用適用于您部署的值來取代參數值，例如百分比。

    ```
    Set-BCCache -Percentage 20
    ```

4.  若要確認託管快取伺服器設定，請輸入下列命令，然後按 ENTER。

    ```
    Get-BCStatus
    ```

    命令的結果會顯示 BranchCache 安裝之所有層面的狀態。 以下是一些 BranchCache 設定和每個專案的正確值：

    -   DataCache |CacheFileDirectoryPath：顯示與您在 SetBCCache 命令的-MoveTo 參數中提供的值相符的硬碟位置。 例如，如果您提供值 D： \\ datacache，則命令輸出中會顯示該值。

    -   DataCache |MaxCacheSizeAsPercentageOfDiskVolume：顯示與您在 SetBCCache 命令的–百分比參數中提供的值相符的數位。 例如，如果您提供值20，該值就會顯示在命令輸出中。

若要繼續使用本指南，請參閱 [在託管快取伺服器上 Prehash 和預先載入內容 &#40;選擇性&#41;](7-Bc-Prehash-Preload.md)。