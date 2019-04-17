---
title: "Kerberos 驗證的概觀"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9f8878fb959c34663b1e1f3858fa7e0b6e7b6105
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-authentication-overview"></a>Kerberos 驗證的概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

Kerberos 是用來確認使用者或主機的身分驗證通訊協定。 本主題包含在 Windows Server 2012 和 Windows 8 中 F:kerberos 驗證資訊。

## <a name="BKMK_OVER"></a>描述的功能
Windows Server 作業系統實作 Kerberos 5 版本驗證通訊協定與公用按鍵驗證傳輸授權資料，以及委派擴充功能。 實作 Kerberos 驗證 client security 支援提供者 \(SSP\)，而且它可以透過存取安全性支援提供者介面 \(SSPI\)。 初次使用者驗證整合 Winlogon 單一 sign\ 上架構。

與其他 Windows Server 安全性服務執行的網域控制站 Kerberos 金鑰 Distribution 中心 \(KDC\) 整合。 \ [KDC 使用網域的 Active Directory Domain Services 資料庫做為其安全性 account 資料庫。 需要預設 Kerberos 實作網域或樹系的 active Directory Domain Services。

## <a name="kerb_tr_Kerb_Benefits"></a>實用的應用程式
藉由 domain\ 驗證 Kerberos 獲得權益︰

-   **委派的驗證。**

    Windows 作業系統執行的服務存取代表 client 的資源時，可模擬 client 的電腦。 很多時候，服務可以存取本機電腦上的資源完成 client 的工作。 當 client 電腦驗證服務時，請 NTLM 和 Kerberos 通訊協定提供服務的需要模擬 client 本機電腦的授權資訊。 不過，有些分散式應用程式的設計，front\ 後端服務連接到其他電腦上的 back\ 後端服務時，必須使用 client 電腦的身分。 F:kerberos 驗證支援委派機制，可連接到其他服務時，代表它 client 做的服務。

-   **單一登入。**

    使用 Kerberos 驗證網域中或樹系可讓使用者或服務存取資源允許的系統管理員而多個要求的認證。 在初始網域登入透過 Winlogon 之後, Kerberos 每次嘗試存取資源時管理樹系的認證。

-   **交互操作。**

    Microsoft Kerberos V5 通訊協定實作根據網際網路工程設計工作人員 \(IETF\) 建議的曲目 standards\ 規格。 如此一來，Windows 作業系統，在 Kerberos 通訊協定列出跨平台與其他網路 Kerberos 通訊協定使用進行驗證基本知識。 此外，Microsoft 發行 Windows 的通訊協定實作 Kerberos 通訊協定的文件。 文件包含技術需求，限制，相依性，以及 Kerberos 通訊協定 Microsoft 實作 Windows\ 特定通訊協定行為。

-   **伺服器的驗證更有效率。**

    之前 Kerberos，NTLM 驗證無法使用，這需要應用程式連接到網域控制站伺服器驗證每個 client 電腦或服務。 Kerberos 通訊協定，以儲值活動門票取代 pass\ 透過驗證。 不需要移至網域控制站伺服器 \（除非該需要驗證權限屬性憑證 \(PAC\)\)。 而是伺服器可以檢查認證出示 client 驗證 client 電腦。 Client 電腦可以一次取得特定伺服器的憑證，並在網路工作階段登入認證重複使用。

-   **互加好友的驗證。**

    使用 Kerberos 通訊協定，上網結尾處一方可以確認的另一端派對的實體就越高。 NTLM 無法讓戶端驗證伺服器的身分，或讓驗證身分另一部伺服器。 網路環境中伺服器已被視為正版軟體被設計 NTLM 驗證。 Kerberos 通訊協定可讓任何這類假設。

## <a name="see-also"></a>也了
[Windows 驗證的概觀](../windows-authentication/windows-authentication-overview.md)


