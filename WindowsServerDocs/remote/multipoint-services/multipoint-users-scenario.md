---
title: MultiPoint 服務使用者帳戶
description: 深入了解在 MultiPoint 服務中，特別是哪種不同的案例中使用的使用者帳戶
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 31279f81d5af597b0b1f1729c953fefaf24a214f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855359"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>示範案例：MultiPoint 服務使用者帳戶
您要實作您為您的 MultiPoint 服務環境的使用者帳戶案例？ 下表描述每個工作以設定使用者帳戶，及準備站台獨立的 MultiPoint 電腦或工作群組或 Active Directory 網域中的網路伺服器上的共用或個別使用者帳戶執行。 選擇適用於您環境的案例。 然後遵循以完成每個必要的組態工作表中的連結。  
  
> [!NOTE]  
> 如果您尚未決定如何設定您的使用者帳戶，請參閱[規劃您的 MultiPoint 服務環境的使用者帳戶](Plan-user-accounts-for-your-MultiPoint-services-environment.md)如需有關每個選擇如何影響使用者。  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>在獨立環境中 （沒有網路） 的單一 MultiPoint 服務電腦  
  
|||  
|-|-|  
|**我的使用者不需要登入。** 站台都能使用走到它們的任何人。 它們不需要個別的 Windows 桌面體驗，包括私人資料夾來儲存資料或個人化的桌面。|1.建立單一的本機使用者帳戶 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br />2.[允許一個帳戶有多個工作階段](Allow-one-account-to-have-multiple-sessions.md)<br />3.[設定自動登入的站台](Configure-stations-for-automatic-logon.md)|  
|**我的使用者可以共用相同的使用者登入。** 它們不需要個別的 Windows 桌面體驗，包括私人資料夾來儲存資料或個人化的桌面。|1.建立單一的本機使用者帳戶 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br />2.[允許一個帳戶有多個工作階段](Allow-one-account-to-have-multiple-sessions.md)|  
|**我的使用者必須有自己個別的 Windows 桌面的體驗。**|建立每個使用者的本機使用者帳戶 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>多個 MultiPoint 服務電腦在網路上，但沒有網域  
  
|||  
|-|-|  
|**我的使用者不需要登入。** 站台都能使用走到它們的任何人。 它們不需要個別的 Windows 桌面體驗，包括私人資料夾來儲存資料或個人化的桌面。|1.在每一部伺服器上建立單一的本機使用者帳戶。 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br />2.[允許一個帳戶有多個工作階段](Allow-one-account-to-have-multiple-sessions.md)每部伺服器上<br />3.[設定自動登入的站台](Configure-stations-for-automatic-logon.md)每部伺服器上|  
|**我的使用者可以共用相同的使用者登入。** 它們不需要個別的 Windows 桌面體驗，包括私人資料夾來儲存資料或個人化的桌面。|1.在每一部伺服器上建立單一的本機使用者帳戶。 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br />2.[允許一個帳戶有多個工作階段](Allow-one-account-to-have-multiple-sessions.md)每部伺服器上。|  
|**我的使用者必須有自己個別的 Windows 桌面的體驗。**<br /><br />-   **選項 A** -我的使用者一律會使用連接到相同的 MultiPoint 服務電腦的本機站台。<br />-   **選項 B** -我的使用者會使用多個 MultiPoint 服務電腦上的本機站台。<br />-   **選項 C** -我的使用者會使用 LAN 上的遠端用戶端。|-   **選項 A** -使用者該伺服器的每一部伺服器上建立單一的本機使用者帳戶。 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br />-   **選項 B** -每一部伺服器上建立的每位使用者的本機使用者帳戶。 **注意：** 這表示每個使用者會有在每一部伺服器上的設定檔。 換句話說，如果他們將檔案儲存在我的文件時登入伺服器 A 的站台時，他們必須看到檔案時登入伺服器 B 的站台。 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br />-   **選項 C** -將每個使用者指派給特定的 MultiPoint 服務電腦。 每部伺服器上建立本機使用者帳戶以進行指派的使用者。 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>網域網路環境中的一或多個 MultiPoint 服務電腦  
  
|||  
|-|-|  
|**我的使用者不需要登入。** 站台都能使用走到它們的任何人。 它們不需要個別的 Windows 桌面體驗，包括私人資料夾來儲存資料或個人化的桌面。|1.建立網域帳戶來登入伺服器。<br />2.[允許一個帳戶有多個工作階段](Allow-one-account-to-have-multiple-sessions.md)每部伺服器上。<br />3.[設定自動登入的站台](Configure-stations-for-automatic-logon.md)每部伺服器上。|  
|**我的使用者可以共用相同的使用者登入。** 它們不需要個別的 Windows 桌面體驗，包括私人資料夾來儲存資料或個人化的桌面。|1.建立群組，或為每個使用者的網域帳戶。<br />2.[允許一個帳戶有多個工作階段](Allow-one-account-to-have-multiple-sessions.md)每部伺服器上。|  
|**我的使用者必須有自己個別的 Windows 桌面的體驗。**<br /><br />-   **選項 A** -搭配網域帳戶的任何使用者可以使用 MultiPoint 服務電腦。<br />-   **選項 B** -我想要限制哪些網域帳戶可以存取伺服器。|-   **選項 A** -不需要設定為必要。 根據預設，所有網域使用者會在網路上都有任何 MultiPoint 服務電腦的存取權。<br />-   **選項 B** -限制到 MultiPoint 服務電腦的網域使用者帳戶的存取權。 如需相關指示，請參閱 <<c0> [ 限制使用者存取伺服器](limit-users--access-to-the-server-in-multipoint-services.md)。|  
|**我想要使用本機使用者帳戶，並從我的網域帳戶分開管理。** 例如，您想要讓人管理 MultiPoint 服務但不是網域或不想讓 MultiPoint 服務的所有使用者的網域帳戶。|在每一部伺服器上建立一或多個本機使用者帳戶。 (如需相關指示，請參閱 <<c0> [ 建立本機使用者帳戶](Create-local-user-accounts.md)。)<br /><br />**注意：** 這表示每個使用者帳戶會有在每一部伺服器上的設定檔。 換句話說，如果他們將檔案儲存在我的文件時登入伺服器 A 的站台時，他們必須看到檔案時登入伺服器 B 的站台。|  