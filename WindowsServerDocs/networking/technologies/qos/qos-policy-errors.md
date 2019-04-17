---
title: 錯誤 QoS 原則和事件訊息
description: 本主題提供適用於 Windows Server 2016 中的品質服務 (QoS) 原則的錯誤和事件簡訊清單。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e2e890a7947d4f7de09159d7de606c0542f45045
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-error-and-event-messages"></a>錯誤 QoS 原則和事件訊息

>適用於：Windows Server（以每年次管道）、Windows Server 2016

以下是 QoS 原則相關聯的錯誤和事件訊息。  
  
## <a name="informational-messages"></a>資訊訊息  

以下是清單 QoS 原則提示訊息。

|||  
|-|-|  
|**訊息**|16500|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**語言**|英文|  
|**訊息**|電腦 QoS 原則整理成功。 偵測到的任何變更。|  
  
|||  
|-|-|  
|**訊息**|16501|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**語言**|英文|  
|**訊息**|電腦 QoS 原則整理成功。 偵測到原則變更。|  
  
|||  
|-|-|  
|**訊息**|16502|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**語言**|英文|  
|**訊息**|使用者 QoS 原則整理成功。 偵測到的任何變更。|  
  
|||  
|-|-|  
|**訊息**|16503|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**語言**|英文|  
|**訊息**|使用者 QoS 原則整理成功。 偵測到原則變更。|  
  
|||  
|-|-|  
|**訊息**|16504|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**語言**|英文|  
|**訊息**|重新整理輸入 TCP 輸送量層級的進階 QoS 設定。 任何 QoS 原則不指定設定值。 會套用到本機電腦的預設值。|  
  
|||  
|-|-|  
|**訊息**|16505|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**語言**|英文|  
|**訊息**|重新整理輸入 TCP 輸送量層級的進階 QoS 設定。 將值設定為層級 0（最低輸送量）。|  
  
|||  
|-|-|  
|**訊息**|16506|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**語言**|英文|  
|**訊息**|重新整理輸入 TCP 輸送量層級的進階 QoS 設定。 將值設定為層級 1。|  
  
|||  
|-|-|  
|**訊息**|16507|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**語言**|英文|  
|**訊息**|重新整理輸入 TCP 輸送量層級的進階 QoS 設定。 將值設定為層級 2。|  
  
|||  
|-|-|  
|**訊息**|16508|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**語言**|英文|  
|**訊息**|重新整理輸入 TCP 輸送量層級的進階 QoS 設定。 將值設定為層級 3（最大的輸送量）。|  
  
|||  
|-|-|  
|**訊息**|16509|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**語言**|英文|  
|**訊息**|[進階] QoS 設定 DSCP 標示會覆寫成功重新整理。 不指定設定值。 應用程式可以設定獨立 QoS 原則 DSCP 值。|  
  
|||  
|-|-|  
|**訊息**|16510|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**語言**|英文|  
|**訊息**|[進階] QoS 設定 DSCP 標示會覆寫成功重新整理。 將忽略應用程式 DSCP 標示要求。 僅限 QoS 原則設定 DSCP 值。|  
  
|||  
|-|-|  
|**訊息**|16511|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**語言**|英文|  
|**訊息**|[進階] QoS 設定 DSCP 標示會覆寫成功重新整理。 應用程式可以設定獨立 QoS 原則 DSCP 值。|  
  
|||  
|-|-|  
|**訊息**|16512|  
|**嚴重性**|資訊|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**語言**|英文|  
|**訊息**|根據網域網路分類 QoS 原則選擇性應用程式已停用。 將 QoS 原則套用到所有網路介面。|  
  
## <a name="warning-messages"></a>警告訊息

以下是清單 QoS 原則警告訊息。

|||  
|-|-|  
|**訊息**|16600|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**語言**|英文|  
|**訊息**|EQOS: * * * Testing\ * \ * \ * [，以一個字串]」%2」。|  
  
|||  
|-|-|  
|**訊息**|16601|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**語言**|英文|  
|**訊息**|EQOS: * * * Testing\ * \ * \ * [，string1 是具有兩個字串，]」%2「[，是 string2]」%3」。|  
  
|||  
|-|-|  
|**訊息**|16602|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**語言**|英文|  
|**訊息**|電腦 QoS 原則」%2」具有不正確的版本號碼。 這項原則不會套用。|  
  
|||  
|-|-|  
|**訊息**|16603|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**語言**|英文|  
|**訊息**|使用者 QoS 原則」%2」具有不正確的版本號碼。 這項原則不會套用。|  
  
|||  
|-|-|  
|**訊息**|16604|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**語言**|英文|  
|**訊息**|電腦 QoS 原則」%2」不會指定 DSCP 值或速度率。 這項原則不會套用。|  
  
|||  
|-|-|  
|**訊息**|16605|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**語言**|英文|  
|**訊息**|使用者 QoS 原則」%2「未指定 DSCP 值或速度率。 這項原則不會套用。|  
  
