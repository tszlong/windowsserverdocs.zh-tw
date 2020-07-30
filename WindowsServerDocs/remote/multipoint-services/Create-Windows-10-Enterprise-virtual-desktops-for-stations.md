---
title: 建立站台的 Windows 10 企業版虛擬桌面
description: 瞭解如何建立適用于工作站的 Windows Server 2016 桌上型電腦
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: 6f6d7d3ef66e8943fbb39cfd96cff1b91ab413eb
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409819"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>建立站台的 Windows 10 企業版虛擬桌面
MultiPoint 服務中的這項選擇性設定主要適用于基本應用程式需要每個使用者自己的用戶端作業系統實例的情況。 範例包括無法安裝在 Windows Server 上的應用程式，以及不會在同一部主機電腦上執行多個實例的應用程式。

> [!NOTE]
> 這些虛擬桌面（也稱為 VDI）比預設的 MultiPoint 服務桌面會話更耗費資源，因此我們建議您盡可能使用預設的 MultiPoint 服務會話。

## <a name="prerequisites"></a>先決條件
若要準備建立站虛擬桌面，請確定您的 MultiPoint 服務系統符合下列需求：

| 硬體 | 需求 |
|--|--|
| CPU （多媒體） | 1個核心或每個虛擬機器的執行緒 |
| 固態硬碟（SSD） | 容量 >= 每一站 20 gb + 40GB，適用于 MultiPoint 服務主機作業系統<p>隨機讀取/寫入 IOPS >= 每台工作站3K |
| RAM | Windows MultiPoint Server 主機作業系統的每個工作站 2GB + 2GB |
| 圖形 | DX11 |
| BIOS | 設定為啟用虛擬化的 BIOS CPU 設定-第二層位址轉譯（SLAT） |

