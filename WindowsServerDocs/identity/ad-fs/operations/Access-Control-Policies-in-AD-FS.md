---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: AD FS Windows Server 2016 中的存取控制原則
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8c28d24484adf73b1a769a02b817ba48f591f194
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87863825"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Windows Server 2016 AD FS 中的存取控制原則

  
## <a name="access-control-policy-templates-in-ad-fs"></a>存取控制中的原則範本 AD FS  
Active Directory 同盟服務現在支援使用存取控制原則範本。  藉由使用存取控制原則範本，系統管理員可以藉由將原則範本指派給 (RPs) 的信賴憑證者群組來強制執行原則設定。 系統管理員也可以對原則範本進行更新，如果不需要使用者互動，則會自動將變更套用至信賴憑證者。  
  
## <a name="what-are-access-control-policy-templates"></a>什麼是存取控制原則範本？  
原則處理的 AD FS 核心管線有三個階段：驗證、授權和宣告發行。 目前，AD FS 系統管理員必須分別設定每個階段的原則。  這也牽涉到了解這些原則的含意，以及這些原則是否有相依性。 此外，系統管理員必須瞭解宣告規則語言，並撰寫自訂規則，以啟用一些簡單/通用原則 (例如。 封鎖外部存取) 。  
  
哪些存取控制原則範本會取代這個舊的模型，系統管理員必須使用宣告語言來設定發佈授權規則。  發佈授權規則的舊 PowerShell Cmdlet 仍然適用，但是它與新模型互斥。 系統管理員可以選擇使用新模型或舊模型。  新的模型可讓系統管理員控制授與存取權的時機，包括強制執行多重要素驗證。  
  
存取控制原則範本會使用允許模型。  這表示根據預設，沒有人可以存取，而且必須明確授與存取權。  不過，這不只是「全部」或「無」允許。  系統管理員可以將例外狀況新增至允許規則。  例如，系統管理員可能會想要根據特定網路來授與存取權，方法是選取此選項並指定 IP 位址範圍。  但系統管理員可能會新增和例外，例如，系統管理員可能會新增特定網路中的例外狀況，並指定該 IP 位址範圍。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>內建存取控制原則範本與自訂存取控制原則範本的比較  
AD FS 包含數個內建的存取控制原則範本。  這些是以一組相同原則需求的常見案例為目標，例如 Office 365 的用戶端存取原則。  這些範本無法修改。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
為了提供更大的彈性來滿足您的商務需求，系統管理員可以建立自己的存取原則範本。  這些可以在建立之後修改，而且自訂原則範本的變更會套用到這些原則範本所控制的所有 RPs。  若要新增自訂原則範本，只需按一下 [AD FS 管理] 內的 [新增存取控制原則] 即可。  
  
若要建立原則範本，系統管理員必須先指定要求將獲得授權以進行權杖發行和/或委派的條件。 下表顯示條件和動作選項。   具有不同或新值的系統管理員可以進一步設定粗體的條件。 系統管理員也可以指定例外狀況（如果有的話）。 當符合條件時，如果指定了例外狀況，且傳入要求符合例外狀況中指定的條件，則不會觸發允許動作。  
  
|允許使用者|Except| 
| --- | --- | 
 |從**特定**網路|從**特定**網路<p>從**特定**群組<p>從具有**特定**信任層級的裝置<p>在要求中使用**特定**宣告|  
|從**特定**群組|從**特定**網路<p>從**特定**群組<p>從具有**特定**信任層級的裝置<p>在要求中使用**特定**宣告|  
|從具有**特定**信任層級的裝置|從**特定**網路<p>從**特定**群組<p>從具有**特定**信任層級的裝置<p>在要求中使用**特定**宣告|  
|在要求中使用**特定**宣告|從**特定**網路<p>從**特定**群組<p>從具有**特定**信任層級的裝置<p>在要求中使用**特定**宣告|  
|和需要多重要素驗證|從**特定**網路<p>從**特定**群組<p>從具有**特定**信任層級的裝置<p>在要求中使用**特定**宣告|  
  
如果系統管理員選取多個條件，它們就是**和**關聯性。 動作是互斥的，而且對於一個原則規則，您只能選擇一個動作。 如果管理員選取多個例外狀況，它們會是**或**關聯性。 以下顯示幾個原則規則範例：  
  
|**原則**|**原則規則**|
| --- | --- |  
|外部網路存取需要 MFA<p>允許所有使用者|**規則 #1**<p>從**外部**網路<p>使用 MFA 的和<p>允許<p>**規則 # 2**<p>來自**內部**網路<p>允許|  
|不允許外部存取，但非 FTE 除外<p>允許已加入工作場所裝置上的 FTE 內部網路存取|**規則 #1**<p>從**外部**網路<p>和來自**非 FTE**群組<p>允許<p>**規則 #2**<p>來自**內部**網路<p>和來自已**加入工作場所**的裝置<p>和來自**FTE**群組<p>允許|  
|外部網路存取需要 MFA，但「服務管理員」除外<p>允許所有使用者存取|**規則 #1**<p>從**外部**網路<p>使用 MFA 的和<p>允許<p>**服務管理群組**除外<p>**規則 #2**<p>always<p>允許|  
|從外部網路存取的非工作位置聯結裝置需要 MFA<p>允許內部網路和外部網路存取 AD 網狀架構|**規則 #1**<p>來自**內部**網路<p>和來自**AD Fabric**群組<p>允許<p>**規則 #2**<p>從**外部**網路<p>和來自**未加入工作場所**的裝置<p>和來自**AD Fabric**群組<p>使用 MFA 的和<p>允許<p>**規則 #3**<p>從**外部**網路<p>和來自已**加入工作場所**的裝置<p>和來自**AD Fabric**群組<p>允許|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>參數化原則範本 vs 非參數化原則範本  
存取控制原則可以是  
  
