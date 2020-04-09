---
title: 為您的 MultiPoint 服務環境規劃使用者帳戶
description: MultiPoint 服務中使用者帳戶的規劃資訊
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 28ee7a1475ec55352fe344842b8df7633abb9137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853391"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>為您的 MultiPoint 服務環境規劃使用者帳戶
在 MultiPoint 服務中執行使用者帳戶的最佳方式，會視部署的大小和複雜度而定：  
  
-   **本機使用者帳戶**-針對只有少數電腦執行 MultiPoind 服務和少數使用者的小型部署，您可能會發現使用在 MultiPoint 服務上建立的*本機使用者帳戶*是最方便的。 您可以為每個將使用系統的人員建立個別帳戶，或為每個工作站建立一個一般帳戶，讓任何人都能用來登入。 MultiPoint 服務系統管理員會使用 MultiPoint 管理員來建立和管理本機使用者帳戶。 本機帳戶可以是系統管理員、具有有限的系統管理許可權，或是無法存取 MultiPoint 服務桌面或 MultiPoint 管理員的一般使用者。  
  
-   **網域帳戶**-如果您的環境中有許多執行 MultiPoint 服務和許多使用者的電腦，您可能會發現設定 Active Directory Domain Services \(AD DS\) 網域並使用*網域使用者帳戶*更有用，這可讓使用者從網域中的任何工作站存取自己的使用者設定檔和設定。 網域使用者帳戶必須由網域系統管理員在網域控制站上建立。  
  
> [!NOTE]  
> 下列各節將討論您可能會針對 MultiPoint 服務中的本機使用者帳戶所執行的案例。 如果您使用網域使用者帳戶，請參閱[範例案例： MultiPoint 服務使用者帳戶](Example-scenarios--MultiPoint-Services-user-accounts.md)中的「網域網路環境中的一或多個 MultiPoint 伺服器」案例。  
  
## <a name="planning-local-user-accounts"></a>規劃本機使用者帳戶  
下列各節將針對在 Windows MultiPoint 服務環境中執行個別或共用本機使用者帳戶的數種方式，考慮其優點、缺點和需求。  
  
### <a name="use-individual-local-user-accounts"></a>使用個別的本機使用者帳戶  
建立本機使用者帳戶時，您可以選擇兩種方法。  將每個使用者指派給執行 MultiPoint 服務的特定伺服器，並為每個使用者建立單一帳戶。 或者，在執行 Multipoint 服務的每一部電腦上，為所有使用者建立本機使用者帳戶。 執行個別使用者帳戶的主要優點是，每位使用者都有自己的 Windows 桌面體驗，其中包含用於儲存資料的私人資料夾。 
  
從系統管理的觀點來看，將使用者指派給特定的 MultiPoint 服務電腦可能會比較方便。 例如，如果您有兩個具有五個工作站的 MultiPoint 伺服器，您可能會建立本機使用者帳戶，如下表所示。  
  
**表1：將本機使用者帳戶指派給執行 MultiPoint 服務的特定電腦**  
  
|電腦 A|電腦 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
在此案例中，每個使用者在特定電腦上都有單一帳戶。 因此，在電腦 A 上具有本機帳戶的每個人，都可以從與電腦 A 相關聯的任何工作站登入他或他的帳戶。不過，如果這些使用者使用與電腦 B 相關聯的工作站，則無法存取其帳戶，反之亦然。 這種方法的優點是，藉由一律連接到同一部電腦，使用者一律可以尋找並存取其檔案。  
  
相反地，您也可以在執行 MultiPoint 服務的所有電腦上複寫個別使用者帳戶，如下表所示。  
  
**表2：在所有執行 MultiPoint 服務的電腦上複寫使用者帳戶**  
  
|電腦 A|電腦 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
這種方法的優點是，使用者在每個可用的 MultiPoint 服務上都有一個本機使用者帳戶。 不過，缺點可能會超過這種優點。 例如，即使這兩部電腦上特定人員的使用者名稱和密碼相同，這些帳戶也不會彼此連結。 因此，如果使用者在星期一的電腦 A 登入自己的帳戶、儲存檔案，然後在星期二的電腦 B 登入其帳戶，他或她將無法存取先前儲存在電腦 A 上的檔案。此外，在多部電腦上複寫使用者帳戶會增加管理負擔和儲存需求。  
  
### <a name="use-generic-local-user-accounts"></a>使用一般本機使用者帳戶  
如果您的 MultiPoint 服務系統未連線到網域，而您不想要為每個使用者建立個別帳戶，您可以為每個工作站建立一般帳戶。 例如，如果您有兩部執行 MultiPoint 服務的電腦，而且有五個工作站與每部電腦相關聯，您可能會決定建立如下表所示的使用者帳戶。  
  
**表3：建立一般使用者帳戶，每個工作站一個帳戶**  
  
|電腦 A|電腦 B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
在此案例中，每個工作站帳戶都具有相同的密碼，而且密碼和一般使用者帳戶名稱都可供所有使用者使用。 這種方法的優點是，管理使用者帳戶的額外負荷可能會小於使用個別帳戶，因為通常會少於使用者的工作站。 此外，在每一部伺服器上複寫使用者帳戶所造成的負擔也會被排除。  
  
另一個選項是在每部伺服器上建立一般帳戶。 每位使用者都以相同的帳戶登入伺服器。 若要允許此情況，您必須啟用每個帳戶的多個會話。 您可以在所有伺服器上使用相同的帳戶名稱和密碼，進一步簡化。 這會簡化使用者的登入，他們只需要知道一個帳戶名稱和密碼，就可以在任何伺服器上使用任何工作站。 請注意，在此案例中，所有使用者都可以看到任何使用者所做的任何變更。 例如，如果將檔案儲存到桌面，所有使用者都可以看到該檔案。  
  
> [!IMPORTANT]  
> 請務必瞭解，當使用者共用使用者帳戶時，每一部伺服器或每個工作站一個檔案，儲存在伺服器上的檔案（即使是儲存在 [我的文件] 中的檔案）都不是私用的。 使用此帳戶登入的任何使用者都可以存取這些檔案。 當您使用每個工作站的一個帳戶時，如果使用者將檔案儲存到一個工作站上的檔，則使用者無法存取不同工作站上的這些檔案。 登入不同的 MultiPoint 服務電腦時，就會發生相同的情況。  
  
若要讓使用者能夠從任何工作站存取他們的檔案，您可以使用檔案伺服器、為每個使用者帳戶建立檔案共用，或讓使用者將其個人資料儲存在 USB 快閃磁片磁碟機或其他私人存放裝置上。 個別的 USB 快閃磁片磁碟機可讓個別使用者儲存私用檔，即使它們是在 MultiPoint 服務上共用使用者帳戶也一樣。