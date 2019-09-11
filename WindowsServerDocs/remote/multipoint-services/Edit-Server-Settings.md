---
title: 編輯伺服器設定
description: 瞭解 MultiPoint 服務設定
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 8a6a9d8e6a76a8fb3c0da59c8fb487d0311f04d7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871715"
---
# <a name="edit-server-settings"></a>編輯伺服器設定
安裝 MultiPoint 服務之後，您會為系統進行設定，包括選擇加入特定計畫。 本主題描述您可以為 MultiPoint 服務系統進行的設定，並說明如何編輯這些設定。  
  
## <a name="about-multipoint-services-settings"></a>關於 MultiPoint 服務設定  
下表描述您可以為 MultiPoint 服務系統變更的不同設定。  
  
|MultiPoint 服務設定|描述|  
|-----------------------------------------------------------------------------------------|---------------|  
|允許一個帳戶有多個工作階段|可讓單一使用者帳戶同時登入多個站台。 在所有學生共用單一帳戶的教室中，此設定會很有用。 使用此設定時，對帳戶資源 (例如文件資料夾或桌面) 所做的任何變更，都可供使用相同帳戶登入的所有使用者使用。|  
|允許遠端管理這部電腦|允許您網路上的其他 MultiPoint 系統管理執行 MultiPoint 服務的電腦。 選取此選項時，如果管理電腦與這部電腦位於相同子網路中，可受管理的伺服器清單中即會顯示這部電腦。 選取此選項時，如果管理電腦與這部電腦位於不同子網路中，管理電腦還是可以管理這部電腦，但您必須指定該電腦的 IP 位址。|
|允許監視這部電腦的桌面|可讓您控制是否可在 MultiPoint 服務系統上監視桌面。 如果此設定為 [關閉（未選取）]，連線到執行 MultiPoint 服務之電腦的工作站（本機和遠端）桌上型電腦，將不會顯示在 MultiPoint 管理員的 [首頁] 索引標籤中（如果電腦正在執行，則會在另一部電腦上包含）從遠端系統管理）。|  
|一律以主控台模式啟動|啟用 RemoteFX 技術，此技術是藉由將處理卸載至 CPU 和 GPU，以提供更快速且更有效率的遠端桌面工作階段。 如果您使用具備 RemoteFX 功能的用戶端連接到 MultiPoint 服務，您可以使用此選項來達到更佳的效能。 其優點取決於您的伺服器和網路能力。 例如，這部分取決於執行額外處理以壓縮資料流所花費的時間，是否比傳輸更少資料所節省的時間還要短。|  
|使用者第一次登入時不要顯示隱私權通知|當使用者第一次登入 MultiPoint 站台時，會顯示一則通知，讓使用者知道其站台活動可能受到監視。|  
|為每個站台指派唯一的 IP|為每個站台指派唯一的 IP 位址。 MultiPoint 服務預設有一個 IP 位址，在系統上執行的所有工作階段會共用該 IP 位址。 不過，此設定可能會導致某些應用程式相容性問題。 例如，如果應用程式需要唯一的 IP 位址，則可能無法在 MultiPoint 服務上正確執行。 選取此選項 (也稱為 IP 虛擬) 可以解決此問題。<br /><br />IP 虛擬也可用來監視 MultiPoint 服務上的使用中工作階段。 某些監視工具會依據 IP 位址報告使用狀況。 若要啟用工作階段監視，您可以使用 IP 虛擬為每個工作階段指派唯一的 IP 位址。 請注意，如果您選取此選項，每個新的工作階段都會收到唯一的 IP 位址。 任何現有的工作階段會繼續使用共用的 IP 位址，直到登出並重新登入為止。|  
|允許在 MultiPoint 儀表板與此電腦上的使用者工作階段之間進行 IM|在 MultiPoint 管理員與此電腦上的使用者工作階段之間啟用聊天功能。 如需詳細資訊，請參閱[使用 IM](Use-IM.md)。|  
|允許系統管理員和 MultiPoint 儀表板使用者工作階段的協調流程|啟用時，可讓系統管理員使用 MultiPoint 儀表板來協調工作階段。 這些工作階段會顯示為縮圖。|  
|允許站台使用 GPU 硬體轉譯|控制站台是否可以使用系統的圖形處理器 (GPU)。|   
  
## <a name="editing-the-computer-settings"></a>編輯電腦設定  
  
1.  以[站模式](Switch-Between-Modes.md)開啟 MultiPoint 管理員，然後按一下 [**首頁**] 索引標籤。  
  
2.  在 [**電腦**] 欄位中，按一下電腦名稱稱，然後在 [*電腦名稱稱*] 工作**底下，按一下**[**編輯電腦設定**]。  
  
3.  選取或清除您要變更的專案，然後按一下 **[確定]** 。  
  
## <a name="see-also"></a>另請參閱  
[使用 MultiPoint 管理員管理系統工作](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
