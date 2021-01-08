---
description: 深入瞭解：建立用來傳送驗證方法宣告的規則
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: 建立規則傳送驗證方法相容宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 63baded3b401a6bd79ed56aa1cff231056c4c190
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039158"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>建立規則傳送驗證方法相容宣告


您可以使用 [ **傳送群組成員資格作為宣告** 規則] 範本或 [ **轉換傳入** 宣告規則範本] 來傳送驗證方法宣告。 信賴憑證者可以使用驗證方法宣告，來判斷使用者用來驗證和取得 Active Directory 同盟服務 AD FS 宣告的登入機制 \( \) 。 您也可以使用 Windows Server 2012 R2 中 Active Directory 同盟服務 AD FS 的「驗證機制保證」功能 \( \) 做為輸入，以針對信賴憑證者想要決定以智慧卡登入為基礎的存取層級的情況，產生驗證方法宣告。 例如，開發人員可以將不同層級的存取權指派給信賴憑證者應用程式的同盟使用者。 存取層級取決於使用者是否以其使用者名稱和密碼認證登入，而不是其智慧卡。

視組織的需求而定，請使用下列其中一個程式：

-   使用 [ **傳送群組成員資格作為宣告** 規則範本] 建立此規則 \- 。當您想要在此範本中指定的群組最後判斷要發出哪些驗證方法宣告時，可以使用此規則範本。

-    \- 當您想要將現有的驗證方法變更為與無法辨識標準 AD FS 驗證方法宣告的產品搭配使用的新驗證方法時，請使用 [轉換傳入宣告規則範本] 來建立此規則。



## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用 Windows Server 2016 中的信賴憑證者信任上的傳送群組成員資格作為宣告規則範本來建立

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [信賴憑證者 **信任**]。
![當您使用 [傳送群組成員資格作為宣告規則] 範本來建立規則時，顯示在主控台樹中選取 [信賴憑證者信任] 的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告發行原則**]。
![當您使用 [傳送群組成員資格作為宣告規則] 範本來建立規則時，顯示在哪裡選取 [編輯宣告發行原則] 功能表選項的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [ **編輯宣告發行原則** ] 對話方塊的 [ **發佈轉換規則** ] 下，按一下 [ **新增規則** ] 以啟動規則嚮導。
![顯示當您使用 [傳送群組成員資格作為宣告規則] 範本建立規則時，如何新增規則的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **傳送群組成員資格作為** 宣告]，然後按 **[下一步]**。
![顯示要在哪裡選取 [傳送群組成員資格] 作為宣告範本的螢幕擷取畫面。](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)

6.  在 [ **設定規則** ] 頁面上，輸入宣告規則名稱。

7.  按一下 **[流覽]**，選取成員應接收此驗證方法宣告的群組，然後按一下 **[確定]**。

8.  在 [ **輸出宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

9. 在 [ **外寄宣告值**] 中，根據您慣用的驗證方法，在下表中輸入其中一個預設的 [統一資源識別項 \( URI] 值， \) 按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

|                            實際的驗證方法                             |                                對應的 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        使用者名稱和密碼驗證                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 驗證                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| \( \) 使用 x.509 憑證的傳輸層安全性 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  不 \- 使用 TLS 的 x.509 型驗證                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [傳送群組成員資格作為宣告規則] 範本來建立規則時，要在哪裡選取 [完成]。](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)

## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要在 Windows Server 2016 的宣告提供者信任上使用傳送群組成員資格作為宣告規則範本來建立

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **宣告提供者信任**]。
![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [傳送群組成員資格作為宣告規則] 範本來建立規則時，要在哪裡選取宣告提供者信任。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [傳送群組成員資格作為宣告規則] 範本來建立規則時，要在哪裡選取 [編輯宣告規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 [ **接受轉換規則** ] 下的 [ **新增規則** ]，以啟動規則嚮導。
![顯示當您使用 Windows Server 2016 中的 [傳送群組成員資格作為宣告規則] 範本來建立規則時，要在哪裡選取 [新增規則] 按鈕的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **傳送群組成員資格作為** 宣告]，然後按 **[下一步]**。
![螢幕擷取畫面，顯示當您在 Windows Server 2016 中建立規則時，要在哪裡選取 [傳送群組成員資格作為宣告範本]。](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)

6.  在 [ **設定規則** ] 頁面上，輸入宣告規則名稱。

7.  按一下 **[流覽]**，選取成員應接收此驗證方法宣告的群組，然後按一下 **[確定]**。

8.  在 [ **輸出宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

9. 在 [ **外寄宣告值**] 中，根據您慣用的驗證方法，在下表中輸入其中一個預設的 [統一資源識別項 \( URI] 值， \) 按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

|                            實際的驗證方法                             |                                對應的 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        使用者名稱和密碼驗證                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 驗證                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| \( \) 使用 x.509 憑證的傳輸層安全性 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  不 \- 使用 TLS 的 x.509 型驗證                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [傳送群組成員資格作為宣告規則] 範本來建立規則時，要在哪裡選取 [完成]。](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用在 Windows Server 2016 中的信賴憑證者信任上轉換連入宣告規則範本來建立此規則

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [信賴憑證者 **信任**]。
![螢幕擷取畫面，顯示當您使用 [轉換傳入宣告] 規則範本建立規則時，在主控台樹中選取 [信賴憑證者信任] 的位置。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告發行原則**]。
![顯示當您使用 [轉換傳入宣告] 規則範本建立規則時，顯示在哪裡選取 [編輯宣告發行原則] 的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [ **編輯宣告發行原則** ] 對話方塊的 [ **發佈轉換規則** ] 下，按一下 [ **新增規則** ] 以啟動規則嚮導。
![螢幕擷取畫面，顯示當您使用 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [新增規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **轉換傳入** 宣告]，然後按 **[下一步]**。
![顯示當您建立規則時，要在哪裡選取 [轉換傳入宣告] 範本的螢幕擷取畫面。](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  在 [ **設定規則** ] 頁面上，輸入宣告規則名稱。

7.  在 [ **傳入宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

8.  在 [ **輸出宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

9. 選取 [ **以不同的外寄宣告值取代傳入** 的宣告值]，然後執行下列動作：

    1.  在 [連 **入宣告值**] 中，輸入下列其中一個以原本使用的實際驗證方法 uri 為基礎的 uri 值，按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

    2.  在 [ **外寄宣告值**] 中，輸入下表中的其中一個預設 URI 值，這取決於您的新慣用驗證方法選擇，請按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

|              實際的驗證方法              |                                對應的 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         使用者名稱和密碼驗證          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 驗證                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 憑證的 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   不 \- 使用 TLS 的 x.509 型驗證    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![螢幕擷取畫面，顯示當您使用 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [完成]。](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)

> [!NOTE]
> 除了資料表中的值之外，還可以使用其他的 URI 值。 如上表所示的 URI 值會反映信賴憑證者預設接受的 Uri。

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要使用在 Windows Server 2016 中的宣告提供者信任上轉換連入宣告規則範本來建立此規則

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **宣告提供者信任**]。
![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [轉換傳入宣告] 規則範本建立規則時，在主控台樹中選取宣告提供者信任的位置。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [編輯宣告規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 [ **接受轉換規則** ] 下的 [ **新增規則** ]，以啟動規則嚮導。
![顯示當您在 Windows Server 2016 中使用 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [新增規則] 的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **轉換傳入** 宣告]，然後按 **[下一步]**。
![顯示當您在 Windows Server 2016 中建立規則時，要在哪裡選取 [轉換傳入宣告] 範本的螢幕擷取畫面。](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  在 [ **設定規則** ] 頁面上，輸入宣告規則名稱。

7.  在 [ **傳入宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

8.  在 [ **輸出宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

9. 選取 [ **以不同的外寄宣告值取代傳入** 的宣告值]，然後執行下列動作：

    1.  在 [連 **入宣告值**] 中，輸入下列其中一個以原本使用的實際驗證方法 uri 為基礎的 uri 值，按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

    2.  在 [ **外寄宣告值**] 中，輸入下表中的其中一個預設 URI 值，這取決於您的新慣用驗證方法選擇，請按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

|              實際的驗證方法              |                                對應的 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         使用者名稱和密碼驗證          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 驗證                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 憑證的 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   不 \- 使用 TLS 的 x.509 型驗證    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![螢幕擷取畫面，顯示當您使用 Windows Server 2016 中的 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [完成]。](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>若要使用 Windows Server 2012 R2 中的傳送群組成員資格作為宣告規則範本來建立此規則

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 **AD FS \\ 信任關係**] 下，按一下 [ **宣告提供者信任** ] 或 [信賴憑證者 **信任**]，然後在清單中按一下您要在其中建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![顯示當您使用 [傳送群組成員資格作為宣告規則] 範本建立規則時，要在哪裡選取 [編輯宣告規則] 的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，選取下列其中一個索引標籤，根據您要編輯的信任以及您要在其中建立此規則的規則集，然後按一下 [ **新增規則** ]，以啟動與該規則集相關聯的規則 wizard：

    -   **接受轉換規則**

    -   **發行轉換規則**

    -   **發佈授權規則**

    -   **委派授權規則** 
 ![顯示當您使用 [傳送群組成員資格作為宣告規則] 範本建立規則時，要在哪裡選取 [新增規則] 的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **傳送群組成員資格作為** 宣告]，然後按 **[下一步]**。
![螢幕擷取畫面，顯示當您建立規則時，要在哪裡選取 [傳送群組成員資格作為宣告範本]。](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  在 [ **設定規則** ] 頁面上，輸入宣告規則名稱。

7.  按一下 **[流覽]**，選取成員應接收此驗證方法宣告的群組，然後按一下 **[確定]**。

8.  在 [ **輸出宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

9. 在 [ **外寄宣告值**] 中，根據您慣用的驗證方法，在下表中輸入其中一個預設的 [統一資源識別項 \( URI] 值， \) 按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

|                            實際的驗證方法                             |                                對應的 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        使用者名稱和密碼驗證                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 驗證                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| \( \) 使用 x.509 憑證的傳輸層安全性 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  不 \- 使用 TLS 的 x.509 型驗證                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![顯示當您使用 [傳送群組成員資格作為宣告規則] 範本來建立規則時，要在哪裡選取 [完成] 的螢幕擷取畫面。](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)

> [!NOTE]
> 除了資料表中的值之外，還可以使用其他的 URI 值。 上表所示的 URI 值會反映信賴憑證者預設接受的 Uri。

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>使用 Windows Server 2012 R2 中的轉換連入宣告規則範本來建立此規則



1.  在伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **AD FS 管理**]。

2.  在主控台樹的 **AD FS \\ 信任關係**] 下，按一下 [ **宣告提供者信任** ] 或 [信賴憑證者 **信任**]，然後在清單中按一下您要在其中建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![螢幕擷取畫面，顯示當您在 Windows Server 2012 R2 中使用 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [編輯宣告規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，選取下列其中一個索引標籤，這取決於您正在編輯的信任，以及您想要建立此規則的規則集，然後按一下 [ **新增規則** ] 以啟動與該規則集相關聯的規則 wizard：

    -   **接受轉換規則**

    -   **發行轉換規則**

    -   **發佈授權規則**

    -   **委派授權規則** 
 ![顯示當您在 Windows Server 2012 R2 中使用 [轉換傳入宣告] 規則範本建立規則時，要在哪裡選取 [新增規則] 的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **轉換傳入** 宣告]，然後按 **[下一步]**。
![顯示當您在 Windows Server 2012 R2 中建立規則時，要在哪裡選取 [轉換傳入宣告] 範本的螢幕擷取畫面。](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)

6.  在 [ **設定規則** ] 頁面上，輸入宣告規則名稱。

7.  在 [ **傳入宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

8.  在 [ **輸出宣告類型**] 中，選取清單中的 [ **驗證方法** ]。

9. 選取 [ **以不同的外寄宣告值取代傳入** 的宣告值]，然後執行下列動作：

    1.  在 [連 **入宣告值**] 中，輸入下列其中一個以原本使用的實際驗證方法 uri 為基礎的 uri 值，按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

    2.  在 [ **外寄宣告值**] 中，輸入下表中的其中一個預設 URI 值，這取決於您的新慣用驗證方法選擇，請按一下 **[完成**]，然後按一下 **[確定]** 以儲存規則。

|              實際的驗證方法              |                                對應的 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         使用者名稱和密碼驗證          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 驗證                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 憑證的 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   不 \- 使用 TLS 的 x.509 型驗證    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)

> [!NOTE]
> 除了資料表中的值之外，還可以使用其他的 URI 值。 如上表所示的 URI 值會反映信賴憑證者預設接受的 Uri。

## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[檢查清單：為宣告提供者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
