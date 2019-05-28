---
title: 建立站台的 Windows 10 企業版虛擬桌面
description: 了解如何建立站台的 Windows Server 2016 桌面
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: 0aa81ef3633adf27a25b45b3b7c00082d83bf0bb
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034620"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>建立站台的 Windows 10 企業版虛擬桌面
此選用的設定，MultiPoint 服務中，主要是用於重要的應用程式需要有自己的執行個體，用戶端作業系統的每個使用者的情況。 範例包括無法在 Windows Server 安裝的應用程式和應用程式無法在相同的主機電腦上執行多個執行個體。  
  
> [!NOTE]  
> 因此，建議您使用預設 MultiPoint 服務工作階段，這些虛擬桌面，也稱為 VDI 會更大量的資源比預設 MultiPoint 服務桌面工作階段，可能的話。  
  
## <a name="prerequisites"></a>先決條件  
若要準備建立站台虛擬桌面時，請確定您的 MultiPoint 服務系統符合下列需求：      
  
|硬體|需求|         |
|------------|----------------|----------------| 
|CPU （多媒體）|1 個核心或每個虛擬機器的執行緒|  
|固態硬碟 (SSD)|容量 > = 針對單一站台的 20 GB + 40 GB MultiPoint 服務會裝載作業系統<br /><br />隨機讀取\/寫入 IOPS > = 3 K 針對單一站台|  
|RAM|每個站台的 2 GB + Windows MultiPoint Server 的主機作業系統的 2 GB|  
|圖形|DX11|  
|BIOS|設定用來啟用虛擬化-第二層位址轉譯 (SLAT) 的 BIOS CPU 設定|  
  
