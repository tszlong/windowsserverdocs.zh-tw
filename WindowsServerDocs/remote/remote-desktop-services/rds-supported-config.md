---
title: 支援遠端桌面服務的設定
description: 提供 Windows Server 2016 中支援 RDS 設定的相關資訊。
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66453081"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Windows Server 2016 中支援遠端桌面服務的設定

> 適用於：Windows Server 2016

討論支援遠端桌面服務環境的設定時，最大的問題通常是版本互通性。 大多數的環境都包含多個 Windows Server 版本，例如，您目前可能有 Windows Server 2012 R2 RDS 部署，但想要升級至 Windows Server 2016 以利用新功能 (像是支援 OpenGL\OpenCL、離散裝置指派或儲存空間直接存取)。 問題接著變成：哪些 RDS 元件可以與不同版本搭配使用，而且它們需要都一樣嗎？

因此，考慮到這一點，以下是 Windows Server 2016 中支援遠端桌面服務設定的基本方針。

> [!NOTE]
> 請務必檢閱 [Windows Server 2016 的系統需求](../../get-started/system-requirements.md)。

## <a name="best-practices"></a>最佳作法
- 針對您的遠端桌面基礎結構使用 Windows Server 2016：Web 存取、閘道、連線代理人和授權伺服器。 Windows Server 2016 可與這些元件回溯相容，因此，2012 R2 RD 工作階段主機可以連線至 2016 RD 連線代理人，但反之則不行。

- 針對 RD 工作階段主機：集合中的所有工作階段主機需要位於相同層級，但可以有多個集合。 您可以有一個 Windows Server 2012 R2 工作階段主機的集合，以及一個 Windows Server 2016 工作階段主機的集合。

- 如果您將 RD 工作階段主機升級至 Windows Server 2016，也請升級授權伺服器。 請記住，2016 授權伺服器可以處理來自所有舊版 Windows Server (最低至 Windows Server 2003) 的 CAL。

- 遵循[升級您的遠端桌面服務環境](upgrade-to-rds.md#flow-for-deployment-upgrades)中建議的升級順序。 

- 如果您要建立高可用性的環境，所有連線代理人皆需位於相同的 OS 層級。

## <a name="rd-connection-brokers"></a>RD 連線代理人

使用同樣執行 Windows Server 2016 的遠端桌面工作階段主機 (RDSH) 和遠端桌面虛擬主機 (RDVH) 時，Windows Server 2016 移除了您可在部署中擁有的連線代理人數目限制。 下表顯示在包含三個或多個連線代理人的高可用性部署中，哪些版本的 RDS 元件可以與 2016 和 2012 R2 版本的連線代理人一起運作。

| HA 中有 3 個以上的 RD 連線代理人              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Windows Server 2016 連線代理人    | 支援 | 支援 | 支援     | 支援     |
| Windows Server 2012 R2 連線代理人 | 不適用       | 不適用       | 支援     | 支援     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>使用 Hyper-V 支援 GPU 加速
下表詳細說明虛擬機器上對 GPU 加速的支援。 請參閱[哪一種圖形虛擬化技術適合您？](rds-graphics-virtualization.md)以協助找出您所需的技術。 如需 DDA 的特定資訊，請參閱[規劃部署離散裝置指派](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

|VM 客體 OS  |Windows Server 2012 R2 或 Windows Server 2016<br> Hyper-V RemoteFX vGPU (第 1 代 VM) |  Windows Server 2016  Hyper-V RemoteFX vGPU (第 2 代 VM) |  Windows Server 2016  Hyper-V 離散裝置指派 (第 2 代 VM) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | 是                                                        | 否                                                     | 否                                                                  |
| Windows 8.1                 | 是                                                        | 否                                                     | 否                                                                  |
| Windows 10 1511 更新      | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2      | 是                                                        | 否                                                     | 是 (需要 KB 3133690)                                           |
| Windows Server 2016         | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2 RDSH | 否                                                         | 否                                                     | 是 (需要 KB 3133690)                                           |
| Windows Server 2016 RDSH    | 否                                                         | 否                                                     | 是                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>VDI 部署 - 支援的客體 OS 
Windows Server 2016 RD 虛擬主機伺服器支援下列客體 OS：

- Windows 10 企業版
- Windows 8.1 Enterprise 
- Windows 8 企業版 
- Windows 7 SP1 企業版 

下表顯示支援的 RD 虛擬主機作業系統和客體作業系統組合：

| RDVH OS 版本        | 客體 OS 版本           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1、Windows 8、Windows 8.1、Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1、Windows 8、Windows 8.1、Windows 10 |
| Windows Server 2012      | Windows 7 SP1、Windows 8、Windows 8.1 |

> [!NOTE]  
> - Windows Server 2016 遠端桌面服務不支援異質集合。 集合中的所有 VM 都必須是同一個 OS 版本。 
> - 您可以在相同主機上具有個別包含不同客體 OS 版本的同質集合。 
> - VM 範本必須建立於 Windows Server 2016 Hyper-V 主機上，以用來作為 Windows Server 2016 Hyper-V 主機上的客體 OS。

## <a name="single-sign-on-sso"></a>單一登入 (SSO)
Windows Server 2016 RDS 支援兩種主要的 SSO 體驗：

 - 應用程式內 (Windows、iOS、Android 及 Mac 上的遠端桌面應用程式)
 - 網頁 SSO
 
使用遠端桌面應用程式，您可以安全地透過每個作業系統獨有的機制，將認證儲存為連線資訊的一部分 ([Mac](clients/remote-desktop-mac.md)) 或儲存為受控帳戶的一部分 ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts)、[Android](clients/remote-desktop-android.md#manage-your-user-accounts)、Windows)。

若要透過 Windows 上的收件匣遠端桌面連線用戶端，利用 SSO 連線到桌面與 RemoteApps，您必須透過 Internet Explorer 連線到 RD 網頁。 以下為伺服器端上必要的設定選項。 Web SSO 不支援任何其他設定：

 - RD Web 會設定為表單型驗證 (預設值)
 - RD 閘道會設定為密碼驗證 (預設值)
 - RDS 部署會在 RD 閘道屬性中設定為 [將 RD 閘道認證用於遠端電腦] (預設值)

> [!NOTE]
> 智慧卡因為必要的設定選項而不支援 Web SSO。 透過智慧卡登入的使用者可能面臨多個登入提示。

如需如何建立遠端桌面服務的 VDI 部署詳細資訊，請查看[遠端桌面服務 VDI 支援的 Windows 10 安全性設定](rds-vdi-supported-config.md)。

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>搭配應用程式 Proxy 服務使用遠端桌面服務

您可以搭配 [Azure AD 應用程式 Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop) \(部分機器翻譯\) 使用遠端桌面服務 (但 Web 用戶端除外)。 遠端桌面服務不支援使用 [Web 應用程式 Proxy](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server) \(部分機器翻譯\)，其隨附於 Windows Server 2016 和更早版本。
