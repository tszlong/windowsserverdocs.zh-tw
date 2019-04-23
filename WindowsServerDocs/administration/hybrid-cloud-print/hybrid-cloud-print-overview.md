---
title: Windows Server 混合式雲端列印概觀
description: 混合式雲端列印可讓 IT 專業人員，以支援列印需求 BYOD 或網域已加入裝置。
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: store
ms.technology: server-general
author: TrudyHa
ms.author: TrudyHa
ms.date: 10/16/2017
ms.openlocfilehash: faa9fde857a9a4ee3f7c03f682b3dbced0340417
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878829"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Windows Server 混合式雲端列印概觀

**適用於**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>什麼是混合式雲端列印？
**混合式雲端列印**是適用於 Windows Server 2016 的新功能可透過**功能隨**。 它可讓 IT 專業人員，以支援列印的需求為攜帶自己的裝置，或使用 Azure Active Directory 已加入裝置的人。 這包括 Windows 電話、 膝上型電腦或執行 Windows 10 或 Windows Mobile 的平板電腦等行動裝置。 它提供列印支援從任何地方的人員具有網際網路存取權。

適用於 IT 管理員**混合式雲端列印**提供安全的使用者存取內部部署的印表機，利用 Azure 的多重要素驗證來驗證使用者的存取權。 單一登入 (SSO) 功能可簡化使用者體驗。 **混合式雲端列印**以 Windows 為基礎**列印伺服器**角色，讓 IT 專業人員的體驗，類似於管理印表機和使用者存取安全性。

**混合式雲端列印**可以讓使用者從他們用來完成其工作-，即使它們不在辦公桌前或是工作場所的裝置列印您組織中。

**混合式雲端列印**支援 Windows 10 Creators Update 及 Windows 10 s。
 
## <a name="feature-summary"></a>功能摘要
**混合式雲端列印**兩個主要的伺服器端元件所組成：**探索**服務，並**Windows 列印**服務。
- **探索**Mopria Alliance 業界標準的支援的印表機探索雲端中的 IIS 服務上執行的服務端點。
- **Windows 列印**服務端點執行 IIS 服務，支援業界標準網際網路列印通訊協定 (IPP) 以確保最廣泛的用戶端作業系統支援。

## <a name="deployment"></a>部署
**混合式雲端列印**支援幾個不同的部署選項，取決於您的組織，需要使用者驗證。 以下是部署可能如下所示：

![圖表，顯示的圖形描述混合式雲端列印方案](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*混合式雲端列印解決方案圖表*

下圖顯示：
- **混合式雲端列印**作為使用者身分識別提供者使用 Azure Active Directory。 
- **Windows 列印**服務和**探索**服務端點會向 Azure Active Directory 來啟用用戶端裝置，以擷取必要的使用者驗證權杖，才能對這些服務使用。 
- MDM 服務，例如**Microsoft Intune**，會在用戶端裝置佈建連線到 Azure Active Directory 所需的原則**Windows 列印**服務和**探索**服務。

這個資料表會有更多的資訊在圖表中的項目上。  

| 元素 | 描述 |
| ------- | ----------- |
| Azure Active Directory  | 提供並控制使用者身分識別和授權功能 |
| Active Directory        | 提供並控制使用者身分識別和授權功能 |
| Azure AD Connect  | 同步處理 Azure AD 之間的使用者認證，在內部部署 AD。 |
| MDM 服務 (Intune) | 提供裝置原則佈建功能，以確保用戶端裝置 （BYOD） 符合公司原則。 |
| Azure AD Proxy | 提供建立，就是從 至 Azure，以允許來自網際網路的特定設定的應用程式流量流至公司網路防火牆後方的長時間執行連線。 |
| Azure Web 應用程式 | 混合式雲端列印解決方案的核心。 提供探索印表機並傳送列印的內容，非網域聯結裝置的所需的 web 端點。 |
| BYOD 裝置 / Windows 列印伺服器多工緩衝處理器 / 印表機 | 這些是做為是。 在部署中的功能並未變更。 |

有兩種方式來安裝**混合式雲端列印**:
- **功能隨**-請參閱 <<c2> [ 設定的功能，在 Windows Server 中以隨](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)若要深入了解新增和移除角色及功能檔案。 
- **Windows Server 2016 設定**-系統管理員可以移至**設定** -> **應用程式** -> **管理選擇性功能** -> **將功能加入**並搜尋上隨選套件的功能 
- PowerShell 命令-PowerShell 中系統管理員 視窗中，執行下列命令：

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
