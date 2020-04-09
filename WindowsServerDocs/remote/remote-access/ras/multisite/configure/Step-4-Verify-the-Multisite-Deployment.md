---
title: 步驟4確認多網站部署
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f17f85104b59052de2e1accc20adb2579ac79f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858351"
---
# <a name="step-4-verify-the-multisite-deployment"></a>步驟4確認多網站部署

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明如何確認您已正確設定遠端存取多網站部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>透過多網站部署確認內部資源的存取權  
  
1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。  
  
2.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。  
  
    您應該能夠存取所有公司資源。  
  
3.  藉由關閉或中斷連接外部網路，而不是其中一部遠端存取服務器，藉此測試多站部署中每部伺服器的連線。 在用戶端電腦上，嘗試存取公司資源。 在不同的多網站伺服器上重複測試。 最多可能需要10分鐘的時間，用戶端電腦才會連接到新的進入點。 這是因為在被認為無法連線的進入點之後，探查會關閉10分鐘，以便將頻寬和電池壽命優化。 或者，您也可以從執行**daprop**時所顯示的下拉式方塊中選擇所需的進入點，以手動方式在各種進入點之間切換。  
  
    您應該能夠透過每個多網站伺服器存取所有公司資源。  
  
4.  將 Windows 7&reg; 用戶端電腦連線到公司網路，並取得群組原則。  
  
5.  將 Windows 7 用戶端電腦連線到外部網路，並嘗試存取內部資源。  
  
    您應該能夠存取所有公司資源。  
  
6.  藉由存取 [Active Directory 使用者和電腦] 主控台，然後將用戶端電腦移至對應至每部伺服器的安全性群組，藉此測試多網站部署中每部伺服器的 Windows 7 用戶端連線能力。 在整個網域中複寫變更之後，請在連線到公司網路時重新開機用戶端電腦，以取得新的群組原則。 嘗試存取公司資源。 在不同的多網站伺服器上重複測試。  
  
    您應該能夠透過每個多網站伺服器存取所有公司資源。  
  
    在生產環境中，此方法可能不可行，因為在整個網域中複寫變更所需的時間量。 您可能想要在可能的情況下強制進行複寫。 測試也可以從多個不同的 Windows 7 用戶端電腦進行，這些電腦已經是多網站部署中不同 Windows 7 安全性群組的成員。  
  


