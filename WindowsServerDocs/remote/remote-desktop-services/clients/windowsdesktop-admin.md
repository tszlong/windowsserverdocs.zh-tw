---
title: 適用於系統管理員的 Windows 桌面用戶端
description: Windows 桌面用戶端上的資訊主要適用於系統管理員。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 09/16/2019
ms.localizationpriority: medium
ms.openlocfilehash: 202b39c33896f6b5e570c8abcf630dad436466c3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861421"
---
# <a name="windows-desktop-client-for-admins"></a>適用於系統管理員的 Windows 桌面用戶端

>適用於：Windows 10 和 Windows 7

本主題包含適用於系統管理員的 Windows 桌面用戶端其他資訊。 如需基本使用方式資訊，請參閱[開始使用 Windows 桌面用戶端](windowsdesktop.md)。

## <a name="installation-options"></a>安裝選項

雖然您的使用者可以在下載後直接安裝用戶端，但如果您要部署到多個裝置，您也可以透過其他方式來部署用戶端。 使用群組原則或 Microsoft Endpoint Configuration Manager 進行部署，可讓您使用命令列以無訊息方式執行安裝程式。 執行下列命令來部署個別裝置或個別使用者的用戶端。

### <a name="per-device-installation"></a>個別裝置安裝

```
msiexec.exe /I <path to the MSI> /qn ALLUSERS=1
```

### <a name="per-user-installation"></a>個別使用者安裝

```
msiexec.exe /i `<path to the MSI>` /qn ALLUSERS=2 MSIINSTALLPERUSER=1
```

## <a name="configuration-options"></a>設定選項

本節說明此用戶端的新設定選項。

### <a name="configure-update-notifications"></a>設定更新通知

根據預設，每當有更新時，用戶端就會通知您。 若要關閉通知，請設定下列登錄資訊：

- **索引鍵：** HKLM\Software\Microsoft\MSRDC\Policies
- **類型：** REG_DWORD
- **名稱：** AutomaticUpdates
- **資料：** 0 = 停用通知。 1 = 顯示通知。

### <a name="configure-user-groups"></a>設定使用者群組

您可以為下列其中一種類型的使用者群組設定用戶端，以決定用戶端接收更新的時間。

#### <a name="insider-group"></a>測試人員群組

測試人員群組適用於早期驗證，並由系統管理員和其選取的使用者所組成。 測試人員群組會做為測試執行，以偵測更新中可能會影響效能的任何問題，然後再將其發行至公用群組。

> [!NOTE]
> 我們建議每個組織在測試人員群組中都有一些使用者，以測試更新並及早攔截問題。

在測試人員群組中，新版本的用戶端會在每個月的第二個星期二發行給使用者，以進行早期驗證。 如果更新沒有問題，則會在兩週後將其發行至公用群組。 當更新準備就緒時，測試人員群組中的使用者將會自動收到更新通知。 您可以在 [Windows 桌面用戶端的新功能](windowsdesktop-whatsnew.md)中，找到更多關於用戶端變更的詳細資訊。

若要設定測試人員群組的用戶端，請設定下列登錄資訊：

- **索引鍵：** HKLM\Software\Microsoft\MSRDC\Policies
- **類型：** REG_SZ
- **名稱：** ReleaseRing
- **資料：** 測試人員

#### <a name="public-group"></a>公用群組

此群組適用於所有使用者，而且是最穩定的版本。 您不需要執行任何動作來設定此群組。

公用群組會在每個月的第四個星期二，收到由測試人員群組測試的用戶端版本。 如果已啟用此設定，則公用群組中的所有使用者都會收到更新通知。
