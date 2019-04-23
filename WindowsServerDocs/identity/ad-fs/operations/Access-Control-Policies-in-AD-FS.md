---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: AD FS 中的存取控制原則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861299"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Windows Server 2016 AD FS 中的存取控制原則

>適用於：Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>在 AD FS 存取控制原則範本  
Active Directory Federation Services 現在支援使用存取控制原則範本。  藉由使用存取控制原則範本，以系統管理員可以強制執行原則設定的原則範本指派給一群信賴憑證者的合作對象 (RPs)。 系統管理員也可以對 [原則] 範本進行更新，並會自動套用變更至信賴憑證者的合作對象是否需要任何使用者互動。  
  
## <a name="what-are-access-control-policy-templates"></a>存取控制原則範本是什麼？  
原則處理的 AD FS core 管線有三個階段： 驗證、 授權和宣告發行。 目前，AD FS 系統管理員必須個別針對每一個階段的設定原則。  這也包括了解這些原則的影響，而且如果這些原則可以間的相依性。 此外，系統管理員必須了解宣告規則語言和撰寫自訂規則，以啟用 （例如某些簡單/一般原則。 封鎖外部存取）。  
  
範本執行何種存取控制原則會取代這個舊的模型，其中系統管理員必須設定使用的發行授權規則的宣告語言。  舊的 PowerShell cmdlet 的發行授權規則仍然適用，但它是互斥的新模型。 系統管理員可以選擇使用新的模型或舊的模型。  新的模型可讓系統管理員控制何時要授與存取權，包括強制執行多重要素驗證。  
  
存取控制原則範本會使用允許的模型。  這表示根據預設，沒有人具有存取權，並必須被明確授與存取。  不過，這不只是 all，或不允許。  系統管理員可新增允許規則的例外狀況。  比方說，系統管理員可能想要授與存取權限的特定網路選取此選項，並指定 IP 位址範圍。  但系統管理員可能會加上例外狀況，比方說，系統管理員可能會從特定的網路上新增例外狀況，並指定該 IP 位址範圍。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>內建的存取控制原則範本與自訂存取控制原則範本  
AD FS 包含數個內建的存取控制原則範本。  指定的目標具有相同的一組原則需求，例如用戶端存取原則適用於 Office 365 的一些常見案例。  無法修改這些範本。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
若要提供增加的彈性來滿足您的業務需求，系統管理員可以建立自己的存取權原則範本。  這些可以在建立後修改和自訂的原則範本的變更將套用至所有 RPs 受控制的原則範本。  若要新增自訂的原則範本，只要按一下從新增存取控制原則 AD FS 管理中。  
  
若要建立的原則範本，必須先指定 在哪些情況下將授權要求的權杖發行和/或委派系統管理員。 條件和動作的選項會顯示在下表中。   以粗體顯示的條件可以進一步設定系統管理員具有不同或新值。 如果有的話，系統管理員也可以指定例外狀況。 當符合條件時，允許的動作將不會觸發，如果沒有指定的例外狀況，並傳入的要求符合例外狀況中指定的條件。  
  
|允許使用者|除了| 
| --- | --- | 
 |從**特定**網路|從**特定**網路<br /><br />從**特定**群組<br /><br />從裝置**特定**信任層級<br /><br />具有**特定**要求中的宣告|  
|從**特定**群組|從**特定**網路<br /><br />從**特定**群組<br /><br />從裝置**特定**信任層級<br /><br />具有**特定**要求中的宣告|  
|從裝置**特定**信任層級|從**特定**網路<br /><br />從**特定**群組<br /><br />從裝置**特定**信任層級<br /><br />具有**特定**要求中的宣告|  
|具有**特定**要求中的宣告|從**特定**網路<br /><br />從**特定**群組<br /><br />從裝置**特定**信任層級<br /><br />具有**特定**要求中的宣告|  
|而且需要多重要素驗證|從**特定**網路<br /><br />從**特定**群組<br /><br />從裝置**特定**信任層級<br /><br />具有**特定**要求中的宣告|  
  
如果系統管理員選取多個條件，它們屬於**AND**關聯性。 動作會互斥，一個原則規則對於您可以只選擇一個動作。 如果系統管理員所選取多個例外狀況，它們屬於**或**關聯性。 有幾個原則規則的範例如下所示：  
  
|**原則**|**原則規則**|
| --- | --- |  
|外部網路存取需要 MFA<br /><br />允許所有使用者|**規則 #1**<br /><br />從**外部網路**<br /><br />與 MFA<br /><br />允許<br /><br />**Rule#2**<br /><br />從**內部網路**<br /><br />允許|  
|除了非 FTE 不允許外部存取<br /><br />允許在已加入工作場所的裝置上的內部網路存取的 FTE|**規則 #1**<br /><br />從**外部網路**<br /><br />進出**非 FTE**群組<br /><br />允許<br /><br />**規則 #2**<br /><br />從**內部網路**<br /><br />進出**已加入工作場所**裝置<br /><br />進出**FTE**群組<br /><br />允許|  
|外部網路存取 「 服務系統管理員 」 除了需要 MFA<br /><br />允許所有使用者存取|**規則 #1**<br /><br />從**外部網路**<br /><br />與 MFA<br /><br />允許<br /><br />除了**服務系統管理員群組**<br /><br />**規則 #2**<br /><br />一律<br /><br />允許|  
|從外部網路存取的非工作場所加入的裝置需要 MFA<br /><br />允許內部網路和外部網路存取的 AD 網狀架構|**規則 #1**<br /><br />從**內部網路**<br /><br />進出**AD 網狀架構**群組<br /><br />允許<br /><br />**規則 #2**<br /><br />從**外部網路**<br /><br />進出**非工作場所聯結**裝置<br /><br />進出**AD 網狀架構**群組<br /><br />與 MFA<br /><br />允許<br /><br />**規則 3**<br /><br />從**外部網路**<br /><br />進出**已加入工作場所**裝置<br /><br />進出**AD 網狀架構**群組<br /><br />允許|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>參數化的原則範本與非參數化的原則範本  
可以是存取控制原則  
  
