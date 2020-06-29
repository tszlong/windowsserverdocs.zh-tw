---
title: 容錯移轉叢集系統記錄檔事件
description: Windows Server 系統記錄中的容錯移轉叢集事件清單。 這些事件可以用來對叢集進行疑難排解。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 01/14/2020
ms.openlocfilehash: 24be2b39c8130b97d22ee182c0064986b3378549
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472975"
---
# <a name="failover-clustering-system-log-events"></a>容錯移轉叢集系統記錄檔事件

> 適用於：Windows Server 2019、Windows Server 2016

本主題列出 Windows Server 系統記錄檔中的容錯移轉叢集事件（可在事件檢視器中看到）。 這些事件全都共用**故障叢集**的事件來源，而且在疑難排解叢集時很有説明。

## <a name="critical-events"></a>重大事件

### <a name="event-1000-unexpected_fatal_error"></a>事件1000： UNEXPECTED_FATAL_ERROR

叢集服務在來源模組 %2 的第 %1 行發生未預期的嚴重錯誤。 錯誤碼為 %3。

### <a name="event-1001-assertion_failure"></a>事件1001： ASSERTION_FAILURE

叢集服務在來源模組 %2 的第 %1 行上的有效性檢查失敗。 ' %3 '

### <a name="event-1006-nm_event_membership_halt"></a>事件1006： NM_EVENT_MEMBERSHIP_HALT

因為與其他叢集節點的連線不完整，所以已停止叢集服務。

### <a name="event-1057-dm_database_corrupt_or_missing"></a>事件1057： DM_DATABASE_CORRUPT_OR_MISSING

無法載入叢集資料庫。 檔案可能已遺失或損毀。
可能會嘗試自動修復。

### <a name="event-1070-nm_event_begin_join_failed"></a>事件1070： NM_EVENT_BEGIN_JOIN_FAILED

節點無法聯結容錯移轉叢集 ' %1 '，因為錯誤碼 ' %2 '。

### <a name="event-1073-cs_event_inconsistency_halt"></a>事件1073： CS_EVENT_INCONSISTENCY_HALT

叢集服務已停止，以避免容錯移轉叢集內發生不一致的情況。 錯誤碼為 ' %1 '。

### <a name="event-1080-cs_diskwrite_failure"></a>事件1080： CS_DISKWRITE_FAILURE

叢集服務無法更新叢集資料庫（錯誤碼 ' %1 '）。
可能的原因是磁碟空間不足或檔案系統損毀。

### <a name="event-1090-cs_event_reg_operation_failed"></a>事件1090： CS_EVENT_REG_OPERATION_FAILED

無法啟動叢集服務。 嘗試從 Windows 登錄讀取設定資料失敗，錯誤為 ' %1 '。 請使用 [容錯移轉叢集管理] 嵌入式管理單元，確保這部電腦是叢集的成員。
如果您想要將這部電腦新增至現有的叢集，請使用 [新增節點嚮導]。 或者，如果這部電腦已設定為叢集的成員，則需要還原叢集服務所需的遺失設定資料，以識別它是否為叢集的成員。
執行這部電腦的系統狀態還原，以便還原設定資料。

### <a name="event-1092-nm_event_mm_form_failed"></a>事件1092： NM_EVENT_MM_FORM_FAILED

無法形成叢集 ' %1 '，錯誤碼為 ' %2 '。 容錯移轉叢集將無法使用。

### <a name="event-1093-nm_event_node_not_member"></a>事件1093： NM_EVENT_NODE_NOT_MEMBER

叢集服務無法將節點 ' %1 ' 識別為容錯移轉叢集 ' %2 ' 的成員。 如果此節點的電腦名稱稱最近已變更，請考慮將還原為先前的名稱。 或者，將節點新增至容錯移轉叢集，如有必要，請重新安裝叢集應用程式。

### <a name="event-1105-cs_event_rpc_init_failed"></a>事件1105： CS_EVENT_RPC_INIT_FAILED

叢集服務無法啟動，因為它無法向 RPC 服務登錄介面。 錯誤碼為 ' %1 '。

### <a name="event-1135-event_node_down"></a>事件1135： EVENT_NODE_DOWN

叢集節點 ' %1 ' 已從作用中的容錯移轉叢集成員資格中移除。 此節點上的叢集服務可能已停止。 這也可能是因為節點與容錯移轉叢集中的其他使用中節點失去通訊。
執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與此節點上網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1146-rcm_event_resmon_died"></a>事件1146： RCM_EVENT_RESMON_DIED

叢集資源主控子系統（RHS）進程已終止，將會重新開機。 這通常與資源的叢集健全狀況偵測和復原相關聯。 請參閱系統事件記錄檔，以判斷造成此問題的資源和資源 DLL。

### <a name="event-1177-mm_event_arbitration_failed"></a>事件1177： MM_EVENT_ARBITRATION_FAILED

叢集服務正在關閉，因為仲裁已遺失。 這可能是因為叢集中的部分或所有節點之間的網路連線中斷，或見證磁碟的容錯移轉。

執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1181-netres_resource_start_error"></a>事件1181： NETRES_RESOURCE_START_ERROR

叢集資源 ' %1 ' 無法上線，因為相關聯的服務無法啟動。 服務傳回碼為 ' %2 '。 請檢查與服務相關聯的其他事件，並確定服務已正確啟動。

### <a name="event-1247-cluster_event_invalid_service_sid_type"></a>事件1247： CLUSTER_EVENT_INVALID_SERVICE_SID_TYPE

叢集服務的安全識別碼（SID）類型已設定為 ' %1 '，但預期的 SID 類型為「不受限制」。 叢集服務會自動使用服務控制管理員（SCM）來修改其 SID 類型設定，而且會重新開機，這項變更才會生效。

### <a name="event-1248-cluster_event_service_sid_missing"></a>事件1248： CLUSTER_EVENT_SERVICE_SID_MISSING

與叢集服務相關聯的安全識別碼（SID） ' %1 ' 不存在於處理常式權杖中。 叢集服務會自動修正此問題，並重新啟動。

### <a name="event-1282-sm_event_handshake_timeout"></a>事件1282： SM_EVENT_HANDSHAKE_TIMEOUT

本機與遠端端點 ' %2 ' 之間的安全性交握未在 ' %1 ' 秒內完成，節點正在終止連線

### <a name="event-1542-service_prerestore_failed"></a>事件1542： SERVICE_PRERESTORE_FAILED

叢集設定資料的還原要求失敗。 此還原在「還原前」階段期間失敗，通常表示組成叢集的某些節點目前無法運作。 確定叢集服務已在組成此叢集的所有節點上順利執行。

### <a name="event-1543-service_postrestore_failed"></a>事件1543： SERVICE_POSTRESTORE_FAILED

叢集設定資料的還原操作失敗。 此還原在「還原後」階段期間失敗，通常表示組成叢集的某些節點目前無法運作。 建議您將目前的叢集設定資料檔案（ClusDB）取代為 ' %1 '。

### <a name="event-1546-service_form_version_incompatible"></a>事件1546： SERVICE_FORM_VERSION_INCOMPATIBLE

節點 ' %1 ' 無法形成容錯移轉叢集。 這是因為一或多個節點執行的叢集服務軟體版本不相容。 如果節點 ' %1 ' 或叢集中的其他節點最近已升級，請重新確認所有節點都在執行相容的叢集服務軟體版本。

### <a name="event-1547-service_connect_version_incompatible"></a>事件1547： SERVICE_CONNECT_VERSION_INCOMPATIBLE

節點 ' %1 ' 已嘗試加入容錯移轉叢集，但因為叢集服務軟體的版本不相容而失敗。 如果節點 ' %1 ' 或叢集中的其他節點最近已升級，請確認支援使用不同版本的叢集服務軟體進行變更的叢集部署。

### <a name="event-1553-service_no_network_connectivity"></a>事件1553： SERVICE_NO_NETWORK_CONNECTIVITY

此叢集節點沒有網路連線能力。 它無法參與叢集，直到連線恢復為止。 執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1554-service_network_connectivity_lost"></a>事件1554： SERVICE_NETWORK_CONNECTIVITY_LOST

此叢集節點已遺失所有的網路連線。 它無法參與叢集，直到連線恢復為止。 此節點上的叢集服務將會終止。

### <a name="event-1556-service_unhandled_exception_in_worker_thread"></a>事件1556： SERVICE_UNHANDLED_EXCEPTION_IN_WORKER_THREAD

叢集服務發生未預期的問題，將會關閉。 錯誤碼為 ' %2 '。

### <a name="event-1561-service_nonstorage_witness_better_tag"></a>事件1561： SERVICE_NONSTORAGE_WITNESS_BETTER_TAG

叢集服務無法啟動，因為此節點偵測到它沒有最新的叢集設定資料複本。 當此節點不在成員資格中，而導致無法接收設定資料更新時，就會發生叢集變更。

#### <a name="guidance"></a>指引

嘗試在叢集中的所有節點上啟動叢集服務，讓具有最新叢集設定資料複本的節點可以先形成叢集。 這個節點接著就能夠加入叢集，並自動取得更新的叢集設定資料。 如果最新的叢集設定資料複本沒有可用的節點，請執行 ' Start-clusternode-FQ ' Windows PowerShell Cmdlet。 使用 ForceQuorum （FQ）參數會啟動叢集服務，並將此節點的叢集設定資料複本標示為已授權。 在具有過期叢集資料庫複本的節點上強制仲裁，可能會導致叢集設定變更，因為節點未參與叢集而無法中斷。

### <a name="event-1564-res_fsw_arbitratefailure"></a>事件1564： RES_FSW_ARBITRATEFAILURE

檔案共用見證資源 ' %1 ' 無法仲裁檔案共用 ' %2 '。
請確認檔案共用 ' %2 ' 存在，而且可由叢集存取。

### <a name="event-1570-service_connect_authentication_failure"></a>事件1570： SERVICE_CONNECT_AUTHENTICATION_FAILURE

節點 ' %1 ' 在加入叢集時，無法建立通訊會話。
這是因為驗證失敗所造成。 請確認節點是否正在執行相容的叢集服務軟體版本。

### <a name="event-1571-service_connect_authorizaion_failure"></a>事件1571： SERVICE_CONNECT_AUTHORIZAION_FAILURE

節點 ' %1 ' 在加入叢集時，無法建立通訊會話。
這是因為授權失敗所造成。 請確認節點是否正在執行相容的叢集服務軟體版本。

### <a name="event-1572-service_netft_route_initial_failure"></a>事件1572： SERVICE_NETFT_ROUTE_INITIAL_FAILURE

節點 ' %1 ' 無法加入叢集，因為它無法傳送和接收與其他叢集節點的失敗偵測網路訊息。 請執行 [驗證設定向導] 以確保網路設定。 另請確認 Windows 防火牆的「容錯移轉叢集」規則。

### <a name="event-1574-dm_database_unload_failed"></a>事件1574： DM_DATABASE_UNLOAD_FAILED

無法卸載容錯移轉叢集資料庫。 如果重新開機叢集服務無法修正問題，請重新開機電腦。

### <a name="event-1575-dm_database_corrupt_or_missing_fixquorum"></a>事件1575： DM_DATABASE_CORRUPT_OR_MISSING_FIXQUORUM

嘗試強制啟動叢集服務失敗，因為此節點上的叢集設定資料遺失或損毀。 請先在具有完整且有效的叢集設定資料複本的另一個節點上啟動叢集服務。 然後，重新嘗試此節點上的啟動作業（這會嘗試自動取得更新的有效設定資訊）。 如果沒有其他可用的節點，請使用 WBAdmin 執行此節點的系統狀態還原，以便還原設定資料。

### <a name="event-1593-dm_could_not_discard_changes"></a>事件1593： DM_COULD_NOT_DISCARD_CHANGES

無法卸載容錯移轉叢集資料庫，也無法捨棄記憶體中可能不正確的任何變更。 叢集服務會嘗試從另一個叢集節點抓取來修復資料庫。 如果叢集服務未上線，請重新開機此節點上的叢集服務。 如果重新開機叢集服務無法修正問題，請重新開機電腦。 如果叢集服務在重新開機後無法上線，請從最後一次備份還原叢集資料庫。 目前的資料庫已複製到 ' %1 '。 如果沒有可用的備份，請將 ' %1 ' 複製到 ' %2 ' 並嘗試啟動叢集服務。 如果叢集服務在此節點上上線，部分叢集設定變更可能會遺失，且叢集可能無法正常運作。 執行 [驗證設定向導] 來檢查您的叢集設定，並確認託管服務和應用程式已上線且正常運作。

### <a name="event-1672-event_node_quarantined"></a>事件1672： EVENT_NODE_QUARANTINED

叢集節點 ' %1 ' 已被隔離。 節點在短時間內發生「%2」連續失敗，並已從叢集中移除，以避免發生進一步的中斷。 節點將會隔離到 ' %3 '，然後節點會自動嘗試重新加入叢集。

請參閱系統和應用程式事件記錄檔，以判斷此節點上的問題。 問題解決後，您可以手動清除隔離，以允許節點與 ' Start-clusternode – ClearQuarantine ' Windows PowerShell Cmdlet 重新加入。

節點名稱： %1

連續叢集成員資格的數目遺失： %2

將自動清除時間隔離： %3

## <a name="error-events"></a>錯誤事件

### <a name="event-1024-cp_reg_ckpt_restore_failed"></a>事件1024： CP_REG_CKPT_RESTORE_FAILED

無法將叢集資源 ' %1 ' 的登錄檢查點還原到登錄機碼 HKEY_LOCAL_MACHINE \\ %2。 資源可能無法正常運作。
請確定沒有其他進程在此登錄子樹中有開啟的登錄機碼控制碼。

### <a name="event-1034-res_disk_missing"></a>事件1034： RES_DISK_MISSING

因為找不到相關聯的磁片，所以無法讓叢集實體磁片資源 ' %1 ' 上線。 磁片的預期簽章為 ' %2 '。
如果更換或復原磁碟，您可以在 [容錯移轉叢集管理員] 嵌入式管理單元中，使用 [修復] 功能（在磁片的 [內容] 工作表中）來修復新的或已還原的磁片。 如果磁片將不會被取代，請刪除相關聯的磁片資源。

