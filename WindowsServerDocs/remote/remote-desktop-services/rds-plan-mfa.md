---
title: 遠端桌面服務-Multi-factor Authentication
description: 搭配 RDS 使用 MFA 的規劃資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 5ca2a29b0287dbd940afeb4404a85f1d978447f9
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805108"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>遠端桌面服務-Multi-factor Authentication

>適用於：Windows Server （半年通道），Windows Server 2019，Windows Server 2016

運用 Active Directory 與 Multi-factor Authentication 的強大功能來強制執行商務資源的高安全性保護。

針對連線到他們的桌上型電腦和應用程式使用者，體驗是類似項目已經面對執行第二個驗證量值來連接到所需的資源：
- 啟動 桌面 或 RemoteApp 從 RDP 檔，或透過遠端桌面用戶端應用程式
- 一旦連線到 RD 閘道的安全遠端存取，會收到簡訊或行動應用程式 MFA 挑戰
- 正確的驗證和連線到其資源 ！

如需設定程序的詳細資訊，請參閱[使用網路原則伺服器 (NPS) 擴充功能和 Azure AD 將遠端桌面閘道基礎結構整合](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)。
