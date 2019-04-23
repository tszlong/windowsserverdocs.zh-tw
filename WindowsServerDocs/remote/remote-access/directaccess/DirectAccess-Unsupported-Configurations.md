---
title: DirectAccess 不支援的設定
description: 本主題提供一份不支援 Windows Server 2016 中的 DirectAccess 設定。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49fa86883e6a590ac8f7dcf0c724f87af419f88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828849"
---
# <a name="directaccess-unsupported-configurations"></a>DirectAccess 不支援的設定

>適用於：Windows Server （半年通道），Windows Server 2016

開始您的部署，來避免不得不開始重新部署之前，請檢閱下列不支援的 DirectAccess 設定的清單。  

## <a name="bkmk_frs"></a>檔案複寫服務 (FRS) 發佈的群組原則物件 （SYSVOL 複寫）  
請勿在網域控制站執行的群組原則物件 （SYSVOL 複寫） 的通訊群組的檔案複寫服務 (FRS) 所在的環境中部署 DirectAccess。 當您使用 FRS，將不支援 DirectAccess 部署  
  
如果您有執行 Windows Server 2003 或 Windows Server 2003 R2 的網域控制站，您會使用 FRS。 此外，您可能會使用 FRS 如果您先前使用 Windows 2000 Server 或 Windows Server 2003 網域控制站，而且您永遠不會移轉 SYSVOL 複寫從 FRS 分散式檔案系統複寫 (DFS-R)。  
  
如果您使用 FRS SYSVOL 複寫部署 DirectAccess，風險不小心刪除 DirectAccess 群組原則物件，包含的 DirectAccess 伺服器和用戶端設定資訊。 如果刪除這些物件，DirectAccess 部署將會發生中斷，並使用 DirectAccess 的用戶端電腦無法連線到網路。  
  
如果您打算部署 DirectAccess，您必須使用晚於 Windows Server 2003 R2，執行作業系統的網域控制站，而且您必須使用 DFS-R。  
  
如需有關從 FRS 移轉至 DFS R，請參閱[SYSVOL 複寫移轉指南：FRS 到 DFS 複寫](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx)。  
  
## <a name="bkmk_nap"></a>DirectAccess 用戶端的的網路存取保護  
網路存取保護 (NAP) 用來判斷他們授與存取公司網路之前，遠端用戶端電腦是否符合 IT 原則。 NAP Windows Server 2012 R2 中已被取代，並不包含在 Windows Server 2016。 基於這個理由，不建議啟動新部署 DirectAccess 與 NAP 搭配使用。 建議您使用不同的 DirectAccess 用戶端的安全性方法的結束點的控制項。  
  
## <a name="bkmk_multi"></a>Windows 7 用戶端的的多站台支援  
當在多站台部署中，Windows 10 設定 DirectAccess&reg;，Windows&reg; 8.1 和 Windows&reg; 8 的用戶端已連接到最接近的站台的功能。  Windows 7&reg;用戶端電腦沒有相同的功能。 Windows 7 用戶端的站台選取範圍設定為特定站台在原則設定時，這些用戶端一律都會連線至該指定的站台，無論其位置為何。  
  
## <a name="bkmk_user"></a>以使用者為基礎的存取控制  
DirectAccess 原則是，使用者基礎的電腦。 不支援指定 DirectAccess 使用者原則，來控制對公司的網路存取。  
  
## <a name="bkmk_policy"></a>自訂 DirectAccess 原則  
使用 DirectAccess 安裝精靈 」、 「 遠端存取管理主控台中或 「 遠端存取 Windows PowerShell cmdlet，可以設定 DirectAccess。 不支援使用 DirectAccess 安裝精靈 以外的任何方式來設定 DirectAccess，例如直接修改 DirectAccess 群組原則物件，或以手動方式修改伺服器或用戶端上的預設原則設定。 這些修改可能會導致無法使用的組態。  
  
