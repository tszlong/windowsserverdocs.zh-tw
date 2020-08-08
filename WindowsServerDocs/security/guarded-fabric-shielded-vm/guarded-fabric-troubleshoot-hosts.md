---
title: 針對受防護主機進行疑難排解
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 5940b2a626a42d639870c98ee740c44b18c02ca3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944058"
---
# <a name="troubleshooting-guarded-hosts"></a>針對受防護主機進行疑難排解

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本主題說明在您的受防護網狀架構中部署或操作受防護的 Hyper-v 主機時，所遇到常見問題的解決方式。
如果您不確定問題的本質，請先嘗試在 Hyper-v 主機上執行受防護的網狀[架構診斷](guarded-fabric-troubleshoot-diagnostics.md)，以縮小可能的原因。

## <a name="guarded-host-feature"></a>受防護主機功能

如果您在 Hyper-v 主機上遇到問題，請先確定已安裝主機守護**者 Hyper-v 支援**功能。
如果沒有這項功能，Hyper-v 主機將會遺失一些重要的設定和軟體，讓它能夠通過證明並布建受防護的 Vm。

若要檢查是否已安裝此功能，請使用伺服器管理員，或在已提升許可權的 PowerShell 視窗中執行下列命令：

```powershell
Get-WindowsFeature HostGuardian
```

如果未安裝此功能，請使用下列 PowerShell 命令進行安裝：

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>證明失敗

如果主機未通過與主機守護者服務的證明，它將無法執行受防護的 Vm。
該主機上[get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914500.aspx)的輸出會顯示為何該主機無法證明的相關資訊。

下表說明可能出現在 [ **AttestationStatus** ] 欄位中的值和可能的後續步驟（如果適用的話）。

AttestationStatus         | 說明
--------------------------|------------
已過期                   | 主機先前已通過證明，但簽發給該主機的健康情況憑證已過期。 請確認主機和 HGS 時間為同步。
InsecureHostConfiguration | 主機未通過證明，因為它不符合 HGS 上設定的證明原則。 如需詳細資訊，請參閱 AttestationSubStatus 資料表。
NotConfigured             | 主機未設定為使用 HGS 進行證明和金鑰保護。 它是針對原生模式而設定的。 如果此主機位於受防護的網狀架構中，請使用[get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914494.aspx)來提供您 HGS 伺服器的 url。
已通過                    | 主機通過證明。
TransientError            | 上次證明嘗試因網路、服務或其他暫時性錯誤而失敗。 請重試您的上一個操作。
TpmError                  | 因為 TPM 發生錯誤，所以主機無法完成最後一個證明嘗試。 如需詳細資訊，請參閱您的 TPM 記錄檔。
UnauthorizedHost          | 主機未通過證明，因為它未獲授權，無法執行受防護的 Vm。 請確定主機屬於 HGS 所信任的安全性群組，以執行受防護的 Vm。
Unknown                   | 主機尚未嘗試使用 HGS 進行證明。

當**AttestationStatus**報告為**InsecureHostConfiguration**時，[ **AttestationSubStatus** ] 欄位中會填入一或多個原因。
下表說明 AttestationSubStatus 的可能值，以及如何解決問題的秘訣。

