---
title: "開始使用群組管理帳號服務"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cd1e92f93701e5b430d1425fcf9a2f3590d1fe5d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-group-managed-service-accounts"></a>開始使用群組管理帳號服務

>適用於：Windows Server（以每年次管道）、Windows Server 2016


本指南讓和 Windows Server 2012 中使用 [群組管理服務帳號，提供逐步指示與背景資訊。

**本文件**

-   [必要條件](#BKMK_Prereqs)

-   [簡介](#BKMK_Intro)

-   [部署新的伺服器陣列](#BKMK_DeployNewFarm)

-   [新增成員主機現有伺服器陣列](#BKMK_AddMemberHosts)

-   [[管理服務 Account 群組] 屬性的更新](#BKMK_Update_gMSA)

-   [從現有的伺服器發電廠解除委任成員主機](#BKMK_DecommMemberHosts)


> [!NOTE]
> 本主題包含範例 Windows PowerShell cmdlet 可供您將部分所述的程序。 如需詳細資訊，請查看[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。

## <a name="BKMK_Prereqs"></a>必要條件
查看此主題中的區段上[適用於群組管理服務帳號需求](#BKMK_gMSA_Req)。

## <a name="BKMK_Intro"></a>簡介
當 client 的電腦連接到所有伺服器都似乎是 client 相同服務伺服器發電廠使用網路負載平衡 (NLB) 或其他方法是裝載的服務時，然後支援互加好友的驗證，例如 Kerberos 驗證通訊協定無法使用除非服務的所有的執行個體使用相同的原則。 這表示使用相同的密碼日金鑰證明他們的身分，有每個服務。

> [!NOTE]
> 容錯 gMSAs 不支援。 不過，上方叢集服務執行的服務，可以使用 gMSA 或 sMSA 如果他們的 Windows 服務，應用程式集區排定的工作，或原生支援 gMSA 或 sMSA。

服務已可供選擇，下列原則，每一個都有特定的限制。

|原則|範圍|支援服務|密碼管理|
|-------|-----|-----------|------------|
|電腦的 Windows Account 系統|網域|一個網域限於加入伺服器|電腦管理|
|電腦帳號，而 Windows 系統|網域|任何網域結合的伺服器|無|
|Virtual Account|本機|一部伺服器限於|電腦管理|
|Windows 7 獨立管理服務 Account|網域|一個網域限於加入伺服器|電腦管理|
|使用者 Account|網域|任何網域結合的伺服器|無|
|群組多媒體受管理的服務 Account|網域|任何 Windows Server 2012 加入網域的伺服器|管理網域控制站，並擷取主機|

Windows 電腦帳號或 Windows 7 獨立管理服務 Account (sMSA)，或在多個系統 virtual 帳號無法共用。 如果您要分享的伺服器農場上設定服務帳號，您必須選取 [使用者 account 或電腦帳號，除了 Windows 系統。 兩種方式，這些帳號不需要的單點-控制密碼管理功能。 這會每個組織建立更新服務 Active Directory 中按鍵，然後將傳送那些服務的所有執行個體按鍵便宜方案所需位置的問題。

與 Windows Server 2012、 服務或服務的系統管理員不需要管理密碼同步之間使用群組管理服務帳號 (gMSA) 時的服務執行個體。 您提供中廣告的 gMSA，然後設定的支援管理服務帳號服務。 您可以提供 gMSA 使用 *-ADServiceAccount cmdlet 的 Active Directory 模組的一部分。 支援服務主機的身分設定：

-   為 sMSA，因此支援 sMSA 你會支援 gMSA 相同 Api

-   服務的使用服務控制項管理員的身分登入的設定

-   使用應用程式集區 IIS 管理員設定身分服務

-   使用工作排程工作。

### <a name="BKMK_gMSA_Req"></a>適用於群組管理服務帳號需求
下表列出 Kerberos 驗證搭配使用 gMSA 服務的作業系統系統需求。 之後表格列出的 Active Directory 需求。

64 位元架構，才能執行 Windows PowerShell 命令可用來管理管理服務帳號群組。

**系統需求**

|項目|需求|作業系統|
|------|--------|----------|
|Client 的應用程式主機|相容 Kerberos client RFC|在至少 Windows XP|
|使用者 account 網域 Dc|RFC 相容 KDC|在至少 Windows Server 2003|
|共用的服務成員主機|| Windows Server 2012 |
|成員主機網域 Dc|RFC 相容 KDC|在至少 Windows Server 2003|
|gMSA account 網域 Dc| Windows Server 2012 Dc 適用於主機擷取密碼|Windows Server 2012 是可使用某些系統更早版本與 Windows Server 2012 的網域 |
|後端服務主機|RFC 相容 Kerberos 應用程式伺服器|在至少 Windows Server 2003|
|後端服務 account 的網域 Dc|RFC 相容 KDC|在至少 Windows Server 2003|
|Windows PowerShell 的 Active Directory|Active Directory 安裝在本機電腦支援 64 位元架構 （例如，使用遠端伺服器管理工具組） 管理遠端電腦上的 Windows PowerShell| Windows Server 2012 |

**Active Directory Domain 服務需求**

-   需要更新，以建立 gMSA Windows Server 2012 中 gMSA 網域的樹系的 Active Directory 結構描述。

    您可以在執行 Windows Server 2012 」 的網域控制站的安裝或執行 Windows Server 2012 的電腦執行的版本 adprep.exe 更新架構。 物件版本屬性值物件 DATA-CN = 區結構描述 DATA-CN = 設定，俠 = Contoso 特區 = Com 必須 52。

-   提供的新 gMSA account

-   如果您使用由群組，然後新增或現有安全性群組 gMSA 服務主機權限管理

-   如果在管理群組，然後新增或現有安全性群組服務存取控制

-   如果第一個主要根金鑰的 Active Directory 部署，不網域中，或是尚未建立，然後建立它。 在操作 KdsSvc 登入，事件 ID 4004 經過建立的結果。

適用於指示如何建立鍵，查看[建立鍵 Distribution 服務 KDS 根金鑰](create-the-key-distribution-services-kds-root-key.md)。 Microsoft 金鑰 Distribution 服務 (kdssvc.dll) 根金鑰的廣告。

**週期**

伺服器陣列通常使用 gMSA 功能的週期包含下列工作：

-   部署新的伺服器陣列

-   新增成員主機現有伺服器陣列

-   從現有的伺服器發電廠解除委任成員主機

-   解除委任現有伺服器陣列

-   必要時移除伺服器陣列危害的成員主機。

## <a name="BKMK_DeployNewFarm"></a>部署新的伺服器陣列
當部署新的伺服器陣列服務系統管理員必須以判斷：

-   如果服務支援使用 gMSAs

-   如果服務會要求驗證輸入或輸出連接情形

-   使用 gMSA 服務成員主機電腦 account 名稱

-   服務 NetBIOS 名稱

-   主機 DNS 名稱服務

-   服務主體名稱 (Spn) 服務

-   變更密碼間隔 （預設值為 30 天）。

### <a name="BKMK_Step1"></a>步驟 1： 提供管理服務帳號群組
您可以建立 gMSA 才樹系架構已更新至 Windows Server 2012、 主要根金鑰的 Active Directory 部署，及 gMSA 會建立的網域中有一個以上的 Windows Server 2012 俠。

資格在**網域系統管理員**， **Account 電信業者**或功能來建立 msDS-GroupManagedServiceAccount 物件，才能完成下列程序最小值。

#### <a name="BKMK_CreateGMSA"></a>若要建立 gMSA 使用新的 ADServiceAccount cmdlet

1.  Windows Server 2012 網域控制站，從工作列上執行 Windows PowerShell。

2.  Windows PowerShell 命令提示字元中，輸入下列命令，以及然後按 ENTER 鍵。 （Active Directory 模組將會自動載入。）

    **新 ADServiceAccount [-名稱] <string> -只能用<string>[-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Null [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>]-SamAccountName <string> -ServicePrincipalNames < 字串 [>**

    |參數|字串|範例|
    |-------|-----|------|
    |名稱|Account 的名稱|ITFarm1|
    |只能用|服務主機 DNS 名稱|ITFarm1.contoso.com|
    |KerberosEncryptionType|支援的主機伺服器任何加密類型|RC4，AES128，AES256|
    |ManagedPasswordIntervalInDays|密碼變更間隔天 （預設是 30 天如果不提供）|90|
    |PrincipalsAllowedToRetrieveManagedPassword|電腦帳戶成員主機或安全小組的成員主機的成員|ITFarmHosts|
    |SamAccountName|如果無法為服務 NetBIOS 相同名稱|ITFarm1|
    |ServicePrincipalNames|服務主體名稱 (Spn) 服務|http/ITFarm1.contoso.com/contoso.com，http/ITFarm1.contoso.com/contoso、 http/ITFarm1/contoso.com，以 contoso http 日 ITFarm1|

    > [!IMPORTANT]
    > 只可在 [建立設定密碼變更長的時間間隔。 如果您需要變更長的時間間隔，您必須建立新的 gMSA 並將其設定建立的時間。

    **範例**

    即使它們可能會出現換透過以下幾個行因為格式限制，請在同一行、 輸入命令。

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

在成員資格**網域系統管理員**， **Account 電信業者**，或建立 msDS-GroupManagedServiceAccount 物件，才能完成此程序最小值。 適用於使用適當帳號和群組成員資格的詳細資訊，請查看[本機和網域預設群組](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>建立輸出驗證 gMSA 僅使用新的 ADServiceAccount cmdlet

1.  Windows Server 2012 網域控制站，從工作列上執行 Windows PowerShell。

2.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **新 ADServiceAccount [-名稱] <string> -RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < Null [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>]**

    |參數|字串|範例|
    |-------|-----|------|
    |名稱|Account 名稱|ITFarm1|
    |ManagedPasswordIntervalInDays|密碼變更間隔天 （預設是 30 天如果不提供）|75|
    |PrincipalsAllowedToRetrieveManagedPassword|電腦帳戶成員主機或安全小組的成員主機的成員|ITFarmHosts|

    > [!IMPORTANT]
    > 只可在 [建立設定密碼變更長的時間間隔。 如果您需要變更長的時間間隔，您必須建立新的 gMSA 並將其設定建立的時間。

**範例**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>步驟 2： 設定服務的身分應用程式
若要在 Windows Server 2012 中設定服務，查看下列功能文件：

-   IIS 應用程式集區

    如需詳細資訊，請查看[的身分指定的應用程式集區 (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx)。

-   Windows 服務

    如需詳細資訊，請查看[服務](https://technet.microsoft.com/library/cc772408.aspx)。

-   工作

    如需詳細資訊，請查看[工作排程器概觀](https://technet.microsoft.com/library/cc721871.aspx)。

其他服務可能會支援 gMSA。 如何設定這些服務看到適當 product 文件，如需詳細資訊。

## <a name="BKMK_AddMemberHosts"></a>新增成員主機現有伺服器陣列
如果使用安全性群組管理成員主機，新增新成員主機 （的 gMSA 的成員主機的成員） 安全性群組的電腦帳號使用其中一項下列方法。

資格在**網域系統管理員**，或新增安全性群組物件，成員的功能，才能完成這些程序最小值。

-   方法 1: Active Directory 使用者與電腦

    程序如何使用此方法，請查看[電腦 account 新增到群組](https://technet.microsoft.com/library/cc733097.aspx)使用 Windows 介面，和[中 Active Directory 管理中心管理不同網域](manage-different-domains-in-active-directory-administrative-center.md)。

-   方法 2: dsmod

    程序如何使用此方法，請查看[電腦 account 新增到群組](https://technet.microsoft.com/library/cc733097.aspx)使用命令列。

-   方法 3: Active Directory 的 Windows PowerShell cmdlet 新增-ADPrincipalGroupMembership

    程序如何使用此方法，請查看[新增-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx)。

如果使用電腦帳號，尋找現有帳號，並再新增新的電腦 account。

資格在**網域系統管理員**， **Account 電信業者**，或管理 msDS-GroupManagedServiceAccount 物件，才能完成此程序最小值。 使用群組成員資格與帳號適當的詳細資訊，會看到本機和網域預設的群組。

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>新增成員主機使用 [設定-ADServiceAccount cmdlet

1.  Windows Server 2012 網域控制站，從工作列上執行 Windows PowerShell。

2.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **取得-ADServiceAccount [-名稱] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **設定-ADServiceAccount [-名稱] <string> -PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>**

|參數|字串|範例|
|-------|-----|------|
|名稱|Account 名稱|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|電腦帳戶成員主機或安全小組的成員主機的成員|Host1，Host2，Host3|

**範例**

例如，新增成員主機輸入下列命令，，然後按 ENTER 鍵。

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1 Host2 Host3

```

## <a name="BKMK_Update_gMSA"></a>更新群組管理服務 Account 屬性
資格在**網域系統管理員**， **Account 電信業者**，或撰寫 msDS-GroupManagedServiceAccount 物件，才能完成這些程序最小值。

打開 Active Directory 模組適用於 Windows PowerShell，並使用 [設定-ADServiceAccount cmdlet 設定任何屬性。

如需詳細資訊如何設定這些屬性，請查看[設定-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx)於 TechNet Library，或輸入**取得-協助的設定 ADServiceAccount**的 Windows PowerShell 模組 Active Directory 在命令提示文字，然後按 ENTER。

## <a name="BKMK_DecommMemberHosts"></a>從現有的伺服器發電廠解除委任成員主機
資格在**網域系統管理員**，或功能移除安全性群組物件的成員，才能完成這些程序最小值。

### <a name="step-1-remove-member-host-from-gmsa"></a>步驟 1： 移除 gMSA 成員主機
如果使用安全性群組管理成員主機，請移除電腦帳號從 gMSA 的成員主機的成員，使用下列方法的安全性群組成員退役主機。

-   方法 1: Active Directory 使用者與電腦

    程序如何使用此方法，請查看[Delete 電腦 Account](https://technet.microsoft.com/library/cc754624.aspx)使用 Windows 介面，和[中 Active Directory 管理中心管理不同網域](manage-different-domains-in-active-directory-administrative-center.md)。

-   方法 2: drsm

    程序如何使用此方法，請查看[Delete 電腦 Account](https://technet.microsoft.com/library/cc754624.aspx)使用命令列。

-   方法 3： 移除-ADPrincipalGroupMembership 的 Windows PowerShell Active Directory cmdlet

    如需詳細資訊如何執行此動作，請查看[移除-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx)於 TechNet Library，或輸入**取得-協助移除-ADPrincipalGroupMembership**的 Windows PowerShell 模組 Active Directory 在命令提示文字，然後按 ENTER。

如果電腦帳號，藉以擷取現有帳號，並移除的電腦 account 以外的所有再新增。

資格在**網域系統管理員**， **Account 電信業者**，或管理 msDS-GroupManagedServiceAccount 物件，才能完成此程序最小值。 使用群組成員資格與帳號適當的詳細資訊，會看到本機和網域預設的群組。

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>若要使用 [設定-ADServiceAccount cmdlet 成員主機中移除

1.  Windows Server 2012 網域控制站，從工作列上執行 Windows PowerShell。

2.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **取得-ADServiceAccount [-名稱] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **設定-ADServiceAccount [-名稱] <string> -PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>**

|參數|字串|範例|
|-------|-----|------|
|名稱|Account 名稱|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|電腦帳戶成員主機或安全小組的成員主機的成員|Host1 Host3|

**範例**

例如，移除成員主機輸入下列命令，，然後按 ENTER 鍵。

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1 Host3

```

### <a name="BKMK_RemoveGMSA"></a>步驟 2： 移除系統管理服務 Account 群組
從主機上的系統上使用解除安裝-ADServiceAccount 或 NetRemoveServiceAccount API 成員主機中移除的快取的 gMSA 認證。

資格在**系統管理員**，或相當於，才能完成這些程序最小值。

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>若要移除使用解除安裝-ADServiceAccount cmdlet gMSA

1.  Windows Server 2012 網域控制站，從工作列上執行 Windows PowerShell。

2.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **解除安裝 ADServiceAccount < ADServiceAccount >**

    **範例**

    例如移除 gMSA 的快取的認證命名的 ITFarm1 輸入下列命令，，然後按 ENTER 鍵：

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

如需有關解除安裝-ADServiceAccount cmdlet，在 Active Directory 模組的 Windows PowerShell 命令提示字元中，輸入**取得-協助解除安裝-ADServiceAccount**，然後按下 ENTER，或在 TechNet 網站上看到資訊[ADServiceAccount 解除安裝的](https://technet.microsoft.com/library/ee617202.aspx)。



## <a name="BKMK_Links"></a>也了

-   [群組多媒體受管理的服務帳號概觀](group-managed-service-accounts-overview.md)



