---
title: 規劃多樹系部署
description: 本主題是部署在 Windows Server 2016 中的多樹系環境中的遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5317ac52d77a9e70711f915a974a7879f24603bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836859"
---
# <a name="plan-a-multi-forest-deployment"></a>規劃多樹系部署

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明在多樹系部署中設定遠端存取時所需的規劃步驟。  
  
## <a name="prerequisites"></a>先決條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  
  
-   需具備雙向信任。  
  
## <a name="plan-trust-between-forests"></a>規劃樹系之間的信任  
當您決定啟用新樹系的資源存取、允許新樹系的用戶端使用 DirectAccess，或者從新樹系新增遠端存取伺服器做為遠端存取部署的進入點時，必須確定已在兩個樹系之間設定完全信任 (也就是雙向可轉移信任)，請參閱 [信任類型](https://technet.microsoft.com/library/cc775736.aspx)。 樹系之間的完全信任是多樹系部署的先決條件，可允許系統管理員執行如下的作業：在新樹系中編輯 GPO、使用新樹系的安全性群組做為用戶端安全性群組、對新樹系中的電腦執行遠端呼叫 (WinRM、RPC)，以及驗證來自新樹系的遠端用戶端。  
  
## <a name="plan-remote-access-administrator-permissions"></a>規劃遠端存取管理員權限  
當您設定遠端存取時，它會在每個包含遠端存取伺服器或用戶端的網域中更新 GPO，有時也會建立 GPO。 在多樹系環境中，做為單一樹系環境，遠端存取管理員必須有權寫入與修改 DirectAccess GPO 及其安全性篩選器，以及選擇性地具備在所有內含的樹系中建立 DirectAccess GPO 連結的權限。 不論遠端存取管理員屬於哪一個樹系，都需要具備這些權限。  
  
此外，遠端存取管理員必須是所有遠端存取伺服器上的本機系統管理員，而這些伺服器會包含於已新增來做為原始遠端存取部署進入點之新樹系的遠端存取伺服器上。  
  
## <a name="ClientSG"></a>規劃用戶端安全性群組  
您至少必須針對新樹系中的 DirectAccess 用戶端電腦，在新樹系中設定一個安全性群組。 這是因為單一安全性群組無法包含來自數個樹系的帳戶。  
  
> [!NOTE]  
> -   DirectAccess 需要至少一個 Windows 10&reg;或 Windows&reg; 8 的用戶端安全性群組，每個樹系。 不過，建議您有 Windows 10 或 Windows 8 包含 Windows 10 或 Windows 8 的用戶端的每個網域的用戶端安全性群組。  
> -   啟用多站台時，DirectAccess 需要至少一個 Windows 7&reg;每個用戶端電腦可支援在 Windows 7 中的每個 DirectAccess 進入點的樹系的用戶端安全性群組。 不過，建議您擁有的不同 Windows 7 用戶端安全性群組包含 Windows 7 用戶端的每個網域的每個進入點。  
>   
> 針對要在其他網域中用戶端電腦上套用的 DirectAccess，必須在這些網域上建立用戶端 GPO。 新增安全性群組會觸發寫入新網域的新用戶端 GPO，如果您將新的安全性群組從新網域新增到 DirectAccess 用戶端安全性群組清單，則會在新網域上自動建立用戶端 GPO，而來自新網域的用戶端電腦將透過用戶端 GPO 取得 DirectAccess 設定。  
>   
> 請注意，如果您將新網域的用戶端新增到已經設定為 DirectAccess 用戶端安全性群組的現有安全性群組，則 DirectAccess 將不會在新網域上自動建立用戶端 GPO。 新網域的用戶端將不會接收到 DirectAccess 設定，而且將無法使用 DirectAcecss 來連線。  
  
## <a name="plan-certification-authorities"></a>規劃憑證授權單位  
如果 DirectAccess 部署已設定為使用單次密碼 (OTP) 驗證，則每個樹系都會包含相同的簽署憑證範本，但會包含不同的 Oid 值。 這會導致無法將樹系設定為單一設定單位。 若要解決此問題，並在多樹系環境中設定 OTP，請參閱主題中的 [設定在多樹系部署中的 OTP] 區段[設定多樹系部署](Configure-a-Multi-Forest-Deployment.md)。  
  
使用 IPsec 電腦憑證驗證時，所有的用戶端與伺服器電腦都必須具備由相同根或中繼憑證授權單位所發行的電腦憑證，而不論它們所屬的樹系為何。  
  
## <a name="plan-otp-exemptions"></a>規劃 OTP 豁免  
如果您使用的是 DirectAccess OTP 驗證，請注意，OTP 豁免安全性群組僅限於單一樹系的使用者。 這是因為每個安全性群組只能包含來自單一樹系的使用者，而且只能設定一個這類的安全性群組。  
  


