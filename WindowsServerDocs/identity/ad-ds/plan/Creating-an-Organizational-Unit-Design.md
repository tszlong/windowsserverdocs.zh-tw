---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: 建立組織單位設計
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 96fd3dd2d090ef6b39b99962e6b639bf2abdcb62
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947739"
---
# <a name="creating-an-organizational-unit-design"></a>建立組織單位設計

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

樹系擁有者負責為其網域建立組織單位 (OU) 設計。 建立 OU 設計牽涉到設計 OU 結構、指派 OU 擁有者角色，以及建立帳戶和資源 Ou。

一開始，請設計您的 OU 結構來啟用管理委派。 當 OU 設計完成時，您可以建立額外的 OU 結構，將群組原則的應用程式提供給使用者和電腦，以及限制物件的可見度。 如需詳細資訊，請參閱[設計群組原則的基礎結構](/previous-versions/windows/it-pro/windows-server-2003/cc786524(v=ws.10))。

## <a name="ou-owner-role"></a>OU 擁有者角色
樹系擁有者會為您為網域設計的每個 OU 指定 OU 擁有者。 OU 擁有者是在 Active Directory Domain Services (AD DS) 中控制物件之子樹的資料管理員。 OU 擁有者可以控制管理委派的方式，以及如何將原則套用至其 OU 內的物件。 他們也可以建立新的子樹，並委派管理這些子樹中的 Ou。

因為 OU 擁有者並未擁有或控制目錄服務的作業，所以您可以區分目錄服務的擁有權和管理，以取得物件的擁有權和管理，進而減少具有高階存取權的服務系統管理員數目。

Ou 提供系統管理自主性，以及控制目錄中物件可見度的方法。 Ou 提供與其他資料管理員的隔離，但不提供與服務系統管理員的隔離。 雖然 OU 擁有者對物件的子樹有控制權，但樹系擁有者仍會保有所有子樹狀目錄的完整控制權。 這可讓樹系擁有者更正錯誤，例如存取控制清單中的錯誤 (ACL) ，以及在資料管理員終止時回收委派的子樹。

## <a name="account-ous-and-resource-ous"></a>帳戶 Ou 和資源 Ou
帳戶 Ou 包含 user、group 和 computer 物件。 樹系擁有者必須建立 OU 結構來管理這些物件，然後將結構的控制權委派給 OU 擁有者。 如果您要部署新的 AD DS 網域，請建立網域的帳戶 OU，讓您可以委派網域中的帳戶控制。

資源 Ou 包含資源，以及負責管理這些資源的帳戶。 樹系擁有者也會負責建立 OU 結構來管理這些資源，以及將該結構的控制權委派給 OU 擁有者。 根據組織內每個群組的需求，視需要建立資源 Ou，以管理資料和設備。

## <a name="documenting-the-ou-design-for-each-domain"></a>記錄每個網域的 OU 設計
組合小組來設計 OU 結構，讓您用來委派樹系中資源的控制權。 樹系擁有者可能牽涉到設計程式，而且必須核准 OU 設計。 您也可能需要至少一個服務系統管理員，以確保設計有效。 其他的設計小組參與者可能包括負責處理 Ou 的資料系統管理員，以及負責管理它們的 OU 擁有者。

記錄您的 OU 設計是很重要的。 列出您打算建立的 Ou 名稱。 而且，針對每個 OU，記錄 OU 的類型、OU 擁有者、父 OU (如果適用的) ，以及該 OU 的來源。

如需協助您記錄 OU 設計的工作表，請從[Windows Server 2003 部署套件的作業輔助](https://microsoft.com/download/details.aspx?id=9608)下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟「找出每個網域的 ou」 ( # A1) 。

## <a name="in-this-section"></a>本節內容

- [檢閱 OU 設計概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)

- [使用 OU 物件委派管理](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)
