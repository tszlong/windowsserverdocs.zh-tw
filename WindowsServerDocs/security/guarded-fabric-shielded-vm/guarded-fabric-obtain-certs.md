---
title: 取得 HGS 的憑證
description: 深入瞭解：取得 HGS 的憑證
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: a4e301ebb8bb11927f86a4af594c31ae02fd6de8
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044026"
---
# <a name="obtain-certificates-for-hgs"></a>取得 HGS 的憑證

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

當您部署 HGS 時，系統會要求您提供簽署和加密憑證，以用來保護啟動受防護 VM 所需的機密資訊。
這些憑證絕對不會離開 HGS，而且只會在其執行所在的主機證明其正常運作時，用來將受防護的 VM 金鑰解密。
租使用者 (VM 擁有者) 使用公開的一半憑證來授權您的資料中心執行受防護的 Vm。
本節涵蓋取得 HGS 相容簽署和加密憑證所需的步驟。

## <a name="request-certificates-from-your-certificate-authority"></a>從您的憑證授權單位單位要求憑證

雖然並非必要，但強烈建議您從受信任的憑證授權單位單位取得憑證。
這麼做可協助 VM 擁有者確認他們正在授權正確的 HGS 伺服器 (亦即服務提供者或資料中心) 來執行受防護的 Vm。
在企業案例中，您可以選擇使用自己的企業 CA 來發出這些憑證。
主控者和服務提供者應該考慮改為使用知名的公用 CA。

簽署和加密憑證都必須使用下列 certificiate 屬性來發出 (除非標示為「建議」 ) ：

憑證範本屬性 | 必要值
------------------------------|----------------
密碼編譯提供者               | 任何金鑰儲存提供者 (KSP) 。 **不** 支援 (csp) 的舊版密碼編譯服務提供者。
金鑰演算法                 | RSA
最小金鑰大小              | 2048 位元
簽章演算法           | 建議： SHA256
金鑰使用量                     | 數位簽章 *和* 資料加密
增強金鑰使用方法            | 伺服器驗證
金鑰更新原則            | 使用相同的金鑰更新。 使用不同的金鑰來更新 HGS 憑證，會導致受防護的 Vm 無法啟動。
主體名稱                  | 建議：您公司的名稱或網址。 這項資訊會顯示在 [防護資料檔案] wizard 中的 VM 擁有者。

無論您使用的是硬體或軟體所支援的憑證，都適用這些需求。
基於安全性理由，建議您在硬體安全模組 (HSM) 中建立 HGS 金鑰，以防止將私密金鑰複製到系統之外。
遵循 HSM 廠商的指導方針，要求具有上述屬性的憑證，並務必在每個 HGS 節點上安裝並授權 HSM KSP。

每個 HGS 節點都需要存取相同的簽署和加密憑證。
如果您使用軟體支援的憑證，您可以使用密碼將憑證匯出至 PFX 檔案，並允許 HGS 為您管理憑證。
您也可以選擇在每個 HGS 節點上將憑證安裝到本機電腦的憑證存放區，並提供憑證指紋給 HGS。
[初始化 HGS](guarded-fabric-initialize-hgs.md)叢集主題中會說明這兩個選項。

## <a name="create-self-signed-certificates-for-test-scenarios"></a>建立適用于測試案例的自我簽署憑證

如果您要建立 HGS 實驗室環境，而且沒有或想要使用憑證授權單位單位，您可以建立自我簽署憑證。
當您在 [防護資料檔案嚮導] 中匯入憑證資訊時，將會收到警告，但是所有功能都會保持不變。

若要建立自我簽署的憑證，並將其匯出至 PFX 檔案，請在 PowerShell 中執行下列命令：

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt 'Enter a password for the PFX file'

$signCert = New-SelfSignedCertificate -Subject 'CN=HGS Signing Certificate' -KeyUsage DataEncipherment, DigitalSignature
Export-PfxCertificate -FilePath '.\signCert.pfx' -Password $certificatePassword -Cert $signCert

# Remove the certificate from "Personal" container
Remove-Item $signCert.PSPath
# Remove the certificate from "Intermediate certification authorities" container
Remove-Item -Path "Cert:\LocalMachine\CA\$($signCert.Thumbprint)"

$encCert = New-SelfSignedCertificate -Subject 'CN=HGS Encryption Certificate' -KeyUsage DataEncipherment, DigitalSignature
Export-PfxCertificate -FilePath '.\encCert.pfx' -Password $certificatePassword -Cert $encCert

# Remove the certificate from "Personal" container
Remove-Item $encCert.PSPath
# Remove the certificate from "Intermediate certification authorities" container
Remove-Item -Path "Cert:\LocalMachine\CA\$($encCert.Thumbprint)"
```

## <a name="request-an-ssl-certificate"></a>要求 SSL 憑證

在 Hyper-v 主機與 HGS 之間傳輸的所有金鑰和敏感性資訊都會在訊息層級進行加密，也就是說，資訊是以 HGS 或 Hyper-v 的已知金鑰加密，防止他人探查您的網路流量，並竊取 Vm 的金鑰。
但是，如果您有合規性需求，或只是想要加密 Hyper-v 與 HGS 之間的所有通訊，您可以使用 SSL 憑證設定 HGS，以加密傳輸層級的所有資料。

Hyper-v 主機和 HGS 節點都必須信任您提供的 SSL 憑證，因此建議您從企業憑證授權單位單位要求 SSL 憑證。 要求憑證時，請務必指定下列各項：

SSL 憑證屬性 | 必要值
-------------------------|---------------
主體名稱             | 請解決 HGS 用戶端 (也就是受防護主機) 將會用來存取 HGS 伺服器。 這通常是 HGS 叢集（稱為「分散式網路名稱」或「虛擬電腦」物件） (VCO) 的 DNS 位址。 這將是您提供的 HGS 服務名稱與 `Initialize-HgsServer` hgs 功能變數名稱的串連。
主體別名 | 如果您要使用不同的 DNS 名稱來連線 HGS 叢集 (例如，如果它是在負載平衡器後方，或您針對複雜拓撲) 中的節點子集使用不同的位址，請務必在您的憑證要求的 [SAN] 欄位中包含這些 DNS 名稱。 請注意，如果已填入 SAN 延伸模組，則會忽略主體名稱，因此 SAN 應該包含所有值，包括通常會放入主體名稱的所有值。

[設定第一個 hgs 節點](guarded-fabric-initialize-hgs.md)時，會涵蓋在初始化 hgs 伺服器時指定此憑證的選項。
您也可以稍後使用 [HgsServer](/powershell/module/hgsserver/set-hgsserver) 指令 Cmdlet 來新增或變更 SSL 憑證。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [安裝 HGS](guarded-fabric-choose-where-to-install-hgs.md)
