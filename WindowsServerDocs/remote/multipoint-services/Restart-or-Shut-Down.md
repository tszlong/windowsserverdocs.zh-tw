---
title: 重新啟動或關閉
description: 瞭解如何在 MultiPoint 服務中重新開機或完全關閉系統
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: fc9ce813-6ecb-4422-8f4b-5226386823f3
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d6c09f6b7809bdcae19afbb6babc75887e1b57d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855671"
---
# <a name="restart-or-shut-down"></a>重新啟動或關閉
如果在安裝硬體、軟體和軟體更新之後出現指示，您可能必須重新啟動在 MultiPoint 服務系統中的主機電腦和所有「站台」。 如果站台加入了新的硬體裝置，您可能也會想要將硬體裝置與站台建立關聯。 如需如何「與站台建立關聯」的詳細資訊，請參閱 [Switch Between Modes ](Switch-Between-Modes.md) (在不同模式間切換)主題。  
  
若要安全地關閉 MultiPoint 服務系統的電腦，電腦必須執行關機程式，關閉任何開啟的程式，關閉 Windows，然後關閉您的電腦及其關聯的*工作站*。 請不要只是拔除或按 [電源] 按鈕來關閉電腦。 您應該在工作結束後，或必須在電腦外殼內安裝新硬體時關閉您的電腦。  如果您將其他硬體加入系統，您可能也需要關閉或重新啟動伺服器。  
  
> [!NOTE]  
> 在您重新啟動或關閉正在執行 MultiPoint 服務的電腦之前，必須結束所有的使用者「工作階段」。  
  
## <a name="restart-the-computer"></a>重新啟動電腦。  
  
1.  結束所有的使用者工作階段。 如需結束使用者工作階段的詳細資訊，請參閱[結束使用者工作階段](End-a-User-Session.md)主題。  
  
2.  在 [MultiPoint 管理員] 中，按一下 [**首頁**]，然後按一下 **[重新開機電腦**]。  
  
## <a name="shut-down-the-computer"></a>關閉電腦  
  
1.  結束所有的使用者工作階段。 如需結束使用者工作階段的詳細資訊，請參閱[結束使用者工作階段](End-a-User-Session.md)主題。  
  
2.  在 [MultiPoint 管理員] 中，按一下 [**首頁**] 索引標籤，然後按一下 **[關閉電腦**]。  
  
## <a name="see-also"></a>另請參閱  
[結束使用者工作階段](End-a-User-Session.md)  
[使用 MultiPoint 管理員管理系統工作](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
[在模式之間切換](Switch-Between-Modes.md)  
[登出使用者工作階段或中斷其連線](Log-off-or-Disconnect-User-Sessions.md)