參數化原則範本是具有參數的原則範本。 將此範本指派給 RPs.An 系統管理員時，系統管理員必須輸入這些參數的值，才能在建立參數化原則範本之後對其進行變更。  參數化原則的範例是內建原則 [允許特定群組]。  每當此原則套用至 RP 時，就必須指定此參數。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
非參數化原則範本是不具有參數的原則範本。 系統管理員可以將此範本指派給 RPs，而不需要任何輸入，而且可以在建立非參數化原則範本之後對其進行變更。  其中一個範例是內建原則，允許所有人和要求 MFA。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>如何建立非參數化存取控制原則  
若要建立非參數化的存取控制原則，請使用下列程式  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>建立非參數化存取控制原則  
  
1.  從左側的 AD FS 管理] 中選取 [存取控制原則]，然後按一下滑鼠右鍵的 [新增存取控制原則]。  
  
2.  輸入名稱和描述。  例如：允許使用者使用已驗證的裝置。  
  
3.  在 **[如果符合下列任何規則，允許存取**] 底下，按一下 [**新增**]。  
  
4.  在 [允許] 底下，勾選 [**來自具有特定信任層級的裝置**] 旁的核取方塊  
  
5.  在底部選取加底線的**特定**  
  
6.  從快顯視窗中，從下拉式選單選取 [**已驗證**]。  按一下 [確定] 。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  按一下 [確定] 。 按一下 [確定] 。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>如何建立參數化存取控制原則  
若要建立參數化存取控制原則，請使用下列程式  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>建立參數化存取控制原則  
  
1.  從左側的 AD FS 管理] 中選取 [存取控制原則]，然後按一下滑鼠右鍵的 [新增存取控制原則]。  
  
2.  輸入名稱和描述。  例如：允許具有特定宣告的使用者。  
  
3.  在 **[如果符合下列任何規則，允許存取**] 底下，按一下 [**新增**]。  
  
4.  在 [允許] 底下，勾選**要求中的特定宣告**旁的核取方塊  
  
5.  在底部選取加底線的**特定**  
  
6.  從快顯視窗中，選取 **[指派存取控制原則時所指定的參數**]。  按一下 [確定] 。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  按一下 [確定] 。 按一下 [確定] 。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>如何建立具有例外狀況的自訂存取控制原則  
若要建立具有例外狀況的存取控制原則，請使用下列程式。  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>若要建立具有例外狀況的自訂存取控制原則  
  
1.  從左側的 AD FS 管理] 中選取 [存取控制原則]，然後按一下滑鼠右鍵的 [新增存取控制原則]。  
  
2.  輸入名稱和描述。  例如：允許使用者使用已驗證的裝置，但未受管理。  
  
3.  在 **[如果符合下列任何規則，允許存取**] 底下，按一下 [**新增**]。  
  
4.  在 [允許] 底下，勾選 [**來自具有特定信任層級的裝置**] 旁的核取方塊  
  
5.  在底部選取加底線的**特定**  
  
6.  從快顯視窗中，從下拉式選單選取 [**已驗證**]。  按一下 [確定] 。  
  
7.  在 [例外] 底下的方塊中，核取 [**來自具有特定信任層級的裝置**] 旁的核取方塊  
  
8.  在 [例外] 底下的底部，選取加底線的**特定**  
  
9. 從快顯視窗中，從下拉式選單選取 [**管理**]。  按一下 [確定] 。  
  
10. 按一下 [確定] 。 按一下 [確定] 。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>如何建立具有多個允許條件的自訂存取控制原則  
若要建立具有多個允許條件的存取控制原則，請使用下列程式  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>建立參數化存取控制原則  
  
1.  從左側的 AD FS 管理] 中選取 [存取控制原則]，然後按一下滑鼠右鍵的 [新增存取控制原則]。  
  
2.  輸入名稱和描述。  例如：允許具有特定宣告和來自特定群組的使用者。  
  
3.  在 **[如果符合下列任何規則，允許存取**] 底下，按一下 [**新增**]。  
  
4.  在 [允許] 底下，勾選 [**來自特定群組**] 旁的方塊，並在**要求中使用特定宣告**  
  
5.  在底部，選取 [群組] 旁第一個條件的加底線的**特定**  
  
6.  從快顯視窗中，選取 **[指派原則時指定的參數**]。  按一下 [確定] 。  
  
7.  在底部，選取 [宣告] 旁第二個條件的加底線的**特定**  
  
8.  從快顯視窗中，選取 **[指派存取控制原則時所指定的參數**]。  按一下 [確定] 。  
  
9. 按一下 [確定] 。 按一下 [確定] 。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>如何將存取控制原則指派給新的應用程式  
將存取控制原則指派給新的應用程式相當簡單，而且現在已整合到 wizard 中以新增 RP。  在 [信賴憑證者信任] 中，您可以選取您想要指派的存取控制原則。  這是建立新的信賴憑證者信任時的必要條件。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>如何將存取控制原則指派給現有的應用程式  
將存取控制原則指派給現有的應用程式，只需從信賴憑證者信任選取應用程式，然後在滑鼠右鍵上按一下 [**編輯存取控制原則**]。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
您可以從這裡選取存取控制原則，並將它套用至應用程式。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../ad-fs-operations.md) 