### <a name="event-1035-res_disk_mount_failed"></a>事件1035： RES_DISK_MOUNT_FAILED

當磁片資源 ' %1 ' 已上線時，存取一或多個磁片區失敗，錯誤為 ' %2 '。 執行 [驗證設定] 嚮導，以檢查您的存放裝置設定。 （選擇性）您可能想要執行 Chkdsk，以確認此磁片上所有磁片區的完整性。

### <a name="event-1037-res_disk_filesystem_failed"></a>事件1037： RES_DISK_FILESYSTEM_FAILED

資源 ' %1 ' 的磁片上有一或多個分割區的檔案系統可能已損毀。 執行 [驗證設定] 嚮導，以檢查您的存放裝置設定。 （選擇性）您可能想要執行 Chkdsk，以確認此磁片上所有磁片區的完整性。

### <a name="event-1038-res_disk_reservation_lost"></a>事件1038： RES_DISK_RESER加值稅ION_LOST

此節點意外遺失叢集磁片 ' %1 ' 的擁有權。 執行 [驗證設定] 嚮導，以檢查您的存放裝置設定。

### <a name="event-1039-res_genapp_create_failed"></a>事件1039： RES_GENAPP_CREATE_FAILED

嘗試建立進程時，一般應用程式 ' %1 ' 無法上線（錯誤為 ' %2 '）。 可能的原因包括：應用程式可能不存在於此節點上，路徑名稱可能指定不正確，二進位檔名稱可能未正確指定。

### <a name="event-1040-res_gensvc_open_failed"></a>事件1040： RES_GENSVC_OPEN_FAILED

嘗試開啟服務時，一般服務 ' %1 ' 無法上線（錯誤為 ' %2 '）。 可能的原因包括：未安裝服務，或指定的服務名稱無效。

### <a name="event-1041-res_gensvc_start_failed"></a>事件1041： RES_GENSVC_START_FAILED

嘗試啟動服務時，一般服務 ' %1 ' 無法上線（錯誤為 ' %2 '）。 可能的原因：指定的服務參數可能無效。

### <a name="event-1042-res_gensvc_failed_after_start"></a>事件1042： RES_GENSVC_FAILED_AFTER_START

一般服務 ' %1 ' 失敗，錯誤為 ' %2 '。 請檢查應用程式事件記錄檔。

### <a name="event-1044-res_ipaddr_nbt_interface_create_failed"></a>事件1044： RES_IPADDR_NBT_INTERFACE_CREATE_FAILED

嘗試建立新的 NetBIOS 介面時，發生失敗，同時將資源 ' %1 ' 上線（錯誤碼 ' %2 '）。 可能已超過 NetBIOS 名稱的最大數目。

### <a name="event-1046-res_ipaddr_invalid_subnet"></a>事件1046： RES_IPADDR_INVALID_SUBNET

叢集 IP 位址資源 ' %1 ' 無法上線，因為子網路遮罩值無效。 請檢查您的 IP 位址資源屬性。

### <a name="event-1047-res_ipaddr_invalid_address"></a>事件1047： RES_IPADDR_INVALID_ADDRESS

叢集 IP 位址資源 ' %1 ' 無法上線，因為位址值無效。 請檢查您的 IP 位址資源屬性。

### <a name="event-1048-res_ipaddr_invalid_adapter"></a>事件1048： RES_IPADDR_INVALID_ADAPTER

叢集 IP 位址資源 ' %1 ' 無法上線。 無法判斷對應至叢集網路介面 ' %2 ' 之網路介面卡的設定資料（錯誤碼為 ' %3 '）。 請檢查 IP 位址資源是否已設定正確的位址和網路屬性。

### <a name="event-1049-res_ipaddr_in_use"></a>事件1049： RES_IPADDR_IN_USE

叢集 IP 位址資源 ' %1 ' 無法上線，因為在網路上偵測到重複的 IP 位址 ' %2 '。 請確定所有 IP 位址都是唯一的。

### <a name="event-1050-res_netname_duplicate"></a>事件1050： RES_NETNAME_DUPLICATE

叢集網路名稱資源 ' %1 ' 無法上線，因為名稱 ' %2 ' 符合此叢集節點名稱。 請確定網路名稱是唯一的。

### <a name="event-1051-res_netname_no_ip_address"></a>事件1051： RES_NETNAME_NO_IP_ADDRESS

叢集網路名稱資源 ' %1 ' 無法上線。 請確定相依 IP 位址資源的網路介面卡可存取至少一部 DNS 伺服器。 或者，針對相依的 IP 位址啟用 NetBIOS。

### <a name="event-1052-res_netname_cant_add_name_status"></a>事件1052： RES_NETNAME_CANT_ADD_NAME_STATUS

叢集網路名稱資源 ' %1 ' 無法上線，因為無法將名稱新增至系統。 相關聯的錯誤碼會儲存在 [資料] 區段中。

### <a name="event-1053-res_smb_cant_create_share"></a>事件1053： RES_SMB_CANT_CREATE_SHARE

叢集檔案共用 ' %1 ' 無法上線，因為無法建立共用。

### <a name="event-1054-res_smb_share_not_found"></a>事件1054： RES_SMB_SHARE_NOT_FOUND

檔案共用資源 ' %1 ' 的健全狀況檢查失敗。 正在抓取共用 ' %2 ' （範圍限定在網路名稱 %3）的資訊，傳回錯誤碼 ' %4 '。 請確認共用存在且可供存取。

### <a name="event-1055-res_smb_share_failed"></a>事件1055： RES_SMB_SHARE_FAILED

檔案共用資源 ' %1 ' 的健全狀況檢查失敗。 正在抓取共用 ' %2 ' 的資訊（範圍限定在網路名稱 %3），表示共用不存在（錯誤碼 ' %4 '）。 請確認共用存在且可供存取。

### <a name="event-1069-rcm_resource_failure"></a>事件1069： RCM_RESOURCE_FAILURE

叢集角色 ' %2 ' 中的叢集資源 ' %1 ' 失敗。

### <a name="event-1069-rcm_resource_failure_with_typename"></a>事件1069： RCM_RESOURCE_FAILURE_WITH_TYPENAME

叢集角色 ' %2 ' 中類型 ' %3 ' 的叢集資源 ' %1 ' 失敗。<br><br>根據資源和角色的失敗原則，叢集服務可能會嘗試讓此節點上的資源上線，或將群組移至叢集的另一個節點，然後重新開機它。 使用容錯移轉叢集管理員或 Get-clusterresource Windows PowerShell Cmdlet 來檢查資源和群組狀態。

### <a name="event-1069-rcm_resource_failure_with_cause"></a>事件1069： RCM_RESOURCE_FAILURE_WITH_CAUSE

叢集角色 ' %2 ' 中類型 ' %3 ' 的叢集資源 ' %1 ' 失敗。 錯誤碼為 ' %5 ' （' %4 '）。<br><br>根據資源和角色的失敗原則，叢集服務可能會嘗試讓此節點上的資源上線，或將群組移至叢集的另一個節點，然後重新開機它。 使用容錯移轉叢集管理員或 Get-clusterresource Windows PowerShell Cmdlet 來檢查資源和群組狀態。

### <a name="event-1069-rcm_resource_failure_with_error_code"></a>事件1069： RCM_RESOURCE_FAILURE_WITH_ERROR_CODE

叢集角色 ' %2 ' 中類型 ' %3 ' 的叢集資源 ' %1 ' 失敗。 錯誤碼為 ' %4 '。<br><br>根據資源和角色的失敗原則，叢集服務可能會嘗試讓此節點上的資源上線，或將群組移至叢集的另一個節點，然後重新開機它。 使用容錯移轉叢集管理員或 Get-clusterresource Windows PowerShell Cmdlet 來檢查資源和群組狀態。

### <a name="event-1069-rcm_resource_failure_due_to_veto"></a>事件1069： RCM_RESOURCE_FAILURE_DUE_TO_VETO

叢集角色 ' %2 ' 中類型 ' %3 ' 的叢集資源 ' %1 ' 失敗，因為嘗試封鎖該叢集資源的必要狀態變更。

### <a name="event-1077-res_ipaddr_ipv4_address_interface_failed"></a>事件1077： RES_IPADDR_IPV4_ADDRESS_INTERFACE_FAILED

IP 介面 ' %1 ' （位址 ' %2 '）的健康情況檢查失敗（狀態為 ' %3 '）。 執行 [驗證設定向導] 以確保網路介面卡正常運作。

### <a name="event-1078-res_ipaddr_wins_address_failed"></a>事件1078： RES_IPADDR_WINS_ADDRESS_FAILED

叢集 IP 位址資源 ' %1 ' 無法上線，因為在介面 ' %2 ' 上的 WINS 註冊失敗，錯誤為 ' %3 '。 請確定已指定有效、可存取的 WINS 伺服器。

### <a name="event-1121-cp_crypto_ckpt_restore_failed"></a>事件1121： CP_CRYPTO_CKPT_RESTORE_FAILED

無法成功將叢集資源 ' %1 ' 的加密設定套用到此節點上的容器名稱 ' %2 '。

### <a name="event-1127-tm_event_cluster_netinterface_failed"></a>事件1127： TM_EVENT_CLUSTER_NETINTERFACE_FAILED

網路 ' %3 ' 上叢集節點 ' %2 ' 的叢集網路介面 ' %1 ' 失敗。 執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1129-tm_event_cluster_network_partitioned"></a>事件1129： TM_EVENT_CLUSTER_NETWORK_PARTITIONED

叢集網路 ' %1 ' 已分割。 某些附加的容錯移轉叢集節點無法透過網路彼此通訊。 容錯移轉叢集無法判斷失敗的位置。 執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1130-tm_event_cluster_network_down"></a>事件1130： TM_EVENT_CLUSTER_NETWORK_DOWN

叢集網路 ' %1 ' 已關閉。 沒有任何可用的節點可以使用此網路進行通訊。 執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1137-rcm_drain_move_failed"></a>事件1137： RCM_DRAIN_MOVE_FAILED

無法完成移動叢集角色 ' %1 ' 以進行清空。 操作失敗，錯誤碼為 %2。

### <a name="event-1138-res_smb_cant_create_dfs_root"></a>事件1138： RES_SMB_CANT_CREATE_DFS_ROOT

叢集檔案共用資源 ' %1 ' 無法上線。 建立 DFS 命名空間根失敗，錯誤為 ' %3 '。 這可能是因為無法啟動「DFS 命名空間」服務，或無法建立共用 ' %2 ' 的 DFS-N 根。

### <a name="event-1141-res_smb_cant_init_dfs_svc"></a>事件1141： RES_SMB_CANT_INIT_DFS_SVC

叢集檔案共用資源 ' %1 ' 無法上線。 重新同步 DFS 根目標 ' %2 ' 失敗，錯誤為 ' %3 '。

### <a name="event-1142-res_smb_cant_online_dfs_root"></a>事件1142： RES_SMB_CANT_ONLINE_DFS_ROOT

因為發生錯誤 ' %2 '，所以 DFS 命名空間的叢集檔案共用資源 ' %1 ' 無法上線。

### <a name="event-1182-netres_resource_stop_error"></a>事件1182： NETRES_RESOURCE_STOP_ERROR

叢集資源 ' %1 ' 無法上線，因為相關聯的服務嘗試重新開機失敗。 錯誤碼為 ' %2 '。 服務需要重新開機，才能更新參數。 不過，服務會在停止並重新啟動之前失敗。 請檢查與服務相關聯的其他事件，並確定服務正常運作。

### <a name="event-1183-res_disk_invalid_mp_source_not_clustered"></a>事件1183： RES_DISK_INVALID_MP_SOURCE_NOT_CLUSTERED

叢集磁片資源 ' %1 ' 包含不正確掛接點。 與掛接點關聯的來源和目標磁片都必須是叢集磁片，而且必須是相同群組的成員。 <br>磁片區 ' %3 ' 的掛接點 ' %2 ' 參照了不正確來源磁片。 請確定來源磁片也是叢集磁片，而且位於與目標磁片相同的群組中（裝載掛接點）。

### <a name="event-1191-res_netname_delete_computer_account_failed_status"></a>事件1191： RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED_STATUS

叢集網路名稱資源 ' %1 ' 無法在網域 ' %2 ' 中刪除其相關聯的電腦物件。 錯誤碼為 ' %3 '。 <br>請讓網域系統管理員手動刪除 Active Directory 網域中的電腦物件。

### <a name="event-1192-res_netname_delete_computer_account_failed"></a>事件1192： RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED

因為下列原因，叢集網路名稱資源 ' %1 ' 無法刪除其在網域 ' %2 ' 中相關聯的電腦物件：<br>%3。 <br><br>請讓網域系統管理員手動刪除 Active Directory 網域中的電腦物件。

### <a name="event-1193-res_netname_add_computer_account_failed_status"></a>事件1193： RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED_STATUS

因為下列原因，叢集網路名稱資源 ' %1 ' 無法在網域 ' %2 ' 中建立其相關聯的電腦物件： %3。<br><br>相關聯的錯誤碼為： %5<br><br>
請與您的網域系統管理員合作，以確保：

- 叢集識別 ' %4 ' 可以建立電腦物件。 根據預設，所有電腦物件都會建立在「電腦」容器中;如果此位置已變更，請洽詢網域系統管理員。
- 尚未到達電腦物件的配額。
- 如果有現有的 computer 物件，請使用 [Active Directory 使用者和電腦] 工具，確認叢集身分識別 ' %4 ' 具有該電腦物件的「完全控制」許可權。

### <a name="event-1194-res_netname_add_computer_account_failed"></a>事件1194： RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED

叢集網路名稱資源 ' %1 ' 無法在： %3 期間，于網域 ' %2 ' 中建立其相關聯的電腦物件。<br><br>相關聯錯誤碼的文字為： %4

請與您的網域系統管理員合作，以確保：

- 叢集身分識別 ' %5 ' 具有「建立電腦物件」許可權。 根據預設，所有電腦物件會建立在與叢集身分識別 ' %5 ' 相同的容器中。
- 尚未到達電腦物件的配額。
- 如果有現有的 computer 物件，請使用 [Active Directory 使用者和電腦] 工具，確認叢集身分識別 ' %5 ' 具有該電腦物件的「完全控制」許可權。

