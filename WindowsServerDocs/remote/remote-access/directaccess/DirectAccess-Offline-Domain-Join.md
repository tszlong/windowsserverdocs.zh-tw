---
title: DirectAccess 離線網域加入
description: 本指南說明在 Windows Server 2016 中使用 DirectAccess 執行離線網域加入的步驟。
manager: brianlic
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d18d643d6c614a90f4c9ab43b59c0b8777b2f6e8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946784"
---
# <a name="directaccess-offline-domain-join"></a>DirectAccess 離線網域加入

>適用於：Windows Server (半年度管道)、Windows Server 2016

本指南說明使用 DirectAccess 執行離線網域加入的步驟。 離線網域加入時，會將電腦設定為加入沒有實體或 VPN 連線的網域。

此指南包含下列各節：

- 離線加入網域總覽

- 離線加入網域的需求

- 離線網域加入程式

- 執行離線網域加入的步驟

## <a name="offline-domain-join-overview"></a>離線加入網域總覽
在 Windows Server 2008 R2 中引進的網域控制站包含一項稱為離線網域加入的功能。 名為 Djoin.exe 的命令列公用程式可讓您將電腦加入網域，而不需要實際聯繫網域控制站，同時完成網域加入作業。 使用 Djoin.exe 的一般步驟如下：

1.  執行 **djoin.exe/provision** 來建立電腦帳戶中繼資料。 此命令的輸出是包含 base-64 編碼 blob 的 .txt 檔案。

2.  執行 **Djoin.exe/requestODJ** ，將電腦帳戶中繼資料從 .txt 檔案插入目的地電腦的 Windows 目錄中。

3.  重新開機目的地電腦，電腦將加入網域。

### <a name="offline-domain-join-with-directaccess-policies-scenario-overview"></a><a name="BKMK_ODJOverview"></a>使用 DirectAccess 原則進行離線加入網域案例總覽
DirectAccess 離線網域加入程式是指執行 Windows Server 2016、Windows Server 2012、Windows 10 和 Windows 8 的電腦可以用來加入網域，而不需實際加入公司網路或透過 VPN 連線。 如此一來，就可以將電腦加入網域，使其無法連線到公司網路。 DirectAccess 的離線網域加入可提供 DirectAccess 原則給用戶端，以允許遠端布建。

網域加入會建立電腦帳戶，並在執行 Windows 作業系統和 Active Directory 網域的電腦之間建立信任關係。

## <a name="prepare-for-offline-domain-join"></a><a name="BKMK_ODJRequirements"></a>準備離線加入網域

1.  建立電腦帳戶。

2.  清查電腦帳戶所屬之所有安全性群組的成員資格。

3.  收集要套用至新的用戶端 () 所需的電腦憑證、群組原則和群組原則物件。

. 下列各節說明使用 Djoin.exe 執行 DirectAccess 離線網域加入的作業系統需求和認證需求。

### <a name="operating-system-requirements"></a>作業系統需求
您只能在執行 Windows Server 2016、Windows Server 2012 或 Windows 8 的電腦上執行 DirectAccess 的 Djoin.exe。 您執行 Djoin.exe 將電腦帳戶資料布建到 AD DS 的電腦必須執行 Windows Server 2016、Windows 10、Windows Server 2012 或 Windows 8。 您要加入網域的電腦也必須執行 Windows Server 2016、Windows 10、Windows Server 2012 或 Windows 8。

### <a name="credential-requirements"></a>認證要求
若要執行離線網域加入，您必須擁有將工作站加入網域所需的許可權。 Domain Admins 群組的成員依預設具有這些許可權。 如果您不是 Domain Admins 群組的成員，Domain Admins 群組的成員必須完成下列其中一項動作，讓您能夠將工作站加入網域：

-   使用群組原則來授與所需的使用者權限。 此方法可讓您在 [預設電腦] 容器中建立電腦，並在稍後建立的任何組織單位中 (OU)  (如果) 未新增 (Ace) 的拒絕存取控制專案。

-   編輯網域的 [預設電腦] 容器 (ACL) 的存取控制清單，以將正確的許可權委派給您。

-   建立 OU 並編輯該 OU 上的 ACL，以授與您「 **建立子允許** 」許可權。 將 **/machineOU** 參數傳遞至 **djoin.exe/provision** 命令。

下列程式示範如何將群組原則的許可權授與使用者，以及如何委派正確的許可權。

#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>授與使用者權限以將工作站加入網域
您可以使用群組原則管理主控台 (GPMC) 修改網域原則，或建立新的原則，該原則的設定會授與使用者將工作站新增至網域的許可權。

