---
title: 網域名稱系統 (DNS)
description: 本主題提供 Windows Server 2016 中 DNS 的概觀
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d4ec63e904dd899a3ddc53a59274ad607136edd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870819"
---
# <a name="domain-name-system-dns"></a>網域名稱系統 (DNS)

>適用於：Windows Server （半年通道），Windows Server 2016

網域名稱系統 (DNS) 是其中一個構成 TCP/IP 通訊協定的業界標準套件，並在一起的 DNS 用戶端和 DNS 伺服器提供 電腦名稱到 IP 位址對應名稱解析服務的電腦和使用者。  
  
> [!NOTE]  
> 本主題中，除了下列的 DNS 內容使用。  
>   
> -   [什麼是 DNS 用戶端的新功能](What-s-New-in-DNS-Client.md)  
> -   [什麼是 DNS 伺服器的新功能](What-s-New-in-DNS-Server.md)  
> -   [DNS 原則案例指南](deploy/DNS-Policy-Scenario-Guide.md)  
> -   視訊：[Windows Server 2016:在 IPAM 中的 DNS 管理](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
在 Windows Server 2016 中，DNS 會是您可以使用伺服器管理員] 或 [Windows PowerShell 命令來安裝伺服器角色。 如果您要安裝新的 Active Directory 樹系和網域，DNS 會自動與 Active Directory 安裝的樹系和網域的全域目錄伺服器。  
  
Active Directory 網域服務 (AD DS) 會使用 DNS，做為網域控制站的位置機制。 任何主體的 Active Directory 作業執行時，例如驗證、 更新或搜尋，電腦使用 DNS 來找出 Active Directory 網域控制站。 此外，網域控制站會使用 DNS，可以找到彼此。  
  
DNS 用戶端服務包含在所有的用戶端和伺服器版本的 Windows 作業系統中，並預設在安裝作業系統時執行。 當您設定 TCP/IP 網路連線的 DNS 伺服器的 IP 位址時，DNS 用戶端會查詢 DNS 伺服器來探索網域控制站，以及電腦名稱解析為 IP 位址。 例如，當網路使用者與 Active Directory 使用者帳戶登入 Active Directory 網域時，DNS 用戶端服務會查詢 DNS 伺服器，以找出 Active Directory 網域的網域控制站。 當 DNS 伺服器回應查詢，並提供給用戶端的網域控制站的 IP 位址時，用戶端連絡網域控制站，可以開始驗證程序。  
  
Windows Server 2016 的 DNS 伺服器和 DNS 用戶端服務會使用 DNS 通訊協定包含 TCP/IP 通訊協定套件中。 DNS 是 TCP/IP 參考模型的應用程式層的一部分，如下圖所示。  
  
![TCP/IP 中的 DNS](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

