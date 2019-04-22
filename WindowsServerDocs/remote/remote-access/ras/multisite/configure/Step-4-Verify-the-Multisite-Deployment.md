---
title: 步驟 4 中確認多站台部署
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17b944bd0e2c13f9a3d324eeda09c67b110ce49d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821849"
---
# <a name="step-4-verify-the-multisite-deployment"></a>步驟 4 中確認多站台部署

>適用於：Windows Server （半年通道），Windows Server 2016

本主題描述如何確認您已正確設定遠端存取多站台部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>若要驗證透過多站台部署的內部資源的存取權  
  
1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。  
  
2.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。  
  
    您應該能夠存取所有公司資源。  
  
3.  測試多站台部署中透過每一部伺服器的連線關閉，或從外部網路，但其中一部遠端存取伺服器中斷連線。 在用戶端電腦上嘗試存取公司資源。 重複的測試在不同的多站台伺服器上。 它可能需要 10 分鐘的時間來連接到新的進入點的用戶端電腦。 這是因為探查已關閉做為進入點的 10 分鐘後將其視為無法連上，以獲得最佳的頻寬和電池壽命。 或者，您可以各種進入點以手動方式是藉由從執行時顯示下拉式方塊選擇所需的項目點之間切換**daprop.exe**。  
  
    您應該能夠存取所有公司資源，透過每個多站台伺服器。  
  
4.  連接 Windows 7&reg;用戶端電腦到公司網路，並取得群組原則。  
  
5.  將 Windows 7 用戶端電腦連線到外部網路，並嘗試存取內部資源。  
  
    您應該能夠存取所有公司資源。  
  
6.  測試 Windows 7 用戶端透過多站台部署中每個伺服器的連線，存取 Active Directory 使用者和電腦 主控台中，並將用戶端電腦移到對應至每一部伺服器的安全性群組。 變更已複寫到整個網域之後，重新啟動用戶端電腦連線到公司網路，以便取得新的群組原則時。 嘗試存取公司資源。 重複的測試在不同的多站台伺服器上。  
  
    您應該能夠存取所有公司資源，透過每個多站台伺服器。  
  
    在生產環境中這個方法可能不可行，因為變更複寫至整個網域所需的時間量。 若要盡可能強制複寫。 也可以測試從多個不同的 Windows 7 用戶端電腦已在多站台部署中的不同 Windows 7 安全性群組的成員。  
  