-   **站台**-設定為您的 MultiPoint 服務系統的站台。 如需詳細資訊，請參閱 <<c0> [ 將其他站台連接到 MultiPoint 服務](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
-   **網域**-在網域環境中，在 Windows MultiPoint Server 的電腦已加入網域，並已新增至 MultiPoint 服務主機作業系統上本機 Administrators 群組的網域使用者。  
  
## <a name="procedures"></a>程序  
請使用下列程序來：  
  
-   [建立適用於虛擬桌面範本](#create-a-template-for-virtual-desktops)  
  
-   [從範本建立虛擬桌面](#create-virtual-machine-desktops-from-the-template)  
  
-   [複製現有的虛擬桌面範本](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>建立適用於虛擬桌面範本  
您可以建立您的虛擬桌面範本之前，您必須啟用 MultiPoint Server 的虛擬桌面功能。  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>若要啟用虛擬桌面功能  
  
1.  登入 MultiPoint 伺服器主機作業系統使用本機系統管理員帳戶，或在網域中，使用網域帳戶是本機 Administrators 群組的成員。  
  
2.  從**啟動**畫面上，開啟 MultiPoint 管理員。  
  
3.  按一下 **虛擬桌面**索引標籤上，按一下 **啟用虛擬桌面**，然後按一下 **確定**，並等待系統重新啟動。  
  
下一步是建立虛擬桌面範本。 實際上，您要建立您可以使用做為範本，以建立 MultiPoint 管理員的站台虛擬桌面的虛擬硬碟 (VHD) 檔案。 您可以使用實體的安裝媒體的 Windows 或。ISO 影像檔做為來源範本。 您也可以使用。Windows 安裝的 VHD。 請注意，若要使用實體的安裝光碟，您必須插入光碟片之前啟動精靈。  
  
##### <a name="to-create-a-virtual-desktop-template"></a>若要建立虛擬桌面範本  
  
1.  登入 MultiPoint 伺服器主機作業系統使用本機系統管理員帳戶，或在網域中的本機 Administrators 群組成員的網域帳戶。  
  
2.  從**啟動**畫面上，開啟 MultiPoint 管理員。  
  
3.  按一下 [**虛擬桌面**] 索引標籤。  
  
4.   將 Windows 10 Enterprise.iso 檔案複製到本機 SSD。  
  
5.  在 虛擬桌面 索引標籤中，按一下 **建立虛擬桌面範本。**   
  
6.  在 **前置詞**，輸入要用來識別此範本並使用範本建立虛擬桌面的前置詞。 預設前置詞是主機電腦的名稱。  
  
    首碼是用來命名範本和虛擬桌面站台。 範本將會 <*前置詞*>-t。 虛擬桌面站台將會命名為 <*前置詞*>-*n*，其中*n*是站台識別碼。  
  
7.  輸入使用者名稱和密碼，才能使用本機系統管理員帳戶的範本。 在網域中，輸入將會新增至本機 Administrators 群組的網域帳戶的認證。 此帳戶可以用來登入的範本，並從範本建立的所有虛擬桌面站台。  
  
8. 按一下 **確定**，並等候完成建立範本。  
  
9. 新的範本將會列在**虛擬桌面** 索引標籤。範本將會關閉。  
  
下一步是設定軟體與您想要在虛擬桌面設定的範本。 從範本建立任何虛擬桌面之前，您必須這麼做。  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>若要自訂虛擬桌面範本  
  
1.  登入 MultiPoint 伺服器主機作業系統使用本機系統管理員帳戶，或在網域中，使用本機 Administrators 群組中的網域帳戶。  
  
2.  從**啟動**畫面上，開啟 MultiPoint 管理員。  
  
3.  按一下 [**虛擬桌面**] 索引標籤。  
  
4.  選取您想要自訂，請按一下範本**自訂範本**，然後按一下**確定**。  
  
    > [!NOTE]  
    > 若要建立虛擬桌面站台未使用的範本可用。 如果您想要更新已在使用範本，您必須使用來進行範本的複本**匯入範本**稍後所述的工作中[複製現有的虛擬桌面範本](#copy-an-existing-virtual-desktop-template)。  
  
    範本會在 HYPER-V 中，開啟**VM Connect**視窗中，並自動登入使用內建的 Administrator 帳戶執行。  
  
5.  此時您可以安裝應用程式和軟體更新、 變更設定，並更新系統管理員設定檔。 對範本的內建的系統管理員設定檔的所有變更將會都複製到預設使用者設定檔，在 從範本建立虛擬桌面站台。  
  
    如果您要連接您的站台的網域，我們建議您建立本機使用者帳戶，並將它新增至本機 Administrators 群組中，在自訂期間。  
  
    > [!NOTE]  
    > 如果要自訂範本時，重新啟動系統，自動登入使用內建的 Administrator 帳戶可能會在系統重新啟動後失敗。 若要解決這個問題，以手動方式上使用您所建立的本機系統管理員帳戶的記錄檔變更內建的 Administrator 帳戶的密碼、 登出，並再重新登入使用內建的 Administrator 帳戶和新的密碼。 （您必須刪除當您使用本機系統管理員帳戶登入所建立的設定檔。）  
  
6.  您完成設定您的系統之後，按兩下**CompleteCustomization**上執行 Sysprep 並關閉範本的系統管理員的桌面捷徑。 進行自訂時，Sysprep 工具會移除所有的唯一系統資訊，以準備 Windows 安裝映像。  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>從範本建立的虛擬機器桌面  
使用您的虛擬桌面範本設定您想要您已準備好開始建立虛擬桌面桌上型電腦的方式。 連接到 MultiPoint Server 電腦的每個站台時，會建立虛擬桌面。 下次使用者登入站台，他們會看到虛擬桌面，而不是工作階段為基礎的桌面顯示之前。  
  
> [!NOTE]  
> 在 MultiPoint 伺服器時，此程序僅適用於*站台模式*。 如果系統處於*主控台模式*，您可以從 MultiPoint 管理員切換到站台模式。 如果您使用多點的預設設定，您也可以啟動站台模式重新啟動電腦。 根據預設，MultiPoint Server 的電腦永遠會啟動在 站台模式  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>若要建立您的站台的虛擬桌面  
  
1.  登入 （例如，從使用遠端桌面連線在 Windows 電腦） 到遠端站台從 Windows MultiPoint server 使用本機系統管理員帳戶或，請在網域中的網域帳戶本機系統管理員群組中。  
  
    > [!NOTE]  
    > 或者，您可以使用本機站台伺服器登入。 不過，當您建立站台的虛擬桌面時，您必須先登出您用來建立虛擬桌面，才能連接至新的虛擬桌上型電腦的其他站台的站台。  
  
2.  從**啟動**畫面上，開啟 MultiPoint 管理員。  
  
3.  如果電腦處於主控台模式時，切換至 站台模式：  
  
    1.  在 **首頁**索引標籤上，按一下**切換至 站台模式**。  
  
    2.  當電腦重新啟動時，系統管理員身分登入。  
  
4.  按一下 [**虛擬桌面**] 索引標籤。  
  
5.  選取您想要使用的電台，請按一下虛擬桌面範本**建立虛擬桌面站台**，然後按一下**確定**。  
  
工作完成時，每個本機站台會連接至虛擬機器為基礎的虛擬桌面。  
  
> [!NOTE]  
> 如果任何本機站台的使用者帳戶登入，您必須登出工作階段，以取得連線到其中的新建立的站台的虛擬桌面站台。  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>複製現有的虛擬桌面範本  
您可以使用下列程序來建立複製現有的虛擬桌面範本，您可以自訂，並使用。 這可能是適用於下列情況：  
  
-   若要使您可以從主要的範本建立虛擬桌面站台，請從 MultiPoint Server 的主機電腦上的網路共用複製主要的範本。  
  
-   若要建立一份目前正在使用，以便您可以建立額外的自訂範本。  
  
##### <a name="to-import-a-virtual-desktop-template"></a>若要匯入虛擬桌面範本  
  
1.  以系統管理員身分登入 MultiPoint server。  
  
2.  從**啟動**畫面上，開啟 MultiPoint 管理員。  
  
3.  按一下 [**虛擬桌面**] 索引標籤。  
  
4.  按一下 **匯入虛擬桌面範本**，並使用**瀏覽**以選取您要匯入的.vhd 檔案 （範本）。 當您匯入範本時，原始.vhd 建立副本。 根據預設，MultiPoint 服務將.vhd 檔案儲存在 c:\\使用者\\公用\\文件\\Hyper-v\-V\\虛擬硬碟\\資料夾。  
  
5.  如需新的範本中，輸入前置詞，然後按一下**確定**。  
  
6.  如果您要進一步進行自訂的本機範本，您可能會變更的首碼名稱遞增版本號碼的前置詞的結尾。 或者，如果您要匯入的主版範本，您可能想要加入預設前置詞名稱的結尾中的主要範本的版本。  
  
7.  工作完成時，您可以自訂範本，或使用它，因為它要建立站台。  
