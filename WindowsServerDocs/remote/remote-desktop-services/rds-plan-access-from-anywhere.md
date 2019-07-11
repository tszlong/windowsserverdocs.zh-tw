---
title: 遠端桌面服務 - 隨處存取
description: RD 閘道的規劃資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: 0d3d8ed036b3befd81da6d5bbe8702ee866c6aa8
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748829"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>遠端桌面服務 - 隨處存取

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

終端使用者可以透過 RD 閘道，從公司防火牆外部安全地連線到內部網路資源。

不論您如何為終端使用者設定桌面，都可以輕鬆地將 RD 閘道插入連線流程以獲得快速且安全的連線。 針對透過已發佈摘要來連線的終端使用者，您可以在設定整體部署屬性時設定 RD 閘道屬性。 針對透過桌面 (不含摘要) 連線的終端使用者，不論使用哪個遠端桌面用戶端應用程式，他們都可以輕鬆地將組織的 RD 閘道名稱新增為連線屬性。

RD 閘道的三個主要目的，按照連線順序依序為：
1. **在終端使用者的裝置和 RD 閘道伺服器之間建立加密 SSL 通道**：若要透過任何 RD 閘道伺服器連線，RD 閘道伺服器必須安裝終端使用者裝置可辨識的憑證。 您可以在概念測試與證明中使用自我簽署憑證，但在任何生產環境中，都只應使用來自憑證授權單位的公開受信任憑證。
2. **在環境中驗證使用者**：RD 閘道使用收件匣 IIS 服務來執行驗證，甚至可以透過 RADIUS 通訊協定來利用[多重要素驗證](rds-plan-mfa.md)解決方案，例如 Azure MFA。 除了使用已建立的預設原則之外，您也可以建立其他 RD 資源授權原則 (RD RAP) 和 RD 連線授權原則 (RD CAP)，更明確地定義哪些使用者可存取安全環境中的哪些資源。
3. **在終端使用者的裝置和指定資源之間來回傳遞流量**：只要連線已建立，RD 閘道即會持續執行此工作。 您可以在 RD 閘道伺服器上指定不同的逾時屬性，以便在使用者離開裝置時保持環境安全。

您可以[在桌面主機參考架構中](desktop-hosting-reference-architecture.md)找到遠端桌面服務部署整體架構的其他詳細資料。