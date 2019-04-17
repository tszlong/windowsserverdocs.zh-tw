---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: "AD FS 中存取控制原則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>在 Windows Server 2016 AD FS 存取控制原則

>適用於：Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>AD FS 中存取控制原則範本  
Active Directory 同盟服務現在支援使用存取控制原則範本。  藉由使用存取控制原則範本，系統管理員可以交給依賴派對每秒（要求數量）的群組原則範本執行原則設定。 系統管理員也可以原則範本進行更新，並所做的變更會套用至信賴的派對自動如果不還有任何需要的使用者互動。  
  
## <a name="what-are-access-control-policy-templates"></a>存取控制原則範本為何？  
AD FS 核心管線原則處理具有三個階段：發行驗證、授權及理賠要求。 目前，AD FS 管理員已經分開這三個階段中的每個設定原則。  這也包含了解這些原則的影響，這些原則如果有間相依性。 此外，系統管理員需要了解理賠要求規則語言和作者自訂規則，可讓某些的簡單日通用原則 (ex。 封鎖外部存取）。  
  
系統管理員必須設定發行授權規則使用此舊模型範本執行哪些存取控制原則會取代宣告語言。  舊 PowerShell cmdlet 發行授權規則仍然適用的選項，但它互相不包括的新模型。 系統管理員可以選擇要使用的新模型或舊的模型。  新型號讓系統管理員權限授與，包括執行多因素驗證的時機控制。  
  
存取控制原則範本使用允許模型。  預設，這表示，任何人有存取和必須明確授權的存取。  不過，這是不只是所有或執行任何動作允許。  系統管理員可以新增例外允許規則。  例如，系統管理員的身分可能會想要權限授與根據選取此選項，然後指定 IP 位址範圍特定的網路。  但系統管理員可能會新增及例外，例如，系統管理員可能會新增例外特定網路從指定 IP 位址範圍。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>建存取控制原則範本與自訂存取控制原則範本  
AD FS 提供了許多建存取控制原則。  這些目標一些常見案例，其中有相同的一組原則需求，例如 client 存取原則的 Office 365。  這些範本便無法修改。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
為了提供來處理您的企業需要提高的彈性，系統管理員可以建立自己的存取原則範本。  這些可以修改後建立和自訂原則範本的變更會套用到所有每秒要求數量的會受這些原則範本。  若要新增自訂原則範本只要按一下新增存取控制原則從中 AD FS 管理。  
  
若要建立的原則範本，系統管理員必須先指定會權杖發行及/或委派授權要求下方的條件。 下表中會顯示條件與控制項目選項。   以粗體顯示條件不同或新增值的系統管理員，可以進一步設定。 如果有的話，系統管理員也可以指定例外。 當符合的條件時，例外指定的時，將不會觸發允許的動作，並要求符合例外指定的條件。  
  
|讓使用者|除了| 
| --- | --- | 
 |從**特定**網路|從**特定**網路<br /><br />從**特定**群組<br /><br />裝置從**特定**信任層級<br /><br />使用**特定**在要求中的宣告|  
|從**特定**群組|從**特定**網路<br /><br />從**特定**群組<br /><br />裝置從**特定**信任層級<br /><br />使用**特定**在要求中的宣告|  
|裝置從**特定**信任層級|從**特定**網路<br /><br />從**特定**群組<br /><br />裝置從**特定**信任層級<br /><br />使用**特定**在要求中的宣告|  
|使用**特定**在要求中的宣告|從**特定**網路<br /><br />從**特定**群組<br /><br />裝置從**特定**信任層級<br /><br />使用**特定**在要求中的宣告|  
|並要求多因素驗證|從**特定**網路<br /><br />從**特定**群組<br /><br />裝置從**特定**信任層級<br /><br />使用**特定**在要求中的宣告|  
  
如果系統管理員的身分選取多個條件，則的**與**的關係。 控制項是互相專屬，您僅能原則規則選擇一個動作。 如果系統管理員選取多個例外，則的**或者**的關係。 有幾個原則規則範例如下所示：  
  
|**原則**|**原則規則**|
| --- | --- |  
|您必須 MFA 外部網路的存取<br /><br />允許所有使用者|**規則 #1**<br /><br />從**外部網路**<br /><br />使用 MFA<br /><br />允許<br /><br />**#2 規則**<br /><br />從**內部網路**<br /><br />允許|  
|不允許外部存取以外非項目<br /><br />允許的項目加入的工作地點裝置上的內部網路存取權|**規則 #1**<br /><br />從**外部網路**<br /><br />從**未項目**群組<br /><br />允許<br /><br />**#2 規則**<br /><br />從**內部網路**<br /><br />從**加入的工作地點**裝置<br /><br />從**項目**群組<br /><br />允許|  
|外部網路存取需要 MFA 以外」服務系統管理員」<br /><br />允許所有使用者存取|**規則 #1**<br /><br />從**外部網路**<br /><br />使用 MFA<br /><br />允許<br /><br />除了**系統管理員的服務群組**<br /><br />**#2 規則**<br /><br />隨時<br /><br />允許|  
|從外部網路存取非工作地點結合的裝置需要 MFA<br /><br />允許 AD fabric 內部和外部網路的存取權|**規則 #1**<br /><br />從**內部網路**<br /><br />從**AD Fabric**群組<br /><br />允許<br /><br />**#2 規則**<br /><br />從**外部網路**<br /><br />從**未地點加入**裝置<br /><br />從**AD Fabric**群組<br /><br />使用 MFA<br /><br />允許<br /><br />**#3 規則**<br /><br />從**外部網路**<br /><br />從**加入的工作地點**裝置<br /><br />從**AD Fabric**群組<br /><br />允許|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>參數型的原則範本與非參數化原則範本  
可以存取控制原則  
  
參數型的原則範本是參數原則範本。 需要輸入的參數值當此範本指派給 RPs.An 管理員無法變更參數型的原則範本之後已建立系統管理員。  參數型原則的一個範例是建原則允許特定的群組。  這項原則套用到資源點數時，只要需要此參數指定。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
非參數化原則範本是不需要參數原則範本。 系統管理員可以將此範本以每秒要求數量指派不需要任何輸入，並可變更的非參數化原則範本之後已建立。  一個範例是建原則，讓每個人，需要 MFA。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>如何建立非參數化存取控制原則  
若要建立的非參數化存取控制原則使用下列程序  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>若要建立非參數化存取控制原則  
  
1.  從左邊 AD FS 管理選取存取控制原則，按一下 [權限存取控制原則。  
  
2.  輸入名稱與描述。  例如：允許的已驗證的裝置的使用者。  
  
3.  在**符合下列規則的任何允許存取**，按一下 [**新增]**。  
  
4.  允許，在將核取方塊中旁邊**的特定信任層級裝置**  
  
5.  在下方，選取 [底線**特定**  
  
6.  Pop 接視窗中，選取 [**驗證**下拉式清單中。  按一下**[確定]**。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  按一下**[確定]**。 按一下**[確定]**。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>如何建立參數的存取控制原則  
若要建立參數的存取控制原則使用下列程序  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要建立參數的存取控制原則  
  
1.  從左邊 AD FS 管理選取存取控制原則，按一下 [權限存取控制原則。  
  
2.  輸入名稱與描述。  例如：允許的特定理賠要求的使用者。  
  
3.  在**符合下列規則的任何允許存取**，按一下 [**新增]**。  
  
4.  允許，在將核取方塊中旁邊**的特定宣告在要求中**  
  
5.  在下方，選取 [底線**特定**  
  
6.  Pop 接視窗中，選取 [**參數指定指派的存取控制原則時**。  按一下**[確定]**。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  按一下**[確定]**。 按一下**[確定]**。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>如何建立自訂存取控制原則例外  
若要建立存取控制例外原則，請使用下列程序。  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>建立自訂存取控制原則例外  
  
1.  從左邊 AD FS 管理選取存取控制原則，按一下 [權限存取控制原則。  
  
2.  輸入名稱與描述。  例如：允許使用者與驗證的裝置，但不是受管理。  
  
3.  在**符合下列規則的任何允許存取**，按一下 [**新增]**。  
  
4.  允許，在將核取方塊中旁邊**的特定信任層級裝置**  
  
5.  在下方，選取 [底線**特定**  
  
6.  Pop 接視窗中，選取 [**驗證**下拉式清單中。  按一下**[確定]**。  
  
7.  在除外在發生核取方塊旁邊**的特定信任層級裝置**  
  
8.  在底部在除外，請選取底線**特定**  
  
9. Pop 接視窗中，選取 [**受管理的**下拉式清單中。  按一下**[確定]**。  
  
10. 按一下**[確定]**。 按一下**[確定]**。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>如何建立自訂存取控制原則，使用多個允許條件  
若要建立存取控制原則，使用多個允許條件使用下列程序  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要建立參數的存取控制原則  
  
1.  從左邊 AD FS 管理選取存取控制原則，按一下 [權限存取控制原則。  
  
2.  輸入名稱與描述。  例如：允許的特定理賠要求和特定群組的使用者。  
  
3.  在**符合下列規則的任何允許存取**，按一下 [**新增]**。  
  
4.  允許，在將核取方塊中旁邊**的特定群組**和**的特定宣告在要求中**  
  
5.  在下方，選取 [底線**特定**群組旁的第一個條件，針對  
  
6.  Pop 接視窗中，選取 [**參數指定指派原則是在**。  按一下**[確定]**。  
  
7.  在下方，選取 [底線**特定**旁邊宣告第二個條件，針對  
  
8.  Pop 接視窗中，選取 [**參數指定指派的存取控制原則時**。  按一下**[確定]**。  
  
9. 按一下**[確定]**。 按一下**[確定]**。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>如何將存取控制原則指派給新的應用程式  
將存取控制原則指派給新的應用程式相當直接易懂且已經現在整合至精靈將新增資源點數。  可以廠商信任精靈中，您可以選擇您想要指派的存取控制原則。  建立新的依賴廠商信任時，這是需求。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>如何將現有的應用程式存取控制原則  
指派的存取控制原則只要選取現有的應用程式應用程式從可以信任派對和按右鍵**編輯存取控制項原則**。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
從您可以選取存取控制原則和套用到應用程式。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>也了  
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md) 

