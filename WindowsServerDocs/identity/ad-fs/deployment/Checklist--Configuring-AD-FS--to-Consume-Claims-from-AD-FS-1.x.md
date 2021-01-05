---
description: 深入瞭解：檢查清單：設定 AD FS 使用 AD FS 1.x 的宣告
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 檢查清單-設定舊版 AD FS 以取用 AD FS 1.x 的宣告
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 6d58a567d4aeed4193d10415e81af0aa1b5e48f8
ms.sourcegitcommit: e2dadc9b0c227a489a945bbc531aca5e101f18cd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97801662"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>檢查清單：設定 AD FS 使用 AD FS 1.x 的宣告


## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>檢查清單：設定 AD FS 使用 AD FS 1.x 的宣告
這份檢查清單包括在 \( \) Windows Server 2012 中設定 Active Directory 同盟服務 AD FS 同盟服務，以取用 AD FS 1 傳送的宣告所需的工作。*x* 同盟服務。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![核取 [標記] 圖示、設定 AD FS 使用宣告。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定 AD FS 使用 AD FS 1.x 的宣告**

|Task|參考|
|--------|-------------|
|規劃 Windows Server 2012 與舊版 AD FS AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。|![圖示，規劃與 AD FS 1.x 的互通性。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃與 AD FS 1.x 的互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
| 您必須先在 AD FS 同盟服務中建立宣告提供者信任，才能與舊版 AD FS 交互操作。 **注意：** 您無法建立 AD FS 1 的信任。*x* 同盟服務使用同盟中繼資料。<p>當您使用右側連結中的程式設定信任時，您必須在 [新增宣告提供者信任] 中執行下列動作，才能設定此信任，使其與 AD FS 1 交互操作。*x* 同盟服務：<p>1. 在 [ **選取資料來源** ] 頁面上，選取 **[手動輸入信賴憑證者信任的相關資料**]。<br />2. 在 [ **選擇設定檔** ] 頁面上，選取 [ **AD FS 1.0 和1.1 設定檔**]。<br />3. 在 [**設定 URL** ] 頁面的 [ **WS \- 同盟被動式 URL**] 底下，輸入 AD FS 1 中定義的 **同盟服務端點 URL** 。合作夥伴的 x 同盟服務。<br />4. 在 [ **設定識別碼** ] 頁面的 [ **宣告提供者信任識別碼**] 下，輸入 AD FS 1 中定義的 **同盟服務 URI** 。合作夥伴的 *x* 同盟服務。|![圖示，手動建立宣告提供者信任，](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[手動建立宣告提供者信任](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|
| 在您稍早建立的宣告提供者信任上，您必須建立宣告規則，以取得從 AD FS 1.x 同盟服務傳入的宣告，並將其傳遞、篩選或轉換成名稱識別碼宣告類型。<p>當您傳遞、篩選或轉換的名稱識別碼宣告類型時，它可以用來做為另一項規則或規則的輸入，讓 Windows Server 2012 中的 AD FS 同盟服務能夠理解和取用它。|![使用 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則來傳送 AD FS 1.X 相容](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)宣告|
| 請聯絡 AD FS 1 的系統管理員。*x* 同盟服務並擁有 AD FS 1 的系統管理員。*x* 同盟服務設定新的資源夥伴信任。 此外，請將同盟服務屬性中的同盟服務 URI \( \) 、同盟服務端點 URL，以及 \- \( 僅限公開金鑰的匯出權杖簽署憑證 \) 檔案提供給該系統管理員。 系統管理員將需要這些專案來設定信任。|N\/A|
