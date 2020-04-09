---
title: 管理用戶端存取使用權
description: 瞭解如何在 MultiPoint 服務中使用 Cal
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2981f22b2b85d90f4102c3a0b67e25901cb12395
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853541"
---
# <a name="manage-client-access-licenses"></a>管理用戶端存取使用權
每個連接到 MultiPoint 服務系統的工作站（包括執行做為工作站的 MultiPoint 服務的電腦）都必須具備有效的每一使用者遠端桌面*用戶端存取許可證（CAL）* 。

如果您使用的是工作站虛擬桌面，而不是實體工作站，則必須為每個工作站虛擬桌面安裝一個 CAL。  
  
1.  為連接到 MultiPoint 服務電腦或伺服器的每個工作站購買用戶端授權。 如需有關購買 Cal 的詳細資訊，請造訪遠端桌面授權的檔。 

2.  從 [**開始**] 畫面中，開啟 [ **MultiPoint 管理員**]。  
  
3.  按一下 [**首頁**] 索引標籤，然後按一下 [**新增用戶端存取授權**]。  這會開啟適用于 CAL 授權的管理工具。

## <a name="set-the-licensing-mode-manually"></a>手動設定授權模式
如果未正確設定，MultiPoint 服務設定會提示您有關寬限期已過期的通知。 請遵循下列步驟來設定授權模式：

1. 啟動**本機群組原則編輯器**（gpedit.msc）。

2. 在左窗格中，流覽至 [**本機電腦原則-> 電腦設定-> 系統管理範本-> Windows 元件-> 遠端桌面服務-> 遠端桌面工作階段主機-> 授權**]。

3. 在右窗格中，以滑鼠右鍵按一下 **[使用指定的遠端桌面授權伺服器**]，然後選取 [**編輯**]：
   - 在 [群組原則編輯器] 對話方塊中，選取 [**已啟用**]
   - 在 [**要使用的授權伺服器**] 欄位中輸入本機電腦名稱稱。
   - 選取 **[確定]**
  
4. 在右窗格中，以滑鼠右鍵按一下 **[設定遠端桌面授權模式]** ，然後選取 [**編輯**]
   - 在 [群組原則編輯器] 對話方塊中，選取 [**已啟用**]
   - 將**授權模式**設定為每個裝置/每位使用者
   - 選取 **[確定]** 

  
## <a name="see-also"></a>另請參閱  
[使用 MultiPoint 管理員管理系統工作](Manage-System-Tasks-Using-MultiPoint-Manager.md)
