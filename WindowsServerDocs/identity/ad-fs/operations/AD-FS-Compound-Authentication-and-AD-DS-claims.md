---
title: Active Directory 同盟服務中的複合驗證和 Active Directory Domain Services 宣告
description: 下列檔討論 AD FS 中的複合驗證和 AD DS 宣告。
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.openlocfilehash: fa976be01d58a2fae17ca0477126c60c519853d3
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781899"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>AD FS 中的複合驗證和 AD DS 宣告
Windows Server 2012 引進複合驗證，藉此增強 Kerberos 驗證。  複合驗證可讓 Kerberos Ticket-Granting 服務 (TGS) 要求包含兩個身分識別：

- 使用者的身分識別
- 使用者裝置的身分識別。

Windows 藉由擴充 [Kerberos 彈性驗證安全通道 (快速) 或 Kerberos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11))防護來完成複合驗證。

AD FS 2012 和更新版本可讓您取用位於 Kerberos 驗證票證中 AD DS 發出的使用者或裝置宣告。 在舊版 AD FS 中，宣告引擎只能從 Kerberos 讀取使用者和群組的安全識別碼 (Sid) ，但無法讀取 Kerberos 票證中包含的任何宣告資訊。

您可以使用 Active Directory Domain Services (AD DS) 發出的使用者和裝置宣告，以及 Active Directory 同盟服務 (AD FS) ，為同盟應用程式啟用更豐富的存取控制。

## <a name="requirements"></a>需求
1.  存取同盟應用程式的電腦，必須使用 **Windows 整合式驗證** 向 AD FS 進行驗證。
    - 只有在連接到後端 AD FS 伺服器時，才能使用 Windows 整合式驗證。
    - 電腦必須能夠觸達同盟服務名稱的後端 AD FS 伺服器
    - AD FS 伺服器必須在其內部網路設定中提供「Windows 整合式驗證」作為主要驗證方法。

2.  **宣告複合驗證和 kerberos 防護的 kerberos 用戶端支援** 必須套用至所有存取複合驗證所保護之同盟應用程式的電腦。 這適用于單一樹系或跨樹系案例。

