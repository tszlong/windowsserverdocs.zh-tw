---
title: 遠端桌面用戶端：支援的設定
description: 了解您可以使用遠端桌面用戶端存取哪些電腦
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: b11b4382603165e5b3ba34cbf63d543f466c7480
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955025"
---
# <a name="remote-desktop-client---supported-configuration"></a>遠端桌面用戶端：支援的設定

## <a name="supported-pcs"></a>支援的電腦
您可以連線至執行下列 Windows 作業系統的電腦：
- Windows 10 Pro
- Windows 10 Enterprise
- Windows 8 企業版
- Windows 8 專業版
- Windows 7 Professional
- Windows 7 Enterprise
- Windows 7 旗艦版
- Windows 7 旗艦版
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

下列電腦可以執行遠端桌面閘道：

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

下列作業系統可以用作 RD Web 存取或 RemoteApp 伺服器：
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>不支援的 Windows 版本

遠端桌面用戶端不會連線到這些 Windows 版本：

- Windows 7 簡易版
- Windows 7 家用版
- Windows 8 家用版
- Windows 8.1 家用版
- Windows 10 Home

如果您想要存取安裝上述其中一個 Windows 版本的電腦，我們建議您升級至支援 RDP 的 Windows 版本。

## <a name="rd-gateway-messaging-is-not-supported"></a>不支援 RD 閘道訊息
遠端桌面用戶端不支援 RD 閘道訊息。 請確認 RD 閘道伺服器的遠端桌面資源存取原則 (RD RAP) 並未指定 [只允許支援 RD 閘道訊息的電腦]  ，否則您將無法連線。