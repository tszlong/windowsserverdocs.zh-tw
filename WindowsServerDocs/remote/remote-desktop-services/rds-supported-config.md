---
title: 支援遠端桌面服務的組態
description: 提供 Windows Server 2016 中的 RDS 的支援組態的相關資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 8571c2220f804a27e4e1a6b744e8e15e38bd53a3
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453081"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>支援的 Windows Server 2016 中的遠端桌面服務的組態

> 適用於：Windows Server 2016

談到 Remote Desktop Services 環境的支援設定時，最大的問題通常是版本互通性。 大部分的環境中有多個版本的 Windows Server-比方說，您可能有現有的 Windows Server 2012 R2 RDS 部署，但想要升級至 Windows Server 2016，以便充分利用新的功能 （像是 OpenGL\OpenCL，離散的支援裝置指派或儲存空間直接存取）。 然後的問題就變成： 哪一個 RDS 元件可以使用不同版本，以及哪些需要相同嗎？

因此這一點之後，以下是基本指導方針的 Windows Server 2016 中的遠端桌面服務的支援設定。

> [!NOTE]
> 請務必檢閱[適用於 Windows Server 2016 的系統需求](../../get-started/system-requirements.md)。

## <a name="best-practices"></a>最佳作法
- 使用遠端桌面基礎結構-Web 存取、 閘道、 連線代理人和授權伺服器的 Windows Server 2016。 Windows Server 2016 是針對這些元件的回溯相容，因此 2012 R2 RD 工作階段主機可以連接到 2016 RD 連線代理人，但是反向執行則不成立。

- RD 工作階段主機-集合中的所有工作階段主機需要是在相同的層級，但可以有多個集合。 您可以使用 Windows Server 2012 R2 工作階段主機收集，與 Windows Server 2016 工作階段主機的另一個。

- 如果您升級至 Windows Server 2016 程式 RD 工作階段主機，也會升級授權伺服器。 請記住 2016年授權伺服器可以處理來自所有舊版的 Windows Server、 Windows Server 2003 下的 Cal。

- 請遵循建議的升級順序[升級您的遠端桌面服務環境](upgrade-to-rds.md#flow-for-deployment-upgrades)。 

- 如果您要建立高可用性環境，所有連線代理人需要位於相同的 OS 層級。

## <a name="rd-connection-brokers"></a>RD 連線代理人

Windows Server 2016 移除部署中連接代理程式，您可以擁有數目的限制，使用遠端桌面工作階段主機 (RDSH) 」 和 「 遠端桌面虛擬化主機 (RDVH) 也執行 Windows Server 2016 時。 下表顯示包含三個或多個連線代理人高可用性部署中的哪些版本的 RDS 元件如何與連線代理人的 2016年和 2012 R2 版本。

| 在 HA 3 + 連線代理人              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Windows Server 2016 連線代理人    | 支援 | 支援 | 支援     | 支援     |
| Windows Server 2012 R2 Connection Broker | N/A       | N/A       | 支援     | 支援     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>踏上雲端的 GPU 加速功能的支援
下表詳細說明虛擬機器上的 GPU 加速功能的支援。 請參閱[哪一種圖形虛擬化技術適合您？](rds-graphics-virtualization.md)協助找出您所需要的。 如需 DDA 特定資訊，請參閱[規劃的部署不連續的裝置指派](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

|VM 客體 OS  |Windows Server 2012 R2 或 Windows Server 2016<br> HYPER-V RemoteFX vGPU (第 1 代 VM) |  Windows Server 2016 HYPER-V RemoteFX vGPU (第 2 代 VM) |  Windows Server 2016 HYPER-V 不連續的裝置指派 （第 2 代 VM） |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | 是                                                        | 否                                                     | 否                                                                  |
| Windows 8.1                 | 是                                                        | 否                                                     | 否                                                                  |
| Windows 10 1511 版更新      | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2      | 是                                                        | 否                                                     | [是] （需要 KB 3133690）                                           |
| Windows Server 2016         | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2 RDSH | 否                                                         | 否                                                     | [是] （需要 KB 3133690）                                           |
| Windows Server 2016 RDSH    | 否                                                         | 否                                                     | 是                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>VDI 部署 – 支援的客體 Os 
Windows Server 2016 的 rd 工作階段主機伺服器支援下列客體作業系統：

- Windows 10 企業版
- Windows 8.1 Enterprise 
- Windows 8 企業版 
- Windows 7 SP1 Enterprise 

下表顯示支援的 RD 虛擬化主機作業系統和客體作業系統組合：

| RDVH OS 版本        | 客體 OS 版本           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1，Windows 8，Windows 8.1，Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1，Windows 8，Windows 8.1，Windows 10 |
| Windows Server 2012      | Windows 7 SP1，Windows 8，Windows 8.1 |

> [!NOTE]  
> - Windows Server 2016 遠端桌面服務不支援異質集合。 集合中的所有 Vm 必須都是相同的 OS 版本。 
> - 您可以在相同主機上有不同的同質集合使用不同的客體 OS 版本。 
> - 必須做為 Windows Server 2016 HYPER-V 主機上的客體 OS 的 Windows Server 2016 HYPER-V 主機上建立 VM 範本。

## <a name="single-sign-on-sso"></a>單一登入 (SSO)
Windows Server 2016 RDS 支援兩種主要的 SSO 體驗：

 - 在應用程式 （Windows、 iOS、 Android 及 Mac 上的遠端桌面應用程式）
 - 網頁 SSO
 
使用遠端桌面應用程式，您可以儲存的認證可以是一部分的連接資訊 ([Mac](clients/remote-desktop-mac.md)) 或做為受管理帳戶的一部分 ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts)， [Android](clients/remote-desktop-android.md#manage-your-user-accounts)，Windows)安全地透過每個作業系統獨有的機制。

若要透過 Windows 的收件匣遠端桌面連線用戶端連接到桌面與 Remoteapp 與 SSO 搭配運作，您必須連接到 RD 網頁透過 Internet Explorer。 下列組態選項所需的伺服器端上。 Web sso 不支援任何其他設定：

 - RD Web 設定為表單型驗證 （預設值）
 - RD 閘道設定為 密碼驗證 （預設值）
 - RDS 部署設定為 「 使用 RD 閘道認證用於遠端電腦 」 （預設） 中的 RD 閘道屬性

> [!NOTE]
> 因為必要的設定選項 中，智慧卡不支援 Web SSO。 透過智慧卡的登入，可能會面臨多個提示登入的使用者。

如需有關如何建立遠端桌面服務的 VDI 部署的詳細資訊，請查看[支援 Windows 10 安全性設定的遠端桌面服務 VDI](rds-vdi-supported-config.md)。

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>使用遠端桌面服務與應用程式 proxy 服務

您可以使用遠端桌面服務中，web 用戶端，除了[Azure AD 應用程式 Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop)。 遠端桌面服務不支援使用[Web 應用程式 Proxy](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server)，隨附於 Windows Server 2016 和更早版本。
