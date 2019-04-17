---
title: "複合驗證以及 Active Directory Domain Services 宣告在 Active Directory 同盟服務"
description: "下列文件討論複合驗證以及 AD DS 宣告中 AD FS。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5f80cbcbdb2deca9b098014627365b8ea819b5f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>複合驗證，AD FS 中的 AD DS 宣告
Windows Server 2012 美化 F:kerberos 驗證引進複合驗證。  複合驗證可讓您以包含兩個身分要求 Kerberos 票證授與服務 (TGS): 

- 使用者的身分
- 使用者的裝置的身分。  

Windows 會複合驗證完成來將它擴展[Kerberos 彈性驗證安全通道（積極型」），或 Kerberos 保護 \](https://technet.microsoft.com/library/hh831747.aspx)。 

AD FS 2012 及更新版本，可讓消耗 AD DS 發出使用者或裝置宣告位於 Kerberos 驗證票證。 在舊版 AD FS，引擎無法從 Kerberos 只朗讀使用者和群組安全性 Id (Sid)，但找不到任何朗讀宣告宣告 Kerberos 票證中所包含的資訊。

您可以使用 Active Directory Domain Services (AD DS) 聯盟應用程式為豐富存取控制-發行宣告使用者和裝置，使用 Active Directory 同盟 Services (AD FS)。

## <a name="requirements"></a>需求
1.  存取聯盟應用程式中，電腦必須驗證，AD FS 使用**Windows 整合式驗證**。 
    - 連接後端 AD FS 伺服器時，只使用 Windows 整合式驗證。
    - 電腦必須同盟服務的名稱，AD FS 伺服器端瑞曲之戰
    - AD FS 伺服器必須為主要的驗證方法其內部網路設定中提供整合式驗證的 Windows。

2.  原則**Kerberos client 支援宣告複合驗證以及 Kerberos 保護 \**必須套用到所有電腦存取聯盟受到複合驗證的應用程式。 這是在單一森林或跨樹系案例適用。

3.  必須與容納 AD FS 伺服器網域**\ [KDC 支援宣告複合驗證以及 Kerberos 保護 \**的網域控制站套用原則設定。

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 設定 AD FS 步驟
使用下列步驟來設定複合驗證以及宣告 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步驟 1：讓 \ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \ 預設的網域控制站原則
1.  在伺服器管理員中，選取 [工具]**群組原則管理**。
2.  瀏覽向下**預設網域控制站原則**，以滑鼠右鍵按一下，然後選取**編輯**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  在**群組原則編輯器] 管理**，在**電腦設定**，展開**原則**，展開**系統管理範本**，展開**系統**，然後選取 [ **KDC**。
4.  在右窗格中，按兩下 [ **\ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  在新] 對話方塊視窗，設定 \ [KDC 支援宣告到**啟用**。
6.  下方 [選項]，選取 [**支援**從下拉式功能表然後按**套用**和**[確定]**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步驟 2：讓 Kerberos client 支援宣告、複合驗證以及 Kerberos 保護 \ 存取聯盟應用程式的電腦上

1.  在群組原則套用到存取聯盟應用程式，在電腦上**群組原則編輯器] 管理**，在**電腦設定**，展開 [**原則**，展開 [**管理範本**，展開 [**系統**，然後選取 [ **Kerberos**。
2.  在 [群組原則編輯器] 管理視窗的右窗格中，按兩下 [ **Kerberos client 支援宣告、複合驗證以及 Kerberos 保護 \。**
3.  在新] 對話方塊視窗中，為 Kerberos client 支援**啟用**，按一下 [**套用**並**[確定]**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  關閉 「 群組原則管理編輯器。

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>步驟 3：確認已更新的 AD FS 伺服器。
您必須以確保您 AD FS 伺服器上安裝的下列更新。

|更新|描述|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|累積安全性更新（包括 KB2919355，KB2932046、KB2934018、KB2937592、KB2938439）|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Server 2012 R2 的更新|
|[3052122 Hotfix](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|此更新在 Active Directory 同盟服務新增複合 ID 宣告的支援。|

### <a name="step-4-configure-the-primary-authentication-provider"></a>步驟 4：設定主要驗證提供者

1. 主要驗證提供者為**Windows 驗證**AD FS 內部網路設定。
2. AD FS 管理，在**驗證原則**、**主要驗證**並在**通用設定**按一下**編輯**。
3. 在**編輯全球驗證原則**在**內部**選取**Windows 驗證**。
4. 按一下**適用於**和**[確定]**。

![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. 使用的 PowerShell，您可以使用**設定為 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在 WID 基礎發電廠，PowerShell 命令必須執行主要 AD FS 伺服器。
>在 SQL 根據陣列，可能會執行 PowerShell 指令任何發電廠的成員，AD FS 伺服器上。

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>步驟 5：加入 AD FS 理賠要求描述
1.  加入發電廠下列宣告描述。 此理賠要求描述在於預設 ADFS 2012 R2 並不需要手動新增。
2.  AD FS 管理，在**服務**，以滑鼠右鍵按一下**取得描述**，然後選取**新增取得描述**
3.  宣告描述中輸入下列資訊
    - 顯示名稱: ' Windows 裝置群組」 
    - 宣告描述: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. 這兩個方塊中檢查的地方。
5. 按一下**[確定]**。

![宣告描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. 使用的 PowerShell，您可以使用**新增-AdfsClaimDescription** cmdlet。
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>在 WID 基礎發電廠，PowerShell 命令必須執行主要 AD FS 伺服器。
>在 SQL 根據陣列，可能會執行 PowerShell 指令任何發電廠的成員，AD FS 伺服器上。

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步驟 6：讓 msDS-SupportedEncryptionTypes 屬性複合驗證的位元

1.  讓您指定 AD FS 服務使用來執行帳號 msDS-SupportedEncryptionTypes 屬性位元的複合驗證**設定為 ADServiceAccount** PowerShell cmdlet。  

>[!NOTE]
>如果您變更服務帳號，則您必須手動執行讓複合驗證**設定為 ADUser-compoundIdentitySupported: $true** Windows PowerShell cmdlet。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  重新開機 ADFS 服務。

>[!NOTE]
>一旦 'CompoundIdentitySupported' 設定為 true，安裝新的伺服器 (2012R2 日 2016) 發生故障下列錯誤碼–相同 gMSA 為**安裝-ADServiceAccount：無法安裝 account 服務。錯誤訊息: ' 提供的操作不符合 target。]**.
>
>**方案**：暫時設定 $false CompoundIdentitySupported。 這個步驟會導致 ADFS 停止發行 WindowsDeviceGroup 主張。 Set-ADServiceAccount 層的身分 ADFS 服務 Account'-CompoundIdentitySupported: $false 新的伺服器上安裝 gMSA 以及然後 CompoundIdentitySupported 回 $True。
停用 CompoundIdentitySupported，然後正在重新啟用不需要將重新 ADFS 服務。

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步驟 7：更新 AD FS 宣告提供者信任的 Active Directory

1. 更新 AD FS 宣告提供者信任的 Active Directory 包含下列規則 '通過' 理賠要求為 'WindowsDeviceGroup' 理賠要求。
2.  在**AD FS 管理**，按一下 [**宣告提供者信任**並在右窗格中，righ 按**Active Directory**，然後選取**編輯理賠要求規則**。
3.  在**編輯理賠要求規則主動導演適用於**按**新增規則**。
4.  在**新增轉換理賠要求規則精靈**選取**傳遞透過或篩選連入宣告**按**下一步**。
5.  新增的顯示名稱，然後選取**Windows 裝置群組**的**傳入取得輸入**下拉式清單。
6.  按一下**完成**。  按一下**適用於**和**[確定]**。 
![宣告描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步驟 8：上可以方的 'WindowsDeviceGroup' 宣告預期的位置，新增類似 '通過' 轉換' 理賠要求規則。
2.  在**AD FS 管理**，按一下 [**可以廠商信任**和就在右窗格中，righ 按一下您資源點數，然後選取**編輯理賠要求規則**。
3.  在**發行轉換規則**按**[新增規則**。
4.  在**新增轉換理賠要求規則精靈**選取**傳遞透過或篩選連入宣告**按**下一步**。
5.  新增的顯示名稱，然後選取**Windows 裝置群組**的**傳入取得輸入**下拉式清單。
6.  按一下**完成**。  按一下**適用於**和**[確定]**。
![宣告描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Windows Server 2016 中設定 AD FS 步驟
下列會詳細顯示設定複合驗證，AD FS 適用於 Windows Server 2016 上的步驟。

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步驟 1：讓 \ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \ 預設的網域控制站原則
1.  在伺服器管理員中，選取 [工具]**群組原則管理**。
2.  瀏覽向下**預設網域控制站原則**，以滑鼠右鍵按一下，然後選取**編輯**。
3.  在**群組原則編輯器] 管理**，在**電腦設定**，展開**原則**，展開**系統管理範本**，展開**系統**，然後選取 [ **KDC**。
4.  在右窗格中，按兩下 [ **\ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \**。
5.  在新] 對話方塊視窗，設定 \ [KDC 支援宣告到**啟用**。
6.  下方 [選項]，選取 [**支援**從下拉式功能表然後按**套用**和**[確定]**。


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步驟 2：讓 Kerberos client 支援宣告、複合驗證以及 Kerberos 保護 \ 存取聯盟應用程式的電腦上

1.  在群組原則套用到存取聯盟應用程式，在電腦上**群組原則編輯器] 管理**，在**電腦設定**，展開 [**原則**，展開 [**管理範本**，展開 [**系統**，然後選取 [ **Kerberos**。
2.  在 [群組原則編輯器] 管理視窗的右窗格中，按兩下 [ **Kerberos client 支援宣告、複合驗證以及 Kerberos 保護 \。**
3.  在新] 對話方塊視窗中，為 Kerberos client 支援**啟用**，按一下 [**套用**並**[確定]**。
4.  關閉 「 群組原則管理編輯器。

### <a name="step-3-configure-the-primary-authentication-provider"></a>步驟 3：設定主要驗證提供者

1. 主要驗證提供者為**Windows 驗證**AD FS 內部網路設定。
2. AD FS 管理，在**驗證原則**、**主要驗證**並在**通用設定**按一下**編輯**。
3. 在**編輯全球驗證原則**在**內部**選取**Windows 驗證**。
4. 按一下**適用於**和**[確定]**。
5. 使用的 PowerShell，您可以使用**設定為 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在 WID 基礎發電廠，PowerShell 命令必須執行主要 AD FS 伺服器。
>在 SQL 根據陣列，可能會執行 PowerShell 指令任何發電廠的成員，AD FS 伺服器上。

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步驟 4：讓 msDS-SupportedEncryptionTypes 屬性複合驗證的位元

1.  讓您指定 AD FS 服務使用來執行帳號 msDS-SupportedEncryptionTypes 屬性位元的複合驗證**設定為 ADServiceAccount** PowerShell cmdlet。  

>[!NOTE]
>如果您變更服務帳號，則您必須手動執行讓複合驗證**設定為 ADUser-compoundIdentitySupported: $true** Windows PowerShell cmdlet。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  重新開機 ADFS 服務。

>[!NOTE]
>一旦 'CompoundIdentitySupported' 設定為 true，安裝新的伺服器 (2012R2 日 2016) 發生故障下列錯誤碼–相同 gMSA 為**安裝-ADServiceAccount：無法安裝 account 服務。錯誤訊息: ' 提供的操作不符合 target。]**.
>
>**方案**：暫時設定 $false CompoundIdentitySupported。 這個步驟會導致 ADFS 停止發行 WindowsDeviceGroup 主張。 Set-ADServiceAccount 層的身分 ADFS 服務 Account'-CompoundIdentitySupported: $false 新的伺服器上安裝 gMSA 以及然後 CompoundIdentitySupported 回 $True。
停用 CompoundIdentitySupported，然後正在重新啟用不需要將重新 ADFS 服務。

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步驟 5：更新 AD FS 宣告提供者信任的 Active Directory

1. 更新 AD FS 宣告提供者信任的 Active Directory 包含下列規則 '通過' 理賠要求為 'WindowsDeviceGroup' 理賠要求。
2.  在**AD FS 管理**，按一下 [**宣告提供者信任**並在右窗格中，righ 按**Active Directory**，然後選取**編輯理賠要求規則**。
3.  在**編輯理賠要求規則主動導演適用於**按**新增規則**。
4.  在**新增轉換理賠要求規則精靈**選取**傳遞透過或篩選連入宣告**按**下一步**。
5.  新增的顯示名稱，然後選取**Windows 裝置群組**的**傳入取得輸入**下拉式清單。
6.  按一下**完成**。  按一下**適用於**和**[確定]**。 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步驟 6：上可以方的 'WindowsDeviceGroup' 宣告預期的位置，新增類似 '通過' 轉換' 理賠要求規則。
2.  在**AD FS 管理**，按一下 [**可以廠商信任**和就在右窗格中，righ 按一下您資源點數，然後選取**編輯理賠要求規則**。
3.  在**發行轉換規則**按**[新增規則**。
4.  在**新增轉換理賠要求規則精靈**選取**傳遞透過或篩選連入宣告**按**下一步**。
5.  新增的顯示名稱，然後選取**Windows 裝置群組**的**傳入取得輸入**下拉式清單。
6.  按一下**完成**。  按一下**適用於**和**[確定]**。

## <a name="validation"></a>驗證
驗證 'WindowsDeviceGroup' 宣告版本、建立測試宣告使用.Net 4.6 感知應用程式。 使用 WIF SDK 4.0。
設定為信賴 ADFS 中的應用程式與更新依照上述步驟理賠要求規則。
驗證時要使用的 ADFS Windows 整合式驗證提供者的應用程式，下列宣告是上。
![驗證](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

適用於電腦/裝置宣告現在可能會使用更豐富的存取控制項。

例如–下列**AdditionalAuthenticationRules**如果–驗證使用者不安全小組的成員叫用 MFA AD FS 可將您的位置告知」-1-5-21-2134745077-1211275016-3050530490-1117」和電腦（位置使用者是從驗證）並不安全性群組」S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)」的成員

不過，如果符合上述條件，不會叫用 MFA。

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```