3.  裝載 AD FS 伺服器的網域必須具有套用至網域控制站的 [ **宣告複合驗證] 和 [Kerberos** 防護] 原則設定的 [KDC 支援]。

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 中設定 AD FS 的步驟
使用下列步驟來設定複合驗證和宣告

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步驟1：在預設網域控制站原則上啟用宣告、複合驗證和 Kerberos 防護的 KDC 支援
1.  在伺服器管理員中，選取 [工具]、[ **群組原則管理**]。
2.  向下流覽至 **預設網域控制站原則**，按一下滑鼠右鍵並選取 [ **編輯**]。
![顯示 [群組原則管理] 對話方塊中 [預設網域原則] 頁面的螢幕擷取畫面。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  在 **群組原則管理編輯器** 的 [ **電腦** 設定] 底下，展開 [ **原則**]，展開 [ **系統管理範本**]，展開 [ **系統**]，然後選取 [ **KDC**]。
4.  在右窗格中，按兩下 [ **宣告、複合驗證和 Kerberos 防護的 KDC 支援**]。
![顯示反白顯示 [宣告、複合驗證和 Kerberos 防護的 KDC 支援] 設定之群組原則管理編輯器的螢幕擷取畫面。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  在新的對話方塊視窗中，將 [宣告的 KDC 支援] 設定為 [ **啟用**]。
6.  在 [選項] 下，從下拉式功能表中選取 [ **支援** ]， **然後** 按一下 **[套用] 和 [確定]**。
![顯示所選支援選項的 [宣告、複合驗證和 Kerberos 防護的 KDC 支援] 對話方塊的螢幕擷取畫面。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步驟2：在存取同盟應用程式的電腦上啟用 Kerberos 用戶端支援宣告、複合驗證和 Kerberos 防護

1.  在套用至存取同盟應用程式之電腦的群組原則上，于 **群組原則管理編輯器** 的 [ **電腦** 設定] 底下，依序展開 [ **原則**]、[ **系統管理範本**]、[ **系統**]，然後選取 [ **Kerberos**]。
2.  在 [群組原則管理編輯器] 視窗的右窗格中，按兩下 [ **宣告、複合驗證和 kerberos 防護的 kerberos 用戶端支援]。**
3.  在 [新增] 對話方塊視窗中，將 [Kerberos 用戶端支援] 設定為 [**已啟用** **]，然後按一下** **[套用]**
![[宣告、複合驗證和 Kerberos 防護的 KDC 支援] 對話方塊的螢幕擷取畫面，其中顯示已選取啟用的選項。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  關閉 [群組原則管理編輯器]。

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>步驟3：確認 AD FS 伺服器已更新。
您必須確保 AD FS 伺服器上已安裝下列更新。

|更新|描述|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|累積安全性更新 (包括 KB2919355、KB2932046、KB2934018、KB2937592、KB2938439) |
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Server 2012 R2 更新|
|[修正程式3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|此更新在 Active Directory 同盟服務中新增了複合識別碼宣告的支援。|

### <a name="step-4-configure-the-primary-authentication-provider"></a>步驟4：設定主要驗證提供者

1. 針對 AD FS 內部網路設定，將主要驗證提供者設定為 [ **Windows 驗證** ]。
2. 在 AD FS 管理] 的 [**驗證原則**] 底下，選取 [**主要驗證**]，然後按一下 [**全域設定**] 下的 [**編輯**
3. 在 [在 **內部** 網路 **編輯全域驗證原則**] 下，選取 [ **Windows 驗證**]。
4. 按一下 [套用 **] 和 [確定]**。

    ![[編輯全域驗證原則] 對話方塊的螢幕擷取畫面，其中顯示已選取 [Windows 驗證] 選項。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. 使用 PowerShell 時，您可以使用 **get-adfsglobalauthenticationpolicy** Cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在 WID 型伺服器陣列中，必須在主要 AD FS 伺服器上執行 PowerShell 命令。
>在以 SQL 為基礎的伺服器陣列中，可以在伺服器陣列成員的任何 AD FS 伺服器上執行 PowerShell 命令。

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>步驟5：將宣告描述新增至 AD FS
1. 將下列宣告描述新增至伺服器陣列。 在 ADFS 2012 R2 中，預設不會出現此宣告描述，需要手動新增。
2. 在 AD FS 管理] 的 [**服務**] 下，以滑鼠右鍵按一下 [宣告 **描述**]，然後選取 [**新增宣告描述**]
3. 在 [宣告描述] 中輸入下列資訊
   - 顯示名稱：「Windows 裝置群組」
   - 宣告描述： ' <https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup> ' '
4. 在這兩個方塊中進行檢查。
5. 按一下 [確定]  。

    ![[加入宣告描述] 對話方塊的螢幕擷取畫面。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. 使用 PowerShell 時，您可以使用 **AdfsClaimDescription** Cmdlet。
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device'
   ```


>[!NOTE]
>在 WID 型伺服器陣列中，必須在主要 AD FS 伺服器上執行 PowerShell 命令。
>在以 SQL 為基礎的伺服器陣列中，可以在伺服器陣列成員的任何 AD FS 伺服器上執行 PowerShell 命令。

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步驟6：在 >msds-supportedencryptiontypes 屬性上啟用複合驗證位

1.  在您指定用來執行 AD FS 服務的帳戶上，使用 **Uninstall-adserviceaccount** PowerShell Cmdlet 來啟用 >msds-supportedencryptiontypes 屬性的複合驗證位。

>[!NOTE]
>如果您變更服務帳戶，則必須執行 **set-aduser-compoundIdentitySupported： $true** Windows PowerShell Cmdlet 以手動方式啟用複合驗證。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true
```
2. 重新開機 ADFS 服務。

>[!NOTE]
>一旦將 ' CompoundIdentitySupported ' 設為 true，在新的伺服器上安裝相同的 gMSA (2012R2/2016) 會失敗，並出現下列錯誤– **Install-uninstall-adserviceaccount：無法安裝服務帳戶。錯誤訊息：「提供的內容不符合目標**」。
>
>**解決方案**：暫時將 CompoundIdentitySupported 設定為 $false。 此步驟會導致 ADFS 停止發出 WindowsDeviceGroup 宣告。
Set-ADServiceAccount 身分識別「ADFS 服務帳戶」-CompoundIdentitySupported： $false 在新的伺服器上安裝 gMSA，然後啟用 CompoundIdentitySupported 回 $True。
停用 CompoundIdentitySupported 後再重新啟用，不需要重新開機 ADFS 服務。

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步驟7：更新 Active Directory 的 AD FS 宣告提供者信任

1. 更新 Active Directory 的 AD FS 宣告提供者信任，以包含 ' WindowsDeviceGroup ' 宣告的下列「傳遞」宣告規則。
2.  在 **AD FS 管理**] 中，按一下 [ **宣告提供者信任** ]，然後在右窗格中，以滑鼠右鍵按一下 **Active Directory** ，然後選取 [ **編輯宣告規則**]。
3.  在 [ **編輯適用于 Active Director** 的宣告規則] 中，按一下 [ **新增規則**]。
4.  在 [ **新增轉換宣告規則]** 中，選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
5.  新增顯示名稱，然後從 [**傳入宣告類型**] 下拉式清單中選取 [ **Windows 裝置群組**]。
6.  按一下 [完成] 。  按一下 [套用 **] 和 [確定]**。
    ![[AD FS]、[編輯 Active Directory 的宣告規則] 和 [編輯規則-Windows 裝置群組] 對話方塊的螢幕擷取畫面，其中包含箭號並顯示上述工作流程的 [呼叫]。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步驟8：在需要 ' WindowsDeviceGroup ' 宣告的信賴憑證者上，新增類似的「傳遞」或「轉換」宣告規則。
2. 在 **AD FS 管理**] 中，按一下 [信賴憑證者 **信任** ]，在右窗格中，以滑鼠右鍵按一下您的 RP，然後選取 [ **編輯宣告規則**]。
3. 在 [ **發行轉換規則** ] 上，按一下 [ **新增規則**]。
4. 在 [ **新增轉換宣告規則]** 中，選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
5. 新增顯示名稱，然後從 [**傳入宣告類型**] 下拉式清單中選取 [ **Windows 裝置群組**]。
6. 按一下 [完成] 。  按一下 [套用 **] 和 [確定]**。
   ![AD FS 的螢幕擷取畫面，其中包含箭號的 [編輯宣告規則] 和 [編輯規則-Windows 裝置 Grp] 對話方塊，其中包含箭號和呼叫，顯示上述工作流程。](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 中設定 AD FS 的步驟
以下將詳細說明在 Windows Server 2016 AD FS 上設定複合驗證的步驟。

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步驟1：在預設網域控制站原則上啟用宣告、複合驗證和 Kerberos 防護的 KDC 支援
1.  在伺服器管理員中，選取 [工具]、[ **群組原則管理**]。
2.  向下流覽至 **預設網域控制站原則**，按一下滑鼠右鍵並選取 [ **編輯**]。
3.  在 **群組原則管理編輯器** 的 [ **電腦** 設定] 底下，展開 [ **原則**]，展開 [ **系統管理範本**]，展開 [ **系統**]，然後選取 [ **KDC**]。
4.  在右窗格中，按兩下 [ **宣告、複合驗證和 Kerberos 防護的 KDC 支援**]。
5.  在新的對話方塊視窗中，將 [宣告的 KDC 支援] 設定為 [ **啟用**]。
6.  在 [選項] 下，從下拉式功能表中選取 [ **支援** ]， **然後** 按一下 **[套用] 和 [確定]**。


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步驟2：在存取同盟應用程式的電腦上啟用 Kerberos 用戶端支援宣告、複合驗證和 Kerberos 防護

1.  在套用至存取同盟應用程式之電腦的群組原則上，于 **群組原則管理編輯器** 的 [ **電腦** 設定] 底下，依序展開 [ **原則**]、[ **系統管理範本**]、[ **系統**]，然後選取 [ **Kerberos**]。
2.  在 [群組原則管理編輯器] 視窗的右窗格中，按兩下 [ **宣告、複合驗證和 kerberos 防護的 kerberos 用戶端支援]。**
3.  在 [新增] 對話方塊視窗中，將 [Kerberos 用戶端支援] 設定為 [**已啟用** **]，然後按一下** **[套用]**
4.  關閉 [群組原則管理編輯器]。

### <a name="step-3-configure-the-primary-authentication-provider"></a>步驟3：設定主要驗證提供者

1. 針對 AD FS 內部網路設定，將主要驗證提供者設定為 [ **Windows 驗證** ]。
2. 在 AD FS 管理] 的 [**驗證原則**] 底下，選取 [**主要驗證**]，然後按一下 [**全域設定**] 下的 [**編輯**
3. 在 [在 **內部** 網路 **編輯全域驗證原則**] 下，選取 [ **Windows 驗證**]。
4. 按一下 [套用 **] 和 [確定]**。
5. 使用 PowerShell 時，您可以使用 **get-adfsglobalauthenticationpolicy** Cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在 WID 型伺服器陣列中，必須在主要 AD FS 伺服器上執行 PowerShell 命令。
>在以 SQL 為基礎的伺服器陣列中，可以在伺服器陣列成員的任何 AD FS 伺服器上執行 PowerShell 命令。

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步驟4：在 >msds-supportedencryptiontypes 屬性上啟用複合驗證位

1.  在您指定用來執行 AD FS 服務的帳戶上，使用 **Uninstall-adserviceaccount** PowerShell Cmdlet 來啟用 >msds-supportedencryptiontypes 屬性的複合驗證位。

>[!NOTE]
>如果您變更服務帳戶，則必須執行 **set-aduser-compoundIdentitySupported： $true** Windows PowerShell Cmdlet 以手動方式啟用複合驗證。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true
```
2. 重新開機 ADFS 服務。

>[!NOTE]
>一旦將 ' CompoundIdentitySupported ' 設為 true，在新的伺服器上安裝相同的 gMSA (2012R2/2016) 會失敗，並出現下列錯誤– **Install-uninstall-adserviceaccount：無法安裝服務帳戶。錯誤訊息：「提供的內容不符合目標**」。
>
>**解決方案**：暫時將 CompoundIdentitySupported 設定為 $false。 此步驟會導致 ADFS 停止發出 WindowsDeviceGroup 宣告。
Set-ADServiceAccount 身分識別「ADFS 服務帳戶」-CompoundIdentitySupported： $false 在新的伺服器上安裝 gMSA，然後啟用 CompoundIdentitySupported 回 $True。
停用 CompoundIdentitySupported 後再重新啟用，不需要重新開機 ADFS 服務。

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步驟5：更新 Active Directory 的 AD FS 宣告提供者信任

1. 更新 Active Directory 的 AD FS 宣告提供者信任，以包含 ' WindowsDeviceGroup ' 宣告的下列「傳遞」宣告規則。
2.  在 **AD FS 管理**] 中，按一下 [ **宣告提供者信任** ]，然後在右窗格中，以滑鼠右鍵按一下 **Active Directory** ，然後選取 [ **編輯宣告規則**]。
3.  在 [ **編輯適用于 Active Director** 的宣告規則] 中，按一下 [ **新增規則**]。
4.  在 [ **新增轉換宣告規則]** 中，選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
5.  新增顯示名稱，然後從 [**傳入宣告類型**] 下拉式清單中選取 [ **Windows 裝置群組**]。
6.  按一下 [完成] 。  按一下 [套用 **] 和 [確定]**。


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步驟6：在需要 ' WindowsDeviceGroup ' 宣告的信賴憑證者上，新增類似的「傳遞」或「轉換」宣告規則。
2. 在 **AD FS 管理**] 中，按一下 [信賴憑證者 **信任** ]，在右窗格中，以滑鼠右鍵按一下您的 RP，然後選取 [ **編輯宣告規則**]。
3. 在 [ **發行轉換規則** ] 上，按一下 [ **新增規則**]。
4. 在 [ **新增轉換宣告規則]** 中，選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
5. 新增顯示名稱，然後從 [**傳入宣告類型**] 下拉式清單中選取 [ **Windows 裝置群組**]。
6. 按一下 [完成] 。  按一下 [套用 **] 和 [確定]**。

## <a name="validation"></a>驗證
若要驗證版本的 ' WindowsDeviceGroup ' 宣告，請使用 .Net 4.6 建立測試宣告感知應用程式。 使用 WIF SDK 4.0。
將應用程式設定為 ADFS 中的信賴憑證者，並使用上述步驟中所指定的宣告規則來更新它。
使用 ADFS 的 Windows 整合式驗證提供者向應用程式進行驗證時，會建立下列宣告。
![驗證](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

電腦/裝置的宣告現在已可供使用，以取得更豐富的存取控制。

例如，下列 **AdditionalAuthenticationRules** 會告訴 AD FS 叫用 MFA （驗證使用者不是安全性群組 "-1-5-21-2134745077-1211275016-3050530490-1117" 的成員）和電腦 (的使用者從) 不是安全性群組 "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup) " 的成員

但是，如果符合上述任何條件，請勿叫用 MFA。

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
