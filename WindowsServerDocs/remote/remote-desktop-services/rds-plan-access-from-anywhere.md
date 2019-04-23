---
title: 遠端桌面服務-隨處存取
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
ms.openlocfilehash: 2b10428ff90c50c4d7e113552ddda3cca5447b06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845829"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>遠端桌面服務-隨處存取

>適用於：Windows Server （半年通道），Windows Server 2016

終端使用者可以連線到透過 RD 閘道在公司防火牆外，安全地從內部網路資源。

不論您如何設定桌面使用者，您可以輕鬆地插入 RD 閘道的快速且安全的連線是連接流程。 透過已發行的摘要連接的使用者，您可以設定 RD 閘道屬性設定整體的部署屬性。 透過將他們的桌面，而不需要摘要連接的使用者，他們可以輕鬆加入組織的 RD 閘道的名稱當做連接屬性，不論哪一個遠端桌面用戶端應用程式使用。

RD 閘道，以連接順序，順序的三個主要目的是：
1. **建立使用者的裝置和 RD 閘道伺服器之間加密的 SSL 通道**:若要連接的任何 RD 閘道伺服器，透過 RD 閘道伺服器必須可辨識使用者的裝置已安裝的憑證。 在測試及概念證明，可以使用自我簽署的憑證，但只公開受信任的憑證，從憑證授權單位應該用在任何生產環境中。
2. **在環境中驗證使用者**:RD 閘道會使用 IIS 來執行驗證，服務，並甚至可以使用 RADIUS 通訊協定，利用收件匣[多重要素驗證](rds-plan-mfa.md)解決方案，例如 Azure MFA。 除了建立的預設原則，您可以建立其他 RD 資源授權原則 (RD Rap) 和 RD 連線授權原則 (RD Cap) 更明確地定義哪些使用者應該可以存取哪些資源內的安全環境。
3. **使用者的裝置和指定的資源之間來回傳遞流量**:RD 閘道會繼續執行此工作，只要在建立連線。 您可以指定不同的逾時屬性來維護環境的安全性，以防使用者逐步從該裝置在 RD 閘道伺服器上。

您可以找到遠端桌面服務部署的整體架構的其他詳細資料[桌面主機參考架構中](desktop-hosting-reference-architecture.md)。