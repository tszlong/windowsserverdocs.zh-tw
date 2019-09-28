---
title: 安裝 RDS 用戶端存取使用權
description: 了解如何為 RD 用戶端安裝 CAL。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 4ee26a0d9ba5ee3a94a4c569a639454e064d0a90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387419"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>在遠端桌面授權伺服器上安裝 RDS 用戶端存取使用權

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

請使用下列資訊，將遠端桌面服務用戶端存取使用權 (CAL) 安裝在授權伺服器上。 安裝 CAL 之後，授權伺服器即會適時地將其核發給使用者。

請注意，您執行遠端桌面授權管理員的電腦 (而不是執行授權伺服器的電腦) 上必須要有網際網路連線。

1. 在授權伺服器 (通常是第一個 RD 連線代理人) 上，開啟 [遠端桌面授權管理員]。
2. 以滑鼠右鍵按一下授權伺服器，然後按一下 [安裝授權]  。
3. 在 [歡迎頁面] 上按 [下一步]  。
4. 選取您用來購買 RDS CAL 的方案，然後按 [下一步]  。 如果您是服務提供者，請選取 [服務提供者授權合約]  。
5. 輸入您的授權方案資訊。 在大部分情況下，這項資訊會是授權碼或合約號碼，但會隨著您所使用的授權方案而不同。
6. 按 [下一步]  。
7. 選取您的環境的產品版本、授權類型和授權數目，然後按 [下一步]  。 授權管理員會連絡 Microsoft Clearinghouse，以驗證及擷取您的授權。
8.  按一下 [完成]  完成訂購程序。