---
title: 版本資訊：Windows Server 版本 1709 中的重要問題
description: 摘要說明需要因應措施以避免損毀、停止回應、安裝失敗、資料遺失的重要問題。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 04/23/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 4eebc498289a81c7f27fcf4b84d81ae13bc38e4f
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133854"
---
# 版本資訊：Windows Server 版本 1709 中的重要問題

>適用於：Windows Server 半年度管道

這些版本資訊摘要說明 Windows Server&reg; 作業系統中最重要的問題，包括可避免或暫時解決問題 (如果已知) 的方式。 如需這個版本的設計變更、新功能及修正的相關資訊，請參閱 [Windows Server 版本 1709 的新功能](whats-new-in-windows-server-1709.md)和特定功能小組的通知。 除非另有說明，否則這其中每個回報的問題都適用於 Windows Server 2016 的所有版本和安裝選項。  

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。  
  
## 儲存空間直接存取
[comment]: # (識別碼： 未知;提交者： stevenek;狀態： 已)  
儲存空間直接存取未隨附於 Windows Server 版本 1709。 如果您在執行 Windows Server 版本 1709 的伺服器上呼叫 *Enable-ClusterStorageSpacesDirect* 或其別名 *Enable-ClusterS2D*，將會收到錯誤訊息「不支援要求的操作」。

此外，也不支援將執行 Windows Server 版本 1709 的伺服器導入 Windows Server 2016 儲存空間直接存取部署。

Windows Server 發行模型提供新的選項，以便與類似的 [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) 和 [Office 365 專業增強版](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US)發行及維護模型取得一致。 半年度管道發行會提供新功能給想要以快速節奏進展的客戶，並且有兩次新版本發行，分別在春季和秋季推出。

Windows Server 半年通道將焦點放在容器和因加快創新而受益的應用程式案例，請參閱此[部落格](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update)，如需詳細資訊。 尋找基礎結構角色，例如儲存空間直接存取的客戶，應該使用長期維護管道發行版本 （適用於現在） 的 Windows Server 2016 和[Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) （即將於本年度稍後） 等。 我們致力於建置超融合式基礎結構的最佳平台，我們會持續開發的新功能，並改善現有的訂閱，根據您的意見反應。 

儲存空間直接存取是在 Windows Server 2016 中導入，而且是我們超交集平台的基礎。 積極採用 Microsoft 超交集平台的市況令人振奮，我們會致力於為客戶提供最好的服務。

我們已經已在車內聆聽您的意見反應，並提供[接下來的創新功能設定](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/)為我們超交集平台運作。 這些功能現已在[Windows 測試人員](https://insider.windows.com/for-business/)組建，而我們希望您來試用，並分享您的意見反應。 正在尋找已驗證超交集解決方案的客戶，建議您加入 [Windows Server 軟體定義](http://microsoft.com/wssd)計畫。
