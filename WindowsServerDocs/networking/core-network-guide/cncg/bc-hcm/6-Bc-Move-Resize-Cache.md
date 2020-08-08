---
title: 移動並調整託管快取的大小 (選用)
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8eed5b91d0634a8409b6b6e26f823ffe86ea8a14
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964264"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移動並調整託管快取的大小（ \( 選擇性）\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式將託管快取移至您偏好的磁片磁碟機和資料夾，並指定託管快取伺服器可用於託管快取的磁碟空間量。

此程序是選用的。 如果預設快取位置 \( % windir% \\ ServiceProfiles \\ NetworkService \\ AppData \\ Local \\ PeerDistPub \) 和 size （這是總硬碟空間的5%）適用于您的部署，則不需要變更它們。

您必須是 Administrators 群組的成員，才能執行此程式。

### <a name="to-move-and-resize-the-hosted-cache"></a>移動和調整託管快取的大小

1. 以系統管理員許可權開啟 Windows PowerShell。

2. 輸入下列命令，將託管快取移至本機電腦上的另一個位置，然後按 ENTER。

    > [!IMPORTANT]
    > 執行下列命令之前，請將參數值（例如– Path 和– MoveTo）取代為適用于您的部署的值。

    ```
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ```

3.  輸入下列命令來調整託管快取的大小–尤其是 \- 本機電腦上的 datacache。 按 ENTER 鍵。

    > [!IMPORTANT]
    > 執行下列命令之前，請將參數值（例如 \- 百分比）取代為適用于您的部署的值。

    ```
    Set-BCCache -Percentage 20
    ```

4.  若要確認託管快取伺服器設定，請輸入下列命令，然後按 ENTER。

    ```
    Get-BCStatus
    ```

    命令的結果會顯示 BranchCache 安裝所有層面的狀態。 以下是幾個 BranchCache 設定和每個專案的正確值：

    -   DataCache |CacheFileDirectoryPath：顯示符合您使用 SetBCCache 命令的– MoveTo 參數提供之值的硬碟位置。 例如，如果您提供值 D： \\ datacache，則該值會顯示在命令輸出中。

    -   DataCache |MaxCacheSizeAsPercentageOfDiskVolume：顯示與您使用 SetBCCache 命令的–百分比參數提供的值相符的數位。 例如，如果您提供值20，則該值會顯示在命令輸出中。

若要繼續進行本指南，請參閱[預先雜湊和預先載入託管快取伺服器上的內容 &#40;選擇性&#41;](7-Bc-Prehash-Preload.md)。