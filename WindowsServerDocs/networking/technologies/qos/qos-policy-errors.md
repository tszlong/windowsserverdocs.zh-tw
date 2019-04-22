---
title: QoS 原則錯誤和事件訊息
description: 本主題會提供 Windows Server 2016 中的服務品質 (QoS) 原則錯誤和事件訊息的清單。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 774d9473beed1da861c6827357710133aeaca15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824049"
---
# <a name="qos-policy-error-and-event-messages"></a>QoS 原則錯誤和事件訊息

>適用於：Windows Server （半年通道），Windows Server 2016

以下是與 QoS 原則相關聯的錯誤和事件訊息。  
  
## <a name="informational-messages"></a>告知性訊息  

以下是一份 QoS 原則的參考用訊息。

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則已成功重新整理。 未偵測到的變更。|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則已成功重新整理。 偵測到的原則變更。|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則已成功重新整理。 未偵測到的變更。|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則已成功重新整理。 偵測到的原則變更。|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**語言**|英文|  
|**Message**|輸入的 TCP 輸送量層級的進階 QoS 設定已成功重新整理。 未指定任何的 QoS 原則設定值。 會套用本機電腦的預設值。|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**語言**|英文|  
|**Message**|輸入的 TCP 輸送量層級的進階 QoS 設定已成功重新整理。 將值設定為層級 0 （最小輸送量）。|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**語言**|英文|  
|**Message**|輸入的 TCP 輸送量層級的進階 QoS 設定已成功重新整理。 將值設定為層級 1。|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**語言**|英文|  
|**Message**|輸入的 TCP 輸送量層級的進階 QoS 設定已成功重新整理。 將值設定為層級 2。|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**語言**|英文|  
|**Message**|輸入的 TCP 輸送量層級的進階 QoS 設定已成功重新整理。 將值設定為層級 3 （最大輸送量）。|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**語言**|英文|  
|**Message**|DSCP 標示進階 QoS 設定會覆寫已成功重新整理。 未指定設定值。 應用程式可以設定與 QoS 原則無關的 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**語言**|英文|  
|**Message**|DSCP 標示進階 QoS 設定會覆寫已成功重新整理。 應用程式的 DSCP 標示要求都會被忽略。 只有 QoS 原則可以設定 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**語言**|英文|  
|**Message**|DSCP 標示進階 QoS 設定會覆寫已成功重新整理。 應用程式可以設定與 QoS 原則無關的 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**語言**|英文|  
|**Message**|網域網路類別為基礎的 QoS 原則的選擇性應用程式已停用。 QoS 原則會套用至所有網路介面。|  
  
## <a name="warning-messages"></a>警告訊息

以下是一份 QoS 原則警告訊息。

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**語言**|英文|  
|**Message**|EQOS: * * * 測試\*\*\*[，使用一個字串]"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**語言**|英文|  
|**Message**|EQOS: * * * 測試\*\*\*[，string1 是兩個字串，]"%2"[，string2 為]"%3"。|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**語言**|英文|  
|**Message**|電腦"%2"的 QoS 原則具有無效的版本號碼。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**語言**|英文|  
|**Message**|使用者"%2"的 QoS 原則具有無效的版本號碼。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**語言**|英文|  
|**Message**|電腦"%2"的 QoS 原則未指定 DSCP 值或節流閥速率。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**語言**|英文|  
|**Message**|使用者"%2"的 QoS 原則未指定 DSCP 值或節流閥速率。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**語言**|英文|  
|**Message**|超過電腦 QoS 原則的最大數目。 將不會套用"%2"的 QoS 原則和後續電腦 QoS 原則。|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**語言**|英文|  
|**Message**|超過使用者 QoS 原則的最大數目。 將不會套用"%2"的 QoS 原則及後續使用者 QoS 原則。|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**語言**|英文|  
|**Message**|電腦"%2"的 QoS 原則可能發生衝突時使用其他的 QoS 原則。 請參閱原則將套用哪些規則的文件。|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**語言**|英文|  
|**Message**|使用者"%2"的 QoS 原則可能發生衝突時使用其他的 QoS 原則。 請參閱原則將套用哪些規則的文件。|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**語言**|英文|  
|**Message**|電腦"%2"的 QoS 原則已忽略，因為無法處理的應用程式路徑。 應用程式路徑可能是無效的、 包含無效的磁碟機代號，或包含對應的網路磁碟機。|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**語言**|英文|  
|**Message**|略過使用者"%2"的 QoS 原則，因為無法處理的應用程式路徑。 應用程式路徑可能是無效的、 包含無效的磁碟機代號，或包含對應的網路磁碟機。|  
  
## <a name="error-messages"></a>錯誤訊息  

以下是一份 QoS 原則的錯誤訊息。

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**語言**|英文|  
|**Message**|無法重新整理電腦 QoS 原則。 錯誤碼:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**語言**|英文|  
|**Message**|無法重新整理使用者的 QoS 原則。 錯誤碼:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟 QoS 原則的電腦層級根金鑰。 錯誤碼:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟 QoS 原則的使用者層級根金鑰。 錯誤碼:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**語言**|英文|  
|**Message**|QoS 原則的電腦超過最大允許的名稱的長度。 違規的原則會列在電腦層級 QoS 原則根機碼下，索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**語言**|英文|  
|**Message**|QoS 原則的使用者超過最大允許的名稱的長度。 違規的原則會列在使用者層級 QoS 原則根機碼下，索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**語言**|英文|  
|**Message**|QoS 原則的電腦具有零長度的名稱。 違規的原則會列在電腦層級 QoS 原則根機碼下，索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**語言**|英文|  
|**Message**|QoS 原則的使用者具有零長度的名稱。 違規的原則會列在使用者層級 QoS 原則根機碼下，索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟電腦 QoS 原則的登錄子機碼。 原則會列在電腦層級 QoS 原則根機碼下，索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟使用者的 QoS 原則的登錄子機碼。 原則會列在使用者層級 QoS 原則根機碼下，索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或驗證電腦 QoS 原則"%3"的"%2"欄位。|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或驗證的使用者"%3"的 QoS 原則的"%2"欄位。|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或設定輸入的 TCP 輸送量層級，錯誤碼:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**語言**|英文|  
|**Message**|無法讀取或設定 DSCP 標示的 QoS 覆寫設定，錯誤碼:"%2"。|  

如本指南中的下一個主題，請參閱 < [QoS 原則常見問題集](qos-policy-faq.md)。

如本指南中的第一個主題，請參閱 <<c0> [ 服務品質 (QoS) 原則](qos-policy-top.md)。