### <a name="event-1195-res_netname_dns_registration_failed_status"></a>事件1195： RES_NETNAME_DNS_REGISTRATION_FAILED_STATUS

叢集網路名稱資源 ' %1 ' 無法註冊一或多個相關聯的 DNS 名稱。 錯誤碼為 ' %2 '。 確定與相依 IP 位址資源相關聯的網路介面卡已設定至少有一部 DNS 伺服器的存取權。

### <a name="event-1196-res_netname_dns_registration_failed"></a>事件1196： RES_NETNAME_DNS_REGISTRATION_FAILED

因為下列原因，叢集網路名稱資源 ' %1 ' 無法註冊一或多個相關聯的 DNS 名稱：<br>%2。<br><br>確定與相依 IP 位址資源相關聯的網路介面卡已設定至少一個可存取的 DNS 伺服器。

### <a name="event-1205-rcm_event_group_failed_online_offline"></a>事件1205： RCM_EVENT_GROUP_FAILED_ONLINE_OFFLINE

叢集服務無法使叢集角色 ' %1 ' 完全上線或離線。 一或多個資源可能處於失敗狀態。 這可能會影響叢集角色的可用性。

### <a name="event-1206-res_netname_update_computer_account_failed_status"></a>事件1206： RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED_STATUS

無法在網域 ' %2 ' 中更新與叢集網路名稱資源 ' %1 ' 相關聯的電腦物件。 錯誤碼為 ' %3 '。 叢集識別 ' %4 ' 可能缺少更新物件所需的許可權。 請與您的網域系統管理員合作，以確保叢集身分識別可以更新網域中的電腦物件。

### <a name="event-1207-res_netname_update_computer_account_failed"></a>事件1207： RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED

無法在網域 ' %2 ' 中更新與叢集網路名稱資源 ' %1 ' 相關聯的電腦物件 <br>%3 作業。<br><br>相關聯錯誤碼的文字為： %4<br> <br>叢集識別 ' %5 ' 可能缺少更新物件所需的許可權。 請與您的網域系統管理員合作，以確保叢集身分識別可以更新網域中的電腦物件。

### <a name="event-1208-res_disk_invalid_mp_target_not_clustered"></a>事件1208： RES_DISK_INVALID_MP_TARGET_NOT_CLUSTERED

叢集磁片資源 ' %1 ' 包含不正確掛接點。 與掛接點關聯的來源和目標磁片都必須是叢集磁片，而且必須是相同群組的成員。 <br>磁片區 ' %3 ' 的掛接點 ' %2 ' 參照了不正確目標磁片。 請確定目標磁片也是叢集磁片，而且位於與來源磁片相同的群組中（裝載掛接點）。

### <a name="event-1211-res_netname_no_writeable_dc_status"></a>事件1211： RES_NETNAME_NO_WRITEABLE_DC_STATUS

叢集網路名稱資源 ' %1 ' 無法上線。 嘗試找出可寫入的網域控制站（在網域 %2 中），以建立或更新與資源相關聯的電腦物件失敗。 錯誤碼為 ' %3 '。
請確定已設定網域內的這個節點可以存取可寫入的網域控制站。 此外，請確定 DNS 伺服器正在執行中，以便解析網域控制站的名稱。

### <a name="event-1212-res_netname_no_writeable_dc"></a>事件1212： RES_NETNAME_NO_WRITEABLE_DC

叢集網路名稱資源 ' %1 ' 無法上線。 嘗試找出可寫入的網域控制站（在網域 %2 中），以建立或更新與資源相關聯的電腦物件失敗，原因如下：<br>%3。<br><br> 錯誤碼為 ' %4 '。 請確定已設定網域內的這個節點可以存取可寫入的網域控制站。 此外，請確定 DNS 伺服器正在執行中，以便解析網域控制站的名稱。

### <a name="event-1213-res_netname_rename_restore_failed"></a>事件1213： RES_NETNAME_RENAME_RESTORE_FAILED

叢集網路名稱資源 ' %1 ' 無法在網域控制站 ' %2 ' 上完全重新命名相關聯的電腦物件。 嘗試將電腦物件從新名稱 ' %3 ' 重新命名回其原始名稱 ' %4 ' 也失敗。 錯誤碼為 ' %5 '。 這可能會影響用戶端連線能力，直到網路名稱及其相關聯的電腦物件名稱一致為止。 請洽詢您的網域系統管理員，以手動方式重新命名 computer 物件。

### <a name="event-1214-res_netname_cant_add_name2"></a>事件1214： RES_NETNAME_CANT_ADD_NAME2

無法向 Netbios 註冊叢集網路名稱資源。 <br><br>網路名稱： ' %3 ' <br>失敗的原因： %2 <br>資源名稱： ' %1 ' <br><br> 針對網路名稱執行 nbtstat，以確定該名稱尚未向 Netbios 註冊。

### <a name="event-1215-res_netname_not_registered_with_rdr"></a>事件1215： RES_NETNAME_NOT_REGISTERED_WITH_RDR

叢集網路名稱資源 ' %1 ' 失敗健康情況檢查。 網路名稱 ' %2 ' 已不再于此節點上註冊。 錯誤碼為 ' %3 '。 檢查與網路介面卡相關的硬體或軟體錯誤。 此外，您也可以執行 [驗證設定向導] 來檢查您的網路設定。

### <a name="event-1218-res_netname_online_rename_recovery_missing_account"></a>事件1218： RES_NETNAME_ONLINE_RENAME_RECOVERY_MISSING_ACCOUNT

叢集網路名稱資源 ' %1 ' 無法執行名稱變更作業（嘗試將原始名稱 ' %3 ' 變更為名稱 ' %4 '）。 在網域控制站 ' %2 ' （建立所在的位置）找不到電腦物件。 下次資源上線時，將會嘗試重新建立電腦物件。 此外，請與您的網域系統管理員合作，以確保該電腦物件存在於網域中。

### <a name="event-1219-res_netname_online_rename_dc_not_found"></a>事件1219： RES_NETNAME_ONLINE_RENAME_DC_NOT_FOUND

叢集網路名稱資源 ' %1 ' 無法執行名稱變更作業。
電腦物件 ' %3 ' 正在重新命名的網域控制站 ' %2 ' 無法連絡。 錯誤碼為 ' %4 '。 請確定可以存取可寫入的網域控制站，並檢查是否有任何連線問題。

### <a name="event-1220-res_netname_online_rename_recovery_failed"></a>事件1220： RES_NETNAME_ONLINE_RENAME_RECOVERY_FAILED

無法使用網域控制站 %4 將資源 ' %1 ' 的電腦帳戶從 %2 重新命名為 %3。 相關聯的錯誤碼會儲存在 [資料] 區段中。<br>
<br>此資源的電腦帳戶正在重新命名，且未完成。 這是在此資源的線上程式中偵測到的。
若要復原，電腦帳戶必須重新命名為 Name 屬性的目前值，也就是網路上顯示的名稱。<br> <br>嘗試重新命名的網域控制站可能無法使用;如果是這種情況，請等候網域控制站再次可供使用。 網域控制站可能會拒絕帳戶的存取權;解析存取之後，請嘗試再次使名稱上線。<br> <br>如果無法這麼做，請停用再重新啟用 Kerberos 驗證，並嘗試在不同的 DC 上尋找電腦帳戶。 您必須將 Name 屬性變更為 %2，才能使用現有的電腦帳戶。

### <a name="event-1223-res_ipaddr_invalid_network_role"></a>事件1223： RES_IPADDR_INVALID_NETWORK_ROLE

叢集 IP 位址資源 ' %1 ' 無法上線，因為叢集網路 ' %2 ' 未設定為允許用戶端存取。 請使用 [容錯移轉叢集管理員] 嵌入式管理單元來檢查叢集網路的已設定屬性。

### <a name="event-1226-res_netname_tcb_not_held"></a>事件1226： RES_NETNAME_TCB_NOT_HELD

網路名稱資源 ' %1 ' （具有相關聯的網路名稱 ' %2 '）已啟用 Kerberos 驗證支援。 無法將必要的認證新增至 LSA-相關聯的錯誤碼 ' %3 ' 指出這種作業通常不需要足夠的許可權。 必要的許可權是「信任的運算基礎」，而且必須在組成叢集的每個節點上啟用本機。

### <a name="event-1227-res_netname_lsa_error"></a>事件1227： RES_NETNAME_LSA_ERROR

網路名稱資源 ' %1 ' （具有相關聯的網路名稱 ' %2 '）已啟用 Kerberos 驗證支援。 無法將必要的認證新增至 LSA-相關聯的錯誤碼為 ' %3 '。

### <a name="event-1228-res_netname_clone_failure"></a>事件1228： RES_NETNAME_CLONE_FAILURE

叢集網路名稱資源 ' %1 ' 在此節點上啟用網路名稱時發生錯誤。 失敗的原因是： <br> ' %2 '。<br><br> 錯誤碼為 ' %3 '。 <br><br> 您可以讓網路名稱資源離線，然後再次上線以重試。

### <a name="event-1229-res_netname_no_ips_for_dns"></a>事件1229： RES_NETNAME_NO_IPS_FOR_DNS

叢集網路名稱資源 ' %1 ' 無法識別任何要向 DNS 伺服器註冊的 IP 位址。 請確定有一或多個網路已啟用叢集搭配使用 [允許用戶端透過此網路連線] 設定，而且每個節點都已針對網路設定有效的 IP 位址。

### <a name="event-1230-rcm_deadlock_or_crash_detected"></a>事件1230： RCM_DEADLOCK_OR_CRASH_DETECTED

伺服器上的元件未及時回應。 這會導致叢集資源 ' %1 ' （資源類型 ' %2 '、DLL ' %3 '）超過其超時閾值。 作為叢集健全狀況偵測的一部分，將會採取修復動作。
叢集會嘗試終止並重新啟動執行此資源的資源主控子系統（RHS）程式，以自動復原。 確認與資源相關聯的基礎結構（例如儲存體、網路或服務）是否正常運作。

### <a name="event-1230-rcm_resource_control_deadlock_detected"></a>事件1230： RCM_RESOURCE_CONTROL_DEADLOCK_DETECTED

伺服器上的元件未及時回應。 這會導致叢集資源 ' %1 ' （資源類型 ' %2 '、DLL ' %3 '）在處理控制碼 ' %4; ' 時超過其超時閾值。 作為叢集健全狀況偵測的一部分，將會採取修復動作。 叢集會嘗試終止並重新啟動執行此資源的資源主控子系統（RHS）程式，以自動復原。 確認與資源相關聯的基礎結構（例如儲存體、網路或服務）是否正常運作。

### <a name="event-1231-res_netname_logon_failure"></a>事件1231： RES_NETNAME_LOGON_FAILURE

叢集網路名稱資源 ' %1 ' 在登入網域時發生錯誤。 失敗的原因是： <br> ' %2 '。<br><br> 錯誤碼為 ' %3 '。
<br><br> 確定網域控制站可在已設定網域內的此節點上存取。

### <a name="event-1232-res_genscript_timeout"></a>事件1232： RES_GENSCRIPT_TIMEOUT

叢集一般腳本資源 ' %1 ' 中的進入點 ' %2 ' 未及時完成執行。 這可能是因為無限迴圈或其他可能造成無限等待的問題所造成。 或者，針對此資源所指定的暫止超時值可能太短。 請檢查 ' %2 ' 腳本進入點，以確保腳本中所有可能的無限等候都已更正。 然後，請考慮增加暫止的超時值（如有必要）。

### <a name="event-1233-res_genscript_hangmode"></a>事件1233： RES_GENSCRIPT_HANGMODE

叢集一般腳本資源 ' %1 '：將不會處理執行 ' %2 ' 作業的要求。 這是因為先前嘗試以及時方式執行 ' %3 ' 進入點失敗。 請檢查此進入點的腳本，以確保不會有任何無限迴圈或其他可能造成無限等待的問題。 然後，視需要考慮增加資源暫止的超時值。

### <a name="event-1234-cluster_event_account_missing_privs"></a>事件1234： CLUSTER_EVENT_ACCOUNT_MISSING_PRIVS

叢集服務偵測到其服務帳戶遺漏了一或多個必要的許可權。 遺漏許可權清單為： ' %1 '，且目前未授與服務帳戶。 使用「sc.exe qprivs clussvc」來確認叢集服務（ClusSvc）的許可權。 此外，請檢查 Active Directory Domain Services 中可能已改變預設許可權的任何安全性原則或群組原則。 輸入下列命令，授與叢集服務必要的許可權才能正常運作：

```
sc.exe privs
clussvc
SeBackupPrivilege/SeRestorePrivilege/SeIncreaseQuotaPrivilege/SeIncreaseBasePriorityPrivilege/SeTcbPrivilege/SeDebugPrivilege/SeSecurityPrivilege/SeAuditPrivilege/SeImpersonatePrivilege/SeChangeNotifyPrivilege/SeIncreaseWorkingSetPrivilege/SeManageVolumePrivilege/SeCreateSymbolicLinkPrivilege/SeLoadDriverPrivilege
```

### <a name="event-1242-res_ipaddr_lease_expired"></a>事件1242： RES_IPADDR_LEASE_EXPIRED

與叢集 IP 位址資源 ' %1 ' 相關聯的 IP 位址 ' %2 ' 租用已過期或即將到期，目前無法更新。 請確定相關聯的 DHCP 伺服器可供存取，並已正確設定，以更新此 IP 位址的租用。

### <a name="event-1245-res_ipaddr_lease_renewal_failed"></a>事件1245： RES_IPADDR_LEASE_RENEWAL_FAILED

叢集 IP 位址資源 ' %1 ' 無法更新 IP 位址 ' %2 ' 的租用。
請確定 DHCP 伺服器可供存取，並已正確設定，以更新此 IP 位址的租用。

### <a name="event-1250-rcm_resource_embedded_failure"></a>事件1250： RCM_RESOURCE_EMBEDDED_FAILURE

叢集角色 ' %2 ' 中的叢集資源 ' %1 ' 已收到重大狀態通知。 若為虛擬機器，這表示虛擬機器內的應用程式或服務處於狀況不良狀態。 確認在虛擬機器內受監視的服務或應用程式的功能。

### <a name="event-1254-rcm_group_terminal_failure"></a>事件1254： RCM_GROUP_TERMINAL_FAILURE

