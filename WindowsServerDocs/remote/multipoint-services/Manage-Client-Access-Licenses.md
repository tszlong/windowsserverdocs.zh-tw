---
title: 管理用戶端存取使用權
description: 了解如何使用 MultiPoint 服務中的 Cal
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: f5f78d3d2387d3b95177a6a8a40fb9b16d8ed8e2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446127"
---
# <a name="manage-client-access-licenses"></a>管理用戶端存取使用權
每個站台連接到 MultiPoint 服務系統，包括執行站台，當做使用 MultiPoint 服務的電腦必須具有有效的每個使用者的遠端桌面*用戶端存取使用權 (CAL)* 。

如果您使用站台虛擬桌面，而不實體的站台，您必須安裝適用於每個站台的虛擬桌面 CAL。  
  
1.  購買每個連接到 MultiPoint 服務電腦或伺服器的站台的用戶端的授權。 如需有關如何購買 Cal 的詳細資訊，請瀏覽遠端桌面授權的文件。 <!--@Liza: add link to RDS licensing here-->

2.  從**開始**畫面上，開啟**MultiPoint 管理員**。  
  
3.  按一下 **首頁**索引標籤，然後再按一下**新增用戶端存取使用權**。  這會開啟 [管理] 工具，cal 授權。

# <a name="set-the-licensing-mode-manually"></a>手動設定的授權模式
如果未正確設定 MultiPoint 服務設定會提示要到期的寬限期的相關通知。 請遵循下列步驟來設定的授權模式：

1. 啟動**本機群組原則編輯器**(gpedit.msc)。

2. 在左窗格中，瀏覽至**本機電腦原則-> 電腦設定-> 系統管理範本-> Windows 元件-> 遠端桌面服務-> 遠端桌面工作階段主機-> 授權**。

3. 在右窗格中，以滑鼠右鍵按一下**使用指定的遠端桌面授權伺服器**，然後選取**編輯**:
   - 在 [群組原則編輯器] 對話方塊中，選取**已啟用**
   - 輸入中的本機電腦名稱**授權伺服器使用**欄位。
   - 選取 **[確定]**
  
4. 在右窗格中，以滑鼠右鍵按一下**設定的遠端桌面授權模式**，然後選取**編輯**
   - 在 [群組原則編輯器] 對話方塊中，選取**已啟用**
   - 設定**授權模式**到每個裝置 / 每位使用者
   - 選取 **[確定]** 

  
## <a name="see-also"></a>另請參閱  
[使用 MultiPoint 管理員管理系統工作](Manage-System-Tasks-Using-MultiPoint-Manager.md)
