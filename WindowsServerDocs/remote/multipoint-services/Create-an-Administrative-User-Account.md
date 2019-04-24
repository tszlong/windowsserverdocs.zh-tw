---
title: 建立系統管理使用者帳戶
description: 建立具有 MultiPoint 服務中的系統管理權限的帳戶
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce4c5a9-3dec-412f-910b-54a252f8f209
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: bf460107e57de5e19f8eaa311e497e9d984680e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879759"
---
# <a name="create-an-administrative-user-account"></a>建立系統管理使用者帳戶
為管理 MultiPoint 服務系統的這些個人建立「系統管理使用者帳戶」。 若要查看誰具有系統管理權限在 MultiPoint 管理員中，按一下**使用者** 索引標籤。系統管理使用者帳戶會在 帳戶類型 欄位中，顯示為「系統管理員」。 *系統管理使用者*有這類變更桌面和系統設定的所有 MultiPoint 管理員工作的存取權：  
  
-   建立帳戶  
  
-   新增和移除程式  
  
-   管理「桌面」和硬體  
  
-   結束其他使用者「工作階段」  
  
系統管理使用者所執行的工作 (例如安裝軟體或變更安全性設定)，可能會影響 MultiPoint 服務系統的所有其他使用者。 因此，系統管理使用者應該擁有只有自己知道的唯一使用者名稱和密碼。  
  
如需身為系統管理使用者的您在建立及管理使用者帳戶時應該考量之問題的詳細資訊，請參閱[使用者帳戶考量](User-Account-Considerations.md)主題。  
  
> [!NOTE]  
> 您可以建立「標準使用者帳戶」，以便在 MultiPoint 服務系統上執行與 MultiPoint 服務系統管理無關的工作時使用。 然後只在必須執行系統管理工作時，才登入您的系統管理使用者帳戶。  
  
#### <a name="to-create-an-administrative-user-account"></a>建立系統管理使用者帳戶  
  
1.  在 MultiPoint 管理員 中，按一下**使用者** 索引標籤。  
  
2.  在 [使用者工作] 下，按一下 [新增使用者帳戶]。 [新增使用者帳戶精靈] 隨即開啟。  
  
3.  在 [使用者帳戶] 欄位中，輸入使用者的登入名稱。 一般而言，登入使用者名稱是將名字和姓氏不含空格一起寫入，或將名字的首字母和姓氏不含空格一起寫入。  
  
4.  在 [全名] 欄位中，以任何您偏好的格式輸入使用者名稱，例如名字、全名或暱稱。  
  
5.  在 [密碼] 欄位中，輸入使用者的密碼。 此密碼應該僅讓您和使用者知道，而且您應該將此資訊儲存在安全的地點。 只有系統管理使用者可以變更密碼。  
  
6.  在 [確認密碼] 欄位中重新輸入密碼，然後按一下 [下一步]。  
  
7.  在存取頁面的層級選取 [系統管理使用者]，然後按一下 [下一步]。  
  
8.  設定帳戶之後，MultiPoint 服務會檢查所有的資訊並顯示一則訊息。 當您看到「成功建立新的使用者帳戶」文字時，按一下 [完成]。  