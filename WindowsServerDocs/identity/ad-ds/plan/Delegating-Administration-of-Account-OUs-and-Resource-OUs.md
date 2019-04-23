---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: 委派帳戶 OU 與資源 OU 的管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70900b44de85774e8e595f9691885d67682fa753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834259"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>委派帳戶 OU 與資源 OU 的管理

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

帳戶組織單位 (Ou) 包含使用者、 群組和電腦物件。 資源 Ou 包含資源，以及負責管理這些資源的帳戶。 樹系擁有者負責建立 OU 結構來管理這些物件和資源，並將該結構的控制項委派給 OU 擁有者。  
  
## <a name="delegating-administration-of-account-ous"></a>委派管理帳戶 Ou  
如果需要建立和修改使用者、 群組和電腦物件，請委派給資料系統管理員的帳戶 OU 結構。 帳戶 OU 結構是 Ou 的樹狀子目錄，必須個別控制每一個帳戶類型。 比方說，OU 擁有者可以委派特定控制項到各種資料的系統管理員帳戶 OU 的使用者、 電腦、 群組和服務帳戶中的子 ou。  
  
下圖顯示的帳戶 OU 結構的其中一個範例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
下表列出並描述您可以建立帳戶的 OU 結構中可能的子 Ou。  
  
|OU|用途|  
|------|-----------|  
|使用者|包含非系統管理人員的使用者帳戶。|  
|服務帳戶|以使用者帳戶執行某些需要存取網路資源的服務。 這個 OU 是從使用者的 OU 中包含的使用者帳戶建立個別的服務使用者帳戶。 此外，將不同類型的使用者帳戶放在不同的 Ou 可讓您根據其特定的系統管理需求來管理它們。|  
|電腦|包含網域控制站以外的電腦帳戶。|  
|群組|包含會分開管理的系統管理群組以外的所有類型的群組。|  
|系統管理員|包含在帳戶 OU 結構中的資料管理員，以允許它們分開管理從一般使用者的使用者和群組帳戶。 啟用稽核此 ou，讓您可以追蹤變更系統管理使用者和群組。|  
  
下圖顯示系統管理群組設計，帳戶 OU 結構的其中一個範例。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
管理子系 Ou 的群組會授與僅透過他們會負責管理的物件的特定類別的完整控制權。  
  
您用來委派內的 OU 結構的控制項群組的類型根據帳戶位於何處相對於要管理的 OU 結構。 如果所有的系統管理員使用者帳戶和 OU 結構存在單一網域內，您建立要用於委派的群組必須是全域群組。 如果您的組織有一個部門中管理它自己的使用者帳戶，且有多個地理區域中，您可能必須一組資料的系統管理員負責管理帳戶中多個網域的 Ou。 如果所有資料管理員的帳戶位於單一網域，而且您有多個您要委派控制的網域中的 OU 結構，讓這些系統管理帳戶群組成員的全域和委派控制權的 OU 結構，在每個這些全域群組的網域。 如果您將委派的 OU 結構的控制項資料的系統管理員帳戶是來自多個網域，您必須使用萬用群組。 萬用群組可以包含來自不同的網域使用者，因此，它們可用來委派控制多個網域中。  
  
## <a name="delegating-administration-of-resource-ous"></a>委派管理的資源 Ou  
資源 Ou 用來管理資源的存取權。 資源 OU 擁有者會建立已加入網域，包括資源，例如檔案共用、 資料庫和印表機的伺服器電腦帳戶。 資源 OU 擁有者也會建立群組來控制這些資源的存取權。  
  
下圖顯示資源 OU 的兩個可能位置。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
資源可以位在網域根目錄下，或是做為子系 OU 在 OU 的系統管理階層中的對應帳戶 OU 的 OU。 資源 Ou 並沒有任何標準的子 Ou。 電腦和群組會直接置於資源 OU。  
  
資源 OU 擁有者擁有的 OU 中的物件，但不是屬於 OU 容器本身。 資源 OU 的擁有者只能管理電腦及群組物件;無法建立物件的 OU 中的其他類別，而且它們無法建立子 Ou。  
  
> [!NOTE]  
> 建立者或物件擁有者對該物件，不論繼承自父容器的權限，能夠設定存取控制清單 (ACL)。 如果資源 OU 的擁有者可以重設的 OU 上的 ACL，該擁有者可以在 [OU]，包括使用者的任何類別的物件。 基於這個理由，不允許資源 OU 的擁有者建立的 Ou。  
  
針對每個資源網域中的 OU 中，建立全域群組來代表資料的系統管理員負責管理 OU 的內容。 此群組具有完整控制權的 OU 中的群組和電腦的物件，但不是在 OU 容器本身。  
  
下圖顯示資源 OU 的系統管理群組設計。  
  
![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
將電腦帳戶放入資源 OU 可讓帳戶物件 OU 擁有者控制但並不 OU 擁有者之電腦的系統管理員。 在 Active Directory 網域，Domain Admins 群組，根據預設，位於所有的電腦上的本機系統管理員群組。 也就是服務系統管理員擁有這些電腦的控制權。 如果資源 OU 的擁有者需要在其 Ou 中的電腦的系統管理控制，樹系擁有者可以套用受限群組 」 群組原則，使資源 OU 擁有者在該 OU 中的電腦上的 Administrators 群組的成員。  
  


