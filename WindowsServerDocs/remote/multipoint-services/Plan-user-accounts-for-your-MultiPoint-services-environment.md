---
title: 為您的 MultiPoint 服務環境規劃使用者帳戶
description: 規劃 MultiPoint 服務中的使用者帳戶的資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 02862c1a317dfe5deff75be4a80595c8dc8bc3f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864169"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>為您的 MultiPoint 服務環境規劃使用者帳戶
MultiPoint 服務中實作的使用者帳戶的最佳方式取決於大小和您的部署複雜度：  
  
-   **本機使用者帳戶**-執行 MultiPoind 服務只有少數電腦的小型部署，並幾位使用者，您可能會覺得最方便的方式使用*本機使用者帳戶*MultiPoint 服務上建立。 您可以建立個別帳戶，對每一個人會使用此系統，或建立一般的每個站台的任何人都可以用來登入帳戶。 MultiPoint 服務系統管理員建立，並使用 MultiPoint 管理員管理本機使用者帳戶。 本機帳戶可以是系統管理員、 具有有限的系統管理權限，或者是一般使用者與 MultiPoint 服務的桌面或 MultiPoint 管理員沒有存取權。  
  
-   **網域帳戶**-如果您的環境具有多部執行 MultiPoint 服務和許多使用者的電腦，您可能會發現它更有用來設定 Active Directory 網域服務\(AD DS\)網域和使用*網域使用者帳戶*，可讓使用者從網域中任何站台存取自己的使用者設定檔和設定。 網域系統管理員，必須在網域控制站上建立網域使用者帳戶。  
  
> [!NOTE]  
> 下列各節會討論您可能會實作在 MultiPoint 服務中的本機使用者帳戶的案例。 如果您使用網域使用者帳戶，請參閱中的 「 一或多個 MultiPoint 伺服器在網域網路環境中 「 案例[範例案例：MultiPoint 服務使用者帳戶](Example-scenarios--MultiPoint-Services-user-accounts.md)。  
  
## <a name="planning-local-user-accounts"></a>規劃本機使用者帳戶  
優點、 缺點和數種方式可在 Windows MultiPoint 服務環境中實作個別或共用的本機使用者帳戶的需求，請考慮下列各節。  
  
### <a name="use-individual-local-user-accounts"></a>使用個別的本機使用者帳戶  
建立本機使用者帳戶時，您會有選項的兩種方法。  將每個使用者指派到特定伺服器執行 MultiPoint 服務，並建立每個使用者的單一帳戶。 或在執行 Multipoint 服務每一部電腦上建立所有使用者的本機使用者帳戶。 實作個別使用者帳戶的主要優點是每個使用者都他或她自己 Windows 桌面體驗，包括私人資料夾來儲存資料。 
  
從 系統管理的觀點而言，將使用者指派給特定的 MultiPoint 服務電腦可能會比較方便。 比方說，如果您有兩部 MultiPoint 五個站台伺服器時，您可能會先建立本機使用者帳戶下, 表所示。  
  
**表 1:將本機使用者帳戶指派給特定的電腦執行 MultiPoint 服務**  
  
|電腦 A|電腦 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
在此案例中，每位使用者會有特定的電腦上的單一帳戶。 因此，每一位使用者具有本機帳戶的電腦可以從任何電腦 a。 相關聯的站台登入其帳戶不過，這些使用者無法存取他們的帳戶，如果他們使用站台相關聯與電腦 B，反之亦然。 這種方法的優點是，藉由一律連接至相同的電腦，使用者可一律尋找及存取其檔案。  
  
相反地，它也可複寫在執行 MultiPoint 服務的所有電腦上的個別使用者帳戶下, 表所示。  
  
**表 2:複寫在執行 MultiPoint 服務的所有電腦上的使用者帳戶**  
  
|電腦 A|電腦 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
這種方法的優點是，使用者會在每個可用的 MultiPoint 服務上有本機使用者帳戶。 不過，缺點可能會超過這項優勢。 比方說，即使使用者名稱和密碼特定個人的兩部電腦上相同，則帳戶是未連結到彼此。 因此，如果使用者登入他或她的帳戶在星期一時，電腦 A 上儲存檔案，然後登入他或她的帳戶，在電腦 B 上星期二，他或她將無法再存取先前儲存在此外電腦 a 上的檔案將多部電腦上的使用者帳戶的複寫會增加系統管理額外負荷和儲存體需求。  
  
### <a name="use-generic-local-user-accounts"></a>使用泛用的本機使用者帳戶  
如果 MultiPoint 服務系統不會連線到網域，而且您不要建立個別針對每個使用者帳戶，您可以建立每個站台的通用的帳戶。 例如，如果您有兩部執行 MultiPoint 服務的電腦，但每一部電腦相關聯五個站台，您可能決定建立使用者帳戶類似於下表所示。  
  
**表 3:建立一般使用者帳戶，每個站台的一個帳戶**  
  
|電腦 A|電腦 B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
在此案例中，每個站台帳戶都相同的密碼，而密碼和一般使用者帳戶名稱是供所有使用者。 此方法的優點是可能會小於如果會使用個別的帳戶，因為通常有較少的站台，比使用者管理使用者帳戶的額外負荷。 此外，藉由將複寫的每部伺服器上的使用者帳戶所造成的額外負荷就會被淘汰。  
  
另一個選項是以每一部伺服器上建立泛型的帳戶。 每個使用者登入伺服器以相同的帳戶。 若要允許這樣做，您必須啟用每個帳戶的多個工作階段。 您可以進一步簡化所有伺服器上使用相同的帳戶名稱和密碼。 這可簡化登入的使用者，只需要知道帳戶名稱和密碼，才能使用任何伺服器上的任何站台。 請注意在此案例中的所有使用者將可以都看到任何使用者進行任何變更。 比方說，如果將檔案儲存到桌面，所有的使用者可以看到該檔案。  
  
> [!IMPORTANT]  
> 請務必了解當使用者共用的使用者帳戶，可能是每一部伺服器的一或一個針對單一站台時，檔案儲存在伺服器 – 甚至是在我的文件中儲存的檔案-不是私用。 任何使用該帳戶登入的使用者可以存取這些檔案。 當您使用一個帳戶每個站台，如果使用者將檔案儲存到我的文件站台上時，使用者沒有存取這些不同的站台上的檔案。 同樣的情況發生時登入不同的 MultiPoint 服務電腦。  
  
若要讓使用者能夠從任何站台存取他們的檔案，您可以使用檔案伺服器、 建立檔案共用，每個使用者帳戶，或讓其個人的文件儲存在 USB 快閃磁碟機或其他私人儲存體裝置上的使用者。 個別的 USB 快閃磁碟機可讓個別儲存私用的文件，即使它們共用 MultiPoint 服務上的使用者帳戶的使用者。