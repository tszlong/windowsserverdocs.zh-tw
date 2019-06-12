---
title: 複合驗證和 Active Directory 網域服務的宣告，在 Active Directory Federation Services
description: 下列文件討論複合驗證以及 AD FS 中的 AD DS 宣告。
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2380060894ff2f365451bbabfd41b8aa7e6792a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445291"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>AD FS 中的複合驗證和 AD DS 宣告
Windows Server 2012 增強藉由引進複合驗證的 Kerberos 驗證。  複合驗證可讓包含兩個身分識別的 Kerberos 票證授權服務 (TGS) 要求︰ 

- 使用者的身分識別
- 使用者的裝置身分識別。  

Windows 會完成複合驗證，藉由擴充[Kerberos 彈性驗證安全通道 (FAST)，或 Kerberos 防護](https://technet.microsoft.com/library/hh831747.aspx)。 

AD FS 2012 和更新版本可讓 AD DS 發給使用者或裝置宣告，位於 Kerberos 驗證票證的耗用量。 在舊版的 AD FS 中，的宣告引擎無法從 Kerberos 只能讀取使用者和群組安全性識別碼 (Sid)，但無法讀取任何宣告的 Kerberos 票證中包含的資訊。

您可以使用 Active Directory 網域服務 (AD DS)，以啟用更豐富的同盟應用程式的存取控制-放在一起，發給使用者和裝置宣告，與 Active Directory Federation Services (AD FS)。

## <a name="requirements"></a>需求
1.  存取同盟應用程式，電腦必須向使用 AD FS **Windows 整合式驗證**。 
    - Windows 整合式驗證連接到後端的 AD FS 伺服器時才可用。
    - 電腦必須能夠觸達同盟服務名稱的後端的 AD FS 伺服器
    - AD FS 伺服器必須為主要驗證方法，在其內部網路設定中提供 Windows 整合式驗證。

2.  原則**宣告複合驗證以及 Kerberos 防護的 Kerberos 用戶端支援**必須套用至存取受到複合驗證的同盟應用程式的所有電腦。 這是適用於單一樹系時，或跨樹系案例。

3.  裝載 AD FS 伺服器的網域必須有**宣告複合驗證以及 Kerberos 防護的 KDC 支援**原則設定套用至網域控制站。

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中設定 AD FS 的步驟
使用下列步驟來設定複合驗證和宣告 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步驟 1：啟用 KDC 支援宣告、 複合驗證以及 Kerberos 防護的預設網域控制站原則
1.  在 [伺服器管理員] 中，選取 [工具]**群組原則管理**。
2.  瀏覽至**預設網域控制站原則**，以滑鼠右鍵按一下，然後選取**編輯**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  在上**群組原則管理編輯器**下方**電腦設定**，展開**原則**，展開**系統管理範本**依序展開**系統**，然後選取**KDC**。
4.  在右窗格中，按兩下**KDC 支援宣告、 複合驗證以及 Kerberos 防護**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  在新的對話方塊視窗中，設定 KDC 支援宣告**已啟用**。
6.  在 Options 底下，選取**支援**從下拉式選單，然後按一下**套用**並**確定**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步驟 2：啟用宣告、 複合驗證以及 Kerberos 防護存取同盟應用程式的電腦上的 Kerberos 用戶端支援

1.  在 群組原則套用到存取同盟的應用程式中的電腦上**群組原則管理編輯器**下方**電腦設定**，展開 **原則**，依序展開**系統管理範本**，展開**系統**，然後選取**Kerberos**。
2.  在 [群組原則管理編輯器] 視窗的右窗格中，按兩下**宣告、 複合驗證以及 Kerberos 防護的 Kerberos 用戶端支援。**
3.  在新的對話方塊視窗中，設定為 Kerberos 用戶端支援**已啟用**，按一下 **套用**並**確定**。
![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  關閉 \[群組原則管理編輯器\]。

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>步驟 3：請確定已更新 AD FS 伺服器。
您需要確保您的 AD FS 伺服器上安裝下列更新。

|Update|描述|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|（包括 KB2919355、 KB2932046、 KB2934018、 KB2937592、 KB2938439） 的累積安全性更新|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Server 2012 r2 的更新|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|這項更新加入支援複合 ID 宣告，在 Active Directory Federation Services。|

### <a name="step-4-configure-the-primary-authentication-provider"></a>步驟 4：設定主要驗證提供者

1. 若要設定主要的驗證提供者**Windows 驗證**AD FS 內部網路設定。
2. 在 [AD FS 管理] 底下**驗證原則**，選取**主要驗證**，並在**全域設定**按一下**編輯**。
3. 在 **編輯全域驗證原則**下方**內部網路**選取**Windows 驗證**。
4. 按一下 **套用**並**確定**。

![群組原則管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. 使用的 PowerShell，您可以使用**組 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>基礎 WID 伺服器陣列，必須在主要 AD FS 伺服器執行命令的 PowerShell。
>SQL 式伺服器陣列中的 PowerShell 命令可能會執行任何成員的伺服器陣列的 AD FS 伺服器上。

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>步驟 5：新增至 AD FS 宣告描述
1. 伺服器陣列中加入下列的理賠申請描述。 此理賠申請描述 ADFS 2012 R2 中預設存在並不需要以手動方式加入。
2. 在 [AD FS 管理] 底下**服務**，以滑鼠右鍵按一下**宣告描述**，然後選取**新增宣告描述**
3. 輸入的理賠申請描述中的下列資訊
   - 顯示名稱：[Windows 裝置群組] 
   - 宣告描述: '<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>' '
4. 勾選這兩個方塊。
5. 按一下 [確定]  。

![宣告描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. 使用的 PowerShell，您可以使用**新增 AdfsClaimDescription** cmdlet。
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>基礎 WID 伺服器陣列，必須在主要 AD FS 伺服器執行命令的 PowerShell。
>SQL 式伺服器陣列中的 PowerShell 命令可能會執行任何成員的伺服器陣列的 AD FS 伺服器上。

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步驟 6：啟用 Msds-supportedencryptiontypes 屬性的複合驗證位元

1.  啟用複合驗證在您指定執行 AD FS 服務使用的帳戶的 Msds-supportedencryptiontypes 屬性位元**Set-adserviceaccount** PowerShell cmdlet。  

>[!NOTE]
>如果您變更服務帳戶，則您必須執行，以手動方式啟用複合驗證**Set-aduser compoundIdentitySupported: $true** Windows PowerShell cmdlet。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. 重新啟動 ADFS 服務。

>[!NOTE]
>一旦 'CompoundIdentitySupported' 設為 true，安裝新的伺服器 (2012R2/2016) 失敗，並出現下列錯誤： 在同一個 gMSA **Install-adserviceaccount:無法安裝服務帳戶。錯誤訊息：' 提供的內容不符合目標。 '** .
>
>**解決方案**：暫時設 $false CompoundIdentitySupported。 這個步驟會導致停止發出 WindowsDeviceGroup 宣告的 ADFS。 Set-adserviceaccount-身分識別 「 ADFS 服務帳戶 」 CompoundIdentitySupported: $false 安裝新的伺服器上的 gMSA，然後再啟用 回到 $True CompoundIdentitySupported。
停用 CompoundIdentitySupported，然後再重新啟用不需要重新啟動 ADFS 服務。

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步驟 7：更新 AD FS 宣告提供者信任的 Active Directory

1. 更新 AD FS 宣告提供者信任的 Active Directory 'WindowsDeviceGroup' 宣告中包含下列的 '傳遞' 宣告規則。
2.  在 [ **AD FS 管理**，按一下**宣告提供者信任**並在右窗格中，用滑鼠右鍵按一下**Active Directory** ，然後選取**編輯宣告規則]** .
3.  在 **編輯宣告規則的 Active Directory**按一下 **加入規則**。
4.  在 **新增轉換宣告規則精靈**選取**傳遞或篩選傳入宣告**然後按一下**下一步**。
5.  新增顯示名稱，然後選取**Windows 裝置群組**從**傳入宣告類型**下拉式清單。
6.  按一下 **[完成]** 。  按一下 **套用**並**確定**。 
![宣告描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步驟 8：在信賴憑證者合作對象有 'WindowsDeviceGroup' 宣告，新增類似的 '傳遞' 轉換' 宣告規則。
2. 在**AD FS 管理**，按一下**信賴憑證者信任**並在右窗格中，用滑鼠右鍵按一下您的 RP 並選取**編輯宣告規則**。
3. 在 **發佈轉換規則**按一下 **加入規則**。
4. 在 **新增轉換宣告規則精靈**選取**傳遞或篩選傳入宣告**然後按一下**下一步**。
5. 新增顯示名稱，然後選取**Windows 裝置群組**從**傳入宣告類型**下拉式清單。
6. 按一下 **[完成]** 。  按一下 **套用**並**確定**。
   ![宣告描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>設定 AD FS Windows Server 2016 中的步驟
以下將詳細說明適用於 Windows Server 2016 的 AD FS 上設定複合驗證的步驟。

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步驟 1：啟用 KDC 支援宣告、 複合驗證以及 Kerberos 防護的預設網域控制站原則
1.  在 [伺服器管理員] 中，選取 [工具]**群組原則管理**。
2.  瀏覽至**預設網域控制站原則**，以滑鼠右鍵按一下，然後選取**編輯**。
3.  在上**群組原則管理編輯器**下方**電腦設定**，展開**原則**，展開**系統管理範本**依序展開**系統**，然後選取**KDC**。
4.  在右窗格中，按兩下**KDC 支援宣告、 複合驗證以及 Kerberos 防護**。
5.  在新的對話方塊視窗中，設定 KDC 支援宣告**已啟用**。
6.  在 Options 底下，選取**支援**從下拉式選單，然後按一下**套用**並**確定**。


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步驟 2：啟用宣告、 複合驗證以及 Kerberos 防護存取同盟應用程式的電腦上的 Kerberos 用戶端支援

1.  在 群組原則套用到存取同盟的應用程式中的電腦上**群組原則管理編輯器**下方**電腦設定**，展開 **原則**，依序展開**系統管理範本**，展開**系統**，然後選取**Kerberos**。
2.  在 [群組原則管理編輯器] 視窗的右窗格中，按兩下**宣告、 複合驗證以及 Kerberos 防護的 Kerberos 用戶端支援。**
3.  在新的對話方塊視窗中，設定為 Kerberos 用戶端支援**已啟用**，按一下 **套用**並**確定**。
4.  關閉 \[群組原則管理編輯器\]。

### <a name="step-3-configure-the-primary-authentication-provider"></a>步驟 3：設定主要驗證提供者

1. 若要設定主要的驗證提供者**Windows 驗證**AD FS 內部網路設定。
2. 在 [AD FS 管理] 底下**驗證原則**，選取**主要驗證**，並在**全域設定**按一下**編輯**。
3. 在 **編輯全域驗證原則**下方**內部網路**選取**Windows 驗證**。
4. 按一下 **套用**並**確定**。
5. 使用的 PowerShell，您可以使用**組 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>基礎 WID 伺服器陣列，必須在主要 AD FS 伺服器執行命令的 PowerShell。
>SQL 式伺服器陣列中的 PowerShell 命令可能會執行任何成員的伺服器陣列的 AD FS 伺服器上。

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步驟 4：啟用 Msds-supportedencryptiontypes 屬性的複合驗證位元

1.  啟用複合驗證在您指定執行 AD FS 服務使用的帳戶的 Msds-supportedencryptiontypes 屬性位元**Set-adserviceaccount** PowerShell cmdlet。  

>[!NOTE]
>如果您變更服務帳戶，則您必須執行，以手動方式啟用複合驗證**Set-aduser compoundIdentitySupported: $true** Windows PowerShell cmdlet。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. 重新啟動 ADFS 服務。

>[!NOTE]
>一旦 'CompoundIdentitySupported' 設為 true，安裝新的伺服器 (2012R2/2016) 失敗，並出現下列錯誤： 在同一個 gMSA **Install-adserviceaccount:無法安裝服務帳戶。錯誤訊息：' 提供的內容不符合目標。 '** .
>
>**解決方案**：暫時設 $false CompoundIdentitySupported。 這個步驟會導致停止發出 WindowsDeviceGroup 宣告的 ADFS。 Set-adserviceaccount-身分識別 「 ADFS 服務帳戶 」 CompoundIdentitySupported: $false 安裝新的伺服器上的 gMSA，然後再啟用 回到 $True CompoundIdentitySupported。
停用 CompoundIdentitySupported，然後再重新啟用不需要重新啟動 ADFS 服務。

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步驟 5：更新 AD FS 宣告提供者信任的 Active Directory

1. 更新 AD FS 宣告提供者信任的 Active Directory 'WindowsDeviceGroup' 宣告中包含下列的 '傳遞' 宣告規則。
2.  在 [ **AD FS 管理**，按一下**宣告提供者信任**並在右窗格中，用滑鼠右鍵按一下**Active Directory** ，然後選取**編輯宣告規則]** .
3.  在 **編輯宣告規則的 Active Directory**按一下 **加入規則**。
4.  在 **新增轉換宣告規則精靈**選取**傳遞或篩選傳入宣告**然後按一下**下一步**。
5.  新增顯示名稱，然後選取**Windows 裝置群組**從**傳入宣告類型**下拉式清單。
6.  按一下 **[完成]** 。  按一下 **套用**並**確定**。 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步驟 6：在信賴憑證者合作對象有 'WindowsDeviceGroup' 宣告，新增類似的 '傳遞' 轉換' 宣告規則。
2. 在**AD FS 管理**，按一下**信賴憑證者信任**並在右窗格中，用滑鼠右鍵按一下您的 RP 並選取**編輯宣告規則**。
3. 在 **發佈轉換規則**按一下 **加入規則**。
4. 在 **新增轉換宣告規則精靈**選取**傳遞或篩選傳入宣告**然後按一下**下一步**。
5. 新增顯示名稱，然後選取**Windows 裝置群組**從**傳入宣告類型**下拉式清單。
6. 按一下 **[完成]** 。  按一下 **套用**並**確定**。

## <a name="validation"></a>驗證
若要驗證 'WindowsDeviceGroup' 宣告的版本，請建立測試的宣告感知應用程式使用.Net 4.6。 使用 WIF SDK 4.0。
設定為信賴憑證者的合作對象在 ADFS 中的應用程式，並使用宣告規則加以更新，如上述步驟中所指定。
驗證時使用的 ADFS 的 Windows 整合式驗證提供者之應用程式，下列宣告會轉換。
![驗證](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

電腦/裝置的宣告現在可能會耗用更豐富的存取控制項。

例如： 下列**AdditionalAuthenticationRules**會指示如果 – 驗證的使用者不是安全性群組的成員，請叫用 MFA AD FS"-1-5-21-2134745077-1211275016-3050530490-1117 」 和電腦 (其中是使用者是從驗證） 不是"S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup) 」 中的 [安全性] 群組的成員

不過，如果符合上述條件，不會叫用 MFA。

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
