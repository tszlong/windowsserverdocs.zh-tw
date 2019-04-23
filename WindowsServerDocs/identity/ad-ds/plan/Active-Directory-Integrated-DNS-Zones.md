---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Active Directory 整合 DNS 區域
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5e0830944ec5d91dace52db337e89211ee362b98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873249"
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory 整合 DNS 區域

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

網域控制站上執行的網域名稱系統 (DNS) 伺服器可以在 Active Directory 網域服務 (AD DS) 中儲存其區域。 如此一來，不需要設定個別的 DNS 複寫拓樸使用一般的 DNS 區域傳輸，因為所有的區域資料透過 Active Directory 複寫會自動複寫。 這可簡化部署 DNS 的程序，並提供下列優點：  
  
-   多個主機會針對 DNS 複寫。 因此，執行 DNS 伺服器服務的網域中任何網域控制站可以寫入他們有權管理的網域名稱的 Active Directory 整合 DNS 區域的更新。 不需要個別的 DNS 區域傳輸拓撲。  
  
-   支援安全動態更新。 安全動態更新可讓系統管理員可以控制哪些電腦更新何種名稱，並覆寫現有的名稱，在 DNS 中時，防止未經授權的電腦。  
  
Windows Server 2008 中 active Directory 整合的 DNS 會將區域資料儲存在應用程式目錄分割。 （沒有任何行為變更從 Windows Server 2003 DNS 與 Active Directory 整合。）在 AD DS 安裝期間，會建立下列 DNS 特定應用程式目錄磁碟分割：  
  
-   全樹系的應用程式目錄分割，稱為 ForestDnsZones  
  
-   針對每個網域、 樹系的全網域的應用程式目錄分割名為 DomainDnsZones  
  
如需有關 AD DS 應用程式磁碟分割中所儲存的 DNS 資訊的詳細資訊，請參閱 < [DNS 技術參考資料](https://go.microsoft.com/fwlink/?LinkId=106636)。  
  
> [!NOTE]  
> 我們建議您安裝 DNS，當您執行 Active Directory 網域服務安裝精靈 (Dcpromo.exe)。 如果您這麼做時，精靈會自動建立 DNS 區域委派。 如需詳細資訊，請參閱 <<c0> [ 部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  


