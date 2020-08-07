---
title: tapicfg
description: 瞭解如何管理 TAPI 應用程式目錄分割。
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a7df53cc06581ad4014600c2ab7f402ff33530fd
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881842"
---
# <a name="tapicfg"></a>tapicfg

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立、移除或顯示 TAPI 應用程式目錄分割，或設定預設的 TAPI 應用程式目錄分割。 TAPI 3.1 用戶端可以使用此應用程式目錄分割中的資訊搭配目錄服務定位器服務，尋找並與 TAPI 目錄通訊。您也可以使用**tapicfg**來建立或移除服務連接點，讓 TAPI 用戶端有效率地找出網域中的 tapi 應用程式目錄分割。 如需詳細資訊，請參閱備註。 若要查看命令語法，請按一下命令。
-   [tapicfg 安裝](#BKMK_install)
-   [tapicfg 移除](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="tapicfg-install"></a><a name="BKMK_install"></a>tapicfg 安裝
建立 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|安裝/目錄：\<PartitionName>|必要。 指定要建立之 TAPI 應用程式目錄分割的 DNS 名稱。 此名稱必須是完整功能變數名稱。|
|/server\<DCName>|指定在其中建立 TAPI 應用程式目錄分割之網域控制站的 DNS 名稱。 如果未指定網域控制站名稱，則會使用本機電腦的名稱。|
|/forcedefault|指定此目錄為網域的預設 TAPI 應用程式目錄分割。 網域中可以有多個 TAPI 應用程式目錄分割。<p>如果此目錄是在網域上建立的第一個 TAPI 應用程式目錄分割，不論您是否使用 **/forcedefault**選項，它都會自動設定為預設值。|
|/?|在命令提示字元顯示說明。|

## <a name="tapicfg-remove"></a><a name="BKMK_remove"></a>tapicfg 移除
移除 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg remove /directory:<PartitionName>
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|移除/目錄：\<PartitionName>|必要。 指定要移除之 TAPI 應用程式目錄分割的 DNS 名稱。 請注意，此名稱必須是完整功能變數名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="tapicfg-publishscp"></a><a name="BKMK_publishscp"></a>tapicfg publishscp
建立服務連接點，以發佈 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|publishscp/目錄：\<PartitionName>|必要。 指定服務連接點將發佈之 TAPI 應用程式目錄分割的 DNS 名稱。|
|/domain\<DomainName>|指定用來建立服務連接點之網域的 DNS 名稱。 如果未指定功能變數名稱，則會使用本機網域的名稱。|
|/forcedefault|指定此目錄為網域的預設 TAPI 應用程式目錄分割。 網域中可以有多個 TAPI 應用程式目錄分割。|
|/?|在命令提示字元顯示說明。|

## <a name="tapicfg-removescp"></a><a name="BKMK_removescp"></a>tapicfg removescp
移除 TAPI 應用程式目錄分割的服務連接點。

### <a name="syntax"></a>語法
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|removescp/目錄：\<PartitionName>|必要。 指定要移除其服務連接點之 TAPI 應用程式目錄分割的 DNS 名稱。|
|/domain\<DomainName>|指定移除服務連接點之網域的 DNS 名稱。 如果未指定功能變數名稱，則會使用本機網域的名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="tapicfg-show"></a><a name="BKMK_show"></a>tapicfg show
顯示網域中 TAPI 應用程式目錄分割的名稱和位置。

### <a name="syntax"></a>語法
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/defaultonly|只顯示網域中預設 TAPI 應用程式目錄分割的名稱和位置。|
|/domain\<DomainName>|指定要顯示其 TAPI 應用程式目錄分割之網域的 DNS 名稱。 如果未指定功能變數名稱，則會使用本機網域的名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="tapicfg-makedefault"></a><a name="BKMK_makedefault"></a>tapicfg makedefault
設定網域的預設 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|makedefault/目錄：\<PartitionName>|必要。 將 TAPI 應用程式目錄分割集的 DNS 名稱指定為網域的預設磁碟分割。 請注意，此名稱必須是完整功能變數名稱。 指定將 TAPI 應用程式目錄分割設定為預設之網域的 DNS 名稱。 如果未指定功能變數名稱，則會使用本機網域的名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
您必須是 active directory 中 Enterprise Admins 群組的成員，才能執行**tapicfg install** (以建立 tapi 應用程式目錄分割) 或**tapicfg 移除** (移除 tapi 應用程式目錄分割) 。

這個命令列工具可以在網域成員的任何電腦上執行。

只有在安裝適當的字型和語言支援時，才會正確顯示使用者提供的文字 (例如 TAPI 應用程式目錄分割、伺服器，以及使用國際或 Unicode 字元) 的網功能變數名稱稱。

您仍然可以在組織中使用網際網路定位器服務 (ILS) 伺服器，如果需要 ILS 以支援特定應用程式，因為執行 Windows XP 或 Windows Server 2003 作業系統的 TAPI 用戶端可以查詢 ILS 伺服器或 TAPI 應用程式目錄分割。

您可以使用**tapicfg**來建立或移除服務連接點。 如果 TAPI 應用程式目錄分割因為任何原因而重新命名 (例如，如果您將其所在的網域重新命名) ，就必須移除現有的服務連接點，並建立一個新的，其中包含要發行之 TAPI 應用程式目錄分割的新 DNS 名稱。 否則，TAPI 用戶端將無法找到並存取 TAPI 應用程式目錄分割。 您也可以移除服務連接點，以進行維護或安全性用途 (例如，如果您不想要在特定 TAPI 應用程式目錄分割上公開 TAPI 資料) 。

## <a name="examples"></a>範例
若要在名為 testdc.testdom.microsoft.com 的伺服器上建立名為 tapifiction.testdom.microsoft.com 的 TAPI 應用程式目錄分割，然後將它設定為新網域的預設 TAPI 應用程式目錄分割，請輸入：
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
若要顯示新網域的預設 TAPI 應用程式目錄分割名稱，請輸入：
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
