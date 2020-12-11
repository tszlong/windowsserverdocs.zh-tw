---
description: 深入瞭解：使用組織網域樹系模型
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: 使用組織網域樹系模型
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: d087df719c3feab6dfe80b1479de62d5245ba380
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048966"
---
# <a name="using-the-organizational-domain-forest-model"></a>使用組織網域樹系模型

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在組織網域樹系模型中，有數個自主群組各自擁有樹系內的網域。 每個群組都控制網域層級的服務管理，可讓他們在樹系擁有者控制樹系層級的服務管理時，自行管理服務管理的特定層面。

下圖顯示組織網域樹系模型。

![使用組織網域樹系模型](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)

## <a name="domain-level-service-autonomy"></a>網域層級服務自主性

組織網域樹系模型可讓您委派網域層級服務管理的授權。 下表列出可在網域層級控制的服務管理類型。

| 服務管理的類型 | 相關聯的工作 |
| -------------------------- |----------------- |
| 管理網域控制站作業    | -建立和移除網域控制站<br />-監視網域控制站的運作<br />-管理在網域控制站上執行的服務<br />-備份和還原目錄 |
| 設定全網域設定         | -建立網域和網域使用者帳戶原則，例如密碼、Kerberos 和帳戶鎖定原則<br />-建立及套用整個網域的群組原則 |
| 資料層級管理的委派       | -建立組織單位 (Ou) 和委派管理<br />-修復 ou 結構中 OU 擁有者沒有足夠的存取權限可修正問題 |
| 外部信任的管理 | -建立與樹系外網域的信任關係 |

其他類型的服務管理（例如架構或複寫拓撲管理）是樹系擁有者的責任。

## <a name="domain-owner"></a>網域擁有者

在組織網域樹系模型中，網域擁有者會負責網域層級的服務管理工作。 網域擁有者擁有整個網域的授權，以及樹系中所有其他網域的存取權。 基於這個理由，網域擁有者必須是樹系擁有者所選取的受信任個人。

如果符合下列條件，請將網域層級服務管理委派給網域擁有者：

- 參與樹系的所有群組都信任新網域擁有者和新網域的服務管理實務。

- 新的網域擁有者會信任樹系擁有者和所有其他網域擁有者。

- 樹系中的所有網域擁有者都同意新的網域擁有者具有等於或更嚴格的服務管理員管理和選取原則和作法。

- 樹系中的所有網域擁有者都同意新網域中的新網域擁有者所管理的網域控制站實際上是安全的。

請注意，如果樹系擁有者將網域層級服務管理委派給網域擁有者，則其他群組可能會選擇不在不信任該網域擁有者的情況下加入該樹系。

所有網域擁有者都必須注意，如果未來有任何條件變更，就可能需要將組織網域移至多個樹系部署中。

> [!NOTE]
> 將 Windows Server 2008 Active Directory 網域的安全性風險降至最低的另一種方式是採用系統管理員角色隔離，而這需要在 Active Directory 基礎結構中將唯讀網域控制站部署 (RODC) 。 RODC 是 Windows Server 2008 作業系統中裝載 Active Directory 資料庫之唯讀分割區的新網域控制站類型。 在 Windows Server 2008 發行之前，網域控制站上的任何伺服器維護工作都必須由網域系統管理員執行。 在 Windows Server 2008 中，您可以將 RODC 的本機系統管理許可權委派給任何網域使用者，而不將網域或其他網域控制站的任何系統管理許可權授與該使用者。 這可讓委派的使用者登入 RODC，並在伺服器上執行維護工作，例如升級驅動程式。 不過，此委派的使用者無法登入任何其他網域控制站，或在網域中執行任何其他系統管理工作。 如此一來，任何信任的使用者都能有效地管理 RODC，而不會危及網域其餘部分的安全性。 如需 Rodc 的詳細資訊，請參閱 [AD DS： Read-Only 網域控制站](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732801(v=ws.10))。
