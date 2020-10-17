---
title: tpmtool
description: Tpmtool 命令的參考文章，可取得可信賴平臺模組的相關資訊。
ms.topic: reference
author: ashleytqy
ms.author: asteoh
manager: raigner
ms.date: 05/07/2019
ms.openlocfilehash: bc0aaa4bc4bd9c23e0cf22b0315335287867d8cd
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156433"
---
# <a name="tpmtool"></a>tpmtool

您可以使用此公用程式來取得 [信賴平臺模組 (TPM) ](/windows/security/information-protection/tpm/trusted-platform-module-overview)的相關資訊。

>[!IMPORTANT]
>某些資訊可能與預先發行的產品有關，在正式發行之前可能會經過大幅修改。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

## <a name="syntax"></a>語法

```
tpmtool /parameter [<arguments>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| getdeviceinformation | 顯示 TPM 的基本資訊。 如需資訊旗標值的詳細資訊，請參閱 [Win32_Tpm：： IsReadyInformation 方法參數](/windows/win32/secprov/win32-tpm-isreadyinformation#parameters) 一文。 |
| gatherlogs [輸出目錄路徑] | 收集 TPM 記錄，並將它們放在指定的目錄中。 如果該目錄不存在，就會加以建立。 根據預設，記錄檔會放在目前的目錄中。 可能產生的檔案包括：<ul><li>TpmEvents .evtx</li><li>TpmInformation.txt</li><li>SRTMBoot .dat</li><li>SRTMResume .dat</li><li>DRTMBoot .dat</li><li>DRTMResume .dat</li></ul> |
| drivertracing `[start | stop]` | 啟動或停止收集 TPM 驅動程式追蹤。 追蹤記錄 *TPMTRACE*會建立並放置在目前的目錄中。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要顯示 TPM 的基本資訊，請輸入：

```
tpmtool getdeviceinformation
```

若要收集 TPM 記錄，並將它們放在目前的目錄中，請輸入：

```
tpmtool gatherlogs
```

若要收集 TPM 記錄並將其放入 `C:\Users\Public` ，請輸入：

```
tpmtool gatherlogs C:\Users\Public
```

若要收集 TPM 驅動程式追蹤，請輸入：

```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [COM 錯誤碼 (TPM、PLA、FVE) ](/windows/win32/com/com-error-codes-6)
