---
title: 遠端桌面選項
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51076946-ea9b-4ac7-9a6e-d6023816b97d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: f4603adb7da1df0853b4c816254241d21b969fc5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625939"
---
# <a name="remote-desktop-options"></a>遠端桌面選項

## <a name="connection-speed"></a>連線速度
 連線到使用遠端 Web 存取之網路電腦的速度會決定您在主機電腦上可用的桌面選項。 下表顯示您透過遠端 Web 存取連線到遠端電腦的速度，可使用的桌面選項。

| Desktop 選項 | 慢速數據機 (28.8 Kbps) | 快速數據機 (56 Kbps) (預設值) | 寬頻 (128 Kbps - 1.5 Mbps) | 區域網路 (1.5 Mbps 或更高) |
|--|--|--|--|--|
| 字型平滑處理 | 否 | 否 | 否 | 是 |
| 桌面轉譯緩衝處理 | 否 | 否 | 是 | 是 |
| 拖曳時顯示視窗的內容 | 否 | 否 | 是 | 是 |
| 功能表及視窗動畫 | 否 | 否 | 是 | 是 |
| 佈景主題 | 否 | 是 | 是 | 是 |
| 點陣圖快取 | 是 | 是 | 是 | 是 |

## <a name="screen-size"></a>螢幕大小
 此選項會決定當您透過遠端存取網站連線到遠端電腦時，在本機電腦上開啟的視窗大小。 視窗大小以像素表示。

> [!NOTE]
>  當您連線到伺服器時，會開啟儀表板。 儀表板預設大小是 1024 x 741，大小可以調整。

-   全螢幕 (多重監視器)

-   1280 x 720

-   1024 x 768

-   800 x 600

-   640 x 480

## <a name="enable-the-remote-computer-to-print-to-my-local-printer"></a>讓遠端電腦使用我的本機印表機列印
 預設為啟用。 此選項可讓您從遠端電腦列印到連接在本機電腦的印表機。

## <a name="play-sounds-from-the-remote-computer"></a>從遠端電腦播放音效
 預設為啟用。 此選項可讓您從遠端電腦在本機電腦上播放音效，例如系統音效。

## <a name="enable-copy-and-paste-between-the-remote-computer-and-the-local-computer"></a>可在遠端電腦與本機電腦間執行複製與貼上
 預設不啟用。 此選項可讓您在遠端電腦和本機電腦之間以相同的方式複製與貼上檔案，就像從本機電腦上某個位置複製與貼上檔案到另一個位置一樣。

## <a name="enable-the-remote-computer-to-access-drives-on-my-local-computer"></a>讓遠端電腦存取我的本機電腦上的磁碟機
 預設不啟用。 此選項可讓您從遠端電腦存取連接到本機電腦之硬碟上的檔案和資料夾。

## <a name="additional-references"></a>其他參考資料

-   [管理遠端 Web 存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)
