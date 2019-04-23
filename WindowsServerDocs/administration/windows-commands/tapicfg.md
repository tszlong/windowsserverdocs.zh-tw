---
title: tapicfg
description: 了解如何管理 TAPI 應用程式目錄分割。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e89b1e3d7638fbcbe0140658d2d2a9af1ccd852
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886509"
---
# <a name="tapicfg"></a>tapicfg

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立、 移除或顯示 TAPI 應用程式目錄分割，或設定預設 TAPI 應用程式目錄分割。 TAPI 3.1 用戶端可以尋找並與 TAPI 目錄進行通訊，以使用目錄服務定位程式服務與此應用程式目錄分割中的資訊。您也可以使用**tapicfg**來建立或移除服務連接點，可讓 TAPI 用戶端有效地在網域中找到 TAPI 應用程式目錄分割。 如需詳細資訊，請參閱 < 備註 >。 若要檢視命令語法，請按一下命令。 
-   [tapicfg install](#BKMK_install)
-   [tapicfg remove](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>tapicfg 安裝
建立 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|安裝 /directory:\<PartitionName >|必要。 指定要建立的 TAPI 應用程式目錄分割的 DNS 名稱。 此名稱必須是完整的網域名稱。|
|/server:\<DCName>|指定建立 TAPI 應用程式目錄磁碟分割所在的網域控制站的 DNS 名稱。 如果未指定網域控制站名稱，則會使用本機電腦的名稱。|
|/forcedefault|指定此目錄預設 TAPI 應用程式目錄磁碟分割的網域。 在網域中可以有多個 TAPI 應用程式目錄分割。<br /><br />如果此目錄是在網域上建立第一個 TAPI 應用程式目錄分割，它會自動設定，預設值，不論您是使用 **/forcedefault**選項。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_remove"></a>tapicfg 移除
移除 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|移除 /directory:\<PartitionName >|必要。 指定要移除的 TAPI 應用程式目錄分割的 DNS 名稱。 請注意，此名稱必須是完整的網域名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_publishscp"></a>tapicfg publishscp
建立服務連接點来發佈的 TAPI 應用程式目錄磁碟分割。

### <a name="syntax"></a>語法
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|publishscp /directory:\<PartitionName >|必要。 指定將會發佈服務連接點的 TAPI 應用程式目錄分割的 DNS 名稱。|
|/domain:\<網域名稱 >|指定在建立服務連接點所在的網域的 DNS 名稱。 如果未指定的網域名稱，則會使用本機網域的名稱。|
|/forcedefault|指定此目錄預設 TAPI 應用程式目錄磁碟分割的網域。 在網域中可以有多個 TAPI 應用程式目錄分割。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_removescp"></a>tapicfg removescp
移除 TAPI 應用程式目錄分割的服務連接點。

### <a name="syntax"></a>語法
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|removescp /directory:\<PartitionName >|必要。 指定服務連接點就會移除 TAPI 應用程式目錄分割的 DNS 名稱。|
|/domain:\<DomainName>|指定服務連接點要移除之網域的 DNS 名稱。 如果未指定的網域名稱，則會使用本機網域的名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_show"></a>tapicfg show
在網域中顯示的名稱和位置的 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/defaultonly|在網域中顯示的名稱和位置，只有預設 TAPI 應用程式目錄分割。|
|/domain:\<DomainName>|指定 TAPI 應用程式目錄分割會顯示網域的 DNS 名稱。 如果未指定的網域名稱，則會使用本機網域的名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_makedefault"></a>tapicfg makedefault
設定網域預設 TAPI 應用程式目錄分割。

### <a name="syntax"></a>語法
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|makedefault /directory:\<PartitionName >|必要。 指定的 DNS 名稱設定為網域的預設資料分割的 TAPI 應用程式目錄分割。 請注意，此名稱必須是完整的網域名稱。 指定 DNS 網域名稱設定為預設值的 TAPI 應用程式目錄分割。 如果未指定的網域名稱，則會使用本機網域的名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
您必須在執行的 active directory 中 Enterprise Admins 群組的成員**tapicfg 安裝**（若要建立的 TAPI 應用程式目錄磁碟分割） 或**tapicfg 移除**（若要移除 TAPI 的應用程式目錄磁碟分割）。

可以是網域成員的任何電腦上執行這個命令列工具。

如果已安裝適當的字型和語言支援，只會正確顯示使用者提供文字 （例如 TAPI 應用程式目錄分割、 伺服器和網域名稱） 與國際或 Unicode 字元。

您仍然可以使用網際網路定址服務 」 (ILS) 伺服器在您的組織，ILS 方法以支援特定應用程式，因為執行 Windows XP 或 Windows Server 2003 作業系統的 TAPI 用戶端可以查詢 ILS 伺服器或 TAPI 應用程式目錄分割。

您可以使用**tapicfg**來建立或移除服務連接點。 如果 TAPI 應用程式目錄分割已重新命名 （例如，如果您重新命名其所在的網域），因為任何原因，您必須移除現有的服務連接點，並建立新的資源，其中包含新的 DNS 名稱，TAPI 應用程式目錄要發行的資料分割。 否則，TAPI 用戶端都無法找出並存取 TAPI 應用程式目錄分割。 （例如，如果您不想公開特定的 TAPI 應用程式目錄磁碟分割上的 TAPI 資料），您也可以移除服務連接點進行維護或安全性。

## <a name="examples"></a>範例
若要建立 TAPI 應用程式目錄分割的伺服器上的具名的 tapifiction.testdom.microsoft.com 為 testdc.testdom.microsoft.com，然後將它設為預設 TAPI 應用程式目錄分割，新的網域，請輸入：
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
若要顯示預設 TAPI 應用程式目錄分割為新網域的名稱，請輸入：
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