參數化的原則範本是具有參數的原則範本。 系統管理員必須輸入此範本指派給 RPs.An 系統管理員時，這些參數的值無法變更參數化的原則範本建立它之後。  參數化原則的範例是內建原則，允許特定群組。  每當 RP 以套用此原則時，就必須指定此參數。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
非參數化的原則範本是沒有任何參數的原則範本。 系統管理員可以指派給 Rp 的這個範本不需要任何輸入，並已建立之後，可以進行變更以非參數化的原則範本。  舉例來說，這是內建原則，允許所有人並需要進行 MFA。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>如何建立非參數化的存取控制原則  
若要建立的非參數化的存取控制原則使用下列程序  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>若要建立的非參數化的存取控制原則  
  
1.  從 AD FS 管理，在左側選取 存取控制原則，並在右側按一下 新增存取控制原則。  
  
2.  輸入名稱和描述。  例如: 允許使用者與已驗證的裝置。  
  
3.  底下**符合下列規則會允許存取**，按一下**新增**。  
  
4.  在允許，加上核取方塊中旁**從使用特定的信任層級的裝置**  
  
5.  在底部，選取 加上底線**特定**  
  
6.  從視窗也彈出中，選取**驗證**從下拉式清單。  按一下 [確定]。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  按一下 [確定]。 按一下 [確定]。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>如何建立參數化的存取控制原則  
若要建立參數化的存取控制原則使用下列程序  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要建立參數化的存取控制原則  
  
1.  從 AD FS 管理，在左側選取 存取控制原則，並在右側按一下 新增存取控制原則。  
  
2.  輸入名稱和描述。  例如: 允許使用者與特定的宣告。  
  
3.  底下**符合下列規則會允許存取**，按一下**新增**。  
  
4.  在允許，加上核取方塊中旁**與要求中的特定宣告**  
  
5.  在底部，選取 加上底線**特定**  
  
6.  從視窗也彈出中，選取**參數指定當指派的存取控制原則**。  按一下 [確定]。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  按一下 [確定]。 按一下 [確定]。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>如何建立自訂的存取控制原則，並發生例外狀況  
若要建立存取控制原則的例外狀況，請使用下列程序。  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>若要建立的自訂存取控制原則的例外狀況  
  
1.  從 AD FS 管理，在左側選取 存取控制原則，並在右側按一下 新增存取控制原則。  
  
2.  輸入名稱和描述。  例如: 允許使用者驗證的裝置，但未受管理。  
  
3.  底下**符合下列規則會允許存取**，按一下**新增**。  
  
4.  在允許，加上核取方塊中旁**從使用特定的信任層級的裝置**  
  
5.  在底部，選取 加上底線**特定**  
  
6.  從視窗也彈出中，選取**驗證**從下拉式清單。  按一下 [確定]。  
  
7.  在除了 下，勾選方塊旁**從使用特定的信任層級的裝置**  
  
8.  在底部下除外，請選取 加上底線**特定**  
  
9. 從視窗也彈出中，選取**受控**從下拉式清單。  按一下 [確定]。  
  
10. 按一下 [確定]。 按一下 [確定]。  
  
    ![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>如何建立具有多個允許條件的自訂存取控制原則  
若要建立多個允許存取控制原則條件會使用下列程序  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要建立參數化的存取控制原則  
  
1.  從 AD FS 管理，在左側選取 存取控制原則，並在右側按一下 新增存取控制原則。  
  
2.  輸入名稱和描述。  例如: 允許使用者與特定的宣告，並且從特定的群組。  
  
3.  底下**符合下列規則會允許存取**，按一下**新增**。  
  
4.  下允許，加上核取方塊在旁**從特定群組**和**與要求中的特定宣告**  
  
5.  在底部，選取 加上底線**特定**群組旁邊的第一個條件時，  
  
6.  從視窗也彈出中，選取**參數指定已指派原則**。  按一下 [確定]。  
  
7.  在底部，選取 加上底線**特定**第二個條件旁邊的宣告  
  
8.  從視窗也彈出中，選取**參數指定當指派的存取控制原則**。  按一下 [確定]。  
  
9. 按一下 [確定]。 按一下 [確定]。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>如何將存取控制原則指派給新的應用程式  
指派新的應用程式的存取控制原則是很直接，現在已整合至精靈新增 RP。  您可以從信賴憑證者信任精靈 中，選取您想要指派的存取控制原則。  建立新信賴憑證者信任時，這會是必要條件。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>如何將現有的應用程式指派的存取控制原則  
指派的存取控制原則至現有的應用程式只要選取應用程式從信賴憑證者信任和上按一下滑鼠右鍵**編輯存取控制原則**。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
從這裡您可以選取 存取控制原則，並將它套用至應用程式。  
  
![存取控制原則](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

