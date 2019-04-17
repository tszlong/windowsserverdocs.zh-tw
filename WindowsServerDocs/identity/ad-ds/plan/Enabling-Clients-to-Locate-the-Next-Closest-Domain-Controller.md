---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: "讓用來找出的下一步接近網域控制站戶端"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39b1b79bba944c10b0c74c4bb18f6dcf80f8230e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>讓用來找出的下一步接近網域控制站戶端

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您執行 Windows Server 2008 或 Windows Server 2008 R2 網域控制站，您可以讓 client 的電腦可執行 Windows Vista、Windows 7、Windows Server 2008 或 Windows Server 2008 R2 網域控制站進而更有效率地找出**嘗試下一個接近網站**群組原則設定。 此設定來簡化網路流量，尤其是有許多分公司和網站大型企業来協助改善網域控制站定位器（DC 定位）。  
  
這個新的設定會影響您設定的網站連結費用，因為它會影響網域控制站位於的訂單。 適用於企業的有許多中樞網站和分公司，確保您的戶端無法透過下一步接近中樞網站時接近中樞網站中找不到網域控制站可大幅降低網路上的 Active Directory 傳輸。  
  
一般的最佳做法，您應該簡化您的網站拓撲和網站連結成本盡可能如果您可以**嘗試下一個接近網站**設定。 許多中樞網站的企業，這可以簡化您將處理中的一個台中戶端需要無法到另一個網站的網域控制站的任何計劃。  
  
根據預設，**嘗試下一個接近網站**設定為無法支援。 未設定時，當 DC 定位會使用下列演算法來找出網域控制站：  
  
-   嘗試尋找網域控制站在相同的網站。  
  
-   如果不網域控制站於相同的網站，請嘗試網域中找到的任何網域控制站。  
  
> [!NOTE]  
> 這是 DC 定位用在 Active Directory 舊版相同的演算法。 如需詳細資訊，請查看 [DNS 支援 Active Directory 運作方式 ([https://go.microsoft.com/fwlink/?LinkId=108587](https://go.microsoft.com/fwlink/?LinkId=108587))。  
  
如果您可以**嘗試下一個接近網站**設定，DC 定位使用下列的演算法尋找網域控制站：  
  
-   嘗試尋找網域控制站在相同的網站。  
  
-   如果不網域控制站提供相同的網站，請嘗試尋找網域控制站在下一個最接近的網站。 網站是靠近有成本較高的網站連結比其他網站成本較低的網站連結。  
  
-   如果不網域控制站於下一個最接近的網站，請嘗試網域中找到的任何網域控制站。  
  
根據預設，DC 定位不會將包含唯讀網域控制站 (RODC) 時，它會判斷最接近的下一步網站的任何網站。 此外，因為僅限 Windows Server 2008 和 Windows Server 2008 R2 網域控制站支援下一步接近網站的功能，當 client 取得回應執行較舊版本的 Windows Server 的網域控制站的 DC 定位行為相同時再設定會不支援。  
  
例如，假設網站拓撲四個網站的圖的網站連結值。 在此範例中，所有的網域控制站的寫入網域控制站的 Windows Server 2008 或 Windows Server 2008 R2 執行。  
  
![讓用來尋找俠戶端](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)  
  
當**嘗試下一個接近網站**群組原則設定時在此範例中，如果 client 的電腦可執行 Windows Vista、Windows 7、Windows Server 2008，或 Windows Server 2008 R2 Site_B 在嘗試找出網域控制站，第一次嘗試尋找 Site_B 自己的網域控制站。 如果不提供 Site_B，它會嘗試尋找網域控制站 Site_A 中。  
  
未設定時，如果 client 會嘗試尋找網域控制站 Site_A、Site_C，或 Site_D Site_B 中已不網域控制站。  
  
> [!NOTE]  
> **嘗試下一個接近網站**自動網站涵蓋範圍搭配設定的運作方式。 例如如果的下一步接近網站不網域控制站，DC 定位會嘗試尋找網域控制站自動網站涵蓋範圍執行該網站。  
  
套用**嘗試下一個接近網站**設定，您可以建立群組原則物件 (GPO) 並連結到您的組織的適當物件或修改讓它會影響所有戶端執行 Windows Vista、Windows 7、Windows Server 2008 或 Windows Server 2008 R2 網域中的預設網域原則。 如需詳細資訊，了解如何設定**嘗試下一個接近網站**設定，請[可讓戶端中找不到網域控制站下一步接近網站](https://technet.microsoft.com/library/cc772592.aspx)。  
  


