---
title: DirectAccess 離線網域加入
description: 本指南說明在 Windows Server 2016 中執行與 DirectAccess 的離線網域聯結的步驟。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 229e2955c7f382ff630829990a9dd6485d62652e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388883"
---
# <a name="directaccess-offline-domain-join"></a>DirectAccess 離線網域加入

>適用於：Windows Server (半年度管道)、Windows Server 2016

本指南說明使用 DirectAccess 執行離線網域聯結的步驟。 在離線網域加入期間，會將電腦設定為加入網域，而不需要實體或 VPN 連接。  
  
此指南包含下列各節：  
  
- 離線加入網域總覽  
  
- 離線加入網域的需求
  
- 離線網域加入程式
  
- 執行離線加入網域的步驟  
  
## <a name="offline-domain-join-overview"></a>離線加入網域總覽  
在 Windows Server 2008 R2 中引進的網域控制站包含一個稱為離線網域加入的功能。 名為 Djoin.exe 的命令列公用程式可讓您將電腦加入網域，而不需要在完成加入網域作業時實際聯繫網域控制站。 使用 Djoin.exe 的一般步驟如下：  
  
1.  執行**djoin.exe/provision**以建立電腦帳戶中繼資料。 此命令的輸出是包含基底64編碼 blob 的 .txt 檔案。  
  
2.  執行**Djoin.exe/requestODJ** ，將電腦帳戶中繼資料從 .txt 檔案插入目的地電腦的 Windows 目錄。  
  
3.  重新開機目的地電腦，電腦將會加入網域。  
  
### <a name="BKMK_ODJOverview"></a>使用 DirectAccess 原則進行離線網域聯結案例總覽  
DirectAccess 離線網域加入程式，執行 Windows Server 2016、Windows Server 2012、Windows 10 和 Windows 8 的電腦可以使用來加入網域，而不需要實際加入公司網路或透過 VPN 連線。 如此一來，就可以將電腦從無法連線到公司網路的位置加入網域。 DirectAccess 的離線加入網域會將 DirectAccess 原則提供給用戶端，以允許遠端布建。  
  
網域加入會建立電腦帳戶，並在執行 Windows 作業系統和 Active Directory 網域的電腦之間建立信任關係。  
  
## <a name="BKMK_ODJRequirements"></a>準備離線加入網域  
  
1.  建立電腦帳戶。  
  
2.  清查電腦帳戶所屬的所有安全性群組的成員資格。  
  
3.  收集要套用至新用戶端的必要電腦憑證、群組原則和群組原則物件。  
  
. 下列各節說明使用 Djoin.exe 執行 DirectAccess 離線網域加入的作業系統需求和認證需求。  
  
### <a name="operating-system-requirements"></a>作業系統需求  
您只能在執行 Windows Server 2016、Windows Server 2012 或 Windows 8 的電腦上執行 DirectAccess 的 Djoin.exe。 您執行 Djoin.exe 以將電腦帳戶資料布建到 AD DS 的電腦必須執行 Windows Server 2016、Windows 10、Windows Server 2012 或 Windows 8。 您要加入網域的電腦也必須執行 Windows Server 2016、Windows 10、Windows Server 2012 或 Windows 8。  
  
### <a name="credential-requirements"></a>認證要求  
若要執行離線加入網域，您必須擁有將工作站加入網域所需的許可權。 Domain Admins 群組的成員預設具有這些許可權。 如果您不是 Domain Admins 群組的成員，則 Domain Admins 群組的成員必須完成下列其中一項動作，讓您可以將工作站加入網域：  
  
-   使用群組原則來授與必要的使用者權限。 這個方法可讓您在 [預設電腦] 容器和稍後建立的任何組織單位（OU）中建立電腦（如果未新增拒絕存取控制專案（Ace））。  
  
-   編輯網域的 [預設電腦] 容器的存取控制清單（ACL），以將正確的許可權委派給您。  
  
-   建立 OU 並編輯該 OU 上的 ACL，以授與您「**建立子允許**」許可權。 將 **/machineOU**參數傳遞至**djoin.exe/provision**命令。  
  
下列程式說明如何使用群組原則授與使用者權限，以及如何委派正確的許可權。  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>授與將工作站加入網域的使用者權限  
您可以使用群組原則管理主控台（GPMC）來修改網域原則，或建立新的原則，其設定會授與使用者將工作站新增到網域的許可權。  
  
若要授與使用者權限，至少需要**Domain Admins**的成員資格或同等許可權。  如需使用適當帳戶和群組成員資格的詳細資訊，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)（ https://go.microsoft.com/fwlink/?LinkId=83477) 。   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>授與將工作站加入網域的許可權  
  
