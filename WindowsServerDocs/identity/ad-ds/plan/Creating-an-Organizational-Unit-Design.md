---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: 建立組織單位設計
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5dca71251ec0766720afe15b7c53a628f0335eb6
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067760"
---
# <a name="creating-an-organizational-unit-design"></a>建立組織單位設計

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

樹系擁有者會負責為其網域建立組織單位 (OU) 設計。 建立 OU 設計包括設計 OU 結構、指派 OU 擁有者角色，以及建立帳戶和資源 Ou。

一開始，請設計您的 OU 結構來啟用管理委派。 OU 設計完成後，您可以建立額外的 OU 結構，將群組原則應用程式新增至使用者和電腦，並限制物件的可見度。 如需詳細資訊，請參閱 [設計群組原則基礎結構](/previous-versions/windows/it-pro/windows-server-2003/cc786524(v=ws.10))。

## <a name="ou-owner-role"></a>OU 擁有者角色
樹系擁有者會為您為網域設計的每個 OU 指定 OU 擁有者。 OU 擁有者是資料管理員，負責控制 Active Directory Domain Services (AD DS) 中的物件子樹。 OU 擁有者可以控制如何委派系統管理，以及如何將原則套用至其 OU 內的物件。 他們也可以建立新的子樹，並在這些子樹內委派 Ou 的管理。

由於 OU 擁有者不會擁有或控制目錄服務的作業，您可以將目錄服務的擁有權和系統管理與物件的擁有權和管理分開，以減少擁有高階存取權的服務系統管理員數目。

Ou 提供系統管理的自主性，以及控制目錄中物件可見度的方式。 Ou 提供與其他資料管理員的隔離，但不會提供服務系統管理員的隔離。 雖然 OU 擁有者可以控制物件的子樹狀結構，但樹系擁有者會保留所有子樹的完整控制權。 這可讓樹系擁有者更正錯誤，例如存取控制清單中 (ACL) 的錯誤，以及在資料管理員終止時回收委派的子樹。

## <a name="account-ous-and-resource-ous"></a>帳戶 Ou 和資源 Ou
帳戶 Ou 包含使用者、群組和電腦物件。 樹系擁有者必須建立 OU 結構來管理這些物件，然後將該結構的控制權委派給 OU 擁有者。 如果您要部署新的 AD DS 網域，請建立網域的帳戶 OU，讓您可以委派網域中帳戶的控制權。

資源 Ou 包含資源和負責管理這些資源的帳戶。 樹系擁有者也負責建立 OU 結構來管理這些資源，以及將該結構的控制權委派給 OU 擁有者。 根據組織內每個群組的需求，視需要建立資源 Ou，以管理資料和設備。

## <a name="documenting-the-ou-design-for-each-domain"></a>記錄每個網域的 OU 設計
組合小組以設計用來委派樹系內資源控制權的 OU 結構。 樹系擁有者可能牽涉到設計流程，而且必須核准 OU 設計。 您也可能需要至少一個服務系統管理員，以確保設計有效。 其他設計小組參與者可能包含將負責處理 Ou 的資料管理員，以及負責管理這些 Ou 的 OU 擁有者。

記錄 OU 設計相當重要。 列出您打算建立的 Ou 名稱。 然後，針對每個 OU，記錄 OU 的類型、OU 擁有者、父 OU (如果適用) ，以及該 OU 的來源。

如需協助您記載 OU 設計的工作表，請從 [Windows Server 2003 部署套件的工作輔助](https://microsoft.com/download/details.aspx?id=9608) 程式下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟「識別每個網域的 ou」 ( # A1) 。

## <a name="in-this-section"></a>本節內容

- [檢閱 OU 設計概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)

- [使用 OU 物件委派管理](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)
