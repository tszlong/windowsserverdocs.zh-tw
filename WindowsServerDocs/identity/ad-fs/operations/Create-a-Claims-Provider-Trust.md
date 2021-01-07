---
description: 深入瞭解：建立宣告提供者信任
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: 建立宣告提供者信任
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b768a57d3fa5873ed4e64fe0bf531110ca9333c8
ms.sourcegitcommit: 528bdff90a7c797cdfc6839e5586f2cd5f0506b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97977373"
---
# <a name="create-a-claims-provider-trust"></a>建立宣告提供者信任

若要使用 AD FS 管理嵌入式管理單元新增宣告提供者信任， \- 並手動進行設定，請在資源夥伴組織中的資源夥伴同盟伺服器上執行下列程式。

若要完成此程式，至少需要本機電腦上 **Administrators** 的成員資格或同等許可權。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-create-a-claims-provider-trust-manually"></a>手動建立宣告提供者信任

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在 [ **動作**] 底下，按一下 [ **新增宣告提供者信任**]。
![醒目顯示 [新增宣告提供者信任] 動作的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)

3.  在 [歡迎使用] 頁面上按一下 [開始]。
![顯示 [新增宣告提供者信任] 中 [歡迎使用] 頁面的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)

4.  在 [選取資料來源] 頁面上按一下 [手動輸入宣告提供者信任資料]，然後按 [下一步]。
![顯示 [手動輸入宣告提供者信任] 選項的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)

5.  在 [指定顯示名稱] 頁面上，輸入 [顯示名稱]，在 [記事] 底下，輸入此宣告提供者信任的描述，然後按 [下一步]。
![顯示指定顯示名稱之位置的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)

6.  在 [ **設定 URL** ] 頁面上，指定 **WS-同盟被動式 URL** （若適用），然後按 **[下一步]**。
![顯示指定 WS-Federation 被動 U R L 之位置的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)

8. 在 [設定識別碼] 頁面的 [宣告提供者信任識別碼] 底下，輸入適當的識別碼，然後按 [下一步]。
![顯示要在何處新增宣告提供者信任識別碼的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)

9. 在 [設定憑證] 頁面上，按一下 [新增] 找出憑證檔案，將它新增到憑證清單中，然後按 [下一步]。
![顯示如何尋找憑證檔案，並將它新增至憑證清單的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)

10. 在 [準備新增信任] 頁面上，按 [下一步] 儲存您的宣告提供者信任資訊。
![顯示如何儲存您的宣告提供者信任資訊的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)

11. 在 [完成] 頁面上，按一下 [關閉]。 此動作會自動顯示 [編輯宣告規則] 對話方塊。 如需如何繼續為此宣告提供者信任新增宣告規則的詳細資訊，請參閱下列其他參考。
![顯示如何選取 [關閉] 的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>使用同盟中繼資料建立宣告提供者信任
若要加入新的宣告提供者信任，請使用 AD FS 管理嵌入式管理單元，藉由自動從夥伴發佈到區域網路或網際網路的同盟中繼資料匯入有關夥伴的設定資料，在資源夥伴組織中的同盟伺服器上執行下列程式。

>[!NOTE]
>雖然使用具有不合格主機名稱（例如 HTTPs：/myserver）的憑證是很常見的作法 \/ ，但是這些憑證沒有安全性值，而且可讓攻擊者模擬正在發行同盟中繼資料的同盟服務。 因此，在查詢同盟中繼資料時，您應該只使用完整的功能變數名稱，例如 `https://myserver.contoso.com` 。

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在 [ **動作**] 底下，按一下 [ **新增宣告提供者信任**]。
![顯示要在哪裡選取 [新增宣告提供者信任] 的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)

3.  在 [歡迎使用] 頁面上按一下 [開始]。
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)

4.  在 [選取資料來源] 頁面，按一下 [匯入於線上或區域網路上發行的宣告提供者相關資料]。 在 [同盟中繼資料位址] ([主機名稱] 或 [URL]) 中，輸入夥伴的 **同盟中繼資料 URL** 或主機名稱，然後按 **[下一步]**。
![顯示有關宣告提供者在線上或區域網路上發佈的匯入資料選項的螢幕擷取畫面。](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)

5.  在 [指定顯示名稱] 頁面上，輸入 **顯示名稱**，在 [附注] 下輸入此宣告提供者信任的描述，然後按 **[下一步]**。

6.  在 [準備新增信任] 頁面上，按 **[下一步]** 儲存您的宣告提供者信任資訊。

7.  在 [完成]  頁面上，按一下 [關閉] 。 這將會自動顯示 [編輯宣告規則] 對話方塊。 如需如何繼續為此宣告提供者信任新增宣告規則的詳細資訊，請參閱下面的「其他參考」一節。




## <a name="additional-references"></a>其他參考資料
[檢查清單：設定資源夥伴組織](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)

[檢查清單：為宣告提供者信任建立宣告規則](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)

## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)

