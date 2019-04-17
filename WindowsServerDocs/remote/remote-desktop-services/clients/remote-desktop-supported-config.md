---
title: 遠端桌面用戶端-支援的組態
description: 了解您可以存取使用遠端桌面用戶端的 Pc
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
ms.sourcegitcommit: 96e968bbe8dc50ebb1535ae1c8ce92fa73c83171
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2018
ms.locfileid: "1978045"
---
# <a name="remote-desktop-client---supported-configuration"></a>遠端桌面用戶端-支援的組態

## <a name="supported-pcs"></a>支援的電腦
您可以連線到執行下列 Windows 作業系統的電腦：
- Windows 10 專業版
- Windows 10 企業版
- Windows 8 企業版
- Windows 8 專業版
- Windows 7 專業版
- Windows 7 企業版
- Windows7 旗艦版
- Windows7 旗艦版
- Windows Server 2008
- WindowsServer 2008 R2
- WindowsServer 2012
- WindowsServer 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

下列電腦可執行的遠端桌面閘道：

- Windows Server 2008
- WindowsServer 2008 R2
- WindowsServer 2012
- WindowsServer 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

下列作業系統可當作 RD Web Access] 或 [RemoteApp 伺服器：
- WindowsServer 2008 R2
- WindowsServer 2012
- WindowsServer 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>不支援的 Windows 版本

遠端桌面用戶端不會連線至下列 Windows 版本和版本：

- Windows7 簡易版
- Windows 7 首頁
- Windows 8 首頁
- Windows 8.1 首頁
- Windows10 家用版

如果您要存取已安裝下列 Windows 版本的其中一個電腦，我們建議您升級至 Windows 版本支援 RDP。

## <a name="rd-gateway-messaging-is-not-supported"></a>不支援 RD 閘道通訊
遠端桌面用戶端不支援 RD 閘道通訊。 確認 [遠端桌面資源存取原則 (RD RAP) RD 閘道伺服器不會指定**只允許支援 RD 閘道通訊的電腦**或無法連線。