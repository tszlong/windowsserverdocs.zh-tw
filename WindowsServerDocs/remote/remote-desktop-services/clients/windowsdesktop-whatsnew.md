---
title: Windows 桌面用戶端的新功能
description: 了解適用於 Windows 桌面的遠端桌面用戶端最新變更
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: 587ad44509451497b253689238c3aef233a37153
ms.sourcegitcommit: e210fce039452a9855353520c7f8698acd76ffce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71071287"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Windows 桌面用戶端的新功能

您可以在[開始使用 Windows 桌面用戶端](windowsdesktop.md)找到更多有關 Windows 桌面用戶端的詳細資訊。 您可以在下方找到最新的用戶端更新。

## <a name="latest-client-versions"></a>最新的用戶端版本

用戶端可以針對不同的[使用者群組](windowsdesktop-admin.md#configure-user-groups)進行設定。 下表列出每個使用者群組可用的目前版本：

|使用者群組 |版本  |
|-----------|---------|
|Public     |1.2.246  |
|測試人員    |1.2.247  |

## <a name="updates-for-version-12247"></a>1\.2.247 版的更新

*發行日期：09/17/2019*

- 已修正在連線期間驗證時所發生的損毀。
- 已修正關閉用戶端時所發生的損毀。

## <a name="updates-for-version-12246"></a>1\.2.246 版的更新

*發行日期：08/28/2019*

- 已改善當地語系化版本的遞補語言。 (例如，FR-CA 會正確地以法文顯示，而不是英文。)
- 移除訂用帳戶時，用戶端現在會正確地從認證管理員移除已儲存的認證。
- 用戶端更新程序現已在啟動後自動執行，且用戶端會在完成後重新啟動。
- 用戶端現在可以在 Windows 10 上以 S 模式使用。
- 已修正導致使用者名稱中有空格的使用者更新程序失敗的問題。
