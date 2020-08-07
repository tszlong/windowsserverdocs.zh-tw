---
title: Windows Server 混合式雲端列印總覽
description: 混合式雲端列印可讓 IT 專業人員支援 BYOD 或已加入網域之裝置的列印需求。
ms.topic: conceptual
author: trudyha
ms.author: trudyha
ms.date: 10/16/2017
ms.openlocfilehash: fc76aef0f7fbc9f3c1dd73b94c6510c0ad37034c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879418"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Windows Server 混合式雲端列印總覽

**適用於**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>什麼是混合式雲端列印？
**混合式雲端列印**是 Windows Server 2016 的新功能，可透過**功能隨選**使用。 它可讓 IT 專業人員支援攜帶自己的裝置之人員的列印需求，或使用已加入 Azure Active Directory 的裝置。 這包括行動裝置（例如 Windows phone、筆記本電腦或平板電腦），其執行 Windows 10 或 Windows Mobile。 它可讓任何人都能存取網際網路，提供列印支援。

對於 IT 系統管理員，**混合式雲端列印**會使用 Azure 的多重要素驗證來驗證使用者存取，以提供內部部署印表機的安全使用者存取。 單一登入 (SSO) 功能可簡化使用者體驗。 **混合式雲端列印**是以 Windows**列印伺服器**角色為基礎，為 IT 專業人員提供類似于管理印表機和使用者存取安全性的經驗。

**混合式雲端列印**可讓組織中的人員從他們用來完成工作的裝置進行列印，即使他們離開桌上型電腦或工作場所也一樣。

Windows 10 建立者更新和 Windows 10 S 支援**混合式雲端列印**。

## <a name="feature-summary"></a>功能摘要
**混合式雲端列印**包含兩個主要的伺服器端元件：**探索**服務和**Windows 列印**服務。
- 在支援 Mopria 聯盟產業標準的 IIS 服務上執行的**探索**服務端點，用於在雲端中進行印表機探索。
- 在支援業界標準網際網路列印通訊協定的 IIS 服務上執行的**Windows 列印**服務端點 (IPP) ，以確保最廣泛的用戶端作業系統支援。

## <a name="deployment"></a>部署
**混合式雲端列印**支援幾種不同的部署選項，視您的組織需要使用者驗證的位置而定。 部署的外觀如下：

![顯示混合式雲端列印解決方案之圖形描述的圖表](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*混合式雲端列印解決方案圖表*

此圖顯示：
- 使用 Azure Active Directory 做為使用者識別提供者的**混合式雲端列印**。
- **Windows 列印**服務和**探索**服務端點會向 Azure Active Directory 註冊，讓用戶端裝置能夠抓取所需的使用者驗證權杖，以用於這些服務。
- MDM 服務（例如**Microsoft Intune**）會使用將 Azure Active Directory 連線到**Windows 列印**服務和**探索**服務所需的原則，來布建用戶端裝置。

此資料表包含圖表中元素的詳細資訊。

| 元素 | 描述 |
| ------- | ----------- |
| Azure Active Directory  | 提供和控制使用者身分識別和授權功能 |
| Active Directory        | 提供和控制使用者身分識別和授權功能 |
| Azure AD Connect  | 同步處理 Azure AD 與內部部署 AD 之間的使用者認證。 |
| MDM 服務 (Intune)  | 提供裝置原則布建功能，以確保用戶端裝置 (BYOD 裝置) 符合公司原則的規範。 |
| Azure AD Proxy | 提供從防火牆後方到 Azure 建立的長時間連線，以允許特定設定的應用程式流量從網際網路流到公司網路。 |
| Azure Web 應用程式 | 混合式雲端列印解決方案的核心。 提供必要的 web 端點，以探索印表機並針對未加入網域的裝置傳送列印內容。 |
| BYOD 裝置/Windows 列印伺服器多工緩衝處理器/印表機 | 這些都是現成的。 部署中的功能沒有任何變更。 |

有兩種方式可安裝**混合式雲端列印**：
- * * 依需求提供的功能，請參閱在[Windows Server 中設定隨選功能](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)，以深入瞭解如何新增和移除角色和功能檔案。
- * * Windows Server 2016 設定，系統管理員可以前往 [**設定**] [  ->  **應用程式**] [  ->  **管理選擇性功能**]  ->  [**新增功能**]，並搜尋 [隨選功能] 套件
- PowerShell 命令-在 PowerShell 系統管理員視窗中，執行下列命令：

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
```
