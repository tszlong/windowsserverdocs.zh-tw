---
title: 網路考量和使用者帳戶
description: 使用 MultiPoint 服務為不同的網路和使用者案例提供規劃資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 5369776a0341bf1f4d4d1d13569cf0964fdf11f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405041"
---
# <a name="network-considerations-and-user-accounts"></a>網路考量和使用者帳戶
MultiPoint 服務可以部署在各種不同的網路環境中，而且可以支援本機使用者帳戶和網域使用者帳戶。 通常，MultiPoint 服務使用者帳戶會在下列其中一個網路環境中進行管理：  
  
-   使用本機使用者帳戶執行 MultiPoint 服務的單一電腦  
  
-   多部執行 MultiPoint 服務的電腦，每個都有本機使用者帳戶  
  
-   執行 MultiPoint 服務並使用網域使用者帳戶的多部電腦

根據定義，*本機使用者帳戶*只能從建立它們的電腦存取。 本機使用者帳戶是在執行 MultiPoint 服務的特定電腦上建立的使用者帳戶。 相反地，*網域使用者帳戶*是位於網域控制站上的使用者帳戶，而且可以從任何連線到網域的電腦存取。 當您決定要使用哪種類型的網路環境時，請考慮下列事項：  
  
-   資源會在伺服器之間共用嗎？  
  
-   使用者會在伺服器之間切換嗎？  
  
-   使用者是否會存取需要驗證的資料庫伺服器？  
  
-   使用者是否會存取需要驗證的內部 web 伺服器？  
  
-   現有的 Active Directory 網域基礎結構是否已就緒？  
  
-   誰會使用 MultiPoint 管理員主控台來管理使用者桌面、查看縮圖、新增使用者、限制網站等等？ 這個人是否會管理一部以上的伺服器？ 此人必須具有伺服器的系統管理許可權。  
  
下列各節將說明這些網路環境中的使用者帳戶管理。  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>具有本機使用者帳戶的單一 MultiPoint 伺服器  
在執行 MultiPoint 服務的單一電腦環境中，不需要有網路。 不過，若要利用網際網路資源，網路需求可能會與路由器和網際網路服務提供者（ISP）的連接一樣基本。 預設會設定與 MultiPoint 服務上網路介面卡相關聯的網路連線，以透過 DHCP 自動取得 IP 位址和 DNS 伺服器位址。 網際網路路由器通常會設定為 DHCP 伺服器，並提供私人 IP 位址給連接到內部網路上的電腦。 因此，執行 MultiPoint 服務的單一電腦可能能夠連線到路由器的內部介面、取得自動 IP 資訊，以及連線到網際網路，而不需要由系統管理員進行大量的工作或設定。  
  
在這種環境中管理使用者的常見方式，是為每個會存取系統的人員建立本機使用者帳戶。 在該電腦上擁有本機使用者帳戶的任何人，都可以從任何與系統相關聯的工作站登入 MultiPoint 服務。 您可以從 MultiPoint 管理員建立和管理本機使用者帳戶。  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>具有本機使用者帳戶的多個 MultiPoint 伺服器系統  
假設本機使用者帳戶只能從建立它們的電腦存取，當您在環境中部署多個 MultiPoint 服務系統時，您可以使用下列兩種方式之一來管理本機使用者帳戶：  
  
-   您可以在執行 MultiPoint 服務的特定電腦上，為特定的人員建立使用者帳戶。  
  
-   您可以使用 MultiPoint 管理員，為每一部執行 MultiPoint 服務的電腦上的每位使用者建立帳戶。  
  
例如，如果您打算將使用者指派給執行 MultiPoint 服務的特定電腦，您可能會在電腦 A （user01、user02、user03 和 user04）上建立四個本機使用者帳戶，並在電腦 B （user05、user06、user07 和 user08）上建立四個本機使用者帳戶。 在此案例中，使用者 01\-04 可以從任何已連線的工作站登入電腦 A;不過，他們無法登入電腦 B。這也適用于使用者 05\-08，他們只能登入電腦 B，而不是電腦 A。視特定部署環境而定，這可能是可接受的，甚至是必要的。  
  
不過，如果每個使用者都必須能夠登入任何執行 MultiPoint 服務的電腦，則必須在執行 MultiPoint 服務的每部電腦上，為每個使用者建立本機使用者帳戶。 選擇以這種方式管理使用者會帶來一些複雜的情況。 例如，如果 user01 會在星期一登入電腦 A，並將檔案儲存在 [檔] 資料夾中，然後使用者在星期二登入電腦 B，則電腦 B 上的 [檔] 資料夾中儲存的檔案將無法存取。  
  
此外，如果使用者在電腦 A 和電腦 B 上擁有帳戶，就無法自動同步處理帳戶的密碼。 這可能會導致使用者在登入時遇到困難，而不應在一部電腦上變更帳戶密碼。 您可以將每個使用者指派給執行 MultiPoint 服務的單一電腦，藉此簡化這種網路環境中的使用者帳戶管理。 如此一來，使用者就可以登入與該電腦相關聯的任何工作站，並存取適當的檔案。  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>具有網域帳戶的多個 MultiPoint 服務系統  
網域環境在包含多部伺服器的大型網路環境中很常見。 例如，您可以將執行 MultiPoint 服務角色的一或多部電腦加入網域，然後使用 Microsoft Active Directory 來管理可從網域中的任何電腦存取的使用者帳戶。 這可讓您從已加入網域的任何 MultiPoint 服務系統中的任何工作站建立和存取個別網域使用者帳戶。  
 
當您在網域環境中部署 MultiPoint 服務時，有幾個因素需要考慮：  
  
-   如果使用網域帳戶，則無法從 MultiPoint 管理員進行管理。  
  
-   根據預設，MultiPoint 服務會設定為讓每個使用者一次只能登入一部工作站。 如果您決定讓使用者使用單一帳戶同時登入多個工作站，您可以使用 [MultiPoint 管理員] 中的 [**編輯服務器設定**] 選項。  
  
-   網域控制站的位置可能會影響使用者能夠向網域進行驗證的速度和可靠性，並找出資源。  
  
## <a name="single-user-account-for-multiple-stations"></a>多個工作站的單一使用者帳戶  
MultiPoint 服務能夠使用單一使用者帳戶，同時登入同一部電腦上的多個工作站。 這項功能在使用者未指定唯一使用者名稱的環境中很有用，而且使用單一使用者帳戶可以簡化 MultiPoint 服務系統的管理。  
  
