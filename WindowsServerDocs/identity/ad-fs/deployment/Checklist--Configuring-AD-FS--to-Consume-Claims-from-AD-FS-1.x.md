---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: "檢查清單-設定 AD FS 使用 AD FS 從宣告 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>檢查清單︰ 設定 AD FS 使用 AD FS 從宣告 1.x

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>檢查清單︰ 設定 AD FS 使用 AD FS 從宣告 1.x  
此檢查清單會包含所需的設定您的 Active Directory 同盟服務 \(AD FS\) 同盟服務使用宣告 AD FS 1 所傳送的 Windows Server 2012 中的工作。*x*同盟服務。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![使用宣告 AD FS 從](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定 AD FS 使用 AD FS 從宣告 1.x**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![使用 AD FS 從宣告](media/icon_checkboxo.gif)|規劃跨平台與 Windows Server 2012 中的 AD FS 舊版 AD FS，並了解更多有關名稱 ID 宣告類型。|![使用宣告 AD FS 從](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 使用的跨平台 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![使用 AD FS 從宣告](media/icon_checkboxo.gif)|您可以使用舊版 AD FS 交互之前，您必須先建立宣告 AD FS 同盟服務提供者信任。 **注意：**您無法使用 AD FS 1 中建立信任關係。*x*使用聯盟中繼資料同盟服務。<br /><br />當您設定程序使用中的直接連結信任時，您必須完成以下新增宣告提供者信任精靈交互操作 AD FS 1 信任此設定。*x*同盟服務：<br /><br />1.在**選取資料來源**頁面上，選取 [**輸入資料可以手動廠商信任**。<br />2.在**選擇設定檔**頁面上，選取**的設定檔 AD FS 1.0 和 1.1**。<br />3.在**設定的 URL**頁面上，在**WS\-聯盟被動式 URL**，輸入**同盟服務端點 URL** AD FS 1 中所定義。*x*的合作夥伴同盟服務。<br />4.在**設定識別碼**頁面上，在**宣告提供者信任識別碼**，輸入**同盟服務 URI** AD FS 1 中所定義。*x*的合作夥伴同盟服務。|![使用宣告 AD FS 從](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[宣告提供者信任手動建立](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![使用 AD FS 從宣告](media/icon_checkboxo.gif)|在您先前建立的宣告提供者信任，您必須建立理賠要求規則的需要從 AD FS 傳入的宣告 1.x 同盟服務和通過、篩選，或將它們轉換成名稱 ID 宣告類型。<br /><br />當名稱 ID 宣告類型已已經通過，篩選掉，或轉換，它可以當做輸入到另一個規則或規則，了解並由 AD FS 同盟服務 Windows Server 2012 中。|![使用宣告 AD FS 從](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立傳送給 AD FS 規則 1.x 相容宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![使用 AD FS 從宣告](media/icon_checkboxo.gif)|請連絡 1 AD FS 管理員。*x*同盟服務和 AD FS 1 您的系統管理員。*x*設定新的資源合作夥伴信任同盟服務。 同時，提供同盟服務 URI 的系統管理員 \（以同盟服務 properties\)，同盟服務端點 URL，及匯出 token\-簽署憑證檔案 \（的公用按鍵 only\)。 系統管理員必須設定信任的這些項目。|A N\ 日|  
  

