---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: "判斷所需的樹系的數目"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a461dbb2b5bf9d2ca1bb6a336cb11ace775fb1cd
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-number-of-forests-required"></a>判斷所需的樹系的數目

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要判斷，您必須部署樹系的數目，您需要仔細找出評估隔離與自主性需求適用於您組織中的每個群組並將這些需求對應至適當的樹系設計模型。  
  
時判斷您的組織中部署的樹系的請考慮下列動作：  
  
-   隔離需求限制您的設計選擇。 因此，如果您找出隔離需求，請確定該群組確實需要資料隔離和的資料自主性不足，無法他們的需求。 請確定您在組織中的各種群組清楚了解隔離和自主性的概念。  
  
-   協調的設計可以漫長處理程序。 它可以很難有關擁有權共識群組，並使用可用資源。 請確定您允許時間不足，無法群組的適當研究他們的需求找出您組織中。 設定穩固期限設計決策與的所有派對建立期限取得共識。  
  
-   判斷要部署的樹系的數目，包括平衡權益成本。 單一樹系模型是最具成本效益的選項，並需要最低的 [系統管理成本。 在組織中可能會想要獨立服務作業，但可能會更具成本效益的希望從打造且受信任的資訊 (IT) 技術群組服務傳遞組織。 這樣自己的資料管理群組而不需要建立新增的服務管理成本。 平衡成本效益可能需要從贊助輸入。  
  
    單一樹系是最簡單的設定來管理。 它可以讓的環境中的最大共同作業因為：  
  
    -   通用列出的單一森林中的所有物件。 因此，不同步跨樹系需要。  
  
    -   不需要管理重複的基礎結構。  
  
-   我們不建議單一樹系的 co-ownership 由兩個獨立及獨立 IT 組織。 在未來的兩個群組 IT 目標可能會變更，，讓他們可以不再接受共用的控制。  
  
-   我們不建議外包以多個合作夥伴之外的服務管理。 多語系有不同的國家或地區群組的組織可能會外包至不同的外部夥伴每個國家或地區的服務管理選擇。 多個外協力廠商無法隔離，因為一個合作夥伴的動作可能會影響服務的其他，很難按住合作夥伴負責層級服務合約。  
  
-   Active Directory domain 只有一個執行個體應該隨時存在。 Microsoft 不支援複製、分割，或從一個網域複製網域控制站在嘗試進行通訊相同的網域第二個。 如需有關這個限制，查看 [下一節。  
  
## <a name="restructuring-limitations"></a>重新建構限制  
公司時取得另一家公司，營業，或 product 行購買公司也可能會想要取得對應 IT 資產從賣家。 具體而言，買方可能會想要部分或所有的網域控制站裝載帳號、電腦帳號，以及對取得商務用資產對應安全性群組。 若要取得儲存賣家 Active Directory 森林中的 IT 資產買方僅限支援的方法如下：  
  
1.  取得唯一之子-森林，包括所有網域控制站和 directory 資料賣家整個森林中的執行個體。  
  
2.  賣家的樹系或網域移轉 directory 所需的資料，其中一或多個購買者網域。 這類移轉的目標可能會完全新的樹系或一或多個現有網域已購買者森林中部署。  
  
此支援限制存在，是因為：  
  
-   每個網域 Active Directory 森林中的時建立的樹系指派獨特的身分。 複製網域控制站原始網域到複製的網域折衷網域及樹系的安全性。 原始網域和複製的網域威脅包含下列類型：  
  
    -   分享的密碼，可以用於存取資源  
  
    -   關於帳號權限的使用者和群組了  
  
    -   對應的電腦名稱的 IP 位址  
  
    -   新增、刪除及修改複製網域中的網域控制站曾建立網域控制站原始網域從網路連接 directory 資訊  
  
-   複製的網域分享一般的安全性身分;因此，信任關係無法建立之間，即使一或多個網域已經重新命名。  
  
## <a name="in-this-section"></a>在本區段中  
  
-   [森林設計模型](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [森林設計模型對應設計需求](Forest-Design-Models.md)  
  
-   [使用組織網域樹系模式](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


