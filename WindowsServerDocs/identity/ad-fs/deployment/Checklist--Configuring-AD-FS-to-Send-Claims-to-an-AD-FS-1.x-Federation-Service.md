---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: "檢查清單-設定 AD FS 使用 AD FS 從宣告 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>檢查清單︰ 設定 AD FS 傳送給 AD FS 1.x 同盟服務宣告

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>檢查清單︰ 設定宣告 AD FS 來傳送給 AD FS 1.x 同盟服務  
此檢查清單會包含所需的設定您的 Active Directory 同盟服務 \(AD FS\) 同盟服務，傳送宣告可以 AD FS 1 來了解 Windows Server 2012 中的工作。*x*同盟服務。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![設定宣告傳送給 AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定宣告 AD FS 來傳送給 AD FS 1.x 同盟服務**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|跨平台與 Windows Server 2012 中的 AD FS 舊版 AD FS 計劃以及了解更多有關名稱 ID 宣告類型。|![設定宣告傳送給 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 使用的跨平台 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|您也可以得到與舊版 AD FS 交互操作之前，您必須先建立信賴廠商信任 AD FS 1 AD FS 同盟服務中。*x*同盟服務。 **注意：**您無法使用 AD FS 1 中建立信任關係。*x*使用聯盟中繼資料同盟服務。<br /><br />當您設定程序使用中的直接連結信任時，您必須完成以下新增可以廠商信任精靈中交互操作 AD FS 1 信任此設定。*x*同盟服務：<br /><br />1.在**選取資料來源**頁面上，選取 [**輸入資料可以手動廠商信任**。<br />2.在**選擇設定檔**頁面上，選取**的設定檔 AD FS 1.0 和 1.1**。<br />3.在**設定的 URL**頁面上，在**WS\-聯盟被動式 URL**，輸入**同盟服務端點 URL** AD FS 1 中所定義。*x*的合作夥伴同盟服務。<br />4.在**設定識別碼**頁面上，在**Relying 部分信任識別碼**，輸入**同盟服務 URI** AD FS 1 中所定義。*x*的合作夥伴同盟服務。|![設定宣告傳送給 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[可以廠商信任手動建立](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|在您先前建立的依賴廠商信任，您必須建立理賠要求將需要連入宣告擷取自屬性存放區與通過、篩選或轉換成名稱 ID 規則宣告類型，可以了解，並由 AD FS 1。*x*同盟服務。 **注意：**您建立本規則之前，請確定您建立此規則宣告規則集合有第一次從屬性存放區擷取輕量型 Directory 存取通訊協定 \(LDAP\) 屬性理賠要求前出現的規則。 做為您建立傳送給 AD FS 1 規則輸入，將會使用此理賠要求。*x*\-compatible 理賠要求。 如需如何建立規則解壓縮 LDAP 屬性，請查看[建立規則為宣告傳送 LDAP 屬性，](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定宣告傳送給 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立傳送給 AD FS 規則 1.x 相容宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|請連絡 1 AD FS 管理員。*x*同盟服務和 AD FS 1 您的系統管理員。*x*同盟服務設定新 account 信任合作夥伴。 同時，提供同盟服務 URI 的系統管理員 \（以同盟服務 properties\)，WS\-聯盟被動式端點 URL \（同盟服務端點 URL\），及匯出 token\ 簽署憑證檔案 \（的公用按鍵 only\)。 該系統管理員必須設定信任的這些項目。|A N\ 日|  
  

