---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: 使用組織的網域樹系模型
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 876a15dcdd951e0323fb7ddb7be96317f5512f0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875909"
---
# <a name="using-the-organizational-domain-forest-model"></a>使用組織的網域樹系模型

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在組織的網域樹系模型中，數個自發群組每個擁有的網域樹系中。 每個群組控制網域層級的服務管理，讓他們能夠自主管理服務管理的特定層面，而樹系擁有者可控制樹系層級的服務管理。  

下圖顯示組織的網域樹系模型。  

![使用組織網域樹系模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>網域層級服務自治 」

組織的網域樹系模型可讓授權的網域層級服務管理的委派。 下表列出可控制在網域層級的服務管理的類型。  

|類型的服務管理|相關聯的工作|  
|------------------------------|--------------------|  
|管理的網域控制站作業|-建立和移除網域控制站<br />-監視的網域控制站正常運作<br />-管理網域控制站執行的服務<br />-備份與還原目錄|  
|設定全網域設定|的建立網域和網域使用者帳戶的原則，例如密碼、 Kerberos 和帳戶鎖定原則<br />-建立和套用全網域群組原則|  
|資料層級管理委派|-建立組織單位 (Ou) 和委派管理<br />-修復 OU 擁有者沒有足夠的存取權，以修正在 OU 結構的問題|  
|外部信任的管理|-建立信任關係的樹系之外的網域|  

其他類型的服務管理，例如結構描述或複寫拓撲管理、 是樹系擁有者的責任。  

## <a name="domain-owner"></a>網域擁有者

在組織的網域樹系模型中，網域擁有者負責網域層級的服務管理工作。 網域擁有者可以透過整個網域以及樹系中的所有其他網域存取的授權單位。 基於這個理由，網域擁有者必須是受信任樹系擁有者所選取的個人。  

網域層級的服務將管理委派給網域擁有者必須符合下列條件：  

- 所有群組的參樹系中都信任新的網域擁有者，以及新網域的服務管理作法。  

- 新的網域擁有者會信任樹系擁有者和所有其他網域擁有者。  

- 樹系中的所有網域擁有者都同意新的網域擁有者具有服務系統管理員管理和選取原則和作法會等於或自己比更嚴格。  

- 樹系中的所有網域擁有者都同意新的網域中新的網域擁有者管理的網域控制站的實際安全性。  

請注意，是否樹系擁有者委派網域層級的服務管理的網域擁有者，其他群組可能會選擇不加入該樹系，是否不信任該網域的擁有者。  

所有的網域擁有者必須注意，如果這些條件的任何未來變更，就可能必須將組織的網域移至多個樹系部署。  

> [!NOTE]  
> Windows Server 2008 Active Directory 網域的安全性風險降到最低的另一種方式是採用系統管理員角色隔離，需要您的 Active Directory 基礎結構的唯讀網域控制站 (RODC) 部署。 RODC 是新型的網域控制站，在 Windows Server 2008 作業系統，裝載 Active Directory 資料庫的唯讀分割。 之前版本的 Windows Server 2008 中，網域控制站上的任何伺服器維護工作必須由網域系統管理員執行。 在 Windows Server 2008 中，您可以委派給任何網域使用者 rodc 的本機系統管理權限而不需要任何網域或其他網域控制站的系統管理權限授與該使用者。 這可讓委派的使用者登入 RODC，並執行維護工作，例如升級驅動程式，在伺服器上。 不過，此委派的使用者無法登入任何其他網域控制站或網域中執行任何其他系統管理工作。 如此一來，任何受信任的使用者可以是委派實際上不會危及網域的其餘部分的安全性管理 RODC 的能力。 如需 Rodc 的詳細資訊，請參閱[AD DS:唯讀網域控制站](https://go.microsoft.com/fwlink/?LinkId=106616)。  