|||  
|-|-|  
|**訊息**|16606|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**語言**|英文|  
|**訊息**|超過電腦 QoS 原則的上限。 將不會套用後續電腦 QoS 原則與 QoS 原則」%2」。|  
  
|||  
|-|-|  
|**訊息**|16607|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**語言**|英文|  
|**訊息**|超過使用者 QoS 原則的上限。 將不會套用後續使用者 QoS 原則與 QoS 原則」%2」。|  
  
|||  
|-|-|  
|**訊息**|16608|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**語言**|英文|  
|**訊息**|與其他 QoS 原則可能發生衝突電腦 QoS 原則」%2」。 查看相關的原則會套用到的規則的文件。|  
  
|||  
|-|-|  
|**訊息**|16609|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**語言**|英文|  
|**訊息**|與其他 QoS 原則可能發生衝突使用者 QoS 原則」%2」。 查看相關的原則會套用到的規則的文件。|  
  
|||  
|-|-|  
|**訊息**|16610|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**語言**|英文|  
|**訊息**|略過電腦 QoS 原則」%2」，因為無法處理的應用程式的路徑。 應用程式路徑可能會是無效的包含無效磁碟機代號，或包含對應的網路磁碟機。|  
  
|||  
|-|-|  
|**訊息**|16611|  
|**嚴重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**語言**|英文|  
|**訊息**|略過使用者 QoS 原則」%2」，因為無法處理的應用程式的路徑。 應用程式路徑可能會是無效的包含無效磁碟機代號，或包含對應的網路磁碟機。|  
  
## <a name="error-messages"></a>錯誤訊息  

以下是清單 QoS 原則的錯誤訊息。

|||  
|-|-|  
|**訊息**|16700|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**語言**|英文|  
|**訊息**|重新整理電腦 QoS 原則失敗。 錯誤碼:「%2」。|  
  
|||  
|-|-|  
|**訊息**|16701|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**語言**|英文|  
|**訊息**|若要重新整理無法使用者 QoS 原則。 錯誤碼:「%2」。|  
  
|||  
|-|-|  
|**訊息**|16702|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**語言**|英文|  
|**訊息**|QoS 無法開放電腦層級根金鑰 QoS 原則。 錯誤碼:「%2」。|  
  
|||  
|-|-|  
|**訊息**|16703|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**語言**|英文|  
|**訊息**|QoS 無法開放使用者層級根金鑰 QoS 原則。 錯誤碼:「%2」。|  
  
|||  
|-|-|  
|**訊息**|16704|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**語言**|英文|  
|**訊息**|電腦 QoS 原則超過最多允許的名稱。 電腦層級 QoS 原則根金鑰、與索引」%2] 底下列出的有問題的原則。|  
  
|||  
|-|-|  
|**訊息**|16705|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**語言**|英文|  
|**訊息**|使用者 QoS 原則超過最多允許的名稱。 使用者層級 QoS 原則根金鑰、與索引」%2] 底下列出的有問題的原則。|  
  
|||  
|-|-|  
|**訊息**|16706|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**語言**|英文|  
|**訊息**|電腦 QoS 原則有零長度名稱。 電腦層級 QoS 原則根金鑰、與索引」%2] 底下列出的有問題的原則。|  
  
|||  
|-|-|  
|**訊息**|16707|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**語言**|英文|  
|**訊息**|使用者 QoS 原則有零長度名稱。 使用者層級 QoS 原則根金鑰、與索引」%2] 底下列出的有問題的原則。|  
  
|||  
|-|-|  
|**訊息**|16708|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**語言**|英文|  
|**訊息**|QoS 無法開放登錄子電腦 QoS 原則。 電腦層級 QoS 原則根金鑰、與索引」%2] 底下列出的原則。|  
  
|||  
|-|-|  
|**訊息**|16709|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**語言**|英文|  
|**訊息**|QoS 無法開放登錄子使用者 QoS 原則。 使用者層級 QoS 原則根金鑰、與索引」%2] 底下列出的原則。|  
  
|||  
|-|-|  
|**訊息**|16710|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**語言**|英文|  
|**訊息**|朗讀或驗證 QoS 原則」%3「電腦」%2] 欄位 QoS 失敗。|  
  
|||  
|-|-|  
|**訊息**|16711|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**語言**|英文|  
|**訊息**|朗讀或驗證使用者 QoS 原則」%3「」%2] 欄位 QoS 失敗。|  
  
|||  
|-|-|  
|**訊息**|16712|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**語言**|英文|  
|**訊息**|QoS 無法讀取或設定輸入的 TCP 輸送量層級，錯誤代碼:「%2」。|  
  
|||  
|-|-|  
|**訊息**|16713|  
|**嚴重性**|錯誤|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**語言**|英文|  
|**訊息**|無法朗讀或設定 DSCP 標記 QoS 覆寫設定，錯誤代碼:「%2」。|  

本指南下一步主題，請查看[QoS 原則常見問題集](qos-policy-faq.md)。

本指南中第一次主題，請查看[品質服務 (QoS) 原則](qos-policy-top.md)。
