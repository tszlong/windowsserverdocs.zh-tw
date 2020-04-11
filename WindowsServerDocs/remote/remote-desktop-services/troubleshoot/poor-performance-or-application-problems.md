---
title: 遠端桌面連線期間效能不佳或應用程式發生問題
description: 針對遠端桌面連線期間效能不佳的問題或應用程式問題進行疑難排解。
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 91ced9e729966ee9c46e76d01d7ccbec9a510f5b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857221"
---
# <a name="poor-performance-or-application-problems-during-remote-desktop-connection"></a>遠端桌面連線期間效能不佳或應用程式發生問題

本文將說明使用者在使用遠端桌面功能時，可能會遇到的幾個常見問題。

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>新的 Microsoft Azure 虛擬機器發生間歇性問題

此問題會影響最近佈建的虛擬機器。 使用者連線至虛擬機器後，遠端桌面工作階段不會正確載入所有使用者的設定。

若要處理此問題，請從虛擬機器中斷連線，等候至少 20 分鐘，然後再次連線。

若要解決此問題，請視需要將下列更新套用到虛擬機器：

  - Windows 10 與 Windows Server 2016：KB 4343884，[2018 年 8 月 30 日—KB4343884 (作業系統組建 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2：KB 4343891，[2018 年 8 月 30 日—KB4343891 (每月彙總套件預覽)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Windows 10 版本 1709 上的影片播放問題

使用者連線到執行 Windows 10 版本 1709 的遠端電腦時，就會發生此問題。 當這些使用者使用 VMR9 (影片混合轉譯器 9) 轉碼器播放影片，播放程式僅會顯示黑色視窗。

這是 Windows 10 版本 1709 中的已知問題。 在 Windows 10 版本 1703 中不會發生該問題。

### <a name="desktop-sharing-issues-on-windows-10"></a>Windows 10 上的桌面共用問題

當使用者具有唯讀使用者設定檔 (及關聯的登錄區)，例如在 Kiosk 的情況下，就會發生此問題。 當這類使用者連線到執行 Windows 10 版本 1803 的遠端電腦時，使用者無法共用桌面。

若要修正此問題，請套用 Windows 10 更新 4340917，[2018 年 7 月 24 日—KB4340917 (作業系統組建 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917)。

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>如果已停用 NLA，混合 Windows 10 版本時會發生效能問題

當 NLA 已停用，且執行 Windows 10 的遠端桌面用戶端電腦連線到執行不同版本 Windows 10 的遠端桌面時，就會發生此問題。 遠端桌面用戶端的使用者若使用執行 Windows 10 版本 1709 或較舊版本的電腦，連線到執行 Windows 10 版本 1803 或更新版本的遠端桌面時，會遇到效能不佳的問題。

這是因為若 NLA 已停用，舊版用戶端電腦連線到 Windows 10 版本 1803 或更新版本時，會使用速度較慢的通訊協定。

若要解決此問題，請套用 KB 4340917，[2018 年 7 月 24 日—KB4340917 (作業系統組建 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917)。

### <a name="black-screen-issue"></a>黑色畫面問題

此問題發生在 Windows 8.0、Windows 8.1、Windows 10 RTM 和 Windows Server 2012 R2 中。 使用者在遠端桌面中啟動多個應用程式，然後從工作階段中斷連線。 使用者會定期重新連線到遠端桌面與應用程式互動，並再次中斷連線。 某些時候，當使用者重新連線時，遠端桌面工作階段只會顯示黑色畫面。 若要讓工作階段正確地重新顯示，使用者必須從遠端電腦的主控台或 RDSH 伺服器主控台結束其工作階段，並停止其工作階段的應用程式。

若要解決此問題，請視需要套用下列更新：

  - Windows 8 與 Windows Server 2012：KB4103719，[2018 年 5 月 17 日—KB4103719 (每月彙總套件預覽)](https://support.microsoft.com/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 與 Windows Server 2012 R2：KB4103724，[2018 年 5 月 17 日—KB4103724 (每月彙總套件預覽)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724) 和 KB 4284863，[2018 年 6 月 21 日—KB4284863 (每月彙總套件預覽)](https://support.microsoft.com/help/4284863/windows-81-update-kb4284863)
  - Windows 10：已修正於 KB4284860，[2018 年 6 月 12 日—KB4284860 (作業系統組建 10240.17889)](https://support.microsoft.com/help/4284860/windows-10-update-kb4284860)