## <a name="bkmk_kerb"></a>KerbProxy 驗證  
當您設定 DirectAccess 伺服器使用開始使用精靈時，DirectAccess 伺服器會自動設定使用 KerbProxy 驗證電腦和使用者驗證。 基於這個原因，您應該只使用開始使用精靈部署單一站台在只有 Windows 10&reg;，部署 Windows 8.1 或 Windows 8 用戶端。  
  
此外，下列功能不應使用 KerbProxy 驗證：  
  
-   負載平衡使用外部負載平衡器或 Windows 載入   
    平衡器  
  
-   智慧卡或單次密碼 (OTP) 所需位置的雙因素驗證  
  
如果您啟用 KerbProxy 驗證不支援下列的部署計劃：  
  
-   多站台。  
  
-   DirectAccess 支援 Windows 7 用戶端。  
  
-   強制通道。 若要確保當您使用強制通道時，未啟用 KerbProxy 驗證，請執行精靈時設定下列項目：  
  
    -   啟用強制通道  
  
    -   啟用 DirectAccess Windows 7 用戶端  
  
> [!NOTE]  
> 針對先前的部署，您應該使用 [進階組態精靈]，這會透過憑證為基礎的電腦和使用者驗證的雙通道設定。 如需詳細資訊，請參閱 <<c0> [ 部署單一 DirectAccess 伺服器使用進階設定](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
## <a name="bkmk_isa"></a>使用 ISATAP  
ISATAP 是一種轉換技術，可提供在僅 IPv4 的公司網路中的 IPv6 連線。 它僅限於使用單一 DirectAccess 伺服器部署中，小型和中型大小的組織，並且允許遠端管理 DirectAccess 用戶端。 如果 ISATAP deployedin 多站台、 負載平衡，或多重網域環境，您必須將它移除，或將它移至原生的 IPv6 部署中，您在設定 DirectAccess 之前。  
  
## <a name="bkmk_iphttps"></a>IPHTTPS 和一次性密碼 (OTP) 的端點組態  
當您使用 IP-HTTPS 時，IP-HTTPS 連線必須終止在 DirectAccess 伺服器上，不在其他裝置，例如負載平衡器上。 同樣地，必須終止單次密碼 (OTP) 驗證時就會建立出頻外安全通訊端層 (SSL) 連線，DirectAccess 伺服器上。 這些連線的端點之間的所有裝置必須都設定為傳遞的模式。  
  
## <a name="bkmk_ft"></a>使用 OTP 驗證的強制通道  
請勿部署 OTP 和強制通道，雙因素驗證的 DirectAccess 伺服器或 OTP 驗證將會失敗。 需要 DirectAccess 伺服器和 DirectAccess 用戶端之間的頻外安全通訊端層 (SSL) 連線。 此連線需要傳送 DirectAccess 通道外部流量的豁免。 在強制通道設定中，所有流量必須都通過 DirectAccess 通道，並建立通道後，系統即允許沒有豁免。 基於這個原因，它不支援在強制通道設定中有 OTP 驗證。  
  
## <a name="bkmk_rodc"></a>部署 DirectAccess 的唯讀網域控制站  
DirectAccess 伺服器必須能夠存取讀寫網域控制站，並無法正常使用唯讀網域控制站 (RODC)。  
  
讀寫網域控制站是必要的有許多原因，包括下列：  
  
-   在 DirectAccess 伺服器上，為讀寫網域控制站，才能開啟 遠端存取 Microsoft Management Console (MMC)。  
  
-   DirectAccess 伺服器必須讀取和寫入至 DirectAccess 用戶端與 DirectAccess 伺服器群組原則物件 (Gpo)。  
  
-   DirectAccess 伺服器會讀取並寫入用戶端 GPO，特別是從主要網域控制站模擬器 (PDCe)。  
  
因為這些需求，而不會部署 DirectAccess 與 RODC。  
  


