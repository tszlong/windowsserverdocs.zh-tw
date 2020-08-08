---
title: QoS 原則錯誤和事件訊息
description: 本主題提供 Windows Server 2016 中的服務品質 (QoS) 原則的錯誤和事件訊息清單。
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dc100c7ae8ff4ab65dc8cacdde9d46ade8d2b20e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940019"
---
# <a name="qos-policy-error-and-event-messages"></a>QoS 原則錯誤和事件訊息

>適用於：Windows Server (半年度管道)、Windows Server 2016

以下是與 QoS 原則相關聯的錯誤和事件訊息。

## <a name="informational-messages"></a>參考用訊息

以下是 QoS 原則參考訊息的清單。

|||
|-|-|
|**Id**|16500|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|
|**語言**|英文|
|**Message**|已成功重新整理電腦 QoS 原則。 未偵測到任何變更。|

|||
|-|-|
|**Id**|16501|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|
|**語言**|英文|
|**Message**|已成功重新整理電腦 QoS 原則。 偵測到原則變更。|

|||
|-|-|
|**Id**|16502|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|
|**語言**|英文|
|**Message**|已成功重新整理使用者 QoS 原則。 未偵測到任何變更。|

|||
|-|-|
|**Id**|16503|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|
|**語言**|英文|
|**Message**|已成功重新整理使用者 QoS 原則。 偵測到原則變更。|

|||
|-|-|
|**Id**|16504|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|
|**語言**|英文|
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值不是由任何 QoS 原則指定。 將套用本機電腦預設值。|

|||
|-|-|
|**Id**|16505|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|
|**語言**|英文|
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級0， (最小輸送量) 。|

|||
|-|-|
|**Id**|16506|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|
|**語言**|英文|
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級1。|

|||
|-|-|
|**Id**|16507|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|
|**語言**|英文|
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級2。|

|||
|-|-|
|**Id**|16508|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|
|**語言**|英文|
|**Message**|已成功重新整理輸入 TCP 輸送量層級的 Advanced QoS 設定。 設定值為層級 3 (最大輸送量) 。|

|||
|-|-|
|**Id**|16509|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|
|**語言**|英文|
|**Message**|DSCP 標記覆寫的 Advanced QoS 設定已成功重新整理。 未指定設定值。 應用程式可以設定與 QoS 原則無關的 DSCP 值。|

|||
|-|-|
|**Id**|16510|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|
|**語言**|英文|
|**Message**|DSCP 標記覆寫的 Advanced QoS 設定已成功重新整理。 將忽略應用程式 DSCP 標記要求。 只有 QoS 原則可以設定 DSCP 值。|

|||
|-|-|
|**Id**|16511|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|
|**語言**|英文|
|**Message**|DSCP 標記覆寫的 Advanced QoS 設定已成功重新整理。 應用程式可以設定與 QoS 原則無關的 DSCP 值。|

|||
|-|-|
|**Id**|16512|
|**嚴重性**|資訊|
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|
|**語言**|英文|
|**Message**|已停用根據網域網路類別的選擇性應用程式 QoS 原則。 QoS 原則會套用至所有網路介面。|

## <a name="warning-messages"></a>警告訊息

以下是 QoS 原則警告訊息的清單。

|||
|-|-|
|**Id**|16600|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|
|**語言**|英文|
|**Message**|EQOS： * * * 測試 \* \* \* [，有一個字串] "%2"。|

|||
|-|-|
|**Id**|16601|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|
|**語言**|英文|
|**Message**|EQOS： * * * 測試 \* \* \* [，有兩個字串，string1 是] "%2" [，string2 是] "%3"。|

|||
|-|-|
|**Id**|16602|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|
|**語言**|英文|
|**Message**|電腦 QoS 原則 "%2" 的版本號碼無效。 將不會套用此原則。|

|||
|-|-|
|**Id**|16603|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|
|**語言**|英文|
|**Message**|使用者 QoS 原則 "%2" 的版本號碼無效。 將不會套用此原則。|

|||
|-|-|
|**Id**|16604|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|
|**語言**|英文|
|**Message**|電腦 QoS 原則 "%2" 未指定 DSCP 值或節流速率。 將不會套用此原則。|

|||
|-|-|
|**Id**|16605|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|
|**語言**|英文|
|**Message**|使用者 QoS 原則 "%2" 未指定 DSCP 值或節流速率。 將不會套用此原則。|

|||
|-|-|
|**Id**|16606|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|
|**語言**|英文|
|**Message**|超過電腦 QoS 原則的最大數目。 將不會套用 QoS 原則 "%2" 和後續的電腦 QoS 原則。|

