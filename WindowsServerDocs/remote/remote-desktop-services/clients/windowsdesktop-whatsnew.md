---
title: Windows 桌面用戶端的新功能
description: 了解適用於 Windows 桌面的遠端桌面用戶端最新變更
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 11/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: db9c2b64e018b41b053974b5459bd320098a6d2d
ms.sourcegitcommit: 315f015102c42c6fa7694e76adecdfb448390391
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2019
ms.locfileid: "74019587"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Windows 桌面用戶端的新功能

您可以在[開始使用 Windows 桌面用戶端](windowsdesktop.md)找到更多有關 Windows 桌面用戶端的詳細資訊。 您可以在下方找到最新的用戶端更新。

## <a name="latest-client-versions"></a>最新的用戶端版本

用戶端可以針對不同的[使用者群組](windowsdesktop-admin.md#configure-user-groups)進行設定。 下表列出每個使用者群組可用的目前版本：

|使用者群組 |版本  |
|-----------|---------|
|Public     |1.2.431  |
|測試人員    |1.2.431  |

## <a name="updates-for-version-12431"></a>1\.2.431 版的更新

*發行日期：2019/11/12*

- 現已推出 32 位元和 ARM64 版本的用戶端！
- 現在，用戶端會儲存您對連線列所做的任何變更 (例如其位置、大小和釘選狀態)，並將這些變更套用至工作階段。
- 更新的閘道資訊和連線狀態對話方塊。
- 已解決在 Azure Active Directory 權杖過期之後嘗試連線時，同時提示兩個認證的問題。
- 在 Windows 7 上，如果使用者在伺服器不允許其儲存認證時已儲存認證，則系統會正確提示他們認證。
- 重新連線時，Azure Active Directory 提示現在會出現在連線視窗的前方。
- 釘選到工作列的項目現在會在摘要重新整理期間更新。
- 改善使用觸控時在連線中心的捲動功能。
- 從解決方法下拉式功能表中移除空白行。
- 已從 Windows 認證管理員中移除不必要的項目。
- 結束全螢幕時，桌面工作階段會適當調整大小。
- 當您在進入睡眠模式後恢復工作階段時，[RemoteApp 中斷連線] 對話方塊現在會在前景中顯示。
- 已解決協助工具問題，例如鍵盤瀏覽。

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
