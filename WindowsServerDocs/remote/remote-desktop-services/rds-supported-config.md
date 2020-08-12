---
title: 支援遠端桌面服務的設定
description: 提供 Windows Server 2016 和 Windows Server 2019 中支援 RDS 設定的相關資訊。
ms.author: elizapo
ms.date: 07/14/2020
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 47aa9327e70d07ce46477024fb0c734ea1d64603
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954835"
---
# <a name="supported-configurations-for-remote-desktop-services"></a>支援遠端桌面服務的設定

> 適用於：Windows Server 2016、Windows Server 2019

討論支援遠端桌面服務環境的設定時，最大的問題通常是版本互通性。 大多數的環境都包含多個 Windows Server 版本，例如，您目前可能有 Windows Server 2012 R2 RDS 部署，但想要升級至 Windows Server 2016 以利用新功能 (像是支援 OpenGL\OpenCL、離散裝置指派或儲存空間直接存取)。 問題接著變成：哪些 RDS 元件可以與不同版本搭配使用，而且它們需要都一樣嗎？

因此，考慮到這一點，以下是 Windows Server 中支援遠端桌面服務設定的基本方針。

> [!NOTE]
> 請務必檢閱 [Windows Server 2016 的系統需求](../../get-started/system-requirements.md)和 [Windows Server 2019 的系統需求](../../get-started-19/sys-reqs-19.md)。

## <a name="best-practices"></a>最佳作法

- 針對您的遠端桌面基礎結構使用 Windows Server 2019 (Web 存取、閘道、連線代理人和授權伺服器)。 Windows Server 2019 與這些元件回溯相容，這表示 Windows Server 2016 或 Windows Server 2012 R2 RD 工作階段主機可以連線到 2019 RD 連線代理人，但反之則無法連線。

- 針對 RD 工作階段主機：集合中的所有工作階段主機需要位於相同層級，但可以有多個集合。 您可以有一個 Windows Server 2016 工作階段主機的集合，以及一個 Windows Server 2019 工作階段主機的集合。

- 如果您將 RD 工作階段主機升級至 Windows Server 2019，也請升級授權伺服器。 請記住，2019 授權伺服器可以處理來自所有舊版 Windows Server (最低至 Windows Server 2003) 的 CAL。

