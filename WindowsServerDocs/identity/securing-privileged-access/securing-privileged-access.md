---
title: "保護的存取權限"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eb83903204b00ef6c1eb116554ec54bc2211a399
ms.sourcegitcommit: 7b01b54032ec56432116626e08fbd92508c3a7d5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2018
---
# <a name="securing-privileged-access"></a>保護的存取權限

>適用於：Windows Server 2016

保護權限存取權是現代化在組織中建立的企業資產安全性保證重大第一個步驟。 大部分或全部商務資產是在組織中的安全性，而定的特殊權限帳號，管理管理 IT 系統完整性。 攻擊者充滿網路的目標帳號這些和其他元素，以快速存取目標的資料與系統使用認證竊取攻擊特殊權限存取的[Pass--Hash 和 Pass 票證](https://www.microsoft.com/pth)。

保護管理存取針對判斷對手需要您拍攝以完成而重視方法來找出這些系統的風險。 這個圖描述分隔並保護管理此藍圖中的三個階段建議：

![建議的三個階段分隔和保護管理，在這個藍圖的簡圖](../media/securing-privileged-access/PAW_LP_Fig1.JPG)

藍圖目標：

-   **2 至 4 星期計畫**： 快速減少最常使用的攻擊技術

-   **1-3 個月計畫**： 組建可見性的系統管理員活動控制項

-   **6 + 每月方案**： 繼續建置防禦以主動式更多的安全性狀態

Microsoft 建議您有依照此藍圖安全對有心對手特殊權限的存取。 您可以調整此藍圖以符合您現有的功能與您組織中的特定要求。

> [!NOTE]
> 保護權限存取需要各式各樣的項目包括技術元件 （主機防禦、 account 保護、 身分管理等），以及變更處理及系統管理做法知識。

## <a name="why-is-securing-privileged-access-important"></a>為何保護特殊權限存取重要？
在大部分組織中，大部分或全部商務資產的安全性，而定完整性特殊權限帳號，管理管理 IT 系統。 充滿網路-攻擊者會將焦點放在系統的存取權限的目標 Active Directory 快速存取所有組織的資料，例如。

安全性傳統的方式有專注於使用組織網路的輸入與輸出點主要安全性周邊，但的網路安全性效率明顯降低由兩個趨勢：

-   組織裝載資料和行動裝置企業版電腦上的傳統網路邊界外、 裝置，例如行動裝置版的手機與平板電腦、 雲端服務和 BYOD 裝置

-   對手有示範一致的和傳出功能存取工作站放置網路邊界透過網路釣魚和其他網頁和電子郵件的攻擊。

自然取代網路安全性周邊複雜的現代化企業版的方式是組織的身分層的驗證並授權控制項。 特殊權限管理的帳號都是有效的這個新 」 安全性周邊 」 的控制，請務必保護的存取權限：

![圖表顯示組織的身分層](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

取得管理 account 的控制權對手可以使用這些權限追求他們獲得執行目標組織如如下所示：

![圖表顯示對手侵害管理帳號，如何使用這些權限來追求執行目標組織他們獲得](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

如需有關的攻擊，通常會導致系統帳號控制攻擊者類型的詳細資訊，請瀏覽[傳遞 Hash 網站](https://www.microsoft.com/pth)包含白皮書、 視訊等等。

此圖所管理藍圖建立隔離的風險高標準使用者工作網頁瀏覽和存取電子郵件權限的存取工作指出不同 「 通道 」。

![圖表顯示管理隔離的風險高標準使用者工作網頁瀏覽和存取電子郵件權限的存取工作，建立藍圖不同 「 通道 」](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

因為對手可以使用各種不同的方法授權的存取權的控制權，緩和風險需要整體和詳細技術方法此藍圖中所述。 藍圖會隔離及強化環境中建置防禦中的欄數字的每個區域中的防護功能可讓存取權限的項目：

![下表顯示攻擊並防禦欄](../media/securing-privileged-access/PAW_LP_Fig5.JPG)

## <a name="security-privileged-access-roadmap"></a>安全性特殊權限存取藍圖
放到最大的技術，您可能會使用的設計目的是藍圖部署，利用技術重要的目前與未來的安全性，並整合您可能已經部署任何 3 廠商安全性工具。

Microsoft 建議的藍圖分為 3 個階段中：

-   2 至 4 星期計劃-快速減少最常使用的攻擊技術

-   1-3 個月計劃-組建可見性的系統管理員活動控制項

-   6 + 月份計劃-繼續建置防禦以主動式更多的安全性狀態

每個藍圖階段的設計目的是引發的成本和困難攻擊特殊權限的存取您在場所和雲端資產例子。 藍圖已排程最有效和快實作第一次根據我們與這些的攻擊方案實作體驗的優先順序。

> [!NOTE]
> 藍圖的時間軸會大概和根據我們客戶實作體驗。 持續時間會視複雜的環境並變更管理處理程序在組織中而有所不同。

### <a name="security-privileged-access-roadmap-stage-1"></a>安全性特殊權限存取藍圖： 階段 1
1 階段藍圖的焦點是在快速緩和認證竊取和濫用最常使用的攻擊技術。 階段 1 的設計目的是實作大約 2 至 4 星期中，這個圖所示：

![圖顯示 1 階段的設計目的是大約 2 至 4 星期中實作](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

1 階段安全性特殊權限存取藍圖包含這些元件：

**1.不同的系統管理員負責管理工作**

若要協助分開網際網路風險 (網路釣魚攻擊，瀏覽 web) 的系統管理員權限，建立專用的 account 所有的系統管理員權限的人員。 在這其他指導方針隨附於發行爪指示[在此](http://Aka.ms/CyberPAW)。

**2.特殊權限存取工作站 （腳印） 階段 1: Active Directory 系統管理員**

若要協助分開網際網路風險 (網路釣魚攻擊，瀏覽 web) 的網域系統管理員權限，建立廣告系統管理員權限的人員專用特殊權限的存取工作站 （腳印）。 這是爪程式的第一個步驟，而且階段 1 指南發行的[在此](http://Aka.ms/CyberPAW)。

**3.唯一本機系統管理員密碼工作站**

**4.伺服器唯一本機系統管理員密碼**

若要降低對手竊取從本機坡資料庫本機系統管理員 account 密碼 hash 和濫用它攻擊其他電腦的風險，您應該使用圈工具來設定每個工作站和伺服器隨機的唯一密碼，以及在 Active Directory 登記這些密碼。 您可以取得工作站和伺服器使用本機系統管理員密碼方案[在此](http://Aka.ms/LAPS)。

找不到其他指導方針操作圈與腳印環境[在此](http://aka.ms/securitystandards)。

### <a name="security-privileged-access-roadmap-stage-2"></a>安全性特殊權限存取藍圖： 步驟 2
步驟 2 1 階段的組建上防護功能，並其設計可在大約 1-3 個月實作。 在這個圖表描繪這個階段中的步驟：

![顯示步驟 2 階段的簡圖](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

**1.爪階段 2 和 3： 所有系統管理員 」 和其他強化**

若要網際網路風險分開所有特殊權限管理帳號，繼續與您在步驟 1 開始爪，實作專用的工作站所有的存取權限的人員。 此第 2 階段和 3 種指南 maps 發行[在此](http://Aka.ms/CyberPAW)。

**2.時間繫結權限 （不永久管理員）**

若要降低曝光時間的權限提高看見他們的使用，提供權限在使用例如下方的適當方案的時間 (JIT):

-   Active Directory Domain Services (AD DS)，使用 Microsoft 的身分管理員 (MIM) 的[的存取權限管理員 (PAM)](https://technet.microsoft.com/en-us/library/mt150258.aspx)功能。

-   Azure Active Directory 中，使用[Azure AD 特殊權限的身分管理 (PIM)](http://aka.ms/AzurePIM)功能。

**3.多因素時間繫結提高權限**

若要增加系統管理員驗證保證程度，您應該需要之前的權限授與多因素驗證。
這可以完成 MIM PAM 與 Azure AD PIM 使用 Azure 多因素驗證 (MFA)。

**4.俠維護只系統管理員 (JEA)**

若要減少帳號網域管理權限和相關的曝光量，使用中的 PowerShell 執行一般維護作業網域控制站只達到管理 (JEA) 功能。 JEA 技術允許特定的使用者來執行特定管理工作伺服器 （例如網域控制站），而不將它們提供系統管理員權限。 下載從[TechNet](http://aka.ms/JEA)。

**5.較低攻擊網域和網域控制站的 surface**

若要減少對手控制的樹系的機會，您應該減少攻擊可以取得控制的網域控制站或網域控制物件的路徑。 請依照下列指導方針來減少此風險發行[在此](http://aka.ms/HardenAD)。

**6.攻擊偵測**

若要取得可見性到作用中認證竊取和身分攻擊，讓您可以快速回應的事件和損害，部署及設定[Microsoft 進階威脅 Analytics (ATA)](https://www.microsoft.com/ata)。

之前安裝 ATA，您應該確定您以處理主要安全性事件偵測 ATA 可能會有處理程序。

-   事件回應程序設定的詳細資訊，請查看[回應 IT 安全性事件](https://aka.ms/irr)和 「 回應可疑的活動 」 和 「 從違約復原] 的區段[Mitigating Pass--Hash 和其他認證竊取](https://www.microsoft.com/pth)，版本 2。

-   如需有關包括 Microsoft 服務以協助您針對 ATA 產生事件和部署 ATA 準備您 IR 處理程序，請連絡 Microsoft 代表存取[這個頁面](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)。

-   存取[這個頁面](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)的詳細資訊包括 Microsoft 服務，以協助的調查，並從事件復原

-   實作 ATA，請依照下列部署本指南使用[在此](http://aka.ms/ata)。

### <a name="security-privileged-access-roadmap-stage-3"></a>安全性特殊權限存取藍圖： 階段 3
3 階段藍圖組建上防護功能與階段 1 2 加強和範圍上新增防護功能。 在這個圖表視覺描繪階段 3:

![圖表顯示階段 3](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

這些功能將組建上之前的階段的防護功能，並將防禦移動到更多主動式的狀態。

**1.現代化角色與委派模型**

若要減少安全性風險，您應該會重新設計角色與委派模型層型號的規則與相容、 容納雲端服務的系統管理員角色，並整合主旨為系統管理員使用性的各個的層面。 此模型應該利用 JIT 和 JEA 功能的早期階段，以及工作自動化技術部署以達成下列目的。

**2.智慧卡或護照驗證的所有系統管理員**

增加保證層級和系統管理員驗證的可用性，您應該需要裝載 Azure Active Directory 與 （包括帳號聯盟雲端服務） 您 Windows 的伺服器 Active Directory 中的所有系統帳號穩固驗證。

**3.管理員樹系的 Active Directory 系統管理員**

若要提供最穩定的 Active Directory 系統管理員的防護，設定環境，在您的實際執行 Active Directory 中已不安全性相依性隔所有的攻擊，但最受信任的系統 production 環境中。 如需有關 ESAE 架構瀏覽[這個頁面](http://aka.ms/esae)。

**4.程式碼完整性原則 Dc (Server 2016)**

若要限制的網域控制站的對手攻擊操作與管理意外的錯誤未經授權的程式，請設定 Windows Server 2016 的程式碼完整性核心 （驅動程式） 和使用者模式只允許電腦上執行的授權可執行檔 （應用程式）。

**5.護套 Vm virtual dc (Server 2016 HYPER-V Fabric)**

若要利用一樣既有遺失的實體安全性攻擊的保護模擬的網域控制站，使用此新 Server 2016 HYPER-V 功能 Virtual 網域控制站的 Active Directory 機密遭竊，防止其他。 您可以使用此方案，加密代 2 Vm 保護 VM 的資料免於檢查、 遭竊和竄改來儲存和網路系統管理員以及攻擊 HYPER-V 主機系統管理員的 vm 強化存取。

## <a name="am-i-done"></a>我已完成嗎？
完成此藍圖，將會取得穩固特殊權限的存取保護目前目前已知，並且可用於對手攻擊。 很抱歉，安全性威脅的攻擊會不斷演進和 shift，因此我們建議您檢視持續專注於引發成本和降低對手針對您的環境的成功率與安全性。

保護權限存取重要的第一個步驟的企業資產安全性保證建立在現代化組織中，但不會包括的項目，例如原則，作業，安全性資訊，伺服器、 應用程式、 電腦、 裝置、 雲端 fabric 和其他元件提供安全性保證您需要的完整的安全性程式的僅限部分。

建置完成安全性藍圖的詳細資訊，會看到 「 客戶責任和藍圖 > 一節企業 Architects 文件的 Microsoft Cloud 安全性[在此](http://aka.ms/securecustomer)。

適用於互動協助與下列主題的任何 Microsoft 服務的詳細資訊，請連絡您的 Microsoft 代表或瀏覽[這個頁面](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)。

### <a name="related-topics"></a>相關的主題
[試試的頂級： 如何 Pass Hash 和其他形式的認證竊取降低](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft 進階威脅 Analytics](http://aka.ms/ata)

[保護衍生的網域憑證的 Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[裝置 Guard 概觀](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[使用安全管理員工作站保護的高價值資產](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[目前 Probert （通道 9） 與 Windows 10 中的隔離的使用者模式](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[隔離的使用者模式處理程序和記錄與 Windows 10 中的功能 Gabriel （通道 9）](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[在處理程序與目前 Probert （通道 9） 與 Windows 10 隔離的使用者模式中的功能](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[降低認證竊取使用 Windows 10 隔離的使用者模式 (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[讓 Windows Kerberos 中的 [嚴格] \ [KDC 驗證](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[適用於 Windows Server 2012 F:kerberos 驗證中的新功能](https://technet.microsoft.com/library/hh831747.aspx)

[在 Windows Server 2008 R2 逐步 AD ds 驗證機制保證](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[信賴平台模組](https://docs.microsoft.com/en-us/windows/device-security/tpm/trusted-platform-module-overview)
