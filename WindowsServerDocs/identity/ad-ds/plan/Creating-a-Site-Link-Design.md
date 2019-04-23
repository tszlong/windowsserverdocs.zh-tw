---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: 建立站台連結設計
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4e0607cf66d41e1747b108a3ecc10562120d9174
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861929"
---
# <a name="creating-a-site-link-design"></a>建立站台連結設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立與站台連結連線站台的站台連結設計。 站台連結會反映的站台間的連線能力和用來傳送複寫流量的方法。 每個站台中的網域控制站可以複寫 Active Directory 的變更，您必須連接站台的站台連結。  
  
## <a name="connecting-sites-with-site-links"></a>使用站台連結連線站台

若要使用站台連結連線站台，找出您想要連接的站台連結、 在個別站台間傳輸容器中，建立站台連結物件，然後命名為站台連結的成員網站。 建立站台連結之後，您可以繼續將站台連結屬性。  
  
在建立站台連結，請確定每個站台，會包含在站台連結。 此外，確定所有站台會連線到彼此透過其他站台連結，讓所做的變更可以從任何站台中的網域控制站複寫到所有其他站台。 如果您無法執行這項操作，目錄服務記錄檔中的事件檢視器可讓您指出確認未連線的站台拓撲會產生錯誤訊息。  
  
每當您將網站新增到新建立的站台連結時，判斷要加入的站台是否為成員的其他站台連結，並變更如有需要的站台的站台連結成員資格。 例如，如果您讓站台預設值為第一個站台-連結的成員一開始建立網站時，務必從預設值為第一個-站台連結移除站台之後您將網站新增至新的站台連結。 如果您不要移除站台預設值為第一個站台-連結，知識一致性檢查程式 (KCC) 將這兩個站台連結，這會導致不正確地路由的成員資格為基礎的路由決策。  
  
若要識別您想要與站台連結連線的成員網站，使用位置和連結在 「 地理位置和通訊連結 」 (DSSTOPO_1.doc) 工作表中所記錄的位置的清單。 如果多個站台具有相同的連線，且彼此的可用性，您可以將它們連接相同的站台連結。  
  
站台間傳輸容器會提供方法來對應至連結使用的傳輸的站台連結。 當您建立站台連結物件時，您在它的 IP 容器，其會將關聯的遠端程序呼叫 (RPC) 的站台連結，透過 IP 傳輸或 Simple Mail Transfer Protocol (SMTP) 容器，這會將站台連結至 smtp傳輸。  
  
> [!NOTE]  
> 在 Active Directory 網域服務 (AD DS); 的未來版本中，不支援 SMTP 複寫因此，不建議在 [SMTP] 容器中建立站台連結物件。  
  
當您在個別站台間傳輸容器中建立站台連結物件時，則 AD DS 會使用透過 IP RPC 傳輸網域控制站之間的站台間和站台內複寫。 若要在傳輸時保護資料安全，RPC over IP 複寫會使用這兩個 Kerberos 驗證通訊協定和資料加密。  
  
無法使用直接的 IP 連線時，您可以設定要使用 SMTP 站台間複寫。 不過，SMTP 複寫功能有限，而且需要企業憑證授權單位 (CA)。 SMTP 設定、 架構以及應用程式目錄分割只能複寫及不支援的網域目錄分割複寫。  
  
若要命名的站台連結，請使用一致的命名配置，例如 name_of_site1 name_of_site2。 記錄站台連結的站台及連接這些工作表中的站台的站台連結名稱的清單。 若要協助您在將記錄站台名稱及相關聯的站台連結名稱為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，及開啟 「 站台及相關聯的站台連結 」 (DSSTOPO_5.doc)。  
  
## <a name="in-this-guide"></a>本指南內容

[設定站台連結內容](Setting-Site-Link-Properties.md)  