若要授與使用者權限，至少需要 **Domain Admins** 的成員資格或同等許可權。  請參閱在 [本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) (使用適當帳戶和群組成員資格的詳細資料 https://go.microsoft.com/fwlink/?LinkId=83477) 。

###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>授與將工作站加入網域的許可權

1.  按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按一下 [ **群組原則管理**]。

2.  按兩下樹系的名稱，按兩下 [ **網域**]，按兩下要加入電腦的功能變數名稱，在 [ **預設網域原則**] 上按一下滑鼠右鍵，然後按一下 [ **編輯**]。

3.  在主控台樹中，依序按兩下 [ **電腦** 設定]、[ **原則**]、[ **Windows 設定**]、[ **安全性設定**]、[ **本機原則**]，然後按兩下 [ **使用者權限指派**]。

4.  在詳細資料窗格中，按兩下 [ **將工作站新增到網域**]。

5.  選取 [ **定義這些原則設定** ] 核取方塊，然後按一下 [ **新增使用者或群組**]。

6.  輸入您想要授與使用者權限的帳戶名稱，然後按兩次 **[確定]** 。

## <a name="offline-domain-join-process"></a><a name="BKMK_ODKSxS"></a>離線網域加入程式
在提高許可權的命令提示字元中執行 Djoin.exe，以布建電腦帳戶中繼資料。 當您執行布建命令時，會在您指定為命令一部分的二進位檔案中建立電腦帳戶中繼資料。

如需在離線網域加入期間用來布建電腦帳戶之 NetProvisionComputerAccount 函數的詳細資訊，請參閱 [NetProvisionComputerAccount 函數](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426) 。 如需在本機電腦上執行之 NetRequestOfflineDomainJoin 函數的詳細資訊，請參閱 [NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427) 函式 (https://go.microsoft.com/fwlink/?LinkId=162427) 。

## <a name="steps-for-performing-a-directaccess-offline-domain-join"></a><a name="BKMK_ODJSteps"></a>執行 DirectAccess 離線網域加入的步驟
離線網域加入套裝程式含下列步驟：

1.  為每個遠端用戶端建立新的電腦帳戶，並從公司網路中已加入網域的電腦使用 Djoin.exe 命令產生布建套件。

2.  將用戶端電腦新增至 DirectAccessClients 安全性群組

3.  將布建套件安全地傳輸到將加入網域的遠端電腦 (s) 。

4.  套用布建套件，並將用戶端加入網域。

5.  重新開機用戶端以完成加入網域，並建立連線能力。

建立用戶端的布建封包時，有兩個要考慮的選項。 如果您使用消費者入門 Wizard 來安裝不含 PKI 的 DirectAccess，則應該使用下列選項1。 如果您使用 Advanced Setup Wizard 安裝 DirectAccess 與 PKI，則應該使用下列選項2。

請完成下列步驟，以執行離線網域加入：

##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Option1：建立不具 PKI 之用戶端的布建套件

1.  在遠端存取服務器的命令提示字元中，輸入下列命令以布建電腦帳戶：

    ```
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse
    ```

##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Option2：使用 PKI 建立用戶端的布建套件

1.  在遠端存取服務器的命令提示字元中，輸入下列命令以布建電腦帳戶：

    ```
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse
    ```

##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>將用戶端電腦新增至 DirectAccessClients 安全性群組

1.  在您的網域控制站上，從 [**開始**] 畫面輸入 **Active** ，然後從 [**應用程式**] 畫面選取 [ **Active Directory 消費者和電腦**]。

2.  展開您網域下的樹狀結構，然後選取 [ **使用者** ] 容器。

3.  在詳細資料窗格中，以滑鼠右鍵按一下 [ **DirectAccessClients**]，然後按一下 [ **屬性**]。

4.  在 [成員] 索引標籤上，按一下 [新增]。

5.  按一下 [ **物件類型**]，選取 [ **電腦**]，然後按一下 **[確定]**。

6.  輸入要新增的用戶端名稱，然後按一下 **[確定]**。

7.  按一下 **[確定** ] 關閉 [ **DirectAccessClients** 屬性] 對話方塊，然後關閉 **Active Directory 消費者和電腦**。

##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>複製布建套件，然後將其套用至用戶端電腦

1.  將布建套件從遠端存取服務器上的 c:\files\provision.txt （儲存位置）複製到用戶端電腦上 c:\provision\provision.txt。

2.  在用戶端電腦上，開啟提升許可權的命令提示字元，然後輸入下列命令以要求加入網域：

    ```
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos
    ```

3.  重新開機用戶端電腦。 電腦將加入網域。 在重新開機後，用戶端會加入網域，並可與具有 DirectAccess 的公司網路連線。

## <a name="see-also"></a>另請參閱
[NetProvisionComputerAccount 函式](https://go.microsoft.com/fwlink/?LinkId=162426) 
[NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427)函式