- 遵循[升級您的遠端桌面服務環境](upgrade-to-rds.md#flow-for-deployment-upgrades)中建議的升級順序。

- 如果您要建立高可用性的環境，所有連線代理人皆需位於相同的 OS 層級。

## <a name="rd-connection-brokers"></a>RD 連線代理人

使用同樣執行 Windows Server 2016 的遠端桌面工作階段主機 (RDSH) 和遠端桌面虛擬主機 (RDVH) 時，Windows Server 2016 移除了您可在部署中擁有的連線代理人數目限制。 下表顯示在包含三個或多個連線代理人的高可用性部署中，哪些版本的 RDS 元件可以與 2016 和 2012 R2 版本的連線代理人一起運作。

|HA 中有 3 個以上的 RD 連線代理人|RDSH 或 RDVH 2019|RDSH 或 RDVH 2016|RDSH 或 RDVH 2012 R2|
|---|---|---|---|
 |Windows Server 2019 連線代理人|支援|支援|支援|
 |Windows Server 2016 連線代理人|不適用|支援|支援|
 |Windows Server 2012 R2 連線代理人|不適用|不適用|不支援|

## <a name="support-for-graphics-processing-unit-gpu-acceleration"></a>支援圖形處理器 (GPU) 加速

遠端桌面服務支援已安裝 GPU 的系統。 可以透過遠端連線來使用需要 GPU 的應用程式。 此外，也可以啟用 GPU 加速轉譯和編碼，以改善應用程式效能和擴充性。

遠端桌面服務工作階段裝載和單一工作階段的用戶端作業系統，可以透過許多方式利用提供給作業系統的實體或虛擬 GPU，其中包含 [Azure GPU 最佳化的虛擬機器大小](/en-us/azure/virtual-machines/windows/sizes-gpu)、實體 RDSH 伺服器可用的 GPU 以及由支援的 Hypervisor 提供給 VM 的 GPU。

請參閱[哪一種圖形虛擬化技術適合您？](rds-graphics-virtualization.md)以協助找出您所需的技術。 如需 DDA 的特定資訊，請參閱[規劃部署離散裝置指派](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

GPU 廠商對於 RDSH 案例可能會有個別的授權配置，或限制伺服器作業系統上的 GPU 使用，請向您最愛的廠商確認需求。

由非 Microsoft Hypervisor 或雲端平台所提供的 GPU，必須具有 WHQL 簽署的驅動程式，並由 GPU 廠商提供。

### <a name="remote-desktop-session-host-support-for-gpus"></a>遠端桌面工作階段主機支援 GPU

下表顯示由不同版本 RDSH 主機所支援的案例。

|功能|Windows Server 2008 R2|Windows Server 2012 R2|Windows Server 2016|Windows Server 2019|
|---|---|---|---|---|
|在所有 RDP 工作階段使用硬體 GPU|否|是|是|是|
|H.264/AVC 硬體編碼 (如果受 GPU 支援)|否|否|是|是|
|提供給作業系統之多個 GPU 之間的負載平衡|否|否|否|是|
|H.264/AVC 編碼最佳化，可將頻寬使用量降到最低|否|否|否|是|
|H.264/AVC 支援4K 解析度|否|否|否|是|

### <a name="vdi-support-for-gpus"></a>GPU 的 VDI 支援

下表顯示用戶端作業系統中對 GPU 案例的支援。

|功能|Windows 7 SP1|Windows 8.1|Windows 10|
|---|---|---|---|
|在所有 RDP 工作階段使用硬體 GPU|否|是|是|
|H.264/AVC 硬體編碼 (如果受 GPU 支援)|否|否|Windows 10 版本 1703 和更新版本|
|提供給作業系統之多個 GPU 之間的負載平衡|否|否|Windows 10 版本 1803 和更新版本|
|H.264/AVC 編碼最佳化，可將頻寬使用量降到最低|否|否|Windows 10 版本 1803 和更新版本|
|H.264/AVC 支援4K 解析度|否|否|Windows 10 版本 1803 和更新版本|

### <a name="remotefx-3d-video-adapter-vgpu-support"></a>RemoteFX 3D 視訊卡 (vGPU) 支援

> [!NOTE]
> 基於安全性考量，自 2020 年 7 月 14 日的安全性更新開始，預設會停用所有 Windows 版本上的 RemoteFX vGPU。 若要深入了解，請參閱 [KB 4570006](https://support.microsoft.com/help/4570006)。

當 VM 在 Windows Server 2012 R2 或 Windows Server 2016 上以 Hyper-V 客體執行時，遠端桌面服務支援 RemoteFX vGPU。 下列客體作業系統具有 RemoteFX vGPU 支援：

- Windows 7 SP1
- Windows 8.1
- Windows 10 版本 1703 或更新版本
- 僅限單一工作階段部署中的 Windows Server 2016

### <a name="discrete-device-assignment-support"></a>離散裝置指派支援

遠端桌面服務支援 Windows Server 2016 或 Windows Server 2019 Hyper-V 主機上隨離散裝置指派一起提供的實體 GPU。 如需詳細資訊，請參閱[部署離散裝置指派的規劃](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。


## <a name="vdi-deployment--supported-guest-oses"></a>VDI 部署 - 支援的客體作業系統

Windows Server 2016 和 Windows Server 2019 RD 虛擬主機伺服器支援下列客體作業系統：

- Windows 10 Enterprise
- Windows 8.1 Enterprise
- Windows 7 SP1 企業版

> [!NOTE]
> - 遠端桌面服務不支援異質工作階段集合。 集合中的所有 VM 作業系統都必須是相同的版本。
> - 您可以在相同主機上具有個別包含不同客體 OS 版本的同質集合。
> - 用來執行 VM 的 Hyper-V 主機必須與用來建立原始 VM 範本的 Hyper-V 主機版本相同。

## <a name="single-sign-on"></a>單一登入

Windows Server 2016 和 Windows Server 2019 RDS 支援兩種主要的 SSO 體驗：

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

您可以使用遠端桌面服務搭配 [Azure AD 應用程式 Proxy](/azure/active-directory/application-proxy-publish-remote-desktop)。 遠端桌面服務不支援使用 [Web 應用程式 Proxy](../remote-access/web-application-proxy/web-application-proxy-windows-server.md) \(部分機器翻譯\)，其隨附於 Windows Server 2016 和更早版本。