1.  依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [**群組原則管理**]。  
  
2.  按兩下樹系的名稱，連按兩下 [**網域**]，再按兩下您要加入電腦的功能變數名稱，再以滑鼠右鍵按一下 [**預設網域原則**]，然後按一下 [**編輯**]。  
  
3.  在主控台樹中，依序按兩下 [**電腦**設定]、[**原則**]、[ **Windows 設定**]、[**安全性設定**]、[**本機原則**]，然後按兩下**使用者權限指派**。  
  
4.  在詳細資料窗格中，按兩下 [**將工作站新增到網域**]。  
  
5.  選取 [**定義這些原則設定**] 核取方塊，然後按一下 [**新增使用者或群組**]。  
  
6.  輸入您要授與使用者權限的帳戶名稱，然後按兩次 **[確定]** 。  
  
## <a name="BKMK_ODKSxS"></a>離線網域加入程式  
在提高許可權的命令提示字元中執行 Djoin.exe，以布建電腦帳戶中繼資料。 當您執行布建命令時，會在您指定為命令一部分的二進位檔案中建立電腦帳戶中繼資料。  
  
如需在離線網域加入期間用來布建電腦帳戶的 NetProvisionComputerAccount 函數的詳細資訊，請參閱[NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426)函式（ https://go.microsoft.com/fwlink/?LinkId=162426) 。 如需在目的地電腦本機上執行之 NetRequestOfflineDomainJoin 函數的詳細資訊，請參閱[NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427)函式（ https://go.microsoft.com/fwlink/?LinkId=162427) 。  
  
## <a name="BKMK_ODJSteps"></a>執行 DirectAccess 離線網域加入的步驟  
離線網域加入套裝程式括下列步驟：  
  
1.  為每個遠端用戶端建立新的電腦帳戶，並從公司網路中已加入網域的電腦使用 Djoin.exe 命令產生布建套件。  
  
2.  將用戶端電腦新增至 DirectAccessClients 安全性群組  
  
3.  將布建套件安全地傳輸到將加入網域的遠端電腦。  
  
4.  套用布建套件，並將用戶端加入網域。  
  
5.  重新開機用戶端以完成加入網域，並建立連線能力。  
  
建立用戶端的布建封包時，有兩個要考慮的選項。 如果您使用消費者入門 Wizard 來安裝不含 PKI 的 DirectAccess，則應該使用下列選項1。 如果您使用 [Advanced 安裝精靈] 搭配 PKI 安裝 DirectAccess，則應該使用下面的選項2。  
  
完成下列步驟以執行離線加入網域：  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>選項 1：不使用 PKI 建立用戶端的布建套件  
  
1.  在遠端存取服務器的命令提示字元中，輸入下列命令以提供電腦帳戶：  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Option2使用 PKI 建立用戶端的布建套件  
  
1.  在遠端存取服務器的命令提示字元中，輸入下列命令以提供電腦帳戶：  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>將用戶端電腦新增至 DirectAccessClients 安全性群組  
  
1.  在您的網域控制站上，從 [**開始**] 畫面輸入**Active** ，然後從 [**應用程式**] 畫面中選取 [ **Active Directory 使用者和電腦**]。  
  
2.  展開網域底下的樹狀結構，然後選取 [**使用者**] 容器。  
  
3.  在詳細資料窗格中，以滑鼠右鍵按一下 [ **DirectAccessClients**]，然後按一下 [**屬性**]。  
  
4.  在 [成員] 索引標籤上，按一下 [新增]。  
  
5.  按一下 [**物件類型**]，選取 [**電腦**]，然後按一下 **[確定]** 。  
  
6.  輸入要新增的用戶端名稱，然後按一下 **[確定]** 。  
  
7.  按一下**確定**關閉  **DirectAccessClients**內容 對話方塊，然後關閉**Active Directory 使用者和電腦**。  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>複製布建套件，然後將其套用至用戶端電腦  
  
1.  將布建套件從遠端存取服務器上的 c:\files\provision.txt 複製到用戶端電腦上的 c:\provision\provision.txt。  
  
2.  在用戶端電腦上，開啟提升許可權的命令提示字元，然後輸入下列命令以要求加入網域：  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  重新開機用戶端電腦。 電腦將會加入網域。 重新開機之後，用戶端將會加入網域，並使用 DirectAccess 連線到公司網路。  
  
## <a name="see-also"></a>另請參閱  
[NetProvisionComputerAccount 函式](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin 函式](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


