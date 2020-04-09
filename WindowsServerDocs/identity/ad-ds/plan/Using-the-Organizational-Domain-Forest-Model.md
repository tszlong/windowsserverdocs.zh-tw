---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: 使用組織網域樹系模型
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0a56bf51edc9b4f99d3622a8f0e80e5637af696d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821621"
---
# <a name="using-the-organizational-domain-forest-model"></a>使用組織網域樹系模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在組織網域樹系模型中，有數個自發群組各自擁有一個樹系中的網域。 每個群組都會控制網域層級的服務管理，讓他們能夠自主管理服務管理的特定層面，而樹系擁有者則控制樹系層級的服務管理。  

下圖顯示組織網域樹系模型。  

![使用組織網域樹系模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>網域層級服務獨立性

組織網域樹系模型可讓您委派授權以進行網域層級的服務管理。 下表列出可以在網域層級控制的服務管理類型。  

|服務管理類型|相關聯的工作|  
|------------------------------|--------------------|  
|管理網域控制站作業|-建立和移除網域控制站<br />-監視網域控制站的運作<br />-管理在網域控制站上執行的服務<br />-備份和還原目錄|  
|設定全網域設定|-建立網域和網域使用者帳戶原則，例如密碼、Kerberos 和帳戶鎖定原則<br />-建立和套用全網域的群組原則|  
|資料層級管理的委派|-建立組織單位（Ou）和委派管理<br />-修復 ou 結構中 OU 擁有者沒有足夠的存取權來修正的問題|  
|外部信任的管理|-建立與樹系外部網域的信任關係|  

其他類型的服務管理（例如架構或複寫拓撲管理）是樹系擁有者的責任。  

## <a name="domain-owner"></a>網域擁有者

在組織網域樹系模型中，網域擁有者負責網域層級服務管理工作。 網域擁有者具有整個網域的授權，以及樹系中所有其他網域的存取權。 基於這個理由，網域擁有者必須是樹系擁有者所選取的受信任人員。  

如果符合下列條件，請將網域層級服務管理委派給網域擁有者：  

- 參與樹系的所有群組都信任新網域的新網域擁有者和服務管理實務。  

- 新的網域擁有者會信任樹系擁有者和所有其他網域擁有者。  

- 樹系中的所有網域擁有者都同意新的網域擁有者具有等於或高於自己嚴格的服務管理員管理和選取原則和作法。  

- 樹系中的所有網域擁有者都同意新網域中的新網域擁有者所管理的網域控制站，實際上是安全的。  

請注意，如果樹系擁有者將網域層級服務管理委派給網域擁有者，而其他群組不信任該網域擁有者，則可能會選擇不要加入該樹系。  

所有的網域擁有者都必須注意，如果這些情況下有任何一項變更，可能就需要將組織網域移至多個樹系部署中。  

> [!NOTE]  
> 將 Windows Server 2008 Active Directory 網域的安全性風險降至最低的另一種方法是採用系統管理員角色隔離，這需要在您的 Active Directory 基礎結構中部署唯讀網域控制站（RODC）。 RODC 是 Windows Server 2008 作業系統中一種新類型的網域控制站，主控 Active Directory 資料庫的唯讀磁碟分割。 在 Windows Server 2008 發行之前，網域控制站上的任何伺服器維護工作都必須由網域系統管理員執行。 在 Windows Server 2008 中，您可以將 RODC 的本機系統管理許可權委派給任何網域使用者，而不需要授與該使用者任何網域或其他網域控制站的系統管理許可權。 這允許委派的使用者登入 RODC，並在伺服器上執行維護工作，例如升級驅動程式。 不過，此委派的使用者無法登入任何其他網域控制站，或在網域中執行任何其他系統管理工作。 如此一來，任何受信任的使用者都可以獲得有效管理 RODC 的能力，而不會危及網域其餘部分的安全性。 如需 Rodc 的詳細資訊，請參閱[AD DS：唯讀網域控制站](https://go.microsoft.com/fwlink/?LinkId=106616)。  