AttestationSubStatus       | 代表的意義及應採取的動作
---------------------------|-------------------------------
BitLocker                  | 主機的 OS 磁片區不是由 BitLocker 加密。 若要解決此問題，請在 OS 磁片區上[啟用 bitlocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) ，或[停用 HGS 上的 bitlocker 原則](guarded-fabric-manage-hgs.md#review-attestation-policies)。
CodeIntegrityPolicy        | 主機未設定為使用程式碼完整性原則，或未使用 HGS 伺服器所信任的原則。 請確定已設定程式碼完整性原則、主機已重新開機，而且該原則已向 HGS 伺服器註冊。 如需詳細資訊，請參閱[建立和套用程式碼完整性原則](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy)。
DumpsEnabled               | 主機已設定為允許損毀傾印或即時記憶體傾印，這不是您的 HGS 原則所允許的。 若要解決此問題，請停用主機上的傾印。
DumpEncryption             | 主機已設定為允許損毀傾印或即時記憶體傾印，但不會加密這些傾印。 請停用主機上的傾印或設定傾印[加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
DumpEncryptionKey          | 主機已設定為允許及加密傾印，但不使用 HGS 已知的憑證來加密。 若要解決此問題，請更新主機上的傾印[加密金鑰](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)，或向[HGS 註冊金鑰](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
FullBoot                   | 主機已從睡眠狀態恢復或休眠。 重新開機主機，以允許進行乾淨的完整開機。
HibernationEnabled         | 主機已設定為允許休眠，而不需要加密休眠檔案，而 HGS 原則不允許這種情況。 停用休眠並重新啟動主機，或設定傾印[加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
HypervisorEnforcedCodeIntegrityPolicy | 主機未設定為使用「執行程式碼完整性」原則。 確認程式碼完整性已啟用、設定，並由虛擬程式強制執行。 如需詳細資訊，請參閱[Device Guard 部署指南](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。
Iommu                      | 主機的虛擬化型安全性功能未設定為需要 IOMMU 裝置，以根據 HGS 原則的要求來保護直接記憶體存取攻擊。 確認主機具有 IOMMU、已啟用，而且裝置防護已設定為在啟動 VBS 時[需要 DMA 保護](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard)。
PagefileEncryption         | 未在主機上啟用分頁檔案加密。 若要解決此問題，請執行 `fsutil behavior set encryptpagingfile 1` 以啟用分頁檔加密。 如需詳細資訊，請參閱[fsutil 行為](https://technet.microsoft.com/library/cc785435.aspx)。
SecureBoot                 | 未在此主機上啟用安全開機，或未使用 Microsoft 安全開機範本。 請使用 Microsoft 安全開機範本來[啟用安全開機](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot)，以解決此問題。
SecureBootSettings         | 此主機上的 TPM 基準不符合 HGS 信任的任何一個。 藉由安裝新的硬體或軟體來變更 UEFI 啟動授權單位、.DBX 變數、debug 旗標或自訂安全開機原則時，可能會發生這種情況。 如果您信任這部電腦目前的硬體、固件和軟體設定，您可以[捕捉新的 TPM 基準](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware)並[向 HGS 註冊](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
TcgLogVerification         | 無法取得或驗證 TCG 記錄 (TPM 基準) 。 這可能表示主機的 [固件]、[TPM] 或其他硬體元件發生問題。 如果您的主機設定為在啟動 Windows 之前嘗試 PXE 開機，則 (NBP) 的過時 Net 開機程式也會造成此錯誤。 請確定在啟用 PXE 開機時，所有 Nbp 都是最新狀態。
VirtualSecureMode          | 虛擬化的安全性功能未在主機上執行。 請確定已啟用 VBS，而且您的系統符合已設定的[平臺安全性功能](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)。 如需有關 VBS 需求的詳細資訊，請參閱[Device Guard 檔](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)。

## <a name="modern-tls"></a>新式 TLS

如果您已部署群組原則，或已將 Hyper-v 主機設定為防止使用 TLS 1.0，則在嘗試啟動受防護的 VM 時，可能會遇到「主機守護者服務用戶端無法代表呼叫程式解除包裝金鑰保護裝置」錯誤。
這是因為 .NET 4.6 中的預設行為，在與 HGS 伺服器協商支援的 TLS 版本時，不會考慮系統預設的 TLS 版本。

若要解決此行為，請執行下列兩個命令，將 .NET 設定為使用所有 .NET 應用程式的系統預設 TLS 版本。

```cmd
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:64
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:32
```

> [!WARNING]
> 系統預設的 TLS 版本設定會影響您電腦上的所有 .NET 應用程式。 請務必先在隔離的環境中測試登錄機碼，再將它們部署到您的實際執行電腦。

如需有關 .NET 4.6 和 TLS 1.0 的詳細資訊，請參閱[解決 TLS 1.0 問題第2版](https://docs.microsoft.com/security/solving-tls1-problem)。
