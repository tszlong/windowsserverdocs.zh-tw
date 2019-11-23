---
title: 遷移 AD FS web 代理程式
description: 提供 AD FS web 代理程式到 Windows Server 2012 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ddba9668b54e325dae6dfc0cf67d50d3ae5d90be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408193"
---
# <a name="migrate-the-ad-fs-web-agent"></a>遷移 AD FS web 代理程式

若要將 Windows Server 2008 R2 或 Windows Server 2008 所安裝的 AD FS 1.1 Windows 權杖型代理程式或 AD FS 1.1 宣告感知代理程式遷移至 Windows Server 2012，請執行裝載任一代理程式之電腦的作業系統就地升級。到 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 不需要進一步的設定。  
  
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
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備遷移 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [遷移 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [遷移 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)