叢集角色 ' %1 ' 已超過其容錯移轉閾值。 它已在配置給它的容錯移轉期間內，耗盡已設定的容錯移轉次數，而且會處於失敗狀態。 不會進行任何額外的嘗試，讓該角色上線，或將它容錯回復到叢集中的另一個節點。
請檢查與失敗相關聯的事件。 解決導致失敗的問題之後，可以手動將角色上線，或叢集可能會在重新啟動延遲一段時間之後，再次嘗試讓它上線。

### <a name="event-1255-rcm_resource_network_failure"></a>事件1255： RCM_RESOURCE_NETWORK_FAILURE

叢集角色 ' %2 ' 中的叢集資源 ' %1 ' 已收到重大狀態通知。 若為虛擬機器，這表示虛擬機器的重要網路處於狀況不良狀態。 確認虛擬機器的網路連線能力，以及虛擬機器設定使用的虛擬網路。

### <a name="event-1256-res_netname_dns_registration_failed_dynamic_dns_zone"></a>事件1256： RES_NETNAME_DNS_REGISTRATION_FAILED_DYNAMIC_DNS_ZONE

叢集網路名稱資源無法註冊一或多個相關聯的 DNS 名稱，因為對應的 DNS 區域不接受動態更新。<br><br>叢集網路名稱： ' %1 '<br>DNS 區域： ' %2 '

#### <a name="guidance"></a>指引

確定 DNS 已設定為動態 DNS 區域。 如果 DNS 伺服器不接受動態更新，請取消核取網路介面卡內容中的 [在 DNS 中註冊此連線的位址]。

### <a name="event-1257-res_netname_dns_registration_failed_secure_dns_zone"></a>事件1257： RES_NETNAME_DNS_REGISTRATION_FAILED_SECURE_DNS_ZONE

叢集網路名稱資源無法註冊一或多個相關聯的 DNS 名稱，因為已拒絕更新安全 DNS 區域的存取權。<br><br>叢集網路名稱： ' %1 '<br>DNS 區域： ' %2 '<br><br>請確定叢集名稱物件（CNO）已授與安全 DNS 區域的許可權。

### <a name="event-1258-res_netname_dns_registration_failed_timeout"></a>事件1258： RES_NETNAME_DNS_REGISTRATION_FAILED_TIMEOUT

叢集網路名稱資源無法註冊一或多個相關聯的 DNS 名稱，因為無法連線到 DNS 伺服器。<br><br>叢集網路名稱： ' %1 '<br>DNS 區域： ' %2 '<br>DNS 伺服器： ' %3 '<br><br>確定與相依 IP 位址資源相關聯的網路介面卡已設定至少一個可存取的 DNS 伺服器。

### <a name="event-1259-res_netname_dns_registration_failed_cleanup"></a>事件1259： RES_NETNAME_DNS_REGISTRATION_FAILED_CLEANUP

叢集網路名稱資源無法註冊一或多個相關聯的 DNS 名稱，因為叢集服務無法清除對應至網路名稱的現有記錄。<br><br>叢集網路名稱： ' %1 '<br>DNS 區域： ' %2 '<br><br>請確定叢集名稱物件（CNO）已授與安全 DNS 區域的許可權。

### <a name="event-1260-res_netname_dns_registration_modify_failed"></a>事件1260： RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED

叢集網路名稱資源無法修改 DNS 註冊。<br><br>叢集網路名稱： ' %1 '<br>錯誤碼： ' %2 '

#### <a name="guidance"></a>指引

確定與相依 IP 位址資源相關聯的網路介面卡已設定至少有一部 DNS 伺服器的存取權。

### <a name="event-1261-res_netname_dns_registration_modify_failed_status"></a>事件1261： RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED_STATUS

叢集網路名稱資源無法修改 DNS 註冊。<br><br>叢集網路名稱： ' %1 '<br>原因： ' %2 '

#### <a name="guidance"></a>指引

確定與相依 IP 位址資源相關聯的網路介面卡已設定至少有一部 DNS 伺服器的存取權。

### <a name="event-1262-res_netname_dns_registration_publish_ptr_failed"></a>事件1262： RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED

叢集網路名稱資源無法發佈 DNS 反向對應區域中的 PTR 記錄。<br><br>叢集網路名稱： ' %1 '<br>錯誤碼： ' %2 '

#### <a name="guidance"></a>指引

請確定與相依 IP 位址資源相關聯的網路介面卡已設定至少有一部 DNS 伺服器的存取權，且 DNS 反向對應區域存在。

### <a name="event-1264-res_netname_dns_registration_publish_ptr_failed_status"></a>事件1264： RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED_STATUS

叢集網路名稱資源無法發佈 DNS 反向對應區域中的 PTR 記錄。<br><br>叢集網路名稱： ' %1 '<br>原因： ' %2 '

#### <a name="guidance"></a>指引

請確定與相依 IP 位址資源相關聯的網路介面卡已設定至少有一部 DNS 伺服器的存取權，且 DNS 反向對應區域存在。

### <a name="event-1265-res_type_control_timed_out"></a>事件1265： RES_TYPE_CONTROL_TIMED_OUT

在處理控制碼 %2 時，叢集資源類型 ' %1 ' 超時。 叢集會嘗試藉由終止並重新啟動處理呼叫的資源主控子系統（RHS）程式，來自動復原。

### <a name="event-1289-netft_adapter_not_found"></a>事件1289： NETFT_ADAPTER_NOT_FOUND

叢集服務無法存取網路介面卡 ' %1 '。 確認其他網路介面卡是否正常運作，並檢查裝置管理員是否有與介面卡 ' %1 ' 關聯的錯誤。 如果介面卡 ' %1 ' 的設定已變更，可能會需要在這部電腦上重新安裝容錯移轉叢集功能。

### <a name="event-1360-res_ipaddr_invalid_network"></a>事件1360： RES_IPADDR_INVALID_NETWORK

叢集 IP 位址資源 ' %1 ' 無法上線。 請確認網路屬性 ' %2 ' 與叢集網路名稱相符，或位址屬性 ' %3 ' 符合叢集網路上的其中一個子網。 如果這是 IPv6 網址類別型，請確認符合此資源的叢集網路至少有一個不是連結-本機或通道的 IPv6 首碼。

### <a name="event-1361-res_ipaddr_missing_dependant"></a>事件1361： RES_IPADDR_MISSING_DEPENDANT

IPv6 通道位址資源 ' %1 ' 無法上線，因為它不依存于 IP 位址（IPv4）資源。 至少需要一個 IP 位址（IPv4）資源的相依性。

### <a name="event-1362-res_ipaddr_missing_data"></a>事件1362： RES_IPADDR_MISSING_DATA

叢集 IP 位址資源 ' %1 ' 無法上線，因為無法讀取 ' %2 ' 屬性。 請確定已正確設定資源。

### <a name="event-1363-res_ipaddr_no_isatap_support"></a>事件1363： RES_IPADDR_NO_ISATAP_SUPPORT

IPv6 通道位址資源 ' %1 ' 無法上線。 與相依 IP 位址（IPv4）資源 ' %3 ' 相關聯的叢集網路 ' %2 ' 不支援 ISATAP 通道。 請確定叢集網路支援 ISATAP 通道。

### <a name="event-1540-service_backup_noquorum"></a>事件1540： SERVICE_BACKUP_NOQUORUM

叢集設定資料的備份操作已中止，因為尚未達到叢集的仲裁。 請在叢集達到仲裁之後重試此備份作業。

### <a name="event-1554-service_restore_invaliduser"></a>事件1554： SERVICE_RESTORE_INVALIDUSER

叢集設定資料的還原操作失敗。 這是因為與執行還原的使用者帳戶相關聯的許可權不足。 請確定使用者帳戶具有本機系統管理員許可權。

### <a name="event-1557-service_witness_attach_failed"></a>事件1557： SERVICE_WITNESS_ATTACH_FAILED

叢集服務無法更新見證資源上的叢集設定資料。 請確定見證資源已上線且可供存取。

### <a name="event-1559-res_witness_new_node_conflict"></a>事件1559： RES_WITNESS_NEW_NODE_CONFLICT

與檔案共用見證資源相關聯的檔案共用 ' %1 ' 目前由伺服器 ' %2 ' 主控。 此伺服器 ' %2 ' 剛新增為容錯移轉叢集中的新節點。 不建議將檔案共用見證由組成相同叢集的任何節點裝載。 請選擇不是由相同叢集中的任何節點所裝載的檔案共用見證，並據以修改檔案共用見證資源的設定。

### <a name="event-1560-res_smb_share_conflict"></a>事件1560： RES_SMB_SHARE_CONFLICT

叢集檔案共用資源 ' %1 ' 偵測到共用資料夾衝突。 因此，其中有些共用資料夾可能無法存取。 若要修正這種情況，請確定多個共用資料夾沒有相同的共用名稱。

### <a name="event-1563-res_fsw_onlinefailure"></a>事件1563： RES_FSW_ONLINEFAILURE

檔案共用見證資源 ' %1 ' 無法上線。 請確認檔案共用 ' %2 ' 存在，而且可由叢集存取。

### <a name="event-1566-res_netname_timedout"></a>事件1566： RES_NETNAME_TIMEDOUT

叢集網路名稱資源 ' %1 ' 無法上線。 網路名稱資源已由資源主機子系統終止，因為它未在可接受的時間內完成作業。 作業在執行時超時：<br> ' %2 '

### <a name="event-1567-service_failed_to_change_log_size"></a>事件1567： SERVICE_FAILED_TO_CHANGE_LOG_SIZE

叢集服務無法變更追蹤記錄檔大小。 請使用「取得叢集 \| 格式-清單」 PowerShell Cmdlet 來驗證 ClusterLogSize 設定 \* 。 此外，使用 [效能監視器] 嵌入式管理單元來確認故障叢集的事件追蹤會話設定。

### <a name="event-1567-res_vipaddr_address_interface_failed"></a>事件1567： RES_VIPADDR_ADDRESS_INTERFACE_FAILED

IP 介面 ' %1 ' （位址 ' %2 '）的健康情況檢查失敗（狀態為 ' %3 '）。 檢查與實體或虛擬網路介面卡相關的硬體或軟體錯誤。

### <a name="event-1568-res_cloud_witness_cant_communicate_to_azure"></a>事件1568： RES_CLOUD_WITNESS_CANT_COMMUNICATE_TO_AZURE

雲端見證資源無法連接 Microsoft Azure 儲存體服務。<br><br>叢集資源： %1 <br>叢集節點： %2

#### <a name="guidance"></a>指引

這可能是因為叢集節點與封鎖的 Microsoft Azure 服務之間的網路通訊。 確認節點的網際網路連線到 Microsoft Azure。 連接到 Microsoft Azure 入口網站，並確認儲存體帳戶存在。

### <a name="event-1569-service_using_restricted_network"></a>事件1569： SERVICE_USING_RESTRICTED_NETWORK

已針對容錯移轉叢集使用停用的網路 ' %1 '，發現節點 ' %2 ' 可用來與叢集中的其他節點通訊的目前唯一可能的網路。 這可能會影響節點加入叢集的能力。 請確認節點 ' %2 ' 的網路連線，並啟用至少一個網路來進行叢集通訊。 執行 [驗證設定向導] 來檢查您的網路設定。

### <a name="event-1569-res_cloud_witness_token_expired"></a>事件1569： RES_CLOUD_WITNESS_TOKEN_EXPIRED

雲端見證資源無法向 Microsoft Azure 儲存體服務進行驗證。 嘗試聯繫 Microsoft Azure 儲存體帳戶時，傳回「拒絕存取」錯誤。 <br><br>叢集資源： %1

#### <a name="guidance"></a>指引

儲存體帳戶的存取金鑰可能不再有效。 使用容錯移轉叢集管理員或 Set-clusterquorum Windows PowerShell Cmdlet 中的 [設定叢集仲裁]，以更新的儲存體帳戶存取金鑰來設定雲端見證資源。

### <a name="event-1573-service_form_witness_failed"></a>事件1573： SERVICE_FORM_WITNESS_FAILED

節點 ' %1 ' 無法形成叢集。 這是因為無法存取見證。 請確定見證資源已上線且可供使用。

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>事件1580： RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

叢集網路名稱資源 ' %1 ' 無法在安全的 DNS 區域中，透過介面卡 ' %4 ' 註冊名稱 ' %2 '。 這是因為具有此名稱的現有記錄，而且叢集識別沒有足夠的許可權可以更新該記錄。 錯誤碼為 ' %3 '。 請洽詢您的 DNS 伺服器管理員，確認叢集身分識別具有 DNS 記錄 ' %2 ' 的許可權。

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>事件1580： RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

叢集網路名稱資源 ' %1 ' 無法在安全的 DNS 區域中，透過介面卡 ' %4 ' 註冊名稱 ' %2 '。 這是因為具有此名稱的現有記錄，而且叢集識別沒有足夠的許可權可以更新該記錄。 錯誤碼為 ' %3 '。 請洽詢您的 DNS 伺服器管理員，確認叢集身分識別具有 DNS 記錄 ' %2 ' 的許可權。

### <a name="event-1585-res_fileserver_fscheck_srvsvc_stopped"></a>事件1585： RES_FILESERVER_FSCHECK_SRVSVC_STOPPED

叢集檔案伺服器資源 ' %1 ' 失敗健康情況檢查。 這是因為伺服器服務未啟動。 請使用伺服器管理員來確認此叢集節點上的伺服器服務狀態。

### <a name="event-1586-res_fileserver_fscheck_scoped_name_not_registered"></a>事件1586： RES_FILESERVER_FSCHECK_SCOPED_NAME_NOT_REGISTERED

叢集檔案伺服器資源 ' %1 ' 失敗健康情況檢查。 這是因為它的部分共用資料夾無法存取。 確認可從用戶端存取資料夾。 此外，請使用伺服器管理員確認此叢集節點上伺服器服務的狀態，並尋找與此叢集節點上的伺服器服務相關的其他事件。 可能需要重新開機此叢集角色中的網路名稱資源 ' %2 '。

### <a name="event-1587-res_fileserver_fscheck_failed"></a>事件1587： RES_FILESERVER_FSCHECK_FAILED

