---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: "使用組織網域樹系模式"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 22d871d9157622375619dd90336e597d4bfb3d68
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="using-the-organizational-domain-forest-model"></a>使用組織網域樹系模式

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 [組織網域森林型號，數個獨立群組每個擁有樹系網域。 每個群組控制網域層級的服務管理，讓它們，而樹系擁有者控制森林層級的服務管理獨立管理某些方面的服務管理。  
  
下圖顯示組織的網域森林模型。  
  
![使用組織網域森林模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  
  
## <a name="domain-level-service-autonomy"></a>層級網域服務自主  
組織網域森林型號可讓您的授權單位網域層級的服務管理委派。 下表列出的服務管理可以控制網域層級的類型。  
  
|類型的服務管理|相關聯的工作|  
|------------------------------|--------------------|  
|管理的網域控制站作業|-建立並移除網域控制站<br />監視網域控制站的功能<br />-網域控制站執行的服務管理<br />-備份及還原 directory|  
|設定的網域全設定|-建立網域和使用者網域 account 原則，例如密碼、 Kerberos，以及 account 鎖定原則<br />-建立及套用網域全群組原則|  
|層級資料的管理委派|-建立組織單位 (Ou) 和管理委派<br />修復組織單位擁有者不需要修正存取權的組織單位結構中的問題|  
|外部信任的管理|-建立信任關係的樹系外的網域|  
  
其他類型的服務管理，例如架構或複寫拓撲管理的樹系擁有者的責任。  
  
## <a name="domain-owner"></a>網域擁有者  
在組織網域森林模式下，網域擁有負責網域層級的服務管理工作。 網域擁有透過整個網域，以及存取森林中的所有其他網域擁有授權。 基於這個原因，網域擁有必須信任的樹系擁有者所選取的個人。  
  
網域擁有者網域層級的服務管理委派符合下列條件：  
  
-   參與森林中所有群組都信任的新的網域擁有者和新的網域的服務管理做法。  
  
-   新的網域擁有者信任的樹系擁有者和所有其他網域擁有者。  
  
-   森林中的所有網域擁有都同意服務的系統管理員管理並選擇原則等於或自己更嚴格的做法，有新的網域擁有者。  
  
-   森林中的所有網域擁有都同意網域控制站由新的網域擁有者新的網域中的實體安全。  
  
請注意，如果森林擁有者代理人網域層級的服務管理的網域所有人，其他群組可能未加入該樹系，是否您不信任的網域擁有者。  
  
必須注意，如果上述條件的任何變更未來，它可能會將組織網域多個的樹系部署所需的所有網域擁有者。  
  
> [!NOTE]  
> Windows Server 2008 Active Directory domain 安全性風險降到最低的另一個方法是使用系統管理員角色分離需要唯讀網域控制站 (RODC) 在您的基礎結構 Active Directory 部署。 RODC 是一種全新的網域控制站在 Windows Server 2008 作業系統裝載的 Active Directory 資料庫唯讀磁碟分割。 之前版本的 Windows Server 2008 網域控制站的任何伺服器維護工作必須執行網域系統管理員。 Windows Server 2008，您可以在不網域或其他網域控制站任何系統管理員權限授與使用者委派 RODC 網域中的所有使用者的本機系統管理員權限。 這可讓委派的 RODC 登入並執行維護工作，例如升級的驅動程式，在伺服器上的使用者。 不過，這委派的使用者無法登入其他網域控制站或執行網域中的任何其他管理工作。 如此一來，任何信任使用者可以委派有效的網域中的其餘部分安全性危害管理 RODC 的能力。 如需 Rodc，查看 AD DS: Read-Only 網域控制站 ([https://go.microsoft.com/fwlink/?LinkId=106616](https://go.microsoft.com/fwlink/?LinkId=106616))。  
  


