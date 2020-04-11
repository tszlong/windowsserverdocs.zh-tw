---
title: 遠端桌面服務 - 多重要素驗證
description: 將 MFA 用於 RDS 的規劃資訊。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: c46ad24c62510b4a100a89b5c10a8f52c1a66151
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857351"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>遠端桌面服務 - 多重要素驗證

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

搭配運用 Active Directory 與多重要素驗證的強大功能，強制執行商務資源的高安全性保護。

對連線至桌面和應用程式的使用者而言，其體驗會類似於他們在執行第二項驗證機制以連線至所需資源時曾經歷的體驗：
- 從 RDP 檔案或透過遠端桌面用戶端應用程式，啟動桌面或 RemoteApp
- 在連線至 RD 閘道以進行安全的遠端存取時，會收到 SMS 或行動應用程式 MFA 挑戰
- 正確地進行驗證並連線至其資源！

如需設定程序的詳細資訊，請參閱[使用網路原則伺服器 (NPS) 擴充功能和 Azure AD 整合遠端桌面閘道基礎結構](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway) (機器翻譯)。
