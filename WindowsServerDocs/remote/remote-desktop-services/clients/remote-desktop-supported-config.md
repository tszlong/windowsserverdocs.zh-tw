---
title: 遠端桌面用戶端：支援的設定
description: 了解您可以使用遠端桌面用戶端存取哪些電腦
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d38008b6387385917ad21ce7e169b8ff3f4d18ba
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748970"
---
# <a name="remote-desktop-client---supported-configuration"></a>遠端桌面用戶端：支援的設定

## <a name="supported-pcs"></a>支援的電腦
您可以連線至執行下列 Windows 作業系統的電腦：
- Windows 10 專業版
- Windows 10 企業版
- Windows 8 企業版
- Windows 8 專業版
- Windows 7 專業版
- Windows 7 企業版
- Windows 7 旗艦版
- Windows 7 旗艦版
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

下列電腦可以執行遠端桌面閘道：

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

下列作業系統可以用作 RD Web 存取或 RemoteApp 伺服器：
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

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