---
title: 使用者帳戶考量
description: 提供使用者帳戶、 使用者名稱和密碼考量 MultiPoint 服務
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 00fb5e83921ba0b8ad86a6f75bdfd7bf16419b73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850789"
---
# <a name="user-account-considerations"></a>使用者帳戶考量
本主題描述您身為系統管理使用者，應該考慮在建立及管理使用者帳戶的問題。 您管理 MultiPoint 管理員中 [使用者] 索引標籤中的使用者帳戶。 如需詳細資訊，請參閱[管理使用者帳戶](Manage-User-Accounts.md)主題。  
  
## <a name="user-account-types"></a>使用者帳戶類型  
使用者帳戶是一個資訊集合，這些資訊讓 MultiPoint 服務知道使用者可以存取哪些檔案和資料夾、使用者可以對 MultiPoint 服務系統進行哪些變更，以及每位使用者的喜好設定 (例如桌面背景)。 每個人都可以使用唯一的使用者名稱和密碼，來存取自己的使用者帳戶。 MultiPoint 服務支援三種類型的使用者帳戶：  
  
-   **系統管理使用者帳戶**，適用於個人需要使用 MultiPoint 管理員，來使用及管理 MultiPoint 服務系統的人員。 如需詳細資訊，請參閱[建立系統管理使用者帳戶](Create-an-Administrative-User-Account.md)。  
  
-   「標準使用者帳戶」，適用於會定期存取「站台」，但不會管理系統的個人。 一般而言，您應該為大多數 MultiPoint 服務系統使用者建立標準使用者帳戶。 如需詳細資訊，請參閱[建立標準使用者帳戶](Create-a-Standard-User-Account.md)。  
  
-   「MultiPoint 儀表板使用者帳戶」，適用於使用 MultiPoint 儀表板管理標準使用者工作階段，並可從任何站台登入的個人。 如需詳細資訊，請參閱[建立 MultiPoint 儀表板使用者帳戶](Create-a-MultiPoint-Dashboard-User-Account.md)。  
  
## <a name="user-name-and-password-considerations"></a>使用者名稱和密碼考量  
系統管理使用者所執行的工作 (例如安裝軟體或變更安全性設定)，可能會影響 MultiPoint 服務系統的所有其他使用者。 因此，系統管理使用者應該擁有只有自己知道的唯一使用者名稱和密碼。  
  
使用者帳戶的一個重要考量是，系統會為每個使用者帳戶在 Windows 檔案總管中配置唯一的 [文件] 媒體櫃，其中包含 [我的文件] 資料夾。 如果 MultiPoint 服務系統上的標準使用者會將私人文件儲存在 Windows 檔案總管的 [文件] 媒體櫃中，他們也應該使用只有自己知道的唯一使用者名稱和密碼登入 MultiPoint 服務系統。 如需將文件儲存在 Windows 檔案總管中的詳細資訊，請參閱[管理使用者檔案](Manage-User-Files.md)主題。  
  
> [!TIP]  
> 為了增強系統安全性，所有使用者的密碼都應該是強式密碼。 強式密碼是無法輕易猜測或破解，至少八個字元、 不包含全部或部分使用者的帳戶名稱，且包含以下四種字元類別中至少三： 大寫字元、 小寫字母字元、 數字和符號鍵盤上找到 (例如 ！、 @、 #)。  
  
## <a name="see-also"></a>另請參閱  
[建立系統管理使用者帳戶](Create-an-Administrative-User-Account.md)  
[建立標準使用者帳戶](Create-a-Standard-User-Account.md)  
[管理使用者檔案](Manage-User-Files.md)
[管理使用者帳戶](Manage-User-Accounts.md)