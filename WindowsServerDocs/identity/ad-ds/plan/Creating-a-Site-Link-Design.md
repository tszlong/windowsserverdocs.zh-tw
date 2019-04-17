---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: "建立一個網站連結設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>建立一個網站連結設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立的網站連結設計連接瀏覽網站的網站連結。 網站連結反映間連接及傳送複寫流量方法。 使網域控制站在每個網站可以複寫 Active Directory 變更，您必須連接網站的網站連結。  
  
## <a name="connecting-sites-with-site-links"></a>連接網站的網站連結  
網站連結，連接的網站，找出您想要的網站連結、建立的網站連結物件各自台間傳輸容器，然後命名為網站連結成員網站。 建立的網站連結之後，您就可以設定此網站的連結。  
  
在建立時網站的連結，請確定的網站連結中包含每個網站。 此外，確定所有網站的都連接到彼此透過其他網站的連結，變更可以從中的任何網站網域控制站複製到 [所有其他網站。 如果您無法執行此動作，也在事件檢視器這部該網站拓撲未連接 Directory 服務木頭中的錯誤訊息。  
  
每當您將網站新增到新建立的網站連結，判斷要新增網站其他網站的連結的成員，以及變更如有需要網站的網站連結成員資格。 例如，如果您將網站 Default-First-Site-Link 最初建立網站時，務必將網站從 Default-First-Site-Link 移除之後，您將網站新增到新的網站連結。 如果您不要移除 Default-First-Site-Link 網站，知識一致性檢查程式 (KCC) 將路由根據這兩個網站連結，可能會導致路由不正確的成員資格。  
  
找出您想要使用的網站連結連接成員網站，使用清單中的位置連結錄製」地理位置和通訊連結「(DSSTOPO_1.doc) 試算表中的位置。 如果多個網站相同連接與可用性彼此，您可以使用相同的網站連結連接它們。  
  
台間傳輸容器提供的連結使用傳輸到對應的網站連結。 當您建立一個網站連結物件時，您的 IP 容器，透過 IP 傳輸關聯遠端程序呼叫 (RPC) 的網站連結或簡易郵件傳輸通訊協定 (SMTP) 容器關聯 SMTP 傳輸網站連結中建立它。  
  
> [!NOTE]  
> SMTP 複寫將不支援在未來版本中的 Active Directory Domain Services (AD DS)。因此，不建議的網站連結物件建立 SMTP 容器中。  
  
當您在各間台傳輸容器建立的網站連結物件時，AD DS 會使用透過 IP RPC 網域控制站之間傳送台間和站台間複寫。 在傳送時保護資料安全，透過 IP 複寫 RPC 使用這兩個 Kerberos 驗證通訊協定與資料加密。  
  
無法使用直接 IP 連接時，您可以設定複寫之間使用 SMTP 網站。 不過，SMTP 複寫功能有限且需要企業憑證授權單位。 SMTP 只能複寫設定、架構，以及應用程式 directory 磁碟分割，而且不支援的複寫網域 directory 磁碟分割。  
  
若要命名網站連結、使用一致命名配置，例如 name_of_site1-name_of_site2。 記錄清單的網站、連結的網站和連接試算表中的這些網站的網站連結的名稱。 為協助您錄製網站和相關的網站連結名稱試算表，查看工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及打開 (DSSTOPO_5.doc)」網站和相關聯的網站連結」。  
  
## <a name="in-this-guide"></a>本指南  
[設定的網站連結屬性](Setting-Site-Link-Properties.md)  
  


