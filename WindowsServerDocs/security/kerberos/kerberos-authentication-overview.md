---
title: Kerberos Authentication Overview
description: 瞭解 Windows Server 2012 和 Windows 8 中的 Kerberos 驗證。
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: dd3ce34602f39ea678e78a958d3efaa016c38d74
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113164"
---
# <a name="kerberos-authentication-overview"></a>Kerberos Authentication Overview

>適用於：Windows Server (半年度管道)、Windows Server 2016

Kerberos 是用來驗證的使用者或主機身分識別的驗證通訊協定。 本主題包含有關 Windows Server 2012 和 Windows 8 中的 Kerberos 驗證的資訊。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
Windows Server 作業系統會針對公開金鑰驗證、傳輸授權資料及委派，實作 Kerberos 第 5 版的驗證通訊協定和延伸。 Kerberos 驗證用戶端會實作為安全性支援提供者 \( SSP \) ，並且可透過安全性支援提供者介面 SSPI 來存取 \( \) 。 初始使用者驗證已與 Winlogon 單一登入架構整合 \- 。

Kerberos 金鑰發佈中心 \( KDC \) 會與網域控制站上執行的其他 Windows Server 安全性服務整合。 KDC 使用網域的 Active Directory Domain Services 資料庫做為其安全性帳戶資料庫。 在網域或樹系中實作預設的 Kerberos 時，需要 Active Directory 網域服務。

## <a name="practical-applications"></a><a name="kerb_tr_Kerb_Benefits"></a>實際應用
使用 Kerberos 進行網域驗證所獲得的好處 \- 如下：

-   **委派的驗證。**

    在 Windows 作業系統上執行的服務，可在代表用戶端存取資源時模擬用戶端電腦。 在許多情況下，服務可以透過存取本機電腦上的資源，為用戶端完成其工作。 當向服務驗證用戶端電腦時，NTLM 和 Kerberos 通訊協定會提供服務需要的授權資訊，以在本機模擬用戶端電腦。 不過，有些分散式應用程式的設計，是為了讓前端 \- 服務連線至 \- 其他電腦上的後端服務時，必須使用用戶端電腦的身分識別。 Kerberos 驗證支援委派機制，讓服務在連接至其他服務時代表其用戶端。

-   **單一登入。**

    在網域內或樹系中使用 Kerberos 驗證，可以讓使用者或服務存取系統管理員允許的資源，而不用認證的多個要求。 透過 Winlogon 初始登入網域之後，每當資源被嘗試存取時，Kerberos 會管理整個樹系的認證。

-   **互通性。**

    Microsoft 的 Kerberos V5 通訊協定的執行是根據標準 \- 追蹤規格，建議使用於網際網路工程任務推動的 \( IETF \) 。 因此，在 Windows 作業系統中，Kerberos 通訊協定為 Kerberos 通訊協定用於驗證所在的其他網路互通性立下基礎。 此外，Microsoft 發行 Windows 通訊協定文件，以實作 Kerberos 通訊協定。 本檔包含 Microsoft 的 Kerberos 通訊協定實行的技術需求、限制、相依性和 Windows \- 特定的通訊協定行為。

-   **向伺服器驗證更有效率。**

    在 Kerberos 之前，也可以使用 NTLM 驗證，這需要連線到網域控制站的應用程式伺服器，來驗證每個用戶端電腦或服務。 使用 Kerberos 通訊協定時，可再生會話票證會取代 \- 通過驗證。 \(除非伺服器需要驗證許可權屬性憑證 PAC，否則不需要將伺服器移至網域控制站 \( \) \) 。 伺服器可以透過檢查用戶端所呈現的認證，驗證該用戶端電腦。 用戶端電腦可以取得一次特定伺服器的認證，然後在整個網路登入工作階段重複使用這些認證。

-   **相互驗證。**

    在網路連線任一端的一方可以使用 Kerberos 通訊協定來驗證另一端的一方是它所宣稱的實體。 NTLM 不會讓用戶端驗證服務器的身分識別，或讓一部伺服器驗證另一部伺服器的身分識別。 NTLM 驗證是針對假設伺服器為正版的網路環境所設計。 Kerberos 通訊協定不會做這種假設。

## <a name="see-also"></a>另請參閱
[Windows 驗證總覽](../windows-authentication/windows-authentication-overview.md)


