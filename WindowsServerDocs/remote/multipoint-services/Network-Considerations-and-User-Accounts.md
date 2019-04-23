---
title: 網路考量和使用者帳戶
description: 使用 MultiPoint 服務會提供不同的網路和使用者案例的規劃資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9133f28d2c3b36b18a2b6bc81d238835156bf447
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880799"
---
# <a name="network-considerations-and-user-accounts"></a>網路考量和使用者帳戶
MultiPoint 服務可以部署在各種不同的網路環境中，而且它可以支援本機使用者帳戶和網域使用者帳戶。 一般而言，將其中一個下列的網路環境中管理 MultiPoint 服務使用者帳戶：  
  
-   使用本機使用者帳戶執行 MultiPoint 服務的單一電腦  
  
-   執行 MultiPoint 服務，每個都具有本機使用者帳戶的多部電腦  
  
-   多部電腦執行 MultiPoint 服務，使用網域使用者帳戶

根據定義，*本機使用者帳戶*只能從已建立的電腦存取。 本機使用者帳戶是正在執行 MultiPoint 服務的特定電腦所建立的使用者帳戶。 相反地，*網域使用者帳戶*是位於網域控制站上的使用者帳戶，並從任何連線到網域的電腦可以存取它們。 當您決定哪些類型的網路環境使用時，請考慮下列各項：  
  
-   將在伺服器之間共用資源嗎？  
  
-   將伺服器之間切換使用者？  
  
-   使用者會存取需要驗證的資料庫伺服器？  
  
-   使用者會存取內部 web 伺服器需要驗證嗎？  
  
-   在位置上有現有的 Active Directory 網域基礎結構嗎？  
  
-   誰會使用 MultiPoint 管理員主控台來管理使用者桌面，檢視縮圖中，新增使用者，限制網站等等？ 這位人員需要管理多部伺服器？ 這位人員必須在伺服器上具有系統管理權限。  
  
下列各節會解決在這些網路環境中的使用者帳戶管理。  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>單一的 MultiPoint 伺服器使用本機使用者帳戶  
在執行 MultiPoint 服務的單一電腦環境中，沒有要有網路的需求。 不過，若要利用網際網路資源，可能會一樣路由器和網際網路服務提供者 (ISP) 連線的基本的網路需求。 根據預設，若要取得的 IP 位址和 DNS 伺服器位址，自動透過 DHCP，已與 MultiPoint 服務的網路介面卡相關聯的網路連線。 網際網路的路由器通常設定為 DHCP 伺服器，並且會提供私人 IP 位址來連接到內部網路用的電腦。 因此，執行 MultiPoint 服務的單一電腦可能無法將連線到內部路由器的介面，取得自動 IP 資訊，並連線到網際網路，而不需要更多心力或由系統管理員的組態。  
  
在這種環境中管理使用者的常見方式是建立本機使用者帳戶的每個人將存取系統。 任何具有該電腦上的本機使用者帳戶的人員可以從任何系統相關聯的站台來登入 MultiPoint 服務。 可以建立本機使用者帳戶，並從 MultiPoint 管理員管理。  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>使用本機使用者帳戶的多個 MultiPoint Server 系統  
假設本機使用者帳戶才可從電腦所在他們所建立，當您部署的環境中的多個 MultiPoint 服務系統時，您可以管理本機使用者帳戶中有兩種：  
  
-   您可以在執行 MultiPoint 服務的特定電腦上的特定個人建立使用者帳戶。  
  
-   您可以使用 MultiPoint 管理員來執行 MultiPoint 服務每一部電腦上建立每個使用者的帳戶。  
  
比方說，如果您打算將使用者指派到執行 MultiPoint 服務的特定電腦，您可能會建立四個本機使用者帳戶在電腦 A 上 （user01、 user02、 user03 和 user04） 和四個本機使用者帳戶 （user05、 user06、 user07 和 user08） 的電腦 B 上。 在此案例中，使用者 01\-04 可以登入電腦 A 從連線至轉送任何站台，不過，它們無法登入到電腦 b。這也適用於使用者 05\-08，能夠登入，只給電腦 B，而非電腦 a。 視特定部署環境，這可以是可接受或不受歡迎。  
  
不過，如果每個使用者必須能夠登入任何執行 MultiPoint 服務的電腦，必須建立本機使用者帳戶執行 MultiPoint 服務每一部電腦上每位使用者。 若要管理使用者，以這種方式選擇導入了一些複雜性。 例如，如果 user01 登入電腦 A 上星期一，並儲存文件 資料夾中的檔案，然後使用者登入電腦 B，在星期二，儲存在電腦 A 將 文件 資料夾中的檔案無法存取電腦 b 上  
  
此外，如果使用者在電腦 A 和電腦 B 的帳戶，就無法自動同步處理帳戶的密碼。 這會導致無法登入應該變更帳戶密碼在上一部電腦，但沒有其他的使用者。 您可以將每個使用者指派給正在執行 MultiPoint 服務在單一電腦，以簡化在這種網路環境中的使用者帳戶管理。 如此一來，使用者可以登入任何站台，與該電腦建立關聯，並且存取適當的檔案。  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>使用網域帳戶的多個 MultiPoint 服務系統  
網域環境是在多部伺服器的大型網路環境中常見的。 比方說，您可能會將一或多個網域，請執行 MultiPoint 服務角色的電腦，並接著使用來管理可從網域中的任何電腦存取的使用者帳戶的 Microsoft Active Directory。 這可讓您建立並從任何站台存取任何已加入網域的 MultiPoint 服務系統中的個別的網域使用者帳戶。  
 
當您在網域環境中部署 MultiPoint 服務時，有數個因素需要考慮：  
  
-   如果使用網域帳戶，他們無法從 MultiPoint 管理員管理。  
  
-   根據預設，MultiPoint 服務設定為每個使用者的權限一次登入只有一個站台。 如果您決定要允許使用者登入多個站台使用單一帳戶的同時，您可以使用**編輯伺服器設定**MultiPoint 管理員中的選項。  
  
-   速度和可靠性，使用者將能夠通過網域驗證，並尋找資源，可能會影響網域控制站的位置。  
  
## <a name="single-user-account-for-multiple-stations"></a>多個站台的單一使用者帳戶  
MultiPoint 服務能夠同時使用單一使用者帳戶的相同電腦上的多個站台登入。 使用者系統不會提供唯一的使用者名稱，以及使用單一使用者帳戶，可以簡化管理 MultiPoint 服務系統，這項功能是在環境中很有用。  
  
