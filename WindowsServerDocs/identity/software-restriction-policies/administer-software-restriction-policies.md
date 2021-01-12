---
title: 管理軟體限制原則
description: 從 Windows Server 2008 和 Windows Vista 開始，瞭解如何使用軟體限制原則來管理應用程式控制原則 (SRP) 。
ms.topic: article
ms.assetid: 8cc22093-67d1-47b6-9ddd-4569b6761ce9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 84667034011ee655720e14f370fcd338e29dc98b
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112934"
---
# <a name="administer-software-restriction-policies"></a>管理軟體限制原則

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題適用于 IT 專業人員，其中包含的程式如何使用軟體限制原則來管理應用程式控制原則 (SRP) 從 Windows Server 2008 和 Windows Vista 開始。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些都與 Microsoft Active Directory Domain Services 和群組原則整合，但也可以在獨立電腦上進行設定。 如需 SRP 的詳細資訊，請參閱 [軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，您可以使用 Windows AppLocker 來取代或搭配 SRP 使用，以取得應用程式控制策略的一部分。

本主題包含：

-   [開啟軟體限制原則](#BKMK_Open_SRP)

-   [建立新的軟體限制原則](#BKMK_Create_SRP)

-   [新增或刪除指定的檔案類型](#BKMK_Add_Del)

-   [防止軟體限制原則套用至本機系統管理員](#BKMK_Prevent_Admin)

-   [變更軟體限制原則的預設安全性等級](#BKMK_Sec_Lvl)

-   [將軟體限制原則套用到 Dll](#BKMK_Apply_SRP_DLLs)

如需如何使用 SRP 完成特定工作的詳細資訊，請參閱下列各項：

-   [判斷軟體限制原則的 Allow-Deny 清單和應用程式清查](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

-   [使用軟體限制原則協助保護您的電腦免于電子郵件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="to-open-software-restriction-policies"></a><a name="BKMK_Open_SRP"></a>開啟軟體限制原則

-   [針對本機電腦](#BKMK_1)

-   [如果是網域、網站或組織單位，而且您是在成員伺服器上，或在已加入網域的工作站上](#BKMK_2)

-   [如果是網域或組織單位，而且您是在網域控制站上，或是安裝了遠端伺服器管理工具的工作站上](#BKMK_3)

-   [若是網站，且您是在網域控制站上，或在已安裝遠端伺服器管理工具的工作站上](#BKMK_4)

### <a name="for-your-local-computer"></a><a name="BKMK_1"></a>針對本機電腦

1.  開啟 [本機安全性設定]。

2.  在主控台樹中，按一下 [ **軟體限制原則**]。

    **：?**

    -   安全性設定/軟體限制原則

> [!NOTE]
> 若要執行此程序，您必須是本機電腦上的 Administrators 群組成員或是已經委派您適當的權限。

### <a name="for-a-domain-site-or-organizational-unit-and-you-are-on-a-member-server-or-on-a-workstation-that-is-joined-to-a-domain"></a><a name="BKMK_2"></a>如果是網域、網站或組織單位，而且您是在成員伺服器上，或在已加入網域的工作站上

1.  開啟 Microsoft Management Console (MMC)。

2.  **在 [檔案**] 功能表上，按一下 [**新增/移除嵌入式管理單元**]，然後按一下 [**新增**]。

3.  按一下 [本機群組原則物件編輯器]，然後按一下 [新增]。

4.  在 [選取群組原則物件] 中，按一下 [瀏覽]。

5.  在 **[流覽群組原則物件]** 中，選取適當網域、網站或組織單位中 (GPO) 的群組原則物件，或建立新的物件，然後按一下 **[完成]**。

6.  按一下 [關閉]，然後按一下 [確定]。

7.  在主控台樹中，按一下 [ **軟體限制原則**]。

    **：?**

    -   *群組原則物件* [*ComputerName*] 原則/電腦設定或

        使用者設定/Windows 設定/安全性設定/軟體限制原則

> [!NOTE]
> 您必須是 Domain Admins 群組的成員，才可以執行這個程序。

### <a name="for-a-domain-or-organizational-unit-and-you-are-on-a-domain-controller-or-on-a-workstation-that-has-the-remote-server-administration-tools-installed"></a><a name="BKMK_3"></a>如果是網域或組織單位，而且您是在網域控制站上，或是安裝了遠端伺服器管理工具的工作站上

1.  開啟群組原則管理主控台。

2.  在主控台樹中，以滑鼠右鍵按一下您想要開啟軟體限制原則的群組原則物件 (GPO) 。

3.  按一下 [編輯]，開啟要編輯的 GPO。 您也可以按一下 [新增] 來建立新的 GPO，然後按一下 [編輯]。

4.  在主控台樹中，按一下 [ **軟體限制原則**]。

    **：?**

    -   *群組原則物件* [*ComputerName*] 原則/電腦設定或

        使用者設定/Windows 設定/安全性設定/軟體限制原則

> [!NOTE]
> 您必須是 Domain Admins 群組的成員，才可以執行這個程序。

### <a name="for-a-site-and-you-are-on-a-domain-controller-or-on-a-workstation-that-has-the-remote-server-administration-tools-installed"></a><a name="BKMK_4"></a>若是網站，且您是在網域控制站上，或在已安裝遠端伺服器管理工具的工作站上

1.  開啟群組原則管理主控台。

2.  在主控台樹中，以滑鼠右鍵按一下您想要設定群組原則的網站。

    **：?**

    -   Active Directory 網站和服務 [*Domain_Controller_Name. Domain_Name*]/Sites/Site

3.  按一下 **群組原則物件連結** ] 中的專案，以選取現有的群組原則物件 (GPO) ，然後按一下 [ **編輯**]。 您也可以按一下 [新增] 來建立新的 GPO，然後按一下 [編輯]。

4.  在主控台樹中，按一下 [ **軟體限制原則**]。

    **Where**

    -   *群組原則物件* [*ComputerName*] 原則/電腦設定或

        使用者設定/Windows 設定/安全性設定/軟體限制原則

> [!NOTE]
> -   若要執行此程序，您必須是本機電腦上的 Administrators 群組成員或是已經委派您適當的權限。 如果該電腦已加入網域，則 Domain Admins 群組的成員便可以執行這項程序。
> -   若要設定將套用到電腦的原則設定，不論哪些使用者登入電腦，請按一下 [ **電腦** 設定]。
> -   若要設定將套用至使用者的原則設定，不論使用者登入哪一台電腦，請按一下 [ **使用者** 設定]。

## <a name="to-create-new-software-restriction-policies"></a><a name="BKMK_Create_SRP"></a>建立新的軟體限制原則

1.  開啟 [軟體限制原則]。

2.  在 [執行] 功能表上，按一下 [新軟體限制原則]。

> [!WARNING]
> -   視您的環境而定，執行此程序所需的系統管理認證會有所不同：
>
>     -   如果您為本機電腦建立新的軟體限制原則：若要完成此程式，至少需要本機 **Administrators** 群組的成員資格或同等許可權。
>     -   如果您為已加入網域的電腦建立新軟體限制原則，則 Domain Admins 群組的成員便可執行此程序。
> -   如果已為群組原則物件 (GPO) 建立軟體限制原則，則 [新軟體限制原則] 命令不會顯示於 [執行] 功能表上。 若要刪除已套用至 GPO 的軟體限制原則，在主控台樹狀目錄中，以滑鼠右鍵按一下 [軟體限制原則]，然後按一下 [刪除軟體限制原則]。 當您刪除 GPO 的軟體限制原則時，也會刪除該 GPO 的所有軟體限制原則規則。 刪除軟體限制原則之後，您可為該 GPO 建立新軟體限制原則。

## <a name="to-add-or-delete-a-designated-file-type"></a><a name="BKMK_Add_Del"></a>新增或刪除指定的檔案類型

1.  開啟 [軟體限制原則]。

2.  在詳細資料窗格中，按兩下 [指定的檔案類型]。

3.  執行下列其中一個動作：

    -   若要新增檔案類型，在 [副檔名] 中，輸入副檔名，然後按一下 [新增]。

    -   若要刪除檔案類型，在 [指定的檔案類型] 中，按一下檔案類型，然後按一下 [移除]。

> [!NOTE]
> -   視您新增或刪除指定的檔案類型的環境而定，執行此程序所需的系統管理認證會有所不同：
>
>     -   如果您為本機電腦新增或刪除指定的檔案類型：若要完成此程式，至少需要本機 **Administrators** 群組的成員資格或同等許可權。
>     -   如果您為已加入網域的電腦建立新軟體限制原則，則 Domain Admins 群組的成員便可執行此程序。
> -   如果您尚未為群組原則物件 (GPO) 建立新軟體限制原則設定，可能需要進行這項動作。
> -   指定檔案類型清單是由電腦設定的所有規則和 GPO 的使用者設定所共用。

## <a name="to-prevent-software-restriction-policies-from-applying-to-local-administrators"></a><a name="BKMK_Prevent_Admin"></a>防止軟體限制原則套用至本機系統管理員

1.  開啟 [軟體限制原則]。

2.  在詳細資料窗格中，按兩下 [強制]。

3.  在 [將軟體限制原則套用到下列使用者] 下，按一下 [本機系統管理員之外的所有使用者]。

> [!WARNING]
> -   您至少必須有本機 **Administrators** 群組的成員資格或同等權限，才能完成此程序。
> -   如果您尚未為群組原則物件 (GPO) 建立新軟體限制原則設定，可能需要進行這項動作。
> -   如果使用者成為組織中電腦的本機 Administrators 群組成員是很常見的，就不需要啟用這個選項。
> -   如果您是為本機電腦定義軟體限制原則設定，可使用此程序以防止在本機系統管理員套用軟體限制原則。 如果您要定義網路的軟體限制原則設定，請根據安全性群組中的成員資格，根據群組原則來篩選使用者原則設定。

## <a name="to-change-the-default-security-level-of-software-restriction-policies"></a><a name="BKMK_Sec_Lvl"></a>變更軟體限制原則的預設安全性等級

1.  開啟 [軟體限制原則]。

2.  在詳細資料窗格中，按兩下 [安全性等級]。

3.  在想設定為預設的安全性等級上按一下滑鼠右鍵，然後按一下 [設定成預設值]。

> [!CAUTION]
> 在特定的目錄中，若將預設的安全性等級設定成 [不允許]，對作業系統會有不良影響。

> [!NOTE]
> -   視您變更軟體限制原則預設安全性等級的環境而定，執行此程序所需的系統管理認證會有所不同。
> -   如果您尚未為此群組原則物件 (GPO) 建立新軟體限制原則設定，可能需要進行這項動作。
> -   在詳細資料窗格中，會以有個核取記號的黑色圓形來表示目前預設的安全性等級。 如果您在目前預設的安全性等級上按一下滑鼠右鍵，[設定成預設值] 命令不會出現在功能表中。
> -   系統會建立軟體限制原則規則，以指定預設安全性等級的例外狀況。 當預設的安全性等級設定成 [沒有限制] 時，規則可指定不允許執行的軟體。 當預設的安全性等級設定成 [不允許] 時，規則可指定允許執行的軟體。
> -   安裝時，系統上所有檔案的軟體限制原則的預設安全性等級是設定成 [沒有限制]。

## <a name="to-apply-software-restriction-policies-to-dlls"></a><a name="BKMK_Apply_SRP_DLLs"></a>將軟體限制原則套用到 Dll

1.  開啟 [軟體限制原則]。

2.  在詳細資料窗格中，按兩下 [強制]。

3.  在 [將軟體限制原則套用到下列項目] 下，按一下 [所有軟體檔案]。

> [!NOTE]
> -   若要執行此程序，您必須是本機電腦上的 Administrators 群組成員或是已經委派您適當的權限。 如果該電腦已加入網域，則 Domain Admins 群組的成員便可以執行這項程序。
> -   根據預設，軟體限制原則不會檢查動態連結程式庫 (DLL)。 檢查 DLL 會降低系統效能，因為軟體限制原則必須在每次載入 DLL 時進行評估。 不過，如果您擔心感染到以 DLL 為攻擊目標的病毒，可決定是否檢查 DLL。 如果預設的安全性等級設定為 [不 **允許**]，而且您啟用了 dll 檢查，就必須建立允許每個 DLL 執行的軟體限制原則規則。


