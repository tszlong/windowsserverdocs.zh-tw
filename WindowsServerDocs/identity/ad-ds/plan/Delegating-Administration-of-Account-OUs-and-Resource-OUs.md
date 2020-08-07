---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: 委派帳戶 OU 與資源 OU 的管理
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a73089ce50b90689460f347c2d4f5587ac11c3cb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947770"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>委派帳戶 OU 與資源 OU 的管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

帳戶組織單位 (Ou) 包含使用者、群組和電腦物件。 資源 Ou 包含資源，以及負責管理這些資源的帳戶。 樹系擁有者負責建立 OU 結構來管理這些物件和資源，以及將該結構的控制權委派給 OU 擁有者。

## <a name="delegating-administration-of-account-ous"></a>委派帳戶 Ou 的管理
如果需要建立及修改使用者、群組和電腦物件，請將帳戶 OU 結構委派給資料管理員。 帳戶 OU 結構是每個必須獨立控制之帳戶類型的 Ou 子樹。 例如，OU 擁有者可以將特定控制項委派給使用者、電腦、群組和服務帳戶之帳戶 OU 中的子 Ou 的各種資料管理員。

下圖顯示帳戶 OU 結構的其中一個範例。

![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)

下表列出並描述您可以在帳戶 OU 結構中建立的可能子 Ou。

|OU|目的|
|------|-----------|
|使用者|包含非管理員人員的使用者帳戶。|
|服務帳戶|某些需要存取網路資源的服務會以使用者帳戶的身分執行。 建立此 OU 是為了與使用者 OU 中包含的使用者帳戶分開服務使用者帳戶。 此外，將不同類型的使用者帳戶放在不同的 Ou 中，可讓您根據特定的系統管理需求來管理它們。|
|電腦|包含網域控制站以外電腦的帳戶。|
|群組|包含所有類型的群組，除了系統管理群組之外，也會分開管理。|
|管理員|包含帳戶 OU 結構中資料管理員的使用者和群組帳戶，讓他們能夠與一般使用者分開管理。 啟用此 OU 的審核，讓您可以追蹤系統管理使用者和群組的變更。|

下圖顯示帳戶 OU 結構之系統管理群組設計的其中一個範例。

![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)

管理子 Ou 的群組只會授與他們負責管理之特定物件類別的完全控制。

您在 OU 結構中用來委派控制項的群組類型，是根據帳戶相對於所要管理之 OU 結構的所在位置。 如果系統管理使用者帳戶和 OU 結構全都存在於單一網域中，則您建立用來委派的群組必須是全域群組。 如果您的組織有一個部門負責管理自己的使用者帳戶，而且存在於多個地理區域中，您可能會有一組負責管理多個網域中的帳戶 Ou 的資料管理員。 如果資料管理員的帳戶全部都存在於單一網域中，而且您在多個網域中有您需要委派控制項的 OU 結構，請將這些系統管理帳戶的成員設為全域群組，並將每個網域中的 OU 結構控制權委派給這些全域群組。 如果您委派 OU 結構控制權的資料管理員帳戶來自多個網域，您就必須使用萬用群組。 萬用群組可以包含來自不同網域的使用者，因此可以用來將控制項委派給多個網域。

## <a name="delegating-administration-of-resource-ous"></a>委派資源 Ou 的管理
資源 Ou 是用來管理資源的存取權。 資源 OU 擁有者會為已加入網域的伺服器建立電腦帳戶，這些伺服器包含檔案共用、資料庫和印表機等資源。 資源 OU 擁有者也會建立群組，以控制對這些資源的存取。

下圖顯示資源 OU 的兩個可能位置。

![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)

資源 OU 可以位於網域根目錄底下，或作為 OU 系統管理階層中對應帳戶 OU 的子 OU。 資源 Ou 沒有任何標準子 Ou。 電腦和群組會直接放在資源 OU 中。

資源 OU 擁有者擁有 OU 中的物件，但不擁有 OU 容器本身。 資源 OU 擁有者只會管理電腦和群組物件;他們無法在 OU 中建立其他類別的物件，也無法建立子 Ou。

> [!NOTE]
> 物件的建立者或擁有者可以在物件上設定存取控制清單 (ACL) ，而不論繼承自父容器的許可權為何。 如果資源 OU 擁有者可以重設 OU 上的 ACL，該擁有者就可以在 OU 中建立任何物件類別，包括使用者。 基於這個理由，不允許資源 OU 擁有者建立 Ou。

針對網域中的每個資源 OU，建立全域群組來代表負責管理 OU 內容的資料管理員。 此群組可完全控制 OU 中的群組和電腦物件，而不是透過 OU 容器本身。

下圖顯示資源 OU 的系統管理群組設計。

![委派管理](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)

將電腦帳戶放入資源 OU，可讓 OU 擁有者控制帳戶物件，但不會將 OU 擁有者設為電腦的系統管理員。 在 Active Directory 網域中，Domain Admins 群組預設會放在所有電腦的本機系統管理員群組中。 也就是說，服務系統管理員可以控制這些電腦。 如果資源 OU 擁有者需要對其 Ou 中的電腦進行系統管理控制，樹系擁有者可以套用受限制的群組群組原則，讓資源 OU 擁有者成為該 OU 中電腦的 Administrators 群組成員。



