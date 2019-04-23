---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 檢查清單-設定使用 AD fs 的宣告的 AD FS 1.x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887029"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>檢查清單：設定 AD FS 來傳送至 AD FS 1.x Federation Service 的宣告

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x Federation Service  
此檢查清單包含設定 Active Directory 同盟服務所需的工作\(AD FS\)來傳送宣告可以理解的 AD fs 1 的 Windows Server 2012 中的 Federation Service。*x* Federation Service。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定 AD FS 來傳送宣告](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x Federation Service**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|規劃 Windows Server 2012 中的 AD FS 與 AD FS 的先前版本之間的互通性，並了解名稱識別碼宣告類型。|![設定 AD FS 來傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃互通性與 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|您可以達到與舊版 AD FS 的互通性之前，您必須先建立一個信賴憑證者信任 AD FS Federation Service AD fs 1 中。*x* Federation Service。 **注意：** 您無法建立信任關係，與 AD FS 1。*x* Federation Service 使用同盟中繼資料。<br /><br />當您設定信任使用右邊的連結中的程序時，您必須執行下列中新增信賴憑證者信任精靈 來設定此信任的 AD FS 1 與交互操作。*x* Federation Service:<br /><br />1.在 **選取資料來源**頁面上，選取**輸入資料的相關信賴憑證者合作對象信任手動**。<br />2.在 **選擇設定檔**頁面上，選取**AD FS 1.0 和 1.1 設定檔**。<br />3.在 **設定 URL**頁面的  **WS\-同盟被動式 URL**，型別**Federation Service 端點 URL** AD FS 1 中所定義。*x*協力廠商同盟服務。<br />4.在 **設定識別碼**頁面的 **信賴憑證者信任組件的識別項**，型別**Federation Service URI** AD FS 1 中所定義。*x*協力廠商同盟服務。|![設定 AD FS 來傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[信賴憑證者合作對象信任手動建立](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|您稍早建立的信賴憑證者合作對象信任，您必須建立宣告規則，以接受連入宣告，從屬性存放區擷取和傳遞、 篩選，或將它們轉換成名稱識別碼宣告類型，可以了解並使用 ADFS 1。*x* Federation Service。 **注意：** 建立此規則之前，請確定您要在其中建立此規則的宣告規則集有第一次擷取輕量型目錄存取通訊協定從前的規則\(LDAP\)從屬性存放區的屬性宣告。 此宣告將用於做為輸入來建立規則，您將 AD FS 1。*x*\-相容宣告。 如需如何建立規則，以擷取 LDAP 屬性的詳細資訊，請參閱[建立規則，以宣告方式傳送 LDAP 屬性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定 AD FS 來傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則來傳送 AD FS 1.x 相容的宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|請連絡系統管理員的 AD FS 1。*x* Federation Service 與 AD FS 1 的系統管理員。*x* Federation Service 設定新的帳戶夥伴信任。 此外，將該系統管理員提供 Federation Service URI\(中的同盟服務屬性\)，WS\-被動同盟端點 URL \(Federation Service 端點 URL\)，與匯出的語彙基元\-簽署憑證檔案\(僅限公開金鑰與\)。 該系統管理員必須設定信任這些項目。|N\/A|  
  

