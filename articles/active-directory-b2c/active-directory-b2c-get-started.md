---
title: Creación de un inquilino de Azure Active Directory B2C | Microsoft Docs
description: Un tema sobre cómo crear un inquilino de Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/07/2017
ms.author: davidmu
ms.openlocfilehash: 56e0ae7454e86911c894da88b5aa8ccc03a08af3
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2018
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a>Creación de un inquilino de Azure Active Directory B2C en Azure Portal

Este inicio rápido le ayuda a crear un inquilino de Microsoft Azure Active Directory (Azure AD) B2C en tan solo unos minutos. Al finalizar, tendrá un inquilino B2C (también conocido como directorio), que podrá usar para registrar aplicaciones B2C.

## <a name="prerequisites"></a>requisitos previos

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

Las características B2C no pueden habilitarse en los inquilinos existentes. Debe crear un inquilino de Azure AD B2C.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Enhorabuena, ha creado un inquilino de Azure Active Directory B2C. Es administrador global del inquilino. Puede agregar otros administradores globales según sea necesario. Para cambiar al nuevo inquilino, haga clic en el *vínculo de administración del nuevo inquilino*.

![Vínculo de administración del nuevo inquilino](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Si planea usar el inquilino B2C para una aplicación de producción, lea el artículo sobre [escala de producción frente a inquilinos de B2C de versión preliminar](active-directory-b2c-reference-tenant-type.md). Existen problemas conocidos al eliminar un inquilino B2C existente y volver a crearlo con el mismo nombre de dominio. Es necesario crear un inquilino B2C con otro nombre de dominio.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Vinculación del inquilino a la suscripción

Debe vincular su inquilino de Azure AD B2C a su suscripción a Azure para habilitar todas las funcionalidades de B2C y pagar los cargos por uso. Para más información, lea [este artículo](active-directory-b2c-how-to-enable-billing.md). Si no vincula el inquilino de Azure AD B2C a su suscripción a Azure, se bloquearán algunas funcionalidades y aparecerá un mensaje de advertencia "No Subscription linked to this B2C tenant or the Subscription needs your attention" (No hay ninguna suscripción vinculada con este inquilino de B2C o es necesario revisar la suscripción) en la configuración de B2C. Es importante que realice este paso antes de enviar las aplicaciones a producción.

## <a name="easy-access-to-settings"></a>Acceso sencillo a la configuración

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

También puede tener acceso a la hoja escribiendo `Azure AD B2C` en **Buscar recursos** en la parte superior del portal. En la lista de resultados, seleccione **Azure AD B2C** para tener acceso a la hoja de configuración de B2C.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de la aplicación B2C en un inquilino B2C](active-directory-b2c-app-registration.md)