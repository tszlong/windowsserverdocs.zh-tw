---
description: 深入瞭解：委派帳戶 Ou 和資源 Ou 的管理
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: 委派帳戶 OU 與資源 OU 的管理
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 432f68a113055f9e68ca4c18ed8aa54b02cad8b0
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711703"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>委派帳戶 OU 與資源 OU 的管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

帳戶組織單位 (Ou) 包含使用者、群組和電腦物件。 資源 Ou 包含資源和負責管理這些資源的帳戶。 樹系擁有者負責建立 OU 結構來管理這些物件和資源，以及將該結構的控制權委派給 OU 擁有者。

## <a name="delegating-administration-of-account-ous"></a>委派帳戶 Ou 的管理
如果需要建立和修改使用者、群組和電腦物件，請將帳戶 OU 結構委派給資料管理員。 帳戶 OU 結構是每個必須個別控制之帳戶類型的 Ou 子樹。 例如，OU 擁有者可以在使用者、電腦、群組和服務帳戶的帳戶 OU 中，將特定的控制項委派給不同的資料管理員。

下圖顯示帳戶 OU 結構的其中一個範例。

![此圖顯示一個帳戶 OU 結構範例。](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)

下表列出並描述您可以在帳戶 OU 結構中建立的可能子 Ou。

|OU|目的|
|------|-----------|
|使用者|包含非管理員人員的使用者帳戶。|
|服務帳戶|某些需要存取網路資源的服務會以使用者帳戶的形式執行。 建立此 OU 是為了將服務使用者帳戶與使用者 OU 中包含的使用者帳戶分開。 此外，將不同類型的使用者帳戶放在個別的 Ou 中，可讓您根據特定的系統管理需求來管理它們。|
|電腦|包含網域控制站以外之電腦的帳戶。|
|群組|包含所有類型的群組（系統管理群組除外），這些群組會分開管理。|
|管理員|包含帳戶 OU 結構中資料管理員的使用者和群組帳戶，可讓使用者與一般使用者分開管理。 啟用此 OU 的 [審核]，讓您可以追蹤系統管理使用者和群組的變更。|

下圖顯示一個帳戶 OU 結構的系統管理群組設計範例。

![此圖顯示一個帳戶 OU 結構的系統管理群組設計範例。](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)

管理子 Ou 的群組會被授與負責管理的特定物件類別的完全控制。

您用來委派 OU 結構內之控制項的群組類型，是根據帳戶相對於要管理的 OU 結構的位置而定。 如果系統管理員使用者帳戶和 OU 結構全都存在於單一網域中，則您建立用來委派的群組必須是全域群組。 如果您的組織所擁有的部門負責管理自己的使用者帳戶，而且存在於多個地理區域中，您可能會有一組負責管理一個以上網域中帳戶 Ou 的資料管理員。 如果資料管理員的帳戶全部都存在於單一網域中，而您在多個網域中有 OU 結構，而您需要委派控制項，請將這些系統管理帳戶成員設為全域群組，並將每個網域中 OU 結構的控制權委派給這些全域群組。 如果您委派 OU 結構控制權的資料管理員帳戶是來自多個網域，則必須使用萬用群組。 萬用群組可以包含來自不同網域的使用者，因此可以用來將控制項委派給多個網域。

## <a name="delegating-administration-of-resource-ous"></a>委派資源 Ou 的管理
資源 Ou 可用來管理對資源的存取。 資源 OU 擁有者會為加入網域的伺服器建立電腦帳戶，包括檔案共用、資料庫和印表機等資源。 資源 OU 擁有者也會建立群組來控制這些資源的存取權。

下圖顯示資源 OU 的兩個可能位置。

![圖例顯示資源 OU 的兩個可能位置。](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)

資源 OU 可以位於網域根目錄下，或是 OU 系統管理階層中對應之帳戶 OU 的子 OU。 資源 Ou 沒有任何標準子 Ou。 電腦和群組會直接放在資源 OU 中。

資源 OU 擁有者會擁有 OU 內的物件，但不會擁有 OU 容器本身。 資源 OU 擁有者只管理電腦和群組物件;他們無法在 OU 內建立其他類別的物件，也無法建立子 Ou。

> [!NOTE]
> 物件的建立者或擁有者可以在物件上設定 (ACL) 的存取控制清單，而不論繼承自父容器的許可權為何。 如果資源 OU 擁有者可以重設 OU 上的 ACL，該擁有者可以在 OU （包括使用者）中建立任何類別的物件。 基於這個理由，不允許資源 OU 擁有者建立 Ou。

針對網域中的每個資源 OU 建立全域群組，以代表負責管理 OU 內容的資料管理員。 此群組具有 OU 中群組和電腦物件的完整控制權，而非 OU 容器本身的控制權。

下圖顯示資源 OU 的系統管理組設計。

![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)

將電腦帳戶放入資源 OU 中，可讓 OU 擁有者控制帳戶物件，但不會讓 OU 擁有者成為電腦的系統管理員。 在 Active Directory 網域中，Domain Admins 群組預設是放在所有電腦上的本機 Administrators 群組中。 也就是說，服務系統管理員可以控制這些電腦。 如果資源 OU 擁有者需要系統管理 Ou 中電腦的系統管理控制權，樹系擁有者可以將受限制的群組套用群組原則，讓資源 OU 擁有者成為該 OU 中電腦上的 Administrators 群組成員。



