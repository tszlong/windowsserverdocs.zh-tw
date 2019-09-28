---
title: QoS 原則錯誤和事件訊息
description: 本主題提供 Windows Server 2016 中的服務品質（QoS）原則的錯誤和事件訊息清單。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 935bda5ab47f3e9a362c81a8aeb99ebf22095725
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405325"
---
# <a name="qos-policy-error-and-event-messages"></a>QoS 原則錯誤和事件訊息

>適用於：Windows Server (半年度管道)、Windows Server 2016

以下是與 QoS 原則相關聯的錯誤和事件訊息。  
  
## <a name="informational-messages"></a>參考用訊息  

以下是 QoS 原則參考訊息的清單。

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**語言**|英文|  
|**Message**|已成功重新整理電腦 QoS 原則。 未偵測到任何變更。|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**語言**|英文|  
|**Message**|已成功重新整理電腦 QoS 原則。 偵測到原則變更。|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**語言**|英文|  
|**Message**|已成功重新整理使用者 QoS 原則。 未偵測到任何變更。|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**語言**|英文|  
|**Message**|已成功重新整理使用者 QoS 原則。 偵測到原則變更。|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**語言**|英文|  
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值不是由任何 QoS 原則指定。 將套用本機電腦預設值。|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**語言**|英文|  
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級0（最小輸送量）。|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**語言**|英文|  
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級1。|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**語言**|英文|  
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級2。|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**語言**|英文|  
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級3（最大輸送量）。|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**語言**|英文|  
|**Message**|DSCP 標記覆寫的 Advanced QoS 設定已成功重新整理。 未指定設定值。 應用程式可以設定與 QoS 原則無關的 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**語言**|英文|  
|**Message**|DSCP 標記覆寫的 Advanced QoS 設定已成功重新整理。 將忽略應用程式 DSCP 標記要求。 只有 QoS 原則可以設定 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**語言**|英文|  
|**Message**|DSCP 標記覆寫的 Advanced QoS 設定已成功重新整理。 應用程式可以設定與 QoS 原則無關的 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**語言**|英文|  
|**Message**|已停用根據網域網路類別的選擇性應用程式 QoS 原則。 QoS 原則會套用至所有網路介面。|  
  
## <a name="warning-messages"></a>警告訊息

以下是 QoS 原則警告訊息的清單。

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**語言**|英文|  
|**Message**|EQOS： * * * 測試 @ no__t-0 @ no__t-1 @ no__t-2 [，有一個字串] "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**語言**|英文|  
|**Message**|EQOS： * * * 測試 @ no__t-0 @ no__t-1 @ no__t-2 [，有兩個字串，string1 是] "% 2" [，string2 是] "% 3"。|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則 "% 2" 的版本號碼無效。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則 "% 2" 的版本號碼無效。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則 "% 2" 未指定 DSCP 值或節流速率。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則 "% 2" 未指定 DSCP 值或節流速率。 將不會套用此原則。|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**語言**|英文|  
|**Message**|超過電腦 QoS 原則的最大數目。 將不會套用 QoS 原則 "% 2" 和後續的電腦 QoS 原則。|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**語言**|英文|  
|**Message**|已超過使用者 QoS 原則的最大數目。 將不會套用 QoS 原則 "% 2" 和後續的使用者 QoS 原則。|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則 "% 2" 可能與其他 QoS 原則發生衝突。 請參閱檔，以取得將套用原則的規則。|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則 "% 2" 可能與其他 QoS 原則發生衝突。 請參閱檔，以取得將套用原則的規則。|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**語言**|英文|  
|**Message**|已忽略電腦 QoS 原則 "% 2"，因為無法處理應用程式路徑。 應用程式路徑可能無效、包含不正確磁碟機號，或包含網路對應的磁片磁碟機。|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**語言**|英文|  
|**Message**|已忽略使用者 QoS 原則 "% 2"，因為無法處理應用程式路徑。 應用程式路徑可能無效、包含不正確磁碟機號，或包含網路對應的磁片磁碟機。|  
  
## <a name="error-messages"></a>錯誤訊息  

以下是 QoS 原則錯誤訊息的清單。

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則無法重新整理。 錯誤碼： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則無法重新整理。 錯誤碼： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟 QoS 原則的電腦層級根金鑰。 錯誤碼： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟 QoS 原則的使用者層級根金鑰。 錯誤碼： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則超過允許的名稱長度上限。 有問題的原則列在具有索引 "% 2" 的電腦層級 QoS 原則根機碼底下。|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則超過允許的名稱長度上限。 有問題的原則列在索引 "% 2" 的使用者層級 QoS 原則根機碼底下。|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**語言**|英文|  
|**Message**|電腦 QoS 原則的長度為零。 有問題的原則列在具有索引 "% 2" 的電腦層級 QoS 原則根機碼底下。|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**語言**|英文|  
|**Message**|使用者 QoS 原則的名稱長度為零。 有問題的原則列在索引 "% 2" 的使用者層級 QoS 原則根機碼底下。|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟電腦 QoS 原則的登錄子機碼。 原則會列在具有索引 "% 2" 的電腦層級 QoS 原則根機碼底下。|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**語言**|英文|  
|**Message**|QoS 無法開啟使用者 QoS 原則的登錄子機碼。 此原則會列在索引為 "% 2" 的使用者層級 QoS 原則根機碼底下。|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或驗證電腦 QoS 原則 "% 3" 的 "% 2" 欄位。|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或驗證使用者 QoS 原則 "% 3" 的 "% 2" 欄位。|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或設定輸入 TCP 輸送量層級，錯誤碼： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**語言**|英文|  
|**Message**|QoS 無法讀取或設定 DSCP 標記覆寫設定，錯誤碼： "% 2"。|  

如需本指南中的下一個主題，請參閱[QoS 原則](qos-policy-faq.md)的常見問題。

如需本指南的第一個主題，請參閱[服務品質（QoS）原則](qos-policy-top.md)。
