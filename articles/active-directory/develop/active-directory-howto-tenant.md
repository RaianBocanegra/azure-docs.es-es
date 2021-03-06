---
title: Obtención de un inquilino de Azure AD | Microsoft Docs
description: Obtenga un inquilino de Azure Active Directory para el registro y la creación de aplicaciones.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/23/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: ab7db49fa07f260de6ebbe4b2cee943b64cab7fe
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2018
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Obtención de un inquilino de Azure Active Directory
En Azure Active Directory (Azure AD), un [inquilino](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0) es un representante de una organización.  Se trata de una instancia dedicada del servicio de Azure AD que recibe una organización y que posee cuando crea una relación con Microsoft, como al iniciar sesión en un servicio en la nube de Microsoft, como Azure, Microsoft Intune u Office 365.  Cada inquilino de Azure AD es distinto e independiente de los demás inquilinos de Azure AD.  

Un inquilino aloja los usuarios de una empresa y la información sobre ellos: sus contraseñas, datos de perfil de usuario, permisos, etc.  También contiene grupos, aplicaciones y otra información relativa a una organización y su seguridad.

Para permitir que los usuarios de Azure AD inicien sesión en su aplicación, debe registrar la aplicación en un inquilino que elija.  Crear un inquilino de Azure AD y publicar una aplicación en él es **totalmente gratuito** (aunque puede elegir pagar por las características de la edición Premium en el inquilino).  De hecho, muchos programadores crean varios inquilinos y aplicaciones para fines de experimentación, desarrollo, ensayo y pruebas.

## <a name="use-an-existing-azure-ad-tenant"></a>Uso de un inquilino de Azure AD existente

Muchos desarrolladores ya tienen inquilinos mediante servicios o suscripciones que están vinculados a inquilinos de Azure AD (por ejemplo, suscripciones de Office 365 o Azure).  Para comprobar si ya tiene un inquilino, inicie sesión en [Azure Portal](https://portal.azure.com) con la cuenta que desea usar para administrar la aplicación y mire en la esquina superior derecha donde aparece la información de la cuenta.  Si tiene un inquilino, iniciará sesión de forma automática en él y verá el nombre del inquilino directamente bajo el nombre de la cuenta.  Si la cuenta está asociada a varios inquilinos, puede hacer clic en el nombre de la cuenta para abrir un menú donde puede cambiar entre los inquilinos.

Si no tiene un inquilino existente asociado a la cuenta, verá un GUID bajo el nombre de la cuenta y no podrá realizar acciones como registrar aplicaciones hasta que [cree un nuevo inquilino](#create-a-new-azure-ad-tenant).

## <a name="create-a-new-azure-ad-tenant"></a>Creación de un nuevo inquilino de Azure AD

Si no tiene aún un inquilino de Azure AD o si desea crear uno nuevo, puede hacerlo mediante la [experiencia de creación de directorio](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) en [Azure Portal](https://portal.azure.com).  El proceso tardará aproximadamente un minuto y al final se le pedirá que vaya al inquilino recién creado.