叢集檔案伺服器資源 ' %1 ' 失敗健康情況檢查。 這是因為它的部分共用資料夾無法存取。 確認可從用戶端存取資料夾。 此外，請使用伺服器管理員確認此叢集節點上伺服器服務的狀態，並尋找與此叢集節點上的伺服器服務相關的其他事件。

### <a name="event-1588-res_fileserver_share_cant_add"></a>事件1588： RES_FILESERVER_SHARE_CANT_ADD

叢集檔案伺服器資源 ' %1 ' 無法上線。 資源無法建立與網路名稱 ' %3 ' 相關聯的檔案共用 ' %2 '。 錯誤碼為 ' %4 '。 確認資料夾存在且可供存取。 此外，請使用伺服器管理員確認此叢集節點上伺服器服務的狀態，並尋找此叢集節點上的其他相關事件。 可能需要重新開機此叢集角色中的網路名稱資源 ' %3 '。

### <a name="event-1600-clusapi_create_cannot_set_ad_dacl"></a>事件1600： CLUSAPI_CREATE_CANNOT_SET_AD_DACL

叢集服務無法在叢集電腦物件 ' %1 ' 上設定許可權。 請洽詢您的網路系統管理員，以檢查 Active Directory 中電腦物件的叢集安全描述項，確認 DACL 不太大，並在必要時移除物件上任何不必要的額外 ACE。

### <a name="event-1603-res_fileserver_clone_failed"></a>事件1603： RES_FILESERVER_CLONE_FAILED

檔案伺服器無法啟動，因為找不到「網路名稱」資源的預期相依性，或其設定不正確。 錯誤 = 0x %2。

### <a name="event-1606-res_disk_cno_check_failed"></a>事件1606： RES_DISK_CNO_CHECK_FAILED

叢集磁片資源 ' %1 ' 包含受 BitLocker 保護的磁片區 ' %2 '，但針對此磁片區，Active Directory 叢集名稱帳戶（也稱為叢集名稱物件或 CNO）不是磁片區的 BitLocker 保護裝置。 這是受 BitLocker 保護之磁片區的必要參數。 若要修正此錯誤，請先從叢集移除該磁片。 接下來，使用 Manage-bde.exe 命令列工具，將叢集名稱新增為 ADAccountOrGroup 保護裝置，並針對叢集名稱使用 domain \\ ClusterName 格式 \$ 。 然後將磁片新增回叢集。 如需詳細資訊，請參閱 Manage-bde.exe 的檔

### <a name="event-1607-res_disk_cno_unlock_failed"></a>事件1607： RES_DISK_CNO_UNLOCK_FAILED

叢集磁片資源 ' %1 ' 無法解除鎖定受 BitLocker 保護的磁片區 ' %2 '。 叢集名稱物件（CNO）未設定為此磁片區的有效 BitLocker 保護裝置。 若要修正此錯誤，請將磁片從叢集移除。 然後，使用 Manage-bde.exe 命令列工具，將叢集名稱新增為 ADAccountOrGroup 保護裝置，並使用格式 domain \\ ClusterName \$ ，然後將該磁片新增回叢集。 如需詳細資訊，請參閱 Manage-bde.exe 的檔。

### <a name="event-1608-res_fileserver_leader_failed"></a>事件1608： RES_FILESERVER_LEADER_FAILED

檔案伺服器無法啟動，因為找不到「網路名稱」資源的預期相依性，或其設定不正確。 錯誤 = 0x %2。

### <a name="event-1609-res_soda_fileserver_leader_failed"></a>事件1609： RES_SODA_FILESERVER_LEADER_FAILED

Scale Out 檔案伺服器無法啟動，因為找不到「分散式網路名稱」資源的預期相依性，或其設定不正確。 錯誤 = 0x %2。

### <a name="event-1632-clusapi_create_mismatched_ou"></a>事件1632： CLUSAPI_CREATE_MISMATCHED_OU

建立叢集失敗。 無法在 active directory 組織單位 ' %2 ' 中建立叢集名稱物件 ' %1 '。 物件已存在於組織單位 ' %3 ' 中。 請確認指定的辨別名稱路徑和叢集名稱物件是否正確。 如果未指定辨別名稱路徑，則會使用現有的電腦物件 ' %1 '。

### <a name="event-1652-service_tcp_connection_failure"></a>事件1652： SERVICE_TCP_CONNECTION_FAILURE

叢集節點 ' %1 ' 無法加入叢集。 無法為節點 ' %2 ' 建立 TCP 連線。 確認網路連線和網路防火牆的設定。

### <a name="event-1652-service_udp_connection_failure"></a>事件1652： SERVICE_UDP_CONNECTION_FAILURE

叢集節點 ' %1 ' 無法加入叢集。 無法建立節點 ' %2 ' 的 UDP 連接。 確認網路連線和網路防火牆的設定。

### <a name="event-1652-service_virtual_tcp_connection_failure"></a>事件1652： SERVICE_VIRTUAL_TCP_CONNECTION_FAILURE

叢集節點 ' %1 ' 無法加入叢集。 無法為節點 ' %2 ' 建立使用 Microsoft 容錯移轉叢集虛擬介面卡的 TCP 連線。 確認網路連線和網路防火牆的設定。

### <a name="event-1653-service_no_connectivity"></a>事件1653： SERVICE_NO_CONNECTIVITY

叢集節點 ' %1 ' 無法加入叢集，因為它無法透過網路與叢集中的任何其他節點通訊。 確認網路連線和網路防火牆的設定。

### <a name="event-1654-res_vipaddr_invalid_adaptername"></a>事件1654： RES_VIPADDR_INVALID_ADAPTERNAME

叢集脫離的 IP 位址資源 ' %1 ' 無法上線。 無法判斷對應至網路介面卡 ' %2 ' 之網路介面卡的設定資料（錯誤碼為 ' %3 '）。 檢查 IP 位址資源是否已設定正確的位址和網路屬性。

### <a name="event-1655-res_vipaddr_invalid_vsid"></a>事件1655： RES_VIPADDR_INVALID_VSID

叢集脫離的 IP 位址資源 ' %1 ' 無法上線。 無法判斷對應至虛擬子網識別碼 ' %2 ' 和路由網域識別碼 ' %3 ' 之網路介面卡的設定資料（錯誤碼為 ' %4 '）。 檢查 IP 位址資源是否已設定正確的位址和網路屬性。

### <a name="event-1656-res_vipaddr_address_create_failed"></a>事件1656： RES_VIPADDR_ADDRESS_CREATE_FAILED

無法為斷續的 IP 位址資源 ' %1 ' 新增 IP 位址 ' %2 ' （錯誤碼為 ' %3 '）。 檢查與實體或虛擬網路介面卡相關的硬體或軟體錯誤。

### <a name="event-1664-cluster_upgrade_incomplete"></a>事件1664： CLUSTER_UPGRADE_INCOMPLETE

升級叢集的功能等級失敗。 請確認叢集的所有節點目前都在執行，而且是相同版本的 Windows Server，然後再次執行 ClusterFunctionalLevel Windows PowerShell Cmdlet。

### <a name="event-1676-event_local_node_quarantined"></a>事件1676： EVENT_LOCAL_NODE_QUARANTINED

' %1 ' 已隔離本機叢集節點。 節點將會隔離到 ' %2 '，然後節點會自動嘗試重新加入叢集。
<br><br>請參閱系統和應用程式事件記錄檔，以判斷此節點上的問題。 問題解決後，您可以手動清除隔離，以允許節點與 ' Start-clusternode – ClearQuarantine ' Windows PowerShell Cmdlet 重新加入。<br><br>QuarantineType：由 %1 隔離<br>將自動清除時間隔離： %2

### <a name="event-1677-event_node_drain_failed"></a>事件1677： EVENT_NODE_DRAIN_FAILED

叢集節點 %1 上的節點清空失敗。 <br><br>參考節點的系統和應用程式事件記錄檔和叢集記錄檔，以調查清空失敗的原因。 問題解決後，您可以重試清空作業。

### <a name="event-1683-res_netname_computer_account_no_dc"></a>事件1683： RES_NETNAME_COMPUTER_ACCOUNT_NO_DC

叢集服務無法連線到網域上的任何可用網域控制站。 這可能會影響依存于叢集網路名稱驗證的功能。<br><br>DC 伺服器： %1

#### <a name="guidance"></a>指引

確認可在網路上存取叢集節點的網域控制站。

### <a name="event-1684-res_netname_computer_object_vco_not_found"></a>事件1684： RES_NETNAME_COMPUTER_OBJECT_VCO_NOT_FOUND

叢集網路名稱資源在 Active Directory 中找不到相關聯的電腦物件。 這可能會影響依存于叢集網路名稱驗證的功能。<br><br>網路名稱： %1<br>組織單位： %2

#### <a name="guidance"></a>指引

從 Active Directory 回收站還原網路名稱的電腦物件。 或者，將叢集網路名稱資源離線，並執行修復動作，以在 Active Directory 中重新建立電腦物件。

### <a name="event-1685-res_netname_computer_object_cno_not_found"></a>事件1685： RES_NETNAME_COMPUTER_OBJECT_CNO_NOT_FOUND

叢集網路名稱資源在 Active Directory 中找不到相關聯的電腦物件。 這可能會影響依存于叢集網路名稱驗證的功能。<br><br>網路名稱： %1<br>組織單位： %2
#### <a name="guidance"></a>指引

從 Active Directory 回收站還原網路名稱的電腦物件。

### <a name="event-1686-res_netname_computer_object_vco_disabled"></a>事件1686： RES_NETNAME_COMPUTER_OBJECT_VCO_DISABLED

叢集網路名稱資源在要停用的 Active Directory 中找到相關聯的電腦物件。 這可能會影響依存于叢集網路名稱驗證的功能。<br><br>網路名稱： %1<br>組織單位： %2

#### <a name="guidance"></a>指引

在 Active Directory 中啟用網路名稱的電腦物件。

### <a name="event-1687-res_netname_computer_object_cno_disabled"></a>事件1687： RES_NETNAME_COMPUTER_OBJECT_CNO_DISABLED

叢集網路名稱資源在要停用的 Active Directory 中找到相關聯的電腦物件。 這可能會影響依存于叢集網路名稱驗證的功能。<br><br>網路名稱： %1<br>組織單位： %2

#### <a name="guidance"></a>指引

在 Active Directory 中啟用網路名稱的電腦物件。 或者，將叢集網路名稱資源離線，並執行修復動作，以在 Active Directory 中啟用電腦物件。

### <a name="event-1688-res_netname_computer_object_failed"></a>事件1688： RES_NETNAME_COMPUTER_OBJECT_FAILED

叢集網路名稱資源偵測到 Active Directory 中相關聯的電腦物件已停用，並在嘗試啟用它時失敗。 這可能會影響依存于叢集網路名稱驗證的功能。<br><br>網路名稱： %1<br>組織單位： %2

#### <a name="guidance"></a>指引

在 Active Directory 中啟用網路名稱的電腦物件。

### <a name="event-4608-nodecleanup_get_clustered_disks_failed"></a>事件4608： NODECLEANUP_GET_CLUSTERED_DISKS_FAILED

當終結叢集時，叢集服務無法抓取叢集磁片清單。 錯誤碼為 ' %1 '。 如果無法存取這些磁片，請執行 ' Clear-ClusterDiskReservation ' PowerShell Cmdlet。

### <a name="event-4611-nodecleanup_release_clustered_disks_from_partmgr_failed"></a>事件4611： NODECLEANUP_RELEASE_CLUSTERED_DISKS_FROM_PARTMGR_FAILED

損毀叢集時，分割區管理員並未釋放識別碼為 ' %2 ' 的叢集磁片。 錯誤碼為 ' %1 '。 重新開機電腦將會確保磁碟分割管理員釋放。

### <a name="event-4613-nodecleanup_clear_clusdisk_database_failed"></a>事件4613： NODECLEANUP_CLEAR_CLUSDISK_DATABASE_FAILED

叢集服務在終結叢集時，無法正確清除識別碼為 ' %2 ' 的叢集磁片。 錯誤碼為 ' %1 '。 在清除作業順利完成之前，您可能無法存取此磁片。 若要手動清除，請刪除 \\ \\ Windows 登錄中 ' HKEY_LOCAL_MACHINE SYSTEM CurrentControlSet \\ Services \\ ClusDisk \\ Parameters ' 金鑰的 ' AttachedDisks ' 值。

### <a name="event-4615-nodecleanup_disable_cluster_service_failed"></a>事件4615： NODECLEANUP_DISABLE_CLUSTER_SERVICE_FAILED

叢集服務已停止，並在叢集節點清除過程中設定為停用。

### <a name="event-4616-nodecleanup_disable_cluster_service_timeout"></a>事件4616： NODECLEANUP_DISABLE_CLUSTER_SERVICE_TIMEOUT

在叢集節點清除期間終止叢集服務時，在預期的時間週期內未完成。 請重新開機這部電腦，以確定叢集服務已不再執行。

### <a name="event-4618-nodecleanup_reset_cluster_registry_entries_failed"></a>事件4618： NODECLEANUP_RESET_CLUSTER_REGISTRY_ENTRIES_FAILED

在叢集節點清理期間重設叢集服務登錄專案失敗。
錯誤碼為 ' %1 '。 在順利完成清除之前，您可能無法建立或加入此電腦的叢集。 若要手動清除，請在這部電腦上執行 ' Clear-Start-clusternode ' PowerShell Cmdlet。

### <a name="event-4620-nodecleanup_unload_cluster_hive_failed"></a>事件4620： NODECLEANUP_UNLOAD_CLUSTER_HIVE_FAILED

在叢集節點清理期間卸載叢集服務登錄區失敗。
錯誤碼為 ' %1 '。 在順利完成清除之前，您可能無法建立或加入此電腦的叢集。 若要手動清除，請在這部電腦上執行 ' Clear-Start-clusternode ' PowerShell Cmdlet。

### <a name="event-4622-nodecleanup_errors"></a>事件4622： NODECLEANUP_ERRORS

叢集服務在節點清除期間發生錯誤。 在順利完成清除之前，您可能無法建立或加入此電腦的叢集。 在此節點上使用 ' Clear-Start-clusternode ' PowerShell Cmdlet。

### <a name="event-4624-nodecleanup_reset_nlbsflags_failed"></a>事件4624： NODECLEANUP_RESET_NLBSFLAGS_FAILED

