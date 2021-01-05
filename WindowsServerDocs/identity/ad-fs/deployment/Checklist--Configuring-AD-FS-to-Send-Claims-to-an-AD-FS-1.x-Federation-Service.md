---
description: 深入瞭解：檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 同盟服務
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 檢查清單-將 AD FS 設定為使用 AD FS 1.x 的宣告
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 2e5a52ebbb839f2c85c17972a6fe852dd23036bc
ms.sourcegitcommit: e2dadc9b0c227a489a945bbc531aca5e101f18cd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97801732"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>檢查清單：將 AD FS 設定為將宣告傳送至 AD FS 1.x 同盟服務


## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 同盟服務
這份檢查清單包括在 \( \) Windows Server 2012 中設定 Active Directory 同盟服務 AD FS 同盟服務，以傳送可由 AD FS 1 理解的宣告所需的工作。*x* 同盟服務。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![核取記號圖示、設定 AD FS，將宣告傳送至 AD FS 1.x 同盟服務。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 同盟服務**

|Task|參考|
|--------|-------------|
|規劃 Windows Server 2012 與舊版 AD FS AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。|![圖示，在 Windows Server 2012 與舊版 AD FS 的 AD FS 之間進行互通性規劃。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 1.x 的互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
|您必須先在 AD FS 1 的 AD FS 同盟服務中建立信賴憑證者信任，才能達到與舊版 AD FS 之間的互通性。*x* 同盟服務。 **注意：** 您無法建立 AD FS 1 的信任。*x* 同盟服務使用同盟中繼資料。<p>當您使用右側連結中的程式來設定信任時，您必須在 [新增信賴憑證者信任] 嚮導中進行下列步驟，才能設定此信任以與 AD FS 1 互通。*x* 同盟服務：<p>1. 在 [ **選取資料來源** ] 頁面上，選取 **[手動輸入信賴憑證者信任的相關資料**]。<br />2. 在 [ **選擇設定檔** ] 頁面上，選取 [ **AD FS 1.0 和1.1 設定檔**]。<br />3. 在 [**設定 URL** ] 頁面的 [ **WS \- 同盟被動式 URL**] 底下，輸入 AD FS 1 中定義的 **同盟服務端點 URL** 。合作夥伴的 x 同盟服務。<br />4. 在 [ **設定識別碼** ] 頁面的 [ **信賴零件信任識別碼**] 下，輸入 AD FS 1 中定義的 **同盟服務 URI** 。合作夥伴的 *x* 同盟服務。|![圖示，手動建立信賴憑證者信任。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[手動建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任|
|在您稍早建立的信賴憑證者信任上，您必須建立宣告規則，這些規則會接受從屬性存放區解壓縮的傳入宣告，並傳遞、篩選或轉換成名稱識別碼宣告類型，以供 AD FS 1 辨識和取用。*x* 同盟服務。 **注意：** 建立此規則之前，請確定您用來建立此規則的宣告規則集具有先 \( \) 從屬性存放區解壓縮輕量型目錄存取協定 LDAP 屬性宣告之前的規則。 此宣告將用來作為您所建立的規則的輸入，以傳送 AD FS 1。*x* \-相容的宣告。 如需有關如何建立規則以解壓縮 LDAP 屬性的詳細資訊，請參閱 [建立規則以將 Ldap 屬性作為宣告來傳送](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定 AD FS 以傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則，以傳送 AD FS 1.X 相容的](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)宣告|
|請聯絡 AD FS 1 的系統管理員。*x* 同盟服務並擁有 AD FS 1 的系統管理員。*x* 同盟服務設定新的帳戶夥伴信任。 此外，請在同盟服務屬性中提供該系統管理員同盟服務 URI \( \) 、WS \- 同盟被動端點 URL \( 同盟服務端點 url \) ，以及 \- \( 僅含公開金鑰的匯出權杖簽署憑證檔案 \) 。 該系統管理員會需要這些專案來設定信任。|N\/A|

