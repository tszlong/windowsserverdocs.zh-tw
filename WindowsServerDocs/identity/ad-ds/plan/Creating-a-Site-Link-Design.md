---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: 建立站台連結設計
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0d9f167a7721fd98179c30b83cc758aa2079dcb1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822751"
---
# <a name="creating-a-site-link-design"></a>建立站台連結設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立站台連結設計，以透過站台連結來連接您的網站。 站台連結會反映站連線能力和用來傳輸複寫流量的方法。 您必須使用站台連結來連接網站，讓每個網站上的網域控制站可以複寫 Active Directory 變更。  
  
## <a name="connecting-sites-with-site-links"></a>使用站台連結連線站台

若要使用站台連結來連接網站，請識別您想要與站台連結連線的成員網站，在各自的網站間傳輸容器中建立站台連結物件，然後命名站台連結。 建立站台連結之後，您可以繼續設定站台連結屬性。  
  
建立站台連結時，請確定每個網站都包含在站台連結中。 此外，請確定所有網站都透過其他站台連結彼此連線，以便將變更從任何網站中的網域控制站複寫到其他所有網站。 如果您無法這麼做，目錄服務記錄檔中會產生錯誤訊息，事件檢視器指出未連接網站拓撲。  
  
每當您將網站新增至新建立的站台連結時，請判斷所新增的網站是否為其他站台連結的成員，並視需要變更網站的站台連結成員資格。 例如，如果您在一開始建立網站時，將網站設為預設第一個網站連結的成員，請務必在您將網站新增至新的站台連結之後，從預設的第一個網站連結中移除該網站。 如果您未從預設的第一個站台連結中移除網站，知識一致性檢查程式（KCC）會根據兩個站台連結的成員資格來做出路由決策，這可能會導致路由不正確。  
  
若要識別您想要與站台連結連線的成員網站，請使用您在「地理位置和通訊連結」（DSSTOPO_1 .doc）工作表中記錄的位置和連結位置清單。 如果多個網站彼此具有相同的連線能力和可用性，您可以使用相同的站台連結來連接它們。  
  
「網站間傳輸」容器提供將站台連結對應至連結所使用之傳輸的方式。 當您建立站台連結物件時，您會在 IP 容器中建立它，使站台連結與 IP 傳輸上的遠端程序呼叫（RPC）產生關聯，或使用簡易郵件傳送通訊協定（SMTP）容器（將站台連結與 SMTP 傳輸相關聯）。  
  
> [!NOTE]  
> 未來的 Active Directory Domain Services 版本（AD DS）將不支援 SMTP 複寫;因此，不建議在 SMTP 容器中建立站台連結物件。  
  
當您在各自的網站間傳輸容器中建立站台連結物件時，AD DS 會使用 RPC over IP，在網域控制站之間傳輸站對站和網站間複寫。 為了讓資料在傳輸期間保持安全，RPC over IP 複寫會同時使用 Kerberos 驗證通訊協定和資料加密。  
  
當直接 IP 連線無法使用時，您可以設定網站之間的複寫以使用 SMTP。 不過，SMTP 複寫功能會受到限制，而且需要企業憑證授權單位單位（CA）。 SMTP 只能複寫設定、架構和應用程式目錄分割，而不支援複寫網域目錄分割。  
  
若要為站台連結命名，請使用一致的命名配置，例如 name_of_site1 name_of_site2。 記錄網站的清單、連結的網站，以及在工作表中連接這些網站的站台連結名稱。 如需協助您錄製網站名稱和相關網站連結名稱的工作表，請參閱[Windows Server 2003 部署套件的工作輔助工具](https://go.microsoft.com/fwlink/?LinkID=102558)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services .zip，以及開啟「網站與關聯的網站連結」（DSSTOPO_5 .doc）。  
  
## <a name="in-this-guide"></a>在本指南中

[設定站台連結屬性](Setting-Site-Link-Properties.md)  
