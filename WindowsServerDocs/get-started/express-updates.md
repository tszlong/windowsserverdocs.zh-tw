---
title: 快速更新重新啟用 2018 年 11 月的 Windows Server 2016 的更新
description: 提供在 Windows Server 2016 Express 的更新資訊
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867439"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>快速更新重新啟用 2018 年 11 月的 Windows Server 2016 的更新

>由 Joel Frauenheim

>適用於：Windows Server 2016

開始使用 2018 年 11 月 13 日更新星期二，Windows 將會再次發行 Express 更新適用於 Windows Server 2016。 在 2017年中的事件，是嚴重的問題找不到所保留的更新無法順利安裝後停止的適用於 Windows Server 2016 express 的更新。 在 2017 年 11 月中已修正此問題，而更新 team 會花費保守的作法，在發佈 Express 封裝以確保大部分的客戶必須在 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 安裝在伺服器上環境並不會受到此問題。

WSUS 和 System Center Configuration Manager (SCCM) 系統管理員必須要注意在 2018 年 11 月他們再次看到兩個更新套件的 Windows Server 2016： 完整的更新，以及快速更新。 想要使用 Express，必須確認裝置已自 2017 年 11 月 14 日起的完整更新其伺服器環境的系統管理員 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 以確保正確安裝快速更新。 自 2017 年 11 月 14 日更新後, 尚未更新任何裝置 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 會重複使用的頻寬和 CPU 資源在無限迴圈中的，如果嘗試快速更新的失敗。  若要停止推送 Express update，並推播停止失敗迴圈的最新的完整更新的系統管理員即為該狀態的補救措施。

2018 年 11 月 13 日與 Express 更新客戶會看到其管理系統和 Windows Server 2016 結束點之間的套件大小的立即降低。  