---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: 建立組織單位設計
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9ecd0228b50a4e597c3fa1dbd3fdaf1f84ca1d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830399"
---
# <a name="creating-an-organizational-unit-design"></a>建立組織單位設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

樹系擁有者負責建立組織單位 (OU) 設計為自己的網域。 建立 OU 設計的彙總，包括設計 OU 結構中，指派的 OU 的擁有者角色，並建立帳戶和資源的 Ou。  
  
一開始設計您的 OU 結構，來啟用委派的系統管理。 OU 設計完成後，您可以建立應用程式的群組原則的使用者和電腦，或需要限制物件的可見性的其他 OU 結構。 如需詳細資訊，請參閱 < 設計群組原則基礎結構 ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655))。  
  
## <a name="ou-owner-role"></a>OU 擁有者角色  
樹系擁有者指定 OU 擁有者的每個您設計網域的 OU。 OU 擁有者會控制 Active Directory 網域服務 (AD DS) 中的物件的子樹狀結構的資料管理員。 OU 擁有者可以控制管理委派的方式，以及如何將原則套用至物件在其 OU 內。 它們也可以建立新的子樹，並委派系統管理的 Ou 內的子樹。  
  
由於 OU 擁有者不擁有或控制目錄服務的作業，您可以分開擁有權和管理目錄服務的擁有權和管理物件，從數量高的層級的服務管理員存取權限。  
  
Ou 會提供系統管理自治與方法，以控制可見性，目錄中的物件。 Ou 是提供隔離從其他資料的系統管理員，但它們並從服務系統管理員不提供隔離。 雖然 OU 擁有者物件的子樹狀結構的控制，樹系擁有者會保留所有的子樹的完整控制權。 這可讓樹系擁有者來更正錯誤，例如存取控制清單 (ACL)，發生錯誤，並回收委派的子樹，當資料管理員已終止。  
  
## <a name="account-ous-and-resource-ous"></a>帳戶 Ou 與資源 Ou  
帳戶 Ou 包含使用者、 群組和電腦物件。 樹系擁有者必須建立一個 OU 結構，以便管理這些物件，然後將結構的控制項委派給 OU 擁有者。 如果您要部署新的 AD DS 網域，建立網域帳戶 OU，以便您可以委派的網域中帳戶的控制。  
  
資源 Ou 包含資源，以及負責管理這些資源的帳戶。 樹系擁有者也是負責建立 OU 結構來管理這些資源，並且將該結構的控制項委派給 OU 擁有者。 視需要建立資源 Ou 根據組織中的管理 中的自我管理的資料和設備的每個群組的需求。  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>針對每個網域的 OU 設計文件  
召集小組合作設計 OU 結構，您用來委派控制樹系內的資源。 樹系擁有者可能會涉及設計程序，而必須核准 OU 設計。 您也可能會牽涉到至少一個服務系統管理員，以確保設計有效。 其他設計團隊參賽者可能會包含資料工作的系統管理員將需 Ou 與 OU 擁有者會負責管理它們。  
  
請務必記錄 OU 設計。 列出您打算建立 Ou 的名稱。 此外，針對每個 OU 中，文件的 OU、 OU 擁有者、 父系 OU （如果適用），以及該 OU 的來源類型。  
  
為協助您記錄 OU 設計的工作表，請從 工作輔助工具的 Windows Server 2003 Deployment Kit 下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然後開啟 」識別每個網域的 Ou 」 (DSSLOGI_9.doc)。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [檢閱 OU 設計概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [使用 OU 物件委派管理](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