|||
|-|-|
|**Id**|16607|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|
|**語言**|英文|
|**Message**|已超過使用者 QoS 原則的最大數目。 將不會套用 QoS 原則 "%2" 和後續的使用者 QoS 原則。|

|||
|-|-|
|**Id**|16608|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|
|**語言**|英文|
|**Message**|電腦 QoS 原則 "%2" 可能與其他 QoS 原則發生衝突。 請參閱檔，以取得將套用原則的規則。|

|||
|-|-|
|**Id**|16609|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|
|**語言**|英文|
|**Message**|使用者 QoS 原則 "%2" 可能與其他 QoS 原則發生衝突。 請參閱檔，以取得將套用原則的規則。|

|||
|-|-|
|**Id**|16610|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|
|**語言**|英文|
|**Message**|已忽略電腦 QoS 原則 "%2"，因為無法處理應用程式路徑。 應用程式路徑可能無效、包含不正確磁碟機號，或包含網路對應的磁片磁碟機。|

|||
|-|-|
|**Id**|16611|
|**嚴重性**|警告|
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|
|**語言**|英文|
|**Message**|已忽略使用者 QoS 原則 "%2"，因為無法處理應用程式路徑。 應用程式路徑可能無效、包含不正確磁碟機號，或包含網路對應的磁片磁碟機。|

## <a name="error-messages"></a>錯誤訊息

以下是 QoS 原則錯誤訊息的清單。

|||
|-|-|
|**Id**|16700|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|
|**語言**|英文|
|**Message**|電腦 QoS 原則無法重新整理。 錯誤碼： "%2"。|

|||
|-|-|
|**Id**|16701|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|
|**語言**|英文|
|**Message**|使用者 QoS 原則無法重新整理。 錯誤碼： "%2"。|

|||
|-|-|
|**Id**|16702|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|
|**語言**|英文|
|**Message**|QoS 無法開啟 QoS 原則的電腦層級根金鑰。 錯誤碼： "%2"。|

|||
|-|-|
|**Id**|16703|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|
|**語言**|英文|
|**Message**|QoS 無法開啟 QoS 原則的使用者層級根金鑰。 錯誤碼： "%2"。|

|||
|-|-|
|**Id**|16704|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|
|**語言**|英文|
|**Message**|電腦 QoS 原則超過允許的名稱長度上限。 有問題的原則列在具有索引 "%2" 的電腦層級 QoS 原則根機碼底下。|

|||
|-|-|
|**Id**|16705|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|
|**語言**|英文|
|**Message**|使用者 QoS 原則超過允許的名稱長度上限。 有問題的原則列在索引 "%2" 的使用者層級 QoS 原則根機碼底下。|

|||
|-|-|
|**Id**|16706|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|
|**語言**|英文|
|**Message**|電腦 QoS 原則的長度為零。 有問題的原則列在具有索引 "%2" 的電腦層級 QoS 原則根機碼底下。|

|||
|-|-|
|**Id**|16707|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|
|**語言**|英文|
|**Message**|使用者 QoS 原則的名稱長度為零。 有問題的原則列在索引 "%2" 的使用者層級 QoS 原則根機碼底下。|

|||
|-|-|
|**Id**|16708|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|
|**語言**|英文|
|**Message**|QoS 無法開啟電腦 QoS 原則的登錄子機碼。 原則會列在具有索引 "%2" 的電腦層級 QoS 原則根機碼底下。|

|||
|-|-|
|**Id**|16709|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|
|**語言**|英文|
|**Message**|QoS 無法開啟使用者 QoS 原則的登錄子機碼。 此原則會列在索引為 "%2" 的使用者層級 QoS 原則根機碼底下。|

|||
|-|-|
|**Id**|16710|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|
|**語言**|英文|
|**Message**|QoS 無法讀取或驗證電腦 QoS 原則 "%3" 的 "%2" 欄位。|

|||
|-|-|
|**Id**|16711|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|
|**語言**|英文|
|**Message**|QoS 無法讀取或驗證使用者 QoS 原則 "%3" 的 "%2" 欄位。|

|||
|-|-|
|**Id**|16712|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|
|**語言**|英文|
|**Message**|QoS 無法讀取或設定輸入 TCP 輸送量層級，錯誤碼： "%2"。|

|||
|-|-|
|**Id**|16713|
|**嚴重性**|錯誤|
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|
|**語言**|英文|
|**Message**|QoS 無法讀取或設定 DSCP 標記覆寫設定，錯誤碼： "%2"。|

如需本指南中的下一個主題，請參閱[QoS 原則](qos-policy-faq.md)的常見問題。

如需本指南的第一個主題，請參閱[服務品質 (QoS) 原則](qos-policy-top.md)。
