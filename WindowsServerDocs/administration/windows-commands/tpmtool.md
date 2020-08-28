---
title: tpmtool
description: Tpmtool 的參考文章，可取得可信賴平臺模組的相關資訊。
ms.topic: reference
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: b0f234755eefdca15f214dad428f02631592e8c2
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026996"
---
# <a name="tpmtool"></a>tpmtool

您可以使用此公用程式來取得 [信賴平臺模組 (TPM) ](/windows/security/information-protection/tpm/trusted-platform-module-overview)的相關資訊。

>[!IMPORTANT]
>預先發行的產品在正式發行前可能會大幅度修改，本文提供有關該產品的一些資訊。 針對此處提供的資訊，Microsoft 不做任何明示或默許的擔保。

如需如何使用此命令的範例，請參閱[範例](#tpmtool_examples)。

## <a name="syntax"></a>語法

```
tpmtool /parameter [<arguments>]
```
### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|getdeviceinformation|顯示 TPM 的基本資訊。 您可以在 [這裡](/windows/desktop/secprov/win32-tpm-isreadyinformation#parameters)找到資訊旗標值的意義。|
|gatherlogs [輸出目錄路徑]|收集 TPM 記錄，並將它們放在指定的目錄中。 如果該目錄不存在，則會建立該目錄。 預設會將它們放在目前的目錄中。 可能產生的檔案包括： </br>-TpmEvents .evtx</br>-TpmInformation.txt</br>-SRTMBoot .dat</br>-SRTMResume .dat</br>-DRTMBoot .dat</br>-DRTMResume .dat</br>|
|drivertracing [啟動/停止]|啟動/停止收集 TPM 驅動程式追蹤。 系統將會產生追蹤記錄 TPMTRACE，並將其放在目前的目錄中。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a><a name=tpmtool_examples></a>範例

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

## <a name="decoding-error-codes"></a>解碼錯誤碼

TPM 專屬的錯誤碼記載于 [此處](/windows/desktop/com/com-error-codes-6)。
