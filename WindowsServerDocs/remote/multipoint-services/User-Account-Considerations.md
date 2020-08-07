---
title: 使用者帳戶考量
description: 提供 MultiPoint 服務的使用者帳戶、使用者名稱和密碼考慮
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 10e58dcf21c9878ec632eee9160a0d3add4a6958
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951503"
---
# <a name="user-account-considerations"></a>使用者帳戶考量
本主題說明「系統管理使用者」在建立及管理使用者帳戶時應考量的問題。 您可在 MultiPoint 管理員的 [使用者] 索引標籤中管理使用者帳戶。 如需詳細資訊，請參閱[管理使用者帳戶](Manage-User-Accounts.md)主題。

## <a name="user-account-types"></a>使用者帳戶類型
使用者帳戶是一種資訊集合，告訴 MultiPoint 服務使用者可以存取哪些檔案和資料夾、可以對 MultiPoint 服務系統進行哪些變更，以及每個使用者的喜好設定，例如桌面背景。 每個人都可以使用唯一的使用者名稱和密碼，來存取自己的使用者帳戶。 MultiPoint 服務支援三種類型的使用者帳戶：

-   系統**管理使用者帳戶**適用于將使用 multipoint 管理員來使用和管理 multipoint 服務系統的個人。 如需詳細資訊，請參閱[建立系統管理使用者帳戶](Create-an-Administrative-User-Account.md)。

-   「標準使用者帳戶」****，適用於會定期存取「站台」，但不會管理系統的個人。 一般而言，您應該為大多數 MultiPoint 服務系統使用者建立標準使用者帳戶。 如需詳細資訊，請參閱[建立標準使用者帳戶](Create-a-Standard-User-Account.md)。

-   「MultiPoint 儀表板使用者帳戶」****，適用於使用 MultiPoint 儀表板管理標準使用者工作階段，並可從任何站台登入的個人。 如需詳細資訊，請參閱[建立 MultiPoint 儀表板使用者帳戶](Create-a-MultiPoint-Dashboard-User-Account.md)。

## <a name="user-name-and-password-considerations"></a>使用者名稱和密碼考量
系統管理使用者所執行的工作 (例如安裝軟體或變更安全性設定)，可能會影響 MultiPoint 服務系統的所有其他使用者。 因此，系統管理使用者應該擁有只有自己知道的唯一使用者名稱和密碼。

使用者帳戶的一個重要考量是，系統會為每個使用者帳戶在 Windows 檔案總管中配置唯一的 [文件]**** 媒體櫃，其中包含 [我的文件]**** 資料夾。 如果 MultiPoint 服務系統上的標準使用者會將私人文件儲存在 Windows 檔案總管的 [文件]**** 媒體櫃中，他們也應該使用只有自己知道的唯一使用者名稱和密碼登入 MultiPoint 服務系統。 如需將文件儲存在 Windows 檔案總管中的詳細資訊，請參閱[管理使用者檔案](Manage-User-Files.md)主題。

> [!TIP]
> 為了加強系統安全性，所有使用者的密碼都應該是強式密碼。 強式密碼不容易被猜到或破解，長度至少為八個字元，不包含全部或部分的使用者帳戶名稱，且至少包含下列四種字元類別中的三種：大寫字元、小寫字元、數位和在鍵盤上找到的符號 (例如！、@、# ) 。

## <a name="see-also"></a>另請參閱
[建立系統管理使用者帳戶](Create-an-Administrative-User-Account.md) 
[建立標準使用者帳戶](Create-a-Standard-User-Account.md) 
[管理使用者](Manage-User-Files.md) 
 檔案[管理使用者帳戶](Manage-User-Accounts.md)