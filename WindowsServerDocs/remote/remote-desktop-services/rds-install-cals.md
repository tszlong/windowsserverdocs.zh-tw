---
title: 安裝 RDS 用戶端存取使用權
description: 了解如何安裝 RD 用戶端的 Cal。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 2f283b51acc869704a52f09bebc228660cdfbc38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870569"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>在 遠端桌面授權伺服器上安裝 RDS 用戶端存取使用權

>適用於：Windows Server （半年通道），Windows Server 2016

使用下列資訊來安裝遠端桌面服務用戶端存取使用權 (Cal) 授權伺服器上。 一旦安裝 Cal 之後，授權伺服器會發出這些來為適當的使用者。

請注意您必須執行遠端桌面授權管理員，但不是執行的電腦上的 授權伺服器的電腦上的網際網路連線。

1. 在授權伺服器 （通常是第一個 RD 連線代理人） 上，開啟 遠端桌面授權管理員。
2. 授權伺服器，以滑鼠右鍵按一下，然後按一下**安裝授權**。
3. 按一下 [**下一步]** 歡迎頁面上。
4. 選取您用來購買 RDS Cal，計劃，然後按一下**下一步**。 如果您的服務提供者，請選取**服務提供者授權合約**。
5. 輸入您的授權方案的資訊。 在大部分情況下，這將是授權碼或合約號碼，但這會根據您所使用的授權程式而有所不同。
6. 按一下 [下一步] 。
7. 選取的產品版本、 授權類型和您的環境中，授權數目，然後按一下**下一步**。 授權管理員會連絡 Microsoft clearinghouse 要求驗證，並擷取您的授權。
8.  按一下 [完成] 完成訂購程序。