在叢集節點清除期間，重設 IPSec 安全性關聯超時登錄值失敗。 錯誤碼為 ' %1 '。 若要手動清除，請在這部電腦上執行 ' Clear-Start-clusternode ' PowerShell Cmdlet。 或者，您可以從 Windows 登錄中的 HKEY_LOCAL_MACHINE 刪除 ' %2 ' 值和 ' %3 ' 值，以重設 IPSec 安全性關聯超時。

### <a name="event-4627-nodecleanup_delete_cluster_tasks_failed"></a>事件4627： NODECLEANUP_DELETE_CLUSTER_TASKS_FAILED

在節點清除期間刪除叢集工作失敗。 錯誤碼為 ' %1 '。
使用 Windows 工作排程器刪除任何剩餘的叢集工作。

### <a name="event-4629-nodecleanup_delete_local_account_failed"></a>事件4629： NODECLEANUP_DELETE_LOCAL_ACCOUNT_FAILED

在節點清除期間，不會刪除由叢集管理的本機使用者帳戶。 錯誤碼為 ' %1 '。 開啟 [本機使用者和群組] （lusrmgr.msc）以刪除帳戶。

### <a name="event-4864-res_vsstask_open_failed"></a>事件4864： RES_VSSTASK_OPEN_FAILED

磁片區陰影複製服務工作資源 ' %1 ' 建立失敗。 錯誤碼為 ' %2 '。

### <a name="event-4865-res_vsstask_terminate_task_failed"></a>事件4865： RES_VSSTASK_TERMINATE_TASK_FAILED

磁片區陰影複製服務工作資源 ' %1 ' 失敗。 錯誤碼為 ' %2 '。
這是因為在離線作業中無法停止相關聯的工作。 您可能需要使用工作排程器嵌入式管理單元，以手動方式停止它。

### <a name="event-4866-res_vsstask_delete_task_failed"></a>事件4866： RES_VSSTASK_DELETE_TASK_FAILED

磁片區陰影複製服務工作資源 ' %1 ' 失敗。 錯誤碼為 ' %2 '。
這是因為無法在離線作業中刪除相關聯的工作。 您可能需要使用工作排程器嵌入式管理單元，手動將其刪除。

### <a name="event-4867-res_vsstask_online_failed"></a>事件4867： RES_VSSTASK_ONLINE_FAILED

磁片區陰影複製服務工作資源 ' %1 ' 失敗。 錯誤碼為 ' %2 '。
這是因為無法在線上作業中加入相關聯的工作。 請使用工作排程器嵌入式管理單元，以確保您的工作已正確設定。

### <a name="event-4868-unable_to_start_autologger"></a>事件4868： UNABLE_TO_START_AUTOLOGGER

叢集服務無法啟動叢集記錄追蹤會話。 錯誤碼為 ' %2 '。 叢集將會正常運作，但是補充記錄資訊將會遺失。 使用 [效能監視器] 嵌入式管理單元來驗證 ' %1 ' 的事件通道設定。

### <a name="event-4869-netft_watchdog_process_hung"></a>事件4869： NETFT_WATCHDOG_PROCESS_HUNG

使用者模式健全狀況監視偵測到系統沒有回應。 容錯移轉叢集虛擬配接器與 ' %1 ' 進程（處理序識別碼為 ' %2 '）的連絡人遺失了 ' %3 ' 秒。 請使用效能監視器來評估系統的健全狀況，並判斷哪個進程可能對系統造成負面影響。

### <a name="event-4870-netft_watchdog_process_terminated"></a>事件4870： NETFT_WATCHDOG_PROCESS_TERMINATED

使用者模式健全狀況監視偵測到系統沒有回應。 容錯移轉叢集虛擬配接器與 ' %1 ' 進程（處理序識別碼為 ' %2 '）的連絡人遺失了 ' %3 ' 秒。 將採取修復動作。

### <a name="event-4871-netft_miniport_initialization_failure"></a>事件4871： NETFT_MINIPORT_INITIALIZATION_FAILURE

叢集服務無法啟動。 這是因為容錯移轉叢集虛擬配接器無法初始化微型功能介面卡。 錯誤碼為 ' %1 '。 確認其他網路介面卡正常運作，並檢查裝置管理員是否有錯誤。 如果設定已變更，可能需要在這部電腦上重新安裝容錯移轉叢集功能。

### <a name="event-4872-netft_missing_datalink_address"></a>事件4872： NETFT_MISSING_DATALINK_ADDRESS

容錯移轉叢集虛擬介面卡無法產生唯一的 MAC 位址。
可能是因為找不到要從中產生唯一位址的實體 Ethernet 介面卡，或產生的位址與這部電腦上的其他介面卡衝突。 請執行 [驗證設定向導] 來檢查您的網路設定。

### <a name="event-5122-dcm_event_root_creation_failed"></a>事件5122： DCM_EVENT_ROOT_CREATION_FAILED

叢集服務無法建立叢集共用磁片區根目錄 ' %2 '。
錯誤訊息為 ' %1 '。

### <a name="event-5142-dcm_volume_no_access"></a>事件5142： DCM_VOLUME_NO_ACCESS

因為發生錯誤 ' %3 '，所以無法再從這個叢集節點存取叢集共用磁碟區 ' %1 ' （' %2 '）。 請針對此節點對存放裝置和網路連線能力的連線進行疑難排解。

### <a name="event-5143-dcm_veto_resource_move_due_to_cc"></a>事件5143： DCM_VETO_RESOURCE_MOVE_DUE_TO_CC

根據節點 ' %1 ' 上快取管理員的目前狀態，無法移動磁片（' %2 '），以避免潛在的鎖死。 「快取管理員中途分頁閾值 ' 是 %3，而「快取管理員中途分頁」是 %4。 如果「快取管理員中途分頁」小於70% 的「快取管理員中途分頁閾值」，或如果「快取管理員中途分頁閾值」減去「快取管理員中途分頁」大於128000頁（關於頁面大小為4096個位元組的500MB），則允許移動。
叢集拒絕資源移動以防止因快取管理員節流緩衝寫入而造成可能的鎖死，而此磁片上的叢集共用磁片區正在暫停。

### <a name="event-5144-dcm_snapshot_diff_area_failure"></a>事件5144： DCM_SNAPSHOT_DIFF_AREA_FAILURE

將磁片（' %1 '）新增至叢集共用磁片區時，為磁片區（' %2 '）設定明確的快照差異區域關聯失敗，錯誤為 ' %3 '。 叢集共用磁片區唯一支援的軟體快照差異區域關聯為自我。

### <a name="event-5145-dcm_snapshot_diff_area_delete_failure"></a>事件5145： DCM_SNAPSHOT_DIFF_AREA_DELETE_FAILURE

叢集磁片資源 ' %1 ' 無法刪除軟體快照集。 磁片區 ' %3 ' 上的 diff 區域無法與磁片區 ' %2 ' 中斷關聯。 這可能是因為使用中的快照集所造成。 叢集共用磁片區要求軟體快照集必須位於相同的磁片上。

### <a name="event-5146-dcm_veto_resource_move_due_to_dismount"></a>事件5146： DCM_VETO_RESOURCE_MOVE_DUE_TO_DISMOUNT

叢集共用磁碟區資源 ' %1 ' 的移動被否決，因為其中一個屬於該資源的磁片區處於已卸載狀態。 請在卸載操作完成後重試此動作。

### <a name="event-5147-dcm_veto_resource_move_due_to_snapshot"></a>事件5147： DCM_VETO_RESOURCE_MOVE_DUE_TO_SNAPSHOT

叢集共用磁碟區資源 ' %1 ' 的移動被否決，因為其中一個屬於該資源的磁片區處於已卸載狀態。 請在卸載操作完成後重試此動作。

### <a name="event-5148-dcm_veto_resource_move_due_to_io_mode_change"></a>事件5148： DCM_VETO_RESOURCE_MOVE_DUE_TO_IO_MODE_CHANGE

叢集共用磁碟區資源 ' %1 ' 的移動被否決，因為 IO 模式變更作業（直接 IO 到重新導向的 IO，反之亦然）正在其中一個磁片區上的資源中。 請在作業完成後重試此動作。

### <a name="event-5150-dcm_set_resource_in_failed_state"></a>事件5150： DCM_SET_RESOURCE_IN_FAILED_STATE

叢集實體磁片資源 ' %1 ' 失敗。 叢集共用磁碟區已進入失敗狀態，錯誤如下： ' %2 '

### <a name="event-5200-cam_cannot_create_cno_token"></a>事件5200： CAM_CANNOT_CREATE_CNO_TOKEN

叢集服務無法建立叢集共用磁片區的叢集識別權杖。 錯誤碼為 ' %1 '。 請確定網域控制站可供存取，並檢查是否有連線問題。 在恢復網域控制站的連線之前，對叢集共用磁片區的此節點上的某些作業可能會失敗。

### <a name="event-5216-csv_sw_snapshot_failed"></a>事件5216： CSV_SW_SNAPSHOT_FAILED

在叢集共用磁碟區 ' %1 ' （' %2 '）上建立軟體快照集失敗，錯誤為 %3。 資源必須在線上，才能支援建立快照集。 請檢查資源的狀態。

### <a name="event-5217-csv_sw_snapshot_set_failed"></a>事件5217： CSV_SW_SNAPSHOT_SET_FAILED

建立快照集識別碼為 ' %2 ' 的叢集共用磁碟區（' %1 '）上的軟體快照集失敗，發生錯誤 ' %3 '。 請檢查 CSV 資源的狀態和資源擁有者節點的系統事件。

### <a name="event-5219-csv_register_snapshot_prov_with_vss_failed"></a>事件5219： CSV_REGISTER_SNAPSHOT_PROV_WITH_VSS_FAILED

叢集服務無法使用磁片區陰影服務（VSS）註冊叢集共用磁片區快照集提供者。 這可能是因為 VSS 服務關閉或可能是因為 VSS 服務發生問題而導致它無法接受連入要求所致。 <br>錯誤: %1

### <a name="event-5377-operation_exceeded_timeout"></a>事件5377： OPERATION_EXCEEDED_TIMEOUT

內部叢集服務作業已超過定義的臨界值 ' %2 ' 秒。 已終止叢集服務以進行復原。 服務控制管理員將會重新開機叢集服務，而節點會重新加入叢集。

### <a name="event-5396-two_partitions_have_quorum"></a>事件5396： TWO_PARTITIONS_HAVE_QUORUM

此節點上的叢集服務已關閉，因為它偵測到有其他具有仲裁的叢集節點。 當叢集服務偵測到另一個使用強制仲裁交換器（/fq）啟動的節點時，就會發生這種情況。 以強制仲裁交換器啟動的節點將會繼續執行。 當叢集服務重新開機時，請使用容錯移轉叢集管理員來確認此節點已自動加入叢集。

### <a name="event-5397-rlua_account_failed"></a>事件5397： RLUA_ACCOUNT_FAILED

叢集資源 ' %1 ' 無法在此節點上建立或修改已複寫的本機使用者帳戶 ' %2 '。 請檢查叢集記錄檔以取得詳細資訊。

### <a name="event-5398-nm_event_cluster_failed_to_form"></a>事件5398： NM_EVENT_CLUSTER_FAILED_TO_FORM

叢集無法啟動。 嘗試啟動叢集的節點集內，無法使用最新的叢集設定資料複本。 當節點集不在成員資格中，而導致無法接收設定資料更新時，就會發生叢集變更。 .<br><br>啟動叢集所需的投票： %1<br>可用的投票： %2<br>具有投票的節點： %3

#### <a name="guidance"></a>指引

嘗試在叢集中的所有節點上啟動叢集服務，讓具有最新叢集設定資料複本的節點可以先形成叢集。 叢集將能夠啟動，而且節點會自動取得更新的叢集設定資料。 如果最新的叢集設定資料複本沒有可用的節點，請執行 ' Start-clusternode-FQ ' Windows PowerShell Cmdlet。 使用 ForceQuorum （FQ）參數會啟動叢集服務，並將此節點的叢集設定資料複本標示為已授權。 在具有過期叢集資料庫複本的節點上強制仲裁，可能會導致叢集設定變更，因為節點未參與叢集而無法中斷。

## <a name="warning-events"></a>警告事件

### <a name="event-1011-nm_node_evicted"></a>事件1011： NM_NODE_EVICTED

已從容錯移轉叢集中收回叢集節點 %1。

### <a name="event-1045-res_ipaddr_ipv4_address_create_failed"></a>事件1045： RES_IPADDR_IPV4_ADDRESS_CREATE_FAILED

找不到資源 ' %1 ' IP 位址 ' %2 ' （傳回碼為 ' %3 '）的相符網路介面。 如果您的叢集節點跨越不同的子網，這可能是正常的。

### <a name="event-1066-res_disk_corrupt_disk"></a>事件1066： RES_DISK_CORRUPT_DISK

叢集磁片資源 ' %1 ' 指出磁片區 ' %2 ' 的損毀。 正在執行 Chkdsk 來修復問題。 在 Chkdsk 完成之前，磁片將無法使用。
Chkdsk 輸出將會記錄到檔案 ' %3 '。<br> Chkdsk 也可以將資訊寫入應用程式事件記錄檔。

### <a name="event-1068-res_smb_share_cant_add"></a>事件1068： RES_SMB_SHARE_CANT_ADD

叢集檔案共用資源 ' %1 ' 無法上線。 建立檔案共用 ' %2 ' （範圍限定在網路名稱 %3）失敗，因為發生錯誤 ' %4 '。 將會自動重試此操作。

### <a name="event-1071-rcm_resource_online_blocked_by_locked_mode"></a>事件1071： RCM_RESOURCE_ONLINE_BLOCKED_BY_LOCKED_MODE

無法完成叢集角色 ' %2 ' 中類型 ' %3 ' 的叢集資源 ' %1 ' 上所嘗試的作業，因為資源或其中一個提供者已鎖定狀態。

### <a name="event-1071-rcm_resource_offline_blocked_by_locked_mode"></a>事件1071： RCM_RESOURCE_OFFLINE_BLOCKED_BY_LOCKED_MODE

在叢集角色 ' %2 ' 中，于類型 ' %3 ' 的叢集資源 ' %1 ' 上嘗試的作業無法完成，因為資源或其中一個相依性已鎖定狀態。

