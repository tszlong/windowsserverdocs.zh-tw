---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: 電腦命名
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8355b5b54e2ff17a4230c658b63745158f573fce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822811"
---
# <a name="computer-naming"></a>電腦命名

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當執行 Windows 2000、Windows XP、Windows Server 2003、Windows Server 2008 或 Windows Vista 作業系統的電腦加入網域時，根據預設，電腦會指派自己的名稱。 它本身指派的名稱包含電腦的主機名稱（也就是 [系統內容] 中的電腦名稱稱），以及電腦加入之 Active Directory 網域的網域名稱系統（DNS）名稱（也就是系統內容中的主要 DNS 尾碼）。 主機名稱和網域的 DNS 名稱串連就是所謂的完整功能變數名稱（FQDN）。 例如，如果主機名稱為 Server1 的電腦加入網域 corp.contoso.com，則電腦的 FQDN 會是 server1.corp.contoso.com。  
  
如果電腦的 DNS 功能變數名稱是以靜態方式輸入至 DNS 區域，或由整合式 DNS/動態主機設定通訊協定（DHCP）伺服器服務所註冊，則電腦的 FQDN 會與先前註冊的名稱不同。 您可以使用任何名稱來參照電腦。  
  
如需 Active Directory Domain Services （AD DS）中命名慣例的詳細資訊，請參閱電腦、網域、網站和組織單位（[https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)） Active Directory 中的命名慣例。  
  


