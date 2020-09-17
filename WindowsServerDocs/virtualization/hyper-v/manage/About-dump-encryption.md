---
title: 關於傾印加密
description: 說明如何加密傾印檔案和針對加密進行疑難排解。
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.date: 11/03/2016
ms.openlocfilehash: d2258a810993ea903efe670355720a5fc65a888b
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746483"
---
# <a name="about-dump-encryption"></a>關於傾印加密
傾印加密可用來加密為系統產生的損毀傾印和即時傾印。 傾印會使用針對每個傾印產生的對稱式加密金鑰進行加密。 然後，系統會使用主機的受信任系統管理員所指定的公開金鑰來加密此金鑰本身， (損毀傾印加密金鑰保護裝置) 。 這可確保只有具有相符私密金鑰的人可以解密，因而存取傾印的內容。 這項功能是在受防護網狀架構中運用。
注意：如果您設定傾印加密，也請停用 Windows 錯誤報告。 WER 無法讀取加密的損毀傾印。

## <a name="configuring-dump-encryption"></a>正在設定傾印加密
### <a name="manual-configuration"></a>手動設定
若要使用登錄開啟傾印加密，請在下列情況下設定下列登錄值： `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| 值名稱 | 類型 | 值 |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1可啟用傾印加密，0會停用傾印加密 |
| EncryptionCertificates\Certificate.1：:P ublicKey | Binary | 公開金鑰 (RSA、2048位) ，以用於加密傾印。 這必須格式化為 [BCRYPT_RSAKEY_BLOB](/windows/win32/api/bcrypt/ns-bcrypt-bcrypt_rsakey_blob)。 |
| EncryptionCertificates\Certificate.1：： Thumbprint | String | 憑證指紋可在解密損毀傾印時，允許自動查閱本機憑證存放區中的私密金鑰。 |


### <a name="configuration-using-script"></a>使用腳本進行設定
為了簡化設定，可使用 [範例腳本](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) 來根據憑證的公開金鑰啟用傾印加密。

1. 在信任的環境中：建立具有2048位 RSA 金鑰的憑證，並匯出公開憑證
2. 在目標主機上：將公開憑證匯入本機憑證存放區
3. 執行範例設定腳本
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

## <a name="decrypting-encrypted-dumps"></a>解密加密傾印
若要解密現有的加密傾印檔案，您必須下載並安裝適用于 Windows 的偵錯工具。 此工具集包含可用來解密加密傾印檔案的 KernelDumpDecrypt.exe。
如果包含私密金鑰的憑證存在於目前使用者的憑證存放區中，則可以藉由呼叫來解密傾印檔案

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
經過解密之後，WinDbg 之類的工具可以開啟解密的傾印檔案。

## <a name="troubleshooting-dump-encryption"></a>疑難排解傾印加密
如果已在系統上啟用傾印加密，但未產生任何傾印，請檢查系統 `System` 事件記錄檔中的 `Kernel-IO` 事件1207。 無法初始化傾印加密時，會建立此事件並停用傾印。

| 詳細的錯誤訊息 | 減輕的步驟 |
| ---------------------- | ----------------- |
| 遺失公開金鑰或指紋登錄 | 檢查兩個登錄值是否存在於預期的位置 |
| 不正確公開金鑰 | 請確定儲存在 PublicKey 登錄值中的公開金鑰儲存為 [BCRYPT_RSAKEY_BLOB](/windows/win32/api/bcrypt/ns-bcrypt-bcrypt_rsakey_blob)。 |
| 不支援的公開金鑰大小 | 目前只支援2048位 RSA 金鑰。 設定符合這項需求的金鑰 |

也請檢查下的 `GuardedHost` 值 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` 是否設定為0以外的值。 這會完全停用損毀傾印。 如果是這種情況，請將它設定為0。