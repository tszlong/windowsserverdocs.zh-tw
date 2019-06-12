---
title: 設定電子郵件通知
description: 本文說明如何設定電子郵件通知
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 541ceec25e8cb0fae0b55c3de3be269982546c54
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447666"
---
# <a name="configure-e-mail-notifications"></a>設定電子郵件通知

> 適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

建立配額和檔案檢測時，您可以選擇在使用者接近限制配額時或使用者嘗試儲存已被封鎖的檔案之後，傳送電子郵件通知他們。 產生存放裝置報告時，您可以選擇透過電子郵件將報告傳送給特定收件者。 如果您想要例行性向特定系統管理員提供關於配額及檔案檢測事件的通知，或傳送存放裝置報告時，您可以設定一個或多個預設收件者。

若要傳送這些通知和存放裝置報告，您必須指定用於轉送電子郵件的 SMTP 伺服器。

## <a name="to-configure-e-mail-options"></a>若要設定電子郵件選項：

1. 在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]** ，然後按一下 **\[設定選項\]** 。 [檔案伺服器資源管理員選項]  對話方塊隨即開啟。

2. 在 **\[電子郵件通知\]** 索引標籤的 **\[SMTP 伺服器名稱或 IP 位址\]** 下方，輸入要用來轉送電子郵件通知及存放裝置報告的 SMTP 伺服器的主機名稱或 IP 位址。

3. 如果您想要例行性向特定系統管理員提供關於配額及檔案檢測事件的通知，或以電子郵件傳送存放裝置報告時，請在 **\[預設系統管理員收件者\]** 下方輸入每個電子郵件地址。

   使用格式 <em>account@domain</em>。 使用分號隔開多個實作。

4. 若要從檔案伺服器資源管理員，為傳送的電子郵件通知及存放裝置報告指定不同的 \[寄件者\] 地址，請在 **\[預設的 \[寄件者\] 電子郵件地址:\]** 下方輸入您希望在訊息中出現的電子郵件地址。

5. 若要測試您的設定，請按一下 **\[傳送測試電子郵件\]** 。

6. 按一下 [確定]  。


## <a name="see-also"></a>另請參閱

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)