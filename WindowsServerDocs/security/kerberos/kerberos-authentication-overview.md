---
title: Kerberos Authentication Overview
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 33712dc8502035bd9e47e1d2bdd4583eb8347dec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386312"
---
# <a name="kerberos-authentication-overview"></a>Kerberos Authentication Overview

>適用於：Windows Server (半年通道)、Windows Server 2016

Kerberos 是用來驗證使用者或主機身分識別的驗證通訊協定。 本主題包含 Windows Server 2012 和 Windows 8 中 Kerberos 驗證的相關資訊。

## <a name="BKMK_OVER"></a>功能描述
Windows Server 作業系統會針對公開金鑰驗證、傳輸授權資料及委派，實作 Kerberos 第 5 版的驗證通訊協定和延伸。 Kerberos 驗證用戶端會實作為安全性支援提供者，\(SSP\)，而且可以透過安全性支援提供者介面 \(SSPI\)來存取。 初始使用者驗證會與在架構上的 Winlogon 單一登入\-整合。

Kerberos 金鑰發佈中心 \(KDC\) 會與網域控制站上執行的其他 Windows Server 安全性服務整合。 KDC 會使用網域的 Active Directory Domain Services 資料庫做為它的安全性帳戶資料庫。 在網域或樹系中實作預設的 Kerberos 時，需要 Active Directory 網域服務。

## <a name="kerb_tr_Kerb_Benefits"></a>實際應用
針對以網域\-為基礎的驗證，使用 Kerberos 取得的優點如下：

-   **委派的驗證。**

    在 Windows 作業系統上執行的服務可以在代表用戶端存取資源時模擬用戶端電腦。 在許多情況下，服務可以透過存取本機電腦上的資源，為用戶端完成其工作。 當向服務驗證用戶端電腦時，NTLM 和 Kerberos 通訊協定會提供服務需要的授權資訊，以在本機模擬用戶端電腦。 不過，有些分散式應用程式的設計是為了讓 front\-端服務在連接至其他電腦上的\-結束服務時，必須使用用戶端電腦的身分識別。 Kerberos 驗證支援委派機制，讓服務在連接至其他服務時代表其用戶端。

-   **單一登入。**

    在網域內或樹系中使用 Kerberos 驗證，可以讓使用者或服務存取系統管理員允許的資源，而不用認證的多個要求。 透過 Winlogon 初始登入網域之後，每當資源被嘗試存取時，Kerberos 會管理整個樹系的認證。

-   **互通.**

    Microsoft 的 Kerberos V5 通訊協定執行是以標準\-追蹤規格為基礎，\(IETF\)建議使用網際網路工程任務推動小組。 因此，在 Windows 作業系統中，Kerberos 通訊協定為 Kerberos 通訊協定用於驗證所在的其他網路互通性立下基礎。 此外，Microsoft 發行 Windows 通訊協定文件，以實作 Kerberos 通訊協定。 本檔包含 Microsoft 執行 Kerberos 通訊協定的技術需求、限制、相依性，以及 Windows\-特定的通訊協定行為。

-   **對伺服器進行更有效率的驗證。**

    在 Kerberos 之前，也可以使用 NTLM 驗證，這需要連線到網域控制站的應用程式伺服器，來驗證每個用戶端電腦或服務。 使用 Kerberos 通訊協定時，可續訂會話票證會透過驗證取代 pass\-。 伺服器不需要移至網域控制站 \(除非它需要驗證許可權屬性憑證 \(PAC\)\)。 伺服器可以透過檢查用戶端所呈現的認證，驗證該用戶端電腦。 用戶端電腦可以取得一次特定伺服器的認證，然後在整個網路登入工作階段重複使用這些認證。

-   **相互驗證。**

    在網路連線任一端的一方可以使用 Kerberos 通訊協定來驗證另一端的一方是它所宣稱的實體。 NTLM 不會讓用戶端驗證服務器的身分識別，或讓一部伺服器驗證另一部伺服器的身分識別。 NTLM 驗證是針對假設伺服器為正版的網路環境所設計。 Kerberos 通訊協定不會做這種假設。

## <a name="see-also"></a>請參閱
[Windows 驗證概觀](../windows-authentication/windows-authentication-overview.md)