-   **工作站**-設定 MultiPoint 服務系統的工作站。 如需詳細資訊，請參閱[將其他工作站附加至 MultiPoint 服務](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。

-   **網域**-在網域環境中，Windows MultiPoint Server 電腦已加入網域，而且網域使用者已新增至 MultiPoint 服務主機作業系統上的本機系統管理員群組。

## <a name="procedures"></a>程序
請使用下列程序來：

-   [建立虛擬桌面的範本](#create-a-template-for-virtual-desktops)

-   [從範本建立虛擬桌面](#create-virtual-machine-desktops-from-the-template)

-   [複製現有的虛擬桌面範本](#copy-an-existing-virtual-desktop-template)

### <a name="create-a-template-for-virtual-desktops"></a>建立虛擬桌面的範本
在您可以建立虛擬桌面的範本之前，您必須先在 MultiPoint Server 中啟用虛擬桌面功能。

##### <a name="to-enable-the-virtual-desktop-feature"></a>啟用虛擬桌面功能

1.  以本機系統管理員帳戶或在網域中，以本機 Administrators 群組成員的網域帳戶登入 MultiPoint Server 主機作業系統。

2.  從 [**開始**] 畫面中，開啟 [MultiPoint 管理員]。

3.  按一下 [**虛擬桌面**] 索引標籤，按一下 [**啟用虛擬桌面**]，然後按一下 **[確定]**，並等候系統重新開機。

下一步是建立虛擬桌面範本。 您實際上是建立虛擬硬碟（VHD）檔案，您可以使用此檔案作為範本，以建立適用于 MultiPoint 管理員的工作站虛擬桌面。 您可以使用適用于 Windows 或的實體安裝媒體。做為範本來源的 ISO 影像檔案。 您也可以使用。Windows 安裝的 VHD。 請注意，若要使用實體安裝光碟，您必須先插入光碟，然後再啟動精靈。

##### <a name="to-create-a-virtual-desktop-template"></a>建立虛擬桌面範本

1.  使用本機系統管理員帳戶或在網域中，以本機 Administrators 群組成員的網域帳戶登入 MultiPoint Server 主機作業系統。

2.  從 [**開始**] 畫面中，開啟 [MultiPoint 管理員]。

3.  按一下 [**虛擬桌面**] 索引標籤。

4.   將 Windows 10 企業版 .iso 檔案複製到本機 SSD。

5.  在 [虛擬桌面] 索引標籤上，按一下 [**建立虛擬桌面範本]。**

6.  在 [**前置**詞] 中，輸入用來識別範本的前置詞，以及使用範本建立的虛擬桌面。 預設的前置詞是主電腦名稱稱。

    首碼是用來命名範本和虛擬桌面站台。 此範本將會 <*前置*詞>-t。 虛擬桌面工作站會命名 <前置詞*prefix* >- *n*，其中*n*是工作站識別碼。

7.  輸入使用者名稱和密碼，以用於範本的本機系統管理員帳戶。 在 [網域] 中，輸入將新增到本機系統管理員群組的網域帳號憑證。 此帳戶可以用來登入範本，以及從範本建立的所有虛擬桌面工作站。

8. 按一下 **[確定]**，並等候範本建立完成。

9. 新範本會列在 [**虛擬桌面**] 索引標籤上。此範本將會關閉。

下一個步驟是使用您想要在虛擬桌面電腦上的軟體和設定來設定範本。 您必須先執行此動作，才能從範本建立任何虛擬桌面。

##### <a name="to-customize-a-virtual-desktop-template"></a>自訂虛擬桌面範本

1.  以本機系統管理員帳戶或在網域中，以本機 Administrators 群組中的網域帳戶登入 MultiPoint server 主機作業系統。

2.  從 [**開始**] 畫面中，開啟 [MultiPoint 管理員]。

3.  按一下 [**虛擬桌面**] 索引標籤。

4.  選取您要自訂的範本，按一下 [**自訂範本**]，然後按一下 **[確定]**。

    > [!NOTE]
    > 只有尚未用來建立虛擬桌面工作站的範本可供使用。 如果您想要更新已在使用中的範本，您必須使用匯**入範本**工作（稍後在[複製現有的虛擬桌面範本](#copy-an-existing-virtual-desktop-template)中所述）來建立範本的複本。

    範本會在 [Hyper-v **VM**連線] 視窗中開啟，並使用內建的系統管理員帳戶來執行自動登入。

5.  此時，您可以安裝應用程式和軟體更新、變更設定，以及更新系統管理員設定檔。 對範本內建系統管理員設定檔所做的所有變更，都會複製到虛擬桌面工作站中從範本建立的預設使用者設定檔。

    如果您要將您的工作站連接到網域，建議您在自訂期間建立本機使用者帳戶，並將它新增至本機系統管理員群組。

    > [!NOTE]
    > 如果在自訂範本時系統重新開機，則在系統重新開機之後，使用內建的系統管理員帳戶自動登入可能會失敗。 若要解決此問題，請使用您建立的本機系統管理員帳戶手動登入、變更內建系統管理員帳戶的密碼、登出，然後使用內建的系統管理員帳戶和新密碼重新登入。 （當您使用本機系統管理員帳戶登入時，您必須刪除已建立的設定檔）。

6.  在您完成系統的設定之後，請按兩下管理員桌面上的 [ **CompleteCustomization** ] 快捷方式來執行 Sysprep，然後關閉範本。 在自訂期間，Sysprep 工具會移除所有唯一的系統資訊，以準備要製作映射的 Windows 安裝。

### <a name="create-virtual-machine-desktops-from-the-template"></a>從範本建立虛擬機器桌面
當您的虛擬桌面範本設定您想要的桌上型電腦時，您就可以開始建立虛擬桌面。 系統會針對連接到 MultiPoint 伺服器電腦的每個工作站建立虛擬桌面。 下次使用者登入工作站時，他們會看到虛擬桌面，而不是之前顯示的會話型桌面。

> [!NOTE]
> 只有當 MultiPoint Server 處於*工作站模式*時，此程式才有作用。 如果系統處於*主控台模式*，您可以從 MultiPoint 管理員切換到站模式。 如果您使用預設的 MultiPoint 設定，您也可以藉由重新開機電腦來啟動站模式。 根據預設，MultiPoint 伺服器電腦一律會以站模式啟動

##### <a name="to-create-virtual-desktops-for-your-stations"></a>為您的工作站建立虛擬桌面

1.  使用本機系統管理員帳戶，或在網域中的本機 Administrators 群組中的網域帳戶，從遠端工作站（例如，使用遠端桌面連線的 Windows 電腦）登入 Windows MultiPoint server。

    > [!NOTE]
    > 或者，您也可以使用本機工作站登入伺服器。 不過，當您建立站虛擬桌面時，必須登出用來建立虛擬桌面的工作站，才能將其他工作站連接到新的虛擬桌面。

2.  從 [**開始**] 畫面中，開啟 [MultiPoint 管理員]。

3.  如果電腦處於主控台模式，請切換至站模式：

    1.  在 [**首頁**] 索引標籤上，按一下 [**切換到工作站模式]**。

    2.  當電腦重新開機時，以系統管理員身分登入。

4.  按一下 [**虛擬桌面**] 索引標籤。

5.  選取您要與工作站搭配使用的虛擬桌面範本，然後按一下 [**建立虛擬桌面工作站**]，再按一下 **[確定]**。

當工作完成時，每個本機工作站會連線到以虛擬機器為基礎的虛擬桌面。

> [!NOTE]
> 如果使用者帳戶已登入任何本機工作站，您就必須登出會話，讓工作站連接到其中一個新建立的工作站虛擬桌面。

### <a name="copy-an-existing-virtual-desktop-template"></a>複製現有的虛擬桌面範本
使用下列程式，建立您可以自訂和使用的現有虛擬桌面範本複本。 在下列情況下會非常有用：

-   若要從網路共用將主要範本複製到 MultiPoint Server 主機電腦，讓虛擬桌面工作站可以從主要範本建立。

-   建立目前使用中的範本複本，讓您可以進行其他自訂。

##### <a name="to-import-a-virtual-desktop-template"></a>若要匯入虛擬桌面範本

1.  以系統管理員身分登入 MultiPoint 伺服器。

2.  從 [**開始**] 畫面中，開啟 [MultiPoint 管理員]。

3.  按一下 [**虛擬桌面**] 索引標籤。

4.  按一下 [匯**入虛擬桌面範本**]，然後使用 **[流覽]** 來選取您要匯入的 .vhd 檔案（範本）。 當您匯入範本時，會建立原始 .vhd 的複本。 根據預設，MultiPoint 服務會將 .vhd 檔案儲存在 C： \\ Users 公用檔的 [ \\ \\ \\ hyper-v \- \\ 虛擬硬碟] \\ 資料夾中。

5.  輸入新範本的前置詞，然後按一下 **[確定]**。

6.  如果您要對本機範本進行進一步的自訂，您可以在前置詞的結尾遞增版本號碼，藉以變更前置詞名稱。 或者，如果您要匯入主要範本，您可能會想要將版本的主要範本新增到預設前置詞名稱的結尾。

7.  當工作完成時，您可以自訂範本，或使用它來建立工作站。
