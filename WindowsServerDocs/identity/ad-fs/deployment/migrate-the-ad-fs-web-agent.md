---
title: 遷移 AD FS web 代理程式
description: 提供 AD FS web 代理程式到 Windows Server 2012 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: 23e80eee87eb5faac875298f4264607df65253c3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940740"
---
# <a name="migrate-the-ad-fs-web-agent"></a>遷移 AD FS web 代理程式

若要將 Windows Server 2008 R2 或 Windows Server 2008 所安裝的 AD FS 1.1 Windows 權杖型代理程式或 AD FS 1.1 宣告感知代理程式遷移至 Windows Server 2012，請將裝載任一代理程式之電腦的作業系統就地升級到 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11))。 不需要進一步設定。

> [!IMPORTANT]
>  移轉的 AD FS 1.1 Windows 權杖型代理程式只能與隨 Windows Server 2008 R2 或 Windows Server 2008 一起安裝的 AD FS 1.1 同盟服務搭配運作。 如需詳細資訊，請參閱 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。
>
>  移轉的 AD FS 1.1 宣告感知網路代理程式可與下列項目搭配運作：
>
> - 隨 Windows Server 2008 R2 或 Windows Server 2008 一起安裝的 AD FS 1.1 同盟服務
>   -   安裝在 Windows Server 2008 R2 或 Windows Server 2008 上的 AD FS 2.0 同盟服務
>   -   與 Windows Server 2012 一起安裝 AD FS federation service
>
>   如需詳細資訊，請參閱 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。


## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)[準備遷移 AD FS 2.0 同盟伺服器 proxy](prepare-to-migrate-ad-fs-fed-proxy.md) [遷移 AD FS 2.0 同盟](migrate-the-ad-fs-fed-server.md)伺服器遷移[AD FS 2.0 同盟伺服器 proxy](migrate-the-ad-fs-2-fed-server-proxy.md) [遷移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)程式
