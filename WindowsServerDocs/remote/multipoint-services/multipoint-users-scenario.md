---
title: MultiPoint 服務使用者帳戶
description: 瞭解 MultiPoint 服務中的使用者帳戶，特別是用於不同案例的類型
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: ada21ba798dc248d7c488059b52b94e7460f2830
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970475"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>範例案例：MultiPoint 服務使用者帳戶
您需要執行什麼動作，才能在您為 MultiPoint 服務環境選擇的使用者帳戶案例中執行？ 下表說明在獨立 MultiPoint 電腦或工作組或 Active Directory 網域中的網路伺服器上，設定使用者帳戶和準備工作站以進行共用或個別使用者帳戶時，所要執行的每項工作。 選擇適用于您環境的案例。 然後遵循表格中的連結來完成每個必要的設定工作。

> [!NOTE]
> 如果您尚未決定如何設定您的使用者帳戶，請參閱為您的[MultiPoint 服務環境規劃使用者帳戶](Plan-user-accounts-for-your-MultiPoint-services-environment.md)，以取得每個選項如何影響使用者的詳細資訊。

## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>獨立環境中的單一 MultiPoint 服務電腦 (沒有網路) 

|||
|-|-|
|**我的使用者不需要登入。** 所有使用者都可以使用這些工作站。 他們不需要個別的 Windows 桌面體驗，包括用來儲存資料或個人化桌面的私人資料夾。|1. 建立單一本機使用者帳戶 (如需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <br />2.[允許一個帳戶有多個會話](Allow-one-account-to-have-multiple-sessions.md)<br />3.[設定工作站以自動登](Configure-stations-for-automatic-logon.md)入|
|**我的使用者可以共用相同的使用者登入。** 他們不需要個別的 Windows 桌面體驗，包括用來儲存資料或個人化桌面的私人資料夾。|1. 建立單一本機使用者帳戶 (如需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <br />2.[允許一個帳戶有多個會話](Allow-one-account-to-have-multiple-sessions.md)|
|**我的使用者必須有自己的個別 Windows 桌面體驗。**|為每個使用者 (建立本機使用者帳戶，以取得相關指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) |

## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>網路上的多個 MultiPoint 服務電腦，但不含網域

|||
|-|-|
|**我的使用者不需要登入。** 所有使用者都可以使用這些工作站。 他們不需要個別的 Windows 桌面體驗，包括用來儲存資料或個人化桌面的私人資料夾。|1. 在每部伺服器上建立單一本機使用者帳戶。  (需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <br />2.[允許一個帳戶在每部伺服器上有多個會話](Allow-one-account-to-have-multiple-sessions.md)<br />3. 在每部伺服器上[設定工作站以自動登](Configure-stations-for-automatic-logon.md)入|
|**我的使用者可以共用相同的使用者登入。** 他們不需要個別的 Windows 桌面體驗，包括用來儲存資料或個人化桌面的私人資料夾。|1. 在每部伺服器上建立單一本機使用者帳戶。  (需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <br />2.[允許一個帳戶在每部伺服器上有多個會話](Allow-one-account-to-have-multiple-sessions.md)。|
|**我的使用者必須有自己的個別 Windows 桌面體驗。**<p>-   **選項 A** -[我的使用者] 一律會使用連接到相同 MultiPoint 服務電腦的本機工作站。<br />-   **選項 B** -我的使用者將在多個 MultiPoint 服務電腦上使用本機工作站。<br />-   **選項 C** -我的使用者會使用 LAN 上的遠端用戶端。|-   **選項 A** -在每部伺服器上，為該伺服器的使用者建立單一本機使用者帳戶。  (需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <br />-   **選項 B** -針對每部伺服器上的每個使用者建立本機使用者帳戶。 **注意：** 這表示每個使用者在每部伺服器上都有一個設定檔。 換句話說，如果使用者在登入伺服器 A 的工作站時，將檔案儲存在 [我的文件] 中，在登入伺服器 B 的工作站時，將不會看到該檔案。  (需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <br />-   **選項 C** -將每個使用者指派給特定的 MultiPoint 服務電腦。 針對每部伺服器上指派的使用者建立本機使用者帳戶。  (需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) |

## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>網域網路環境中的一或多個 MultiPoint 服務電腦

|||
|-|-|
|**我的使用者不需要登入。** 所有使用者都可以使用這些工作站。 他們不需要個別的 Windows 桌面體驗，包括用來儲存資料或個人化桌面的私人資料夾。|1. 建立網域帳戶以登入伺服器。<br />2.[允許一個帳戶在每部伺服器上有多個會話](Allow-one-account-to-have-multiple-sessions.md)。<br />3.[將工作站設定為每部伺服器上的自動登](Configure-stations-for-automatic-logon.md)入。|
|**我的使用者可以共用相同的使用者登入。** 他們不需要個別的 Windows 桌面體驗，包括用來儲存資料或個人化桌面的私人資料夾。|1. 為群組或每位使用者建立網域帳戶。<br />2.[允許一個帳戶在每部伺服器上有多個會話](Allow-one-account-to-have-multiple-sessions.md)。|
|**我的使用者必須有自己的個別 Windows 桌面體驗。**<p>-   **選項 A** -具有網域帳戶的任何使用者都可以使用 MultiPoint 服務電腦。<br />-   **選項 B** -我想要限制哪些網域帳戶可以存取伺服器。|-   **選項 A** -不需要設定。 根據預設，所有網域使用者都可以存取網路上的任何 MultiPoint 服務電腦。<br />-   **選項 B** -限制網域使用者帳戶對 MultiPoint 服務電腦的存取權。 如需指示，請參閱[限制使用者對伺服器的存取權](limit-users--access-to-the-server-in-multipoint-services.md)。|
|**我想要使用本機使用者帳戶，並分別從我的網域帳戶進行管理。** 例如，您想要讓其他人管理 MultiPoint 服務，而不是網域，或您不想要將網域帳戶授與所有 MultiPoint 服務使用者。|在每部伺服器上建立一或多個本機使用者帳戶。  (需指示，請參閱[建立本機使用者帳戶](Create-local-user-accounts.md)。 ) <p>**注意：** 這表示每個使用者帳戶在每部伺服器上都有一個設定檔。 換句話說，如果使用者在登入伺服器 A 的工作站時，將檔案儲存在 [我的文件] 中，在登入伺服器 B 的工作站時，將不會看到該檔案。|