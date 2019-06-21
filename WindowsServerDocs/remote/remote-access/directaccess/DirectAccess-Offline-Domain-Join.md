---
title: DirectAccess 離線網域加入
description: 本指南說明 Windows Server 2016 中執行與 DirectAccess 離線網域加入的步驟。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59b5933a81c7021e58ea14e6ea4c4da374ce35cb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283665"
---
# <a name="directaccess-offline-domain-join"></a>DirectAccess 離線網域加入

>適用於：Windows Server （半年通道），Windows Server 2016

本指南說明如何執行與 DirectAccess 離線網域加入。 離線網域加入期間，電腦已加入網域，而不需要實體或 VPN 連線。  
  
此指南包含下列各節：  
  
- 離線網域加入概觀  
  
- 離線網域加入的需求
  
- 離線網域加入程序
  
- 執行離線網域加入的步驟  
  
## <a name="offline-domain-join-overview"></a>離線網域加入概觀  
網域控制站導入 Windows Server 2008 R2 中，包含名為離線加入網域的功能。 名為 Djoin.exe 命令列公用程式可讓您將電腦加入網域，而不實際完成加入網域作業時連絡的網域控制站。 使用 Djoin.exe 的一般步驟如下：  
  
1.  執行**djoin /provision**建立電腦帳戶中繼資料。 此命令的輸出是.txt 檔案包含的 base-64 編碼的 blob。  
  
2.  執行**djoin /requestODJ**從.txt 檔案中插入目的地電腦的 Windows 目錄的電腦帳戶中繼資料。  
  
3.  重新啟動目的地電腦，並將電腦加入網域。  
  
### <a name="BKMK_ODJOverview"></a>離線網域加入與 DirectAccess 原則案例概觀  
DirectAccess 離線網域加入是執行 Windows Server 2016、 Windows Server 2012、 Windows 10 和 Windows 8 的電腦加入網域，而不實際聯結到公司網路時，可以使用，或透過 VPN 連線的程序。 這讓您能夠將電腦加入網域中，從位置沒有任何連線到公司網路。 Directaccess 離線網域加入以允許遠端佈建的用戶端提供 DirectAccess 原則。  
  
網域加入建立電腦帳戶，並且建立執行 Windows 作業系統和 Active Directory 網域的電腦之間的信任關係。  
  
## <a name="BKMK_ODJRequirements"></a>準備離線網域加入  
  
1.  建立電腦帳戶。  
  
2.  清查的電腦帳戶所屬的所有安全性群組的成員資格。  
  
3.  收集必要的電腦憑證、 群組原則和群組原則物件套用至新的用戶端。  
  
. 下列各節說明作業系統需求以及執行使用 Djoin.exe DirectAccess 離線網域加入認證需求。  
  
### <a name="operating-system-requirements"></a>作業系統需求  
您可以執行 directaccess 的 Djoin.exe 只在執行 Windows Server 2016、 Windows Server 2012 或 Windows 8 的電腦上。 您用來執行 Djoin.exe 來佈建電腦帳戶資料與 AD DS 的電腦必須執行 Windows Server 2016、 Windows 10，Windows Server 2012 或 Windows 8。 您想要加入網域的電腦也必須執行 Windows Server 2016、 Windows 10、windows Server 2012 或 Windows 8。  
  
### <a name="credential-requirements"></a>認證要求  
若要執行離線網域加入，您必須已加入網域的工作站所需的權限。 根據預設，Domain Admins 群組的成員具有這些權限。 如果您不是 Domain Admins 群組的成員，Domain Admins 群組的成員必須完成下列動作，以讓您將加入網域的工作站的其中一個：  
  
-   您可以使用 群組原則來授與必要的使用者權限。 這個方法可讓您建立的電腦 （如果沒有拒絕存取控制項目 (Ace) 會新增） 更新版本建立的任何組織單位 (OU) 和預設電腦容器中。  
  
-   編輯存取控制清單 (ACL) 的預設電腦容器的網域委派給您正確的權限。  
  
-   建立 OU 並編輯該 OU，以授與您的 ACL**建立子-允許**權限。 傳遞 **/machineOU**參數來**djoin /provision**命令。  
  
下列程序示範如何授與使用者權限，使用群組原則以及如何將正確的權限委派。  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>授與使用者權限，才能加入網域的工作站  
若要修改的網域原則或建立新的原則會授與使用者權限，將工作站新增到網域的設定，您可以使用群組原則管理主控台 (GPMC)。  
  
中的成員資格**Domain Admins**，或同等權限，授與使用者權限所需的最小值。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)(https://go.microsoft.com/fwlink/?LinkId=83477) 。   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>若要授與權限可以加入網域的工作站  
  
1.  按一下 **開始**，按一下**系統管理工具**，然後按一下**群組原則管理**。  
  
2.  按兩下 樹系的名稱中，按兩下**網域**，連按兩下您要加入的電腦，請以滑鼠右鍵按一下網域名稱**預設網域原則**，然後按一下  **編輯**。  
  
3.  在主控台樹狀目錄中，按兩下**電腦組態**，按兩下**原則**，連按兩下**Windows 設定**，連按兩下**安全性設定**，按兩下**本機原則**，然後按兩下**使用者權限指派**。  
  
4.  在 [詳細資料] 窗格中，按兩下**新增工作站到網域**。  
  
5.  選取 **定義這些原則設定**核取方塊，然後按一下**新增使用者或群組**。  
  
6.  輸入您要授與使用者的權限，然後按一下 帳戶名稱**確定**兩次。  
  
## <a name="BKMK_ODKSxS"></a>離線網域加入程序  
執行 Djoin.exe 來佈建電腦帳戶中繼資料以提高權限的命令提示字元。 當您執行佈建的命令時，會將電腦帳戶中繼資料建立的二進位檔案中，您指定為命令的一部分。  
  
如需有關用來佈建的電腦帳戶，離線網域加入期間 NetProvisionComputerAccount 函式的詳細資訊，請參閱[NetProvisionComputerAccount 函式](https://go.microsoft.com/fwlink/?LinkId=162426)(https://go.microsoft.com/fwlink/?LinkId=162426) 。 如需有關在目的地電腦本機執行 NetRequestOfflineDomainJoin 函式的詳細資訊，請參閱[NetRequestOfflineDomainJoin 函式](https://go.microsoft.com/fwlink/?LinkId=162427)(https://go.microsoft.com/fwlink/?LinkId=162427) 。  
  
## <a name="BKMK_ODJSteps"></a>執行 DirectAccess 離線網域加入的步驟  
離線網域加入程序包含下列步驟：  
  
1.  為每個遠端用戶端建立新的電腦帳戶，並產生佈建套件使用 Djoin.exe 命令，從已加入網域的電腦在公司網路中。  
  
2.  將用戶端電腦新增至 DirectAccessClients 安全性群組  
  
3.  將會加入網域的遠端電腦 (s) 來安全地傳輸佈建套件。  
  
4.  套用佈建套件，並將用戶端加入網域。  
  
5.  重新啟動以完成網域加入，並建立連線的用戶端。  
  
有兩個用戶端建立佈建設定的封包時要考量的選項。 如果您使用快速入門精靈 」 來安裝 DirectAccess，而不需要 PKI 時，您應該使用選項 1 以下。 如果您使用 進階安裝精靈來安裝 DirectAccess 使用 PKI 時，您應該使用選項 2 的下方。  
  
請完成下列步驟，以執行離線網域加入：  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>選項 1：建立未使用 PKI 用戶端的佈建套件  
  
1.  在您的遠端存取伺服器的命令提示字元中，輸入下列命令來佈建的電腦帳戶：  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>選項 2:建立 PKI 的用戶端的佈建套件  
  
1.  在您的遠端存取伺服器的命令提示字元中，輸入下列命令來佈建的電腦帳戶：  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>將用戶端電腦新增至 DirectAccessClients 安全性群組  
  
1.  網域控制站，從**開始**畫面上，輸入**Active** ，然後選取**Active Directory 使用者和電腦**從**應用程式**畫面.  
  
2.  展開您的網域下的樹狀目錄，然後選取**使用者**容器。  
  
3.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下**DirectAccessClients**，然後按一下**屬性**。  
  
4.  在 [成員]  索引標籤上，按一下 [新增]  。  
  
5.  按一下 **物件類型**，選取**電腦**，然後按一下**確定**。  
  
6.  輸入用戶端名稱，來新增，然後按一下**確定**。  
  
7.  按一下 [ **[確定]** 以關閉**DirectAccessClients**內容] 對話方塊，然後關閉**Active Directory 使用者和電腦**。  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>複製，然後套用到用戶端電腦的 佈建套件  
  
1.  從遠端存取伺服器上儲存的位置，以用戶端電腦上的 c:\provision\provision.txt c:\files\provision.txt 中複製佈建套件。  
  
2.  用戶端電腦上，開啟提升權限的命令提示字元中，然後輸入下列命令，以要求網域加入：  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  用戶端電腦重新開機。 將電腦加入網域。 在重新開機後, 用戶端會加入網域，並能夠連線到公司網路有了 DirectAccess。  
  
## <a name="see-also"></a>另請參閱  
[NetProvisionComputerAccount 函式](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin 函式](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


