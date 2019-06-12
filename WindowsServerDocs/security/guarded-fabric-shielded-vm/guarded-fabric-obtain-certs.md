---
title: 取得憑證以用於 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2dc232eb7aeb8b0807a8e9989ae3dc893f925f66
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447360"
---
# <a name="obtain-certificates-for-hgs"></a>取得憑證以用於 HGS

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

當您部署 HGS 時，系統會要求您提供用來保護敏感資訊將受防護的 vm 啟動所需的簽署和加密憑證。
這些憑證永遠不會離開 HGS，且只會用來解密受防護 VM 的索引鍵時要執行的主機已證明其狀況良好。
租用戶 （VM 擁有者） 使用公用一半的憑證來授權您的資料中心，以執行受防護的 Vm。
本章節涵蓋取得 HGS 相容的簽章和加密憑證所需的步驟。

## <a name="request-certificates-from-your-certificate-authority"></a>從您的憑證授權單位要求憑證

雖然並非必要，強烈建議您從受信任的憑證授權單位取得憑證。
如此一來，可協助 VM 擁有者確認他們授權正確 HGS 伺服器 （也就是服務提供者或資料中心） 來執行受防護的 Vm。
在企業案例中，您可以選擇使用您自己的企業 CA 發行這些憑證。
主機服務提供者和服務提供者，應該考慮改為使用已知的公用 CA。

（除非標記為 「 建議 」），必須具有下列憑證屬性發出簽署和加密憑證：

憑證範本屬性 | 必要的值 
------------------------------|----------------
密碼編譯提供者               | 任何金鑰儲存提供者 (KSP)。 舊版密碼編譯服務提供者 (Csp) 是**不**支援。
金鑰演算法                 | RSA
最小金鑰大小              | 2048 位元
簽章演算法           | 建議：SHA256
金鑰使用方法                     | 數位簽章*和*資料編密
增強金鑰使用方法            | 伺服器驗證
金鑰更新原則            | 使用相同的金鑰來更新。 具有不同索引鍵更新 HGS 憑證，會造成受防護的 Vm 無法啟動。
主體名稱                  | 建議： 您的公司名稱或網站位址。 這項資訊會顯示 VM 防護資料檔案精靈 中的擁有者。

不論您使用硬體或軟體所支援的憑證，就會套用這些需求。
基於安全性理由，建議您建立您的 HGS 金鑰在硬體安全性模組 (HSM) 來防止關閉系統複製金鑰的私密金鑰。
遵循 HSM 廠商所提供的指導方針，上述的屬性與要求憑證，務必先安裝並授權 HSM KSP HGS 的每個節點上。

HGS 的每個節點將會需要存取相同的簽署和加密憑證。
如果您使用的軟體為基礎的憑證，您可以將您的憑證匯出至 PFX 檔，並設定密碼，並允許 HGS 來管理您的憑證。
您也可以選擇將憑證安裝到每一個 HGS 節點上的本機電腦的憑證存放區，並向 HGS 提供憑證指紋。
這兩個選項所述[初始化 HGS 叢集](guarded-fabric-initialize-hgs.md)主題。

## <a name="create-self-signed-certificates-for-test-scenarios"></a>建立自我簽署的憑證來進行測試案例

如果您要建立的 HGS 實驗室環境和不具有或不想要使用的憑證授權單位，您可以建立自我簽署的憑證。
匯入防護資料檔案精靈 中的憑證資訊時，您會收到警告，但所有的功能會保持相同。

若要建立自我簽署的憑證，並將它們匯出為 PFX 檔案，請在 PowerShell 中執行下列命令：

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>要求的 SSL 憑證

HYPER-V 主機之間傳輸的所有金鑰和敏感性資訊和 HGS 會加密訊息層級，也就是會使用已知的 HGS 或 HYPER-V，防止他人在探查您的網路流量，竊取的金鑰的金鑰加密的資訊以您的 Vm。
不過，如果您有合規性 reqiurements，或只是想要加密所有 HYPER-V 與 HGS 之間的通訊，您可以設定 HGS 使用 SSL 憑證來都加密所有資料在傳輸層級。

將 HYPER-V 主機與 HGS 節點必須信任您提供，SSL 憑證，因此建議您從您的企業憑證授權單位要求 SSL 憑證。 當要求憑證時，請務必指定下列項目：

SSL 憑證屬性 | 必要的值
-------------------------|---------------
主體名稱             | HGS 叢集 （分散式的網路名稱） 的名稱。 這會提供給您 HGS 服務名稱的串連`Initialize-HgsServer`以及 HGS 網域名稱。
主體替代名稱 | 如果您將使用不同的 DNS 名稱連線到您的 HGS 叢集 (例如負載平衡器後方時)，請務必在您的憑證要求的 SAN 欄位中包含這些 DNS 名稱。

初始化 HGS 伺服器時指定此憑證的選項所述[設定的第一個 HGS 節點](guarded-fabric-initialize-hgs.md)。
您也可以新增或變更在稍後使用的 SSL 憑證[組 HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) cmdlet。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [安裝 HGS](guarded-fabric-choose-where-to-install-hgs.md)