### <a name="event-1094-sm_invalid_security_level"></a>事件1094： SM_INVALID_SECURITY_LEVEL

叢集通用屬性 SecurityLevel 無法在此叢集上變更。 叢集安全性層級無法變更，因為叢集目前設定為沒有驗證模式。

### <a name="event-1119-res_netname_dns_registration_missing"></a>事件1119： RES_NETNAME_DNS_REGISTRATION_MISSING

因為下列原因，叢集網路名稱資源 ' %1 ' 無法在介面卡 ' %4 ' 上註冊 DNS 名稱 ' %2 '： <br><br>' %3 '

### <a name="event-1125-tm_event_cluster_netinterface_unreachable"></a>事件1125： TM_EVENT_CLUSTER_NETINTERFACE_UNREACHABLE

至少另一個連接到網路的其他叢集節點無法連線到網路 ' %3 ' 上叢集節點 ' %2 ' 的叢集網路介面 ' %1 '。 容錯移轉叢集無法判斷失敗的位置。 執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1149-res_netname_cant_delete_dns_records"></a>事件1149： RES_NETNAME_CANT_DELETE_DNS_RECORDS

與叢集資源 ' %1 ' 相關聯的 DNS 主機（A）和指標（PTR）記錄並未從資源的相關聯 DNS 伺服器移除。 如有必要，可以手動刪除它們。 請洽詢您的 DNS 系統管理員，協助您完成這方面的工作。

### <a name="event-1150-res_netname_dns_ptr_record_delete_failed"></a>事件1150： RES_NETNAME_DNS_PTR_RECORD_DELETE_FAILED

移除與叢集網路名稱資源 ' %1 ' 相關聯之主機 ' %3 ' 的 DNS 指標（PTR）記錄 ' %2 ' 失敗，錯誤為 ' %4 '。
如有必要，可以手動刪除記錄。 請洽詢您的 DNS 系統管理員以取得協助。

### <a name="event-1151-res_netname_dns_a_record_delete_failed"></a>事件1151： RES_NETNAME_DNS_A_RECORD_DELETE_FAILED

移除與叢集網路名稱資源 ' %1 ' 相關聯的 DNS 主機（A）記錄 ' %2 ' 失敗，錯誤為 ' %3 '。 如有必要，可以手動刪除記錄。 請洽詢您的 DNS 系統管理員以取得協助。

### <a name="event-1155-rcm_event_exited_queuing"></a>事件1155： RCM_EVENT_EXITED_QUEUING

角色 ' %1 ' 的暫止移動未完成。

### <a name="event-1197-res_netname_delete_disable_failed"></a>事件1197： RES_NETNAME_DELETE_DISABLE_FAILED

在資源刪除期間，叢集網路名稱資源 ' %1 ' 無法刪除或停用其相關聯的電腦物件 ' %2 '。 錯誤碼為 ' %3 '。 <br>請檢查網站是否為唯讀。 請確定叢集名稱物件具有 Active Directory 中 ' %2 ' 物件的適當許可權。

### <a name="event-1198-res_netname_delete_vco_guid_failed"></a>事件1198： RES_NETNAME_DELETE_VCO_GUID_FAILED

叢集網路名稱資源 ' %1 ' 無法刪除 guid 為 ' %2 ' 的電腦物件。 錯誤碼為 ' %3 '。 <br>請檢查網站是否為唯讀。 請確定叢集名稱物件具有 Active Directory 中 ' %2 ' 物件的適當許可權。

### <a name="event-1216-service_netname_change_warning"></a>事件1216： SERVICE_NETNAME_CHANGE_WARNING

叢集核心網路資源的名稱變更作業已失敗。
嘗試將名稱變更作業還原回原始名稱也失敗。 錯誤碼為 ' %1 '。 在此情況下，您可能無法使用叢集名稱從遠端系統管理叢集，除非已手動更正這種情況。

### <a name="event-1221-res_netname_rename_out_of_synch_with_compobj"></a>事件1221： RES_NETNAME_RENAME_OUT_OF_SYNCH_WITH_COMPOBJ

叢集網路名稱資源 ' %1 ' 的名稱 ' %2 ' 不符合對應的電腦物件名稱 ' %3 '。 電腦物件先前的名稱變更可能尚未複寫到網域中的所有網域控制站。 在名稱變成一致之前，您將無法重新命名網路名稱資源。 如果電腦物件最近尚未變更，請洽詢您的網域系統管理員，將電腦物件重新命名，使其保持一致。 此外，請確定已成功完成跨網域控制站的複寫。

### <a name="event-1222-res_netname_set_permissions_failed"></a>事件1222： RES_NETNAME_SET_PERMISSIONS_FAILED

無法更新與叢集網路名稱資源 ' %1 ' 相關聯的電腦物件。<br><br>相關聯錯誤碼的文字為： %2<br> <br>叢集識別 ' %3 ' 可能缺少更新物件所需的許可權。 請與您的網域系統管理員合作，以確保叢集身分識別可以更新網域中的電腦物件。

### <a name="event-1240-res_ipaddr_obtain_lease_failed"></a>事件1240： RES_IPADDR_OBTAIN_LEASE_FAILED

叢集 IP 位址資源 ' %1 ' 無法取得租用的 IP 位址。

### <a name="event-1243-res_ipaddr_release_lease_failed"></a>事件1243： RES_IPADDR_RELEASE_LEASE_FAILED

叢集 IP 位址資源 ' %1 ' 無法釋放 IP 位址 ' %2 ' 的租用。

### <a name="event-1251-rcm_group_preempted"></a>事件1251： RCM_GROUP_PREEMPTED

叢集角色 ' %2 ' 已離線。 此角色已被較高優先權的叢集角色 ' %1 ' 佔用。 當系統資源可用時，叢集服務會嘗試讓叢集角色 ' %2 ' 上線。

### <a name="event-1544-service_vss_onabort"></a>事件1544： SERVICE_VSS_ONABORT

叢集設定資料的備份操作已取消。 叢集磁碟區陰影複製服務（VSS）寫入器已收到中止要求。

### <a name="event-1548-service_connect_version_compatible"></a>事件1548： SERVICE_CONNECT_VERSION_COMPATIBLE

節點 ' %1 ' 已建立與節點 ' %2 ' 的通訊，並偵測到它正在執行不同但相容的作業系統版本。 我們建議所有節點都執行相同版本的作業系統。 在所有節點都已升級之後，請執行 ClusterFunctionalLevel Windows PowerShell Cmdlet 來完成叢集的升級。

### <a name="event-1550-service_connect_novercheck"></a>事件1550： SERVICE_CONNECT_NOVERCHECK

節點 ' %1 ' 已建立與節點 ' %2 ' 的通訊會話，但未執行版本相容性檢查，因為版本相容性檢查已停用。 不支援停用版本相容性檢查。

### <a name="event-1551-service_form_novercheck"></a>事件1551： SERVICE_FORM_NOVERCHECK

節點 ' %1 ' 形成容錯移轉叢集，但未執行版本相容性檢查，因為版本相容性檢查已停用。 不支援停用版本相容性檢查。

### <a name="event-1555-service_netft_disable_autoconfig_failed"></a>事件1555： SERVICE_NETFT_DISABLE_AUTOCONFIG_FAILED

嘗試使用 IPv4 進行 ' %1 ' 網路介面卡失敗。 這是因為無法停用 IPv4 自動設定和 DHCP。 如果 DHCP 用戶端服務已停用，可能就會發生這種情況。 如果啟用，則會使用 IPv6，否則此節點可能無法參與容錯移轉叢集。

### <a name="event-1558-service_witness_failover_attempt"></a>事件1558： SERVICE_WITNESS_FAILOVER_ATTEMPT

叢集服務偵測到見證資源發生問題。 在嘗試重新建立叢集設定資料的存取權時，見證資源會故障切換至叢集中的另一個節點。

### <a name="event-1562-res_fsw_alivefailure"></a>事件1562： RES_FSW_ALIVEFAILURE

檔案共用見證資源 ' %1 ' 在檔案共用 ' %2 ' 上的定期健全狀況檢查失敗。 請確認檔案共用 ' %2 ' 存在，而且可由叢集存取。

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>事件1576： RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

叢集網路名稱資源 ' %1 ' 無法在安全的 DNS 區域中，透過介面卡 ' %4 ' 註冊名稱 ' %2 '。 這是因為具有此名稱的現有記錄，而且叢集識別沒有足夠的許可權可以更新該記錄。 錯誤碼為 ' %3 '。 請洽詢您的 DNS 伺服器管理員，確認叢集身分識別具有 DNS 記錄 ' %2 ' 的許可權。

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>事件1576： RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

叢集網路名稱資源 ' %1 ' 無法在安全的 DNS 區域中，透過介面卡 ' %4 ' 註冊名稱 ' %2 '。 這是因為具有此名稱的現有記錄，而且叢集識別沒有足夠的許可權可以更新該記錄。 錯誤碼為 ' %3 '。 請洽詢您的 DNS 伺服器管理員，確認叢集身分識別具有 DNS 記錄 ' %2 ' 的許可權。

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>事件1577： RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

叢集網路名稱資源 ' %1 ' 無法透過介面卡 ' %4 ' 註冊名稱 ' %2 '。 無法連絡 DNS 伺服器。 錯誤碼為 ' %3 '。 請確定 DNS 伺服器可從這個叢集節點存取。 稍後將會重試 DNS 註冊。

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>事件1577： RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

叢集網路名稱資源 ' %1 ' 無法透過介面卡 ' %4 ' 註冊名稱 ' %2 '。 無法連絡 DNS 伺服器。 錯誤碼為 ' %3 '。 請確定 DNS 伺服器可從這個叢集節點存取。 稍後將會重試 DNS 註冊。

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>事件1578： RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

叢集網路名稱資源 ' %1 ' 無法透過介面卡 ' %4 ' 註冊名稱 ' %2 ' 的動態更新。 DNS 伺服器可能未設定成接受動態更新。 錯誤碼為 ' %3 '。 請洽詢您的 DNS 伺服器系統管理員，確認 DNS 伺服器是否可用，以及是否已設定動態更新。<br><br>或者，您可以在 [DNS] 索引標籤底下的介面卡 ' %4 ' 的 advanced TCP/IP 設定中，取消核取 [在 DNS 中註冊此連線的位址] 設定，以停用動態 DNS 更新。

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>事件1578： RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

叢集網路名稱資源 ' %1 ' 無法透過介面卡 ' %4 ' 註冊名稱 ' %2 ' 的動態更新。 DNS 伺服器可能未設定成接受動態更新。 錯誤碼為 ' %3 '。 請洽詢您的 DNS 伺服器系統管理員，確認 DNS 伺服器是否可用，以及是否已設定動態更新。<br><br>或者，您可以在 [DNS] 索引標籤底下的介面卡 ' %4 ' 的 advanced TCP/IP 設定中，取消核取 [在 DNS 中註冊此連線的位址] 設定，以停用動態 DNS 更新。

### <a name="event-1579-res_netname_dns_record_update_failed"></a>事件1579： RES_NETNAME_DNS_RECORD_UPDATE_FAILED

叢集網路名稱資源 ' %1 ' 無法透過介面卡 ' %4 ' 更新名稱 ' %2 ' 的 DNS 記錄。 錯誤碼為 ' %3 '。 請確定 DNS 伺服器可從這個叢集節點存取，並與您的 DNS 伺服器管理員確認叢集身分識別可以更新 DNS 記錄 ' %2 '。

### <a name="event-1579-res_netname_dns_record_update_failed"></a>事件1579： RES_NETNAME_DNS_RECORD_UPDATE_FAILED

叢集網路名稱資源 ' %1 ' 無法透過介面卡 ' %4 ' 更新名稱 ' %2 ' 的 DNS 記錄。 錯誤碼為 ' %3 '。 請確定 DNS 伺服器可從這個叢集節點存取，並與您的 DNS 伺服器管理員確認叢集身分識別可以更新 DNS 記錄 ' %2 '。

### <a name="event-1581-clussvc_unable_to_move_hive_to_safe_file"></a>事件1581： CLUSSVC_UNABLE_TO_MOVE_HIVE_TO_SAFE_FILE

叢集設定資料的還原要求無法建立現有叢集設定資料檔案（ClusDB）的複本。 嘗試保留現有的設定時，還原作業無法在位置 ' %1 ' 建立複本。 如果現有的設定資料檔案已損毀，可能會發生這種情況。 還原作業已繼續，但嘗試還原至現有的叢集設定可能無法正常運作。

### <a name="event-1582-clussvc_unable_to_move_restored_hive_to_current"></a>事件1582： CLUSSVC_UNABLE_TO_MOVE_RESTORED_HIVE_TO_CURRENT

叢集服務無法將已還原的叢集 hive 于 ' %1 ' 移動到 ' %2 '。 這可能會導致還原作業無法順利完成。 如果叢集設定未正確還原，請重試還原動作。

### <a name="event-1583-clussvc_netft_disable_connectionsecurity_failed"></a>事件1583： CLUSSVC_NETFT_DISABLE_CONNECTIONSECURITY_FAILED

叢集服務無法停用容錯移轉叢集虛擬介面卡 ' %1 ' 上的網際網路通訊協定安全性（IPsec）。 這可能會對叢集通訊效能造成負面影響。 如果此問題持續發生，請驗證您的本機和網域連線安全性原則是否套用至 IPSec 和 Windows 防火牆。 此外，請檢查與基礎篩選引擎服務相關的事件。

### <a name="event-1584-shared_volume_not_ready_for_snapshot"></a>事件1584： SHARED_VOLUME_NOT_READY_FOR_SNAPSHOT

備份應用程式已在叢集共用磁碟區 ' %1 ' （' %3 '）上起始 VSS 快照集，但未正確準備快照集的磁片區。 此快照集可能無效，而且備份可能無法用於還原作業。 請洽詢您的備份應用程式廠商，以確認與叢集共用磁片區的相容性。

### <a name="event-1589-res_netname_dns_returning_ip_that_is_not_provider"></a>事件1589： RES_NETNAME_DNS_RETURNING_IP_THAT_IS_NOT_PROVIDER

叢集網路名稱資源 ' %1 ' 找到一個或多個與 DNS 名稱 ' %2 ' 相關聯的 IP 位址，但它們不是依存的 IP 位址資源。 找到的其他位址為 ' %3 '。 這可能會影響用戶端連線能力，直到網路名稱及其相關聯的 DNS 記錄一致為止。 請洽詢您的 DNS 伺服器管理員，以確認與名稱 ' %2 ' 相關聯的記錄。

### <a name="event-1604-res_disk_chkdsk_spotfix_needed"></a>事件1604： RES_DISK_CHKDSK_SPOTFIX_NEEDED

叢集磁片資源 ' %1 ' 已偵測到磁片區 ' %2 ' 的損毀。 需要 Spotfix Chkdsk 才能修復問題。

### <a name="event-1605-res_disk_spotfix_performed"></a>事件1605： RES_DISK_SPOTFIX_PERFORMED

叢集磁片資源 ' %1 ' 已完成執行 ChkDsk.exe 磁片區 ' %2 ' 上的/spotfix。
傳回碼為 ' %4 '。 ChkDsk 的輸出已記錄到檔案 ' %3 '。<br>
檢查應用程式事件記錄檔，以取得 ChkDsk 的其他資訊。

### <a name="event-1671-res_disk_online_set_attributes_completed_failure"></a>事件1671： RES_DISK_ONLINE_SET_ATTRIBUTES_COMPLETED_FAILURE

叢集實體磁片資源無法上線。<br><br>實體磁片資源名稱： %1<br>錯誤碼： %2<br>經過時間（秒）： %3

#### <a name="guidance"></a>指引

執行 [驗證設定] 嚮導，以檢查您的存放裝置設定。 如果錯誤碼是 ERROR_CLUSTER_SHUTDOWN 則系統管理員已取消線上擱置狀態。 如果這是複寫的磁片區，這可能是因為無法設定磁片屬性所導致。 如需其他資訊，請參閱儲存體複寫事件。

### <a name="event-1673-cluster_node_entered_isolated_state"></a>事件1673： CLUSTER_NODE_ENTERED_ISOLATED_STATE

叢集節點 ' %1 ' 已進入隔離狀態。

### <a name="event-1675-event_joining_node_quarantined"></a>事件1675： EVENT_JOINING_NODE_QUARANTINED

' %2 ' 已隔離叢集節點 ' %1 '，且無法加入叢集。 節點將會隔離到 ' %3 '，然後節點會自動嘗試重新加入叢集。 <br><br>請參閱系統和應用程式事件記錄檔，以判斷此節點上的問題。 問題解決後，您可以手動清除隔離，以允許節點與 ' Start-clusternode – ClearQuarantine ' Windows PowerShell Cmdlet 重新加入。<br><br>節點名稱： %1<br>QuarantineType：由 %2 隔離<br>將自動清除時間隔離： %3

### <a name="event-1681-cluster_groups_unmonitored_on_node"></a>事件1681： CLUSTER_GROUPS_UNMONITORED_ON_NODE

節點 ' %1 ' 上的虛擬機器已進入未受監視的狀態。 當節點從隔離的狀態傳回，或如果節點未傳回時可能會容錯移轉，虛擬機器健全狀況就會再次受到監視。 不再受監視的虛擬機器為： %2。

### <a name="event-1689-event_disable_and_stop_other_service"></a>事件1689： EVENT_DISABLE_AND_STOP_OTHER_SERVICE

叢集服務偵測到與容錯移轉叢集不相容的服務。 已停用此服務，以避免容錯移轉叢集發生問題。<br><br>服務名稱： ' %1 '

### <a name="event-4625-nodecleanup_reset_nlbsflags_preserved"></a>事件4625： NODECLEANUP_RESET_NLBSFLAGS_PRESERVED

在叢集節點清除期間，重設 IPSec 安全性關聯超時登錄值失敗。 這是因為在此電腦設定為叢集成員之後，會修改 IPSec 安全性關聯超時。 若要手動清除，請在這部電腦上執行 ' Clear-Start-clusternode ' PowerShell Cmdlet。 或者，您可以從 Windows 登錄中的 HKEY_LOCAL_MACHINE 刪除 ' %1 ' 值和 ' %2 ' 值，以重設 IPSec 安全性關聯超時。

### <a name="event-4873-netft_clussvc_terminate_because_of_watchdog"></a>事件4873： NETFT_CLUSSVC_TERMINATE_BECAUSE_OF_WATCHDOG

叢集服務偵測到容錯移轉叢集虛擬介面卡已停止。 在此節點上執行熱取代 CPU 時，應該會發生這種情況。
叢集服務將會停止，而且應該在作業完成後自動重新開機。 請檢查與服務相關聯的其他事件，並確定已在此節點上重新開機叢集服務。

### <a name="event-5120-dcm_volume_auto_pause_after_failure"></a>事件5120： DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE

叢集共用磁碟區 ' %1 ' （' %2 '）已進入暫停狀態，因為 ' %3 '。
所有 i/o 都會暫時排入佇列，直到磁片區的路徑重新建立為止。

### <a name="event-5123-dcm_event_root_rename_success"></a>事件5123： DCM_EVENT_ROOT_RENAME_SUCCESS

叢集共用磁片區根目錄 ' %1 ' 已經存在。 目錄 ' %1 ' 已重新命名為 ' %2 '。 請確認在此位置存取資料的應用程式已視需要更新。

### <a name="event-5124-dcm_unsafe_filters_found"></a>事件5124： DCM_UNSAFE_FILTERS_FOUND

叢集共用磁碟區 ' %1 ' （' %3 '）已在此裝置堆疊上識別出可能會干擾 CSV 作業的一或多個作用中篩選器驅動程式。 I/o 存取將會透過網路上的另一個叢集節點重新導向至存放裝置。 這可能會導致效能降低。 請洽詢篩選驅動程式廠商，以確認與叢集共用磁片區的互通性。 <br><br>找到的 Active filter 驅動程式：<br>%2

### <a name="event-5125-dcm_unsafe_volfilter_found"></a>事件5125： DCM_UNSAFE_VOLFILTER_FOUND

叢集共用磁碟區 ' %1 ' （' %3 '）已在此裝置堆疊上識別出可能會干擾 CSV 作業的一或多個作用中磁片區驅動程式。 I/o 存取將會透過網路上的另一個叢集節點重新導向至存放裝置。 這可能會導致效能降低。 請洽詢大量驅動程式廠商，以確認與叢集共用磁片區的互通性。 <br><br>找到的有效磁片區驅動程式：<br>%2

### <a name="event-5126-dcm_event_cannot_disable_short_names"></a>事件5126： DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

實體磁片資源 ' %1 ' 不允許停用簡短名稱產生。 這可能會導致應用程式相容性問題。 請使用 ' fsutil 8dot3name set 2 ' 來停用簡短名稱產生，然後再離線/上線資源。

### <a name="event-5128-dcm_event_cannot_disable_short_names"></a>事件5128： DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

實體磁片資源 ' %1 ' 不允許停用簡短名稱產生。 這可能會導致應用程式相容性問題。 請使用 ' fsutil 8dot3name set 2 ' 來停用簡短名稱產生，然後再離線/上線資源。

### <a name="event-5133-dcm_cannot_restore_drive_letters"></a>事件5133： DCM_CANNOT_RESTORE_DRIVE_LETTERS

已移除叢集磁片 ' %1 '，並將其放回「可用的存放裝置」叢集群組。 在此過程中，還原原始磁碟機號的嘗試時間超過預期，可能是因為那些磁碟機號已在使用中。

### <a name="event-5134-dcm_cannot_set_acl_on_root"></a>事件5134： DCM_CANNOT_SET_ACL_ON_ROOT

叢集服務無法在叢集共用磁片區根目錄 ' %1 ' 上設定許可權（ACL）。 錯誤為 ' %2 '。

### <a name="event-5135-dcm_cannot_set_acl_on_volume_folder"></a>事件5135： DCM_CANNOT_SET_ACL_ON_VOLUME_FOLDER

叢集服務無法在叢集共用磁碟區目錄 ' %1 ' （' %2 '）上設定許可權。 錯誤為 ' %3 '。

### <a name="event-5136-dcm_csv_into_redirected_mode"></a>事件5136： DCM_CSV_INTO_REDIRECTED_MODE

已開啟叢集共用磁碟區 ' %1 ' （' %2 '）重新導向存取。 存放裝置的存取將會透過網路從所有存取此磁片區的叢集節點重新導向。 這可能會導致效能降低。 關閉此磁片區的重新導向存取以繼續正常作業。

### <a name="event-5149-dcm_csv_block_cache_resized"></a>事件5149： DCM_CSV_BLOCK_CACHE_RESIZED

快取大小已根據填入的記憶體調整為 ' %1 '，使用者 setsize 太高。

### <a name="event-5156-dcm_volume_auto_pause_after_snapshot_failure"></a>事件5156： DCM_VOLUME_AUTO_PAUSE_AFTER_SNAPSHOT_FAILURE

叢集共用磁碟區 ' %1 ' （' %2 '）已進入暫停狀態，因為 ' %3 '。
當 CSV 磁片區基礎的 volsnap 快照集在使用者要求之外遭到刪除時，就會發生此錯誤。 快照集被刪除的可能原因是空間不足，所以快照集無法成長，或嘗試更新快照集資料時發生 IO 失敗。 所有 i/o 都會暫時排入佇列，直到快照集狀態與 volsnap 同步為止。

### <a name="event-5157-dcm_volume_auto_pause_after_failure_expected"></a>事件5157： DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE_EXPECTED

叢集共用磁碟區 ' %1 ' （' %2 '）已進入暫停狀態，因為 ' %3 '。
所有 i/o 都會暫時排入佇列，直到磁片區的路徑重新建立為止。
此錯誤通常是因基礎結構失敗所造成。 例如，失去對存放裝置的連線，或擁有從作用中叢集成員資格中移除之叢集共用磁碟區的節點。

### <a name="event-5394-pool_online_warnings"></a>事件5394： POOL_ONLINE_WARNINGS

叢集服務嘗試讓存放集區上線時，發生一些儲存體錯誤。 執行叢集存放裝置驗證以對問題進行疑難排解。

### <a name="event-5395-rcm_group_auto_move_storage_pool"></a>事件5395： RCM_GROUP_AUTO_MOVE_STORAGE_POOL

叢集正在移動存放集區 ' %1 ' 的群組，因為目前節點 ' %3 ' 沒有與存放集區實體磁片的最佳連線能力。

## <a name="informational-events"></a>參考事件

### <a name="event-1592-clussvc_tcp_reconnect_connection_reestablished"></a>事件1592： CLUSSVC_TCP_RECONNECT_CONNECTION_REESTABLISHED

叢集節點 ' %1 ' 遺失與叢集節點 ' %2 ' 的通訊。 已重新建立網路通訊。 這可能是因為防火牆或連線安全性原則更新暫時封鎖通訊。 如果問題持續發生，而且未重新建立網路通訊，則會停止一或多個節點上的叢集服務。 如果發生這種情況，請執行 [驗證設定向導] 來檢查您的網路設定。 此外，請檢查與此節點上網路介面卡相關的硬體或軟體錯誤，並檢查節點連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

### <a name="event-1594-cluster_functional_level_upgrade_complete"></a>事件1594： CLUSTER_FUNCTIONAL_LEVEL_UPGRADE_COMPLETE

叢集服務已成功升級叢集功能等級。 <br><br>
功能等級： %1 <br> 升級版本： %2

### <a name="event-1635-rcm_resource_failure_info_with_typename"></a>事件1635： RCM_RESOURCE_FAILURE_INFO_WITH_TYPENAME

叢集角色 ' %2 ' 中類型 ' %3 ' 的叢集資源 ' %1 ' 失敗。

### <a name="event-1636-clussvc_password_changed"></a>事件1636： CLUSSVC_PASSWORD_CHANGED

叢集服務已變更節點 ' %2 ' 上帳戶 ' %1 ' 的密碼。

### <a name="event-1680-cluster_node_exited_isolated_state"></a>事件1680： CLUSTER_NODE_EXITED_ISOLATED_STATE

叢集節點 ' %1 ' 已結束隔離的狀態。

### <a name="event-1682-cluster_group_moved_no_longer_unmonitored"></a>事件1682： CLUSTER_GROUP_MOVED_NO_LONGER_UNMONITORED

在隔離的節點 ' %1 ' 上未受監視的虛擬機器 ' %2 ' 已故障切換至另一個節點。 虛擬機器的健康情況再次受到監視。

### <a name="event-1682-cluster_multiple_groups_no_longer_unmonitored"></a>事件1682： CLUSTER_MULTIPLE_GROUPS_NO_LONGER_UNMONITORED

節點 ' %1 ' 已重新加入叢集，且在該節點上執行的下列虛擬機器再次受到監視的健全狀況狀態： %2。

### <a name="event-4621-nodecleanup_success"></a>事件4621： NODECLEANUP_SUCCESS

已成功從叢集中移除此節點。

### <a name="event-5121-dcm_volume_no_direct_io_due_to_failure"></a>事件5121： DCM_VOLUME_NO_DIRECT_IO_DUE_TO_FAILURE

無法再直接從這個叢集節點存取叢集共用磁碟區 ' %1 ' （' %2 '）。 I/o 存取會透過網路重新導向至擁有該磁片區的節點。 如果這會導致效能降低，請針對此節點與存放裝置的連線問題進行疑難排解，並在重新建立與存放裝置的連線後，i/o 將恢復為狀況良好的狀態。

### <a name="event-5218-csv_old_sw_snapshot_deleted"></a>事件5218： CSV_OLD_SW_SNAPSHOT_DELETED

叢集實體磁片資源 ' %1 ' 已刪除軟體快照集。 叢集共用磁碟區 ' %2 ' 上的軟體快照集已刪除，因為它早于 ' %3 ' 天。 快照集識別碼是 ' %4 '，而它是從 ' %6 ' 的節點 ' %5 ' 所建立。
在備份作業完成之後，備份應用程式應該會刪除快照集。 此快照集超過快照集存在所需的時間。 請向備份應用程式確認備份作業已順利完成。

## <a name="additional-references"></a>其他參考

-   [Windows Server 2008 中容錯移轉叢集元件的詳細事件資訊](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753362(v%3dws.10))
