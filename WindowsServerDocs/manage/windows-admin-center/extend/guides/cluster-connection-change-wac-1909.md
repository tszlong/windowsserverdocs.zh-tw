---
title: Windows 系統管理中心 v1909 中的叢集連線類型變更
description: Windows 系統管理中心 v1909 中的叢集連線類型變更
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 10/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a07b30517f0d45b7e6f4f41f0ef9a6549e6e2117
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71952769"
---
# <a name="cluster-connection-type-changes-in-windows-admin-center-v1909"></a>Windows 系統管理中心 v1909 中的叢集連線類型變更

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!IMPORTANT]
> 本檔說明 Windows 管理中心延伸模組開發人員針對容錯移轉叢集和超融合式叢集（HCI）解決方案開發 Windows 管理中心工具所需的變更。 這是您的延伸模組所需的必要變更，以與 Windows 管理中心 v1909 Preview 版本和未來的 GA 版本相容。

在 Windows 系統管理中心 v1909 中，我們已將兩個不同的叢集連線類型（容錯移轉叢集和超交集叢集連線）統一成單一叢集連線類型。 使用者不再需要先行識別叢集的設定來決定要新增叢集的連線類型，也不會新增叢集兩次做為不同的連線類型，以存取不同的工具集。 現在可以將叢集新增為「Windows Server cluster」，然後載入適當的工具，主要取決於是否已啟用儲存空間直接存取。

由於這需要變更連線類型定義以及叢集相關工具如何決定何時載入，因此提供叢集工具的擴充功能（HCI 或非 HCI 叢集，或兩者）將需要執行中的變更，如下列所述：如下.

## <a name="manifestjson---solutionsids-and-connectiontypes"></a>solutionsIds 和 connectionTypes

先前，若要讓您的工具顯示在容錯移轉叢集或 HCI 叢集連線類型中，您可以使用 ```manifest.json``` 檔案中的下列其中一個定義。

針對容錯移轉叢集：
``` json
    {
        "entryPointType": "tool",
        "name": "MyToolName",
        "urlName": "MyToolUrl",
        "displayName": "MyToolDisplayName",
        "description": "MyToolDescription",
        "icon": "MyToolIcon",
        "path": "MyToolPath",
        "iframeId": "MyToolIframeId",
        "requirements": [
        {
            "solutionIds": [
                "msft.sme.failover-cluster!failover-cluster"
            ],
            "connectionTypes": [
                "msft.sme.connection-type.cluster"
            ]
        }
        ]
    }
```

對於 HCI 叢集，上方反白顯示的區段會取代為下列內容：
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    }
    ]
```

在 Windows 系統管理中心1909和更新版本中，已將兩個 solutionIds 和 connectionTypes 取代為下列內容：
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ]
    }
    ]
```

這是目前唯一支援的叢集相關 solutionIds 和 connectionTypes 類型。 如果您的工具只以此 solutionIds 和 connectionTypes 類型定義，則會針對任何容錯移轉叢集連線載入，不論它是否為 HCI 叢集。 如果您想要將工具限制為僅供 HCI 叢集或非 HCI 叢集使用，您必須另外使用下一節所述的新清查屬性。

## <a name="manifestjson--inventory-properties"></a>資訊清單 json –清查屬性
連線到伺服器或叢集時，Windows 管理中心會查詢一組清查內容，讓您用來建立條件來判斷您的工具何時可以使用（請參閱控制您的工具的「清查內容」一節）。 [可見度](dynamic-tool-display.md)檔以取得詳細資訊）。 在 Windows Admin Center v1909 中，我們已將兩個新屬性新增至此清單，可用來判斷叢集是否為超交集叢集。 

### <a name="iss2denabled"></a>isS2dEnabled
技術上而言，超交集叢集會定義為已啟用儲存空間直接存取（S2D）的容錯移轉叢集。 如果您想要讓工具僅適用于超交集叢集，亦即，啟用 S2D 時，請新增下列清查條件：
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
        ]
    }
    ]
```
> [!TIP] 
> 如果您想要讓工具只有在未啟用 S2D 時才可使用，請將 "operator" 值變更為 "not"。

### <a name="isbritannicaenabled"></a>isBritannicaEnabled
此外，如果您相依于 SDDC 管理叢集資源並使用 SDDC 物件模型，您可以檢查下列條件：
``` json
    "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                },
                "isBritannicaEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
    ]
```

## <a name="backward-compatibility-to-support-previous-versions-of-windows-admin-center"></a>支援舊版 Windows 管理中心的回溯相容性
為了確保您的擴充功能會繼續使用舊版的 Windows 管理中心，例如 v1904 GA 版本，先前的 solutionIds 和 connectionTypes 定義可以與新的定義一起使用。 請參閱下列範例，只針對所有 Windows 系統管理中心版本中的 HCI 叢集顯示您的工具。
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    },
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC",
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster",
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
            {
                "inventory": {
                    "isS2dEnabled": {
                        "type": "boolean",
                        "operator": "is"
                    }
                }
            }
        ]
    }
    ]
```

## <a name="known-issue-appcontextserviceactiveconnectionishyperconvergedclusterisfailovercluster-is-not-set-properly-in-windows-admin-center-v1909"></a>已知問題：Windows 系統管理中心 v1909 中的 AppCoNtextService. activeConnection. isHyperConvergedCluster/isFailoverCluster 未正確設定
最近一次變更的回歸是在 Windows 系統管理中心 v1909 中，未正確設定 @no__t 0 內容，而且一律會是 false。 這會在下一版的 v1910 中修正，但也會在2020的下列 GA 版本中被取代並不再提供使用。 未來，您可以使用下列程式碼取代此項，並使用 ```this.connectHCI```。
```
    import { ClusterInventoryCache } from '@msft-sme/core';

    constructor(
        ...
        private appContextService: AppContextService,
        ...
    ) {
        ...
        this.clusterInventoryCache = new ClusterInventoryCache(
            this.appContextService,
            <SharedCacheOptions>{ expiration: Constants.sharedCacheExpiration }
        );

         this.isBritannicEnabled().subscribe(result => this.connectHCI = result);
    }

    private isBritannicEnabled(): Observable<boolean> {
        return this.clusterInventoryCache.query(this.getClusterInventoryQueryParameters())
        .pipe(
            map(inventory => {
                return inventory.instance.isBritannicaEnabled;
        }));
    }

    private getClusterInventoryQueryParameters(): ClusterInventoryParams {
        const nodeName = this.appContextService.activeConnection.validNodeName;
        const endpoint = this.appContextService.authorizationManager.getJeaEndpoint(nodeName);
        const options = { powerShellEndpoint: endpoint };
        return {
            name: nodeName,
            requestOptions: options
        };
    }
```