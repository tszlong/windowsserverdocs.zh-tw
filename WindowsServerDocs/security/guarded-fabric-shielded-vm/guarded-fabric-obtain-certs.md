---
title: 取得 HGS 的憑證
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b3e6aadbcbf2f2b826ca97d4ebb58c3736528b59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386516"
---
# <a name="obtain-certificates-for-hgs"></a>取得 HGS 的憑證

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

當您部署 HGS 時，系統會要求您提供簽署和加密憑證，以用來保護啟動受防護 VM 所需的機密資訊。
這些憑證永遠不會離開 HGS，而且只會在其執行所在的主機證明其狀況良好時，用來解密受防護的 VM 金鑰。
租使用者（VM 擁有者）會使用憑證的公用部分來授權您的資料中心執行其受防護的 Vm。
本節涵蓋取得 HGS 相容簽署和加密憑證所需的步驟。

## <a name="request-certificates-from-your-certificate-authority"></a>向您的憑證授權單位單位要求憑證

雖然並非必要，但強烈建議您從信任的憑證授權單位單位取得憑證。
這麼做可協助 VM 擁有者確認他們正在授權正確的 HGS 伺服器（也就是服務提供者或資料中心）來執行受防護的 Vm。
在企業案例中，您可以選擇使用您自己的企業 CA 來頒發證書。
主控者和服務提供者應考慮改為使用已知的公用 CA。

簽署和加密憑證都必須使用下列 certificiate 屬性來發行（除非標記為「建議」）：

憑證範本屬性 | 必要值 
------------------------------|----------------
加密提供者               | 任何金鑰儲存提供者（KSP）。 **不**支援舊版密碼編譯服務提供者（csp）。
金鑰演算法                 | RSA
最小金鑰大小              | 2048位
簽章演算法           | 建議：SHA256
金鑰使用方法                     | 數位簽章*和*資料加密
增強金鑰使用方法            | 伺服器驗證
金鑰更新原則            | 以相同的金鑰更新。 使用不同的金鑰來更新 HGS 憑證，會導致受防護的 Vm 無法啟動。
主體名稱                  | 建議：您公司的名稱或網址。 這項資訊會顯示在 [防護資料檔案] 中的 VM 擁有者。

無論您使用的是由硬體或軟體支援的憑證，都適用這些需求。
基於安全性理由，建議您在硬體安全模組（HSM）中建立 HGS 金鑰，以防止將私密金鑰複製到系統中。
遵循 HSM 廠商所提供的指導方針，要求具有上述屬性的憑證，並務必在每個 HGS 節點上安裝及授權 HSM KSP。

每個 HGS 節點都需要存取相同的簽署和加密憑證。
如果您使用的是軟體支援的憑證，您可以使用密碼將憑證匯出到 PFX 檔案，並允許 HGS 為您管理憑證。
您也可以選擇將憑證安裝到每個 HGS 節點上的本機電腦憑證存放區，並將指紋提供給 HGS。
[初始化 HGS](guarded-fabric-initialize-hgs.md)叢集主題中會說明這兩個選項。

## <a name="create-self-signed-certificates-for-test-scenarios"></a>建立適用于測試案例的自我簽署憑證

如果您要建立 HGS 實驗室環境，但不想要使用憑證授權單位單位，您可以建立自我簽署憑證。
當您在 [防護資料檔案] 中匯入憑證資訊時，將會收到警告，但所有功能都將維持不變。

若要建立自我簽署憑證，並將它們匯出至 PFX 檔案，請在 PowerShell 中執行下列命令：

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>要求 SSL 憑證

在 Hyper-v 主機和 HGS 之間傳輸的所有金鑰和機密資訊都會在訊息層級進行加密，亦即，資訊會以 HGS 或 Hyper-v 已知的金鑰加密，防止他人探查您的網路流量和竊取金鑰至您的 Vm。
不過，如果您有合規性 reqiurements，或只是想要加密 Hyper-v 與 HGS 之間的所有通訊，您可以使用 SSL 憑證來設定 HGS，這會加密傳輸層級的所有資料。

Hyper-v 主機和 HGS 節點都必須信任您提供的 SSL 憑證，因此建議您向企業憑證授權單位單位要求 SSL 憑證。 要求憑證時，請務必指定下列各項：

SSL 憑證屬性 | 必要值
-------------------------|---------------
主體名稱             | HGS 叢集的名稱（分散式網路名稱）。 這會將提供給 `Initialize-HgsServer` 和您 HGS 功能變數名稱的 HGS 服務名稱串連。
主體替代名稱 | 如果您將使用不同的 DNS 名稱來連線到 HGS 叢集（例如，如果它位於負載平衡器後方），請務必在憑證要求的 SAN 欄位中包含這些 DNS 名稱。

[設定第一個 hgs 節點](guarded-fabric-initialize-hgs.md)時，會涵蓋在初始化 HGS 伺服器時指定此憑證的選項。
您也可以在稍後使用[HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) Cmdlet 來新增或變更 SSL 憑證。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [安裝 HGS](guarded-fabric-choose-where-to-install-hgs.md)