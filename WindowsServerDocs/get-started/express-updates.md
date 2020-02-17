---
title: 2018 年 11 月更新中已重新啟用 Windows Server 2016 快速更新
description: 提供 Windows Server 2016 快速更新的相關資訊
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 57a6d13836a4f8c633afa6d37e7fa42e6d817deb
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822051"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>2018 年 11 月更新中已重新啟用 Windows Server 2016 快速更新

> 作者：Joel Frauenheim
> 
> 適用於：Windows Server 2016

從 2018 年 11 月 13 日星期二更新開始，Windows 將會再次發佈適用於 Windows Server 2016 的快速更新。 適用於 Windows Server 2016 快速更新在 2017 年中發現導致更新無法正確安裝的重大問題之後已停止。 雖然此問題已在 2017 年 11 月獲得修正，但更新小組採取發佈快速套件的保守做法，以確保大多數客戶會在其伺服器環境中安裝 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953))，而不會受到此問題的影響。

WSUS 和 Configuration Manager 的系統管理員必須注意，在 2018 年 11 月會再次看到適用於 Windows Server 2016 的兩個更新套件：完整更新和快速更新。 如果系統管理員想為其伺服器環境使用「快速」，則必須確認該裝置自 2017 年 11 月 14 日 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 起已接受完整更新，以確保快速更新可正確地安裝。 在 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 以後尚未更新的任何裝置，如果嘗試進行快速更新，則會看到無限迴圈取用頻寬和 CPU 資源的重複失敗。  若要修復該狀態，系統管理員必須停止推送快速更新，並推送最新完整更新，以停止失敗迴圈。

透過 2018 年 11 月 13 日的快速更新，客戶會看到其管理系統與 Windows Server 2016 端點之間的套件大小立即減少。  