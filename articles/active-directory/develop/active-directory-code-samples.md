---
title: Ejemplos de código de Azure Active Directory | Microsoft Docs
description: Un índice de ejemplos de código de Azure Active Directory, organizados por escenario.
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: 130d26828acd5394756c47f22217272338cc33e6
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2018
---
# <a name="azure-active-directory-code-samples"></a>Ejemplos de código de Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Puede usar Microsoft Azure Active Directory (Azure AD) para agregar autenticación y autorización a sus aplicaciones web y API web. Esta sección lo vincula a ejemplos que le muestran cómo se hace y snippets de código que puede usar en sus aplicaciones. En la página de ejemplos de código, encontrará temas Léame detallados que le ayudarán con los requisitos, la instalación y la configuración. Y se comenta el código para ayudarle a comprender las secciones críticas.

Para comprender los escenarios básicos para cada tipo de ejemplo, consulte Escenarios de autenticación para Azure AD.

Efectúe aportaciones a nuestros ejemplos de código en GitHub: [Ejemplos y documentación de Microsoft Azure Active Directory](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Explorador web a aplicación web
Estos ejemplos muestran cómo escribir una aplicación web que dirige el explorador del usuario para que inicie sesión en Azure AD.

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| C#/.NET |[WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Utilice OpenID Connect (middleware ASP.Net OpenID Connect OWIN) para autenticar a los usuarios desde un inquilino de Azure AD. |
| C#/.NET |[WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Una aplicación web .NET MVC multiempresa que usa OpenID Connect (middleware ASP.Net OpenID Connect OWIN) para autenticar a los usuarios desde varios inquilinos de Azure AD. |
| C#/.NET |[WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |Utilice WS-Federation (middleware ASP.Net WS-Federation OWIN) para autenticar a los usuarios desde un inquilino de Azure AD. |
| C#/.NET Core |[WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) |Aplicación web .NET MVC que usa OpenID Connect para el inicio de sesión de los usuarios desde un único inquilino de Azure Active Directory (Azure AD) mediante el middleware ASP.NET Core OpenID Connect. |

## <a name="single-page-application-spa"></a>Aplicación de una sola página (SPA)
En este ejemplo se muestra cómo escribir una aplicación de una sola página protegida con Azure AD.  

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| JavaScript, C#/.NET |[SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |Utilice ADAL para JavaScript y Azure AD para proteger una aplicación de una sola página basada en AngularJS implementada con un back-end ASP.NET Web API. |

## <a name="native-application-to-web-api"></a>Aplicación nativa a Web API
Estos ejemplos de código muestran cómo crear aplicaciones cliente nativas que llaman a API web protegidas por Azure AD. Emplean la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md) y [OAuth 2.0 en Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| Javascript |[NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |El complemento de ADAL para Apache Cordova se utiliza para crear una aplicación de Apache Cordova que llama a una API web y usa Azure AD para la autenticación. |
| C#/.NET |[NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Una aplicación .NET WPF que llama a una API web que está protegida mediante Azure AD. |
| C#/.NET |[NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Una aplicación de la Tienda Windows que llama a una API web que está protegida con Azure AD. |
| C#/.NET |[NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |Una aplicación de la Tienda Windows que llama a una API web multiempresa que está protegida con Azure AD. |
| C#/.NET |[WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |Una aplicación cliente nativa que llama a una API web, que obtiene un token para actuar en nombre del usuario original y utiliza luego el token para llamar a otra API web. |
| C#/.NET |[NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |Una aplicación de la Tienda Windows para Windows Phone 8.1 que llama a una API web que está protegida por Azure AD. |
| ObjC |[NativeClient-iOS](https://github.com/Azure-Samples/active-directory-ios) |Una aplicación iOS que llama a una API web que requiere Azure AD para la autenticación. |
| C#/.NET |[WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |Una aplicación cliente nativa que incluye la lógica para procesar un token JWT en una API web, en lugar de usar middleware OWIN. |
| C#/Xamarin |[NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) |Un enlace Xamarin a la Biblioteca de autenticación nativa de Azure AD (ADAL) para la biblioteca de Android. |
| C#/Xamarin |[NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) |Un enlace Xamarin a la Biblioteca de autenticación nativa de Azure AD (ADAL) para iOS. |
| C#/Xamarin |[NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |Un proyecto Xamarin que se dirige a cinco plataformas y llama a una API web que está protegida por Azure AD. |
| C#/.NET |[NativeClient-Headless-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |Una aplicación nativa que realiza autenticación no interactiva y llamadas a una API web que está protegida por Azure AD. |

## <a name="web-application-to-web-api"></a>Aplicación web a API web
Estos ejemplos de código muestran cómo usar [OAuth 2.0 en Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) para compilar aplicaciones web que llaman a las API web protegidas por Azure AD.

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| C#/.NET |[WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Permite realizar una llamada a una API web con los permisos del usuario que ha iniciado sesión. |
| C#/.NET |[WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Permite realizar una llamada a una API web con los permisos de una aplicación. |
| C#/.NET |[WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Agrega autorización con [OAuth 2.0 en Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) a una aplicación web existente de modo que esta pueda llamar a una API web. |
| Javascript |[WebAPI-Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |Configura un servicio API de REST que se integra con Azure AD para la protección de API. Incluye un servidor Node.js con una Web API. |
| C#/.NET |[WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |Una aplicación web MVC multiempresa que usa OpenID Connect (middleware ASP.Net OpenID Connect OWIN) para autenticar a los usuarios desde un inquilino de Azure AD. Emplea un código de autorización para invocar a Graph API. |

## <a name="server-or-daemon-application-to-web-api"></a>Aplicación servidor o demonio a Web API
Estos ejemplos de código muestran cómo compilar una aplicación servidor o demonio que obtiene recursos de una API web mediante la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md) y [OAuth 2.0 en Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| C#/.NET |[Daemon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |Una aplicación de consola llama a una API web. La credencial del cliente es una contraseña. |
| C#/.NET |[Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Una aplicación de consola que llama a una API web. La credencial del cliente es un certificado. |

## <a name="calling-microsoft-graph-api"></a>Llamada a Microsoft Graph API
Estos ejemplos de código muestran cómo compilar aplicaciones que llaman a Microsoft Graph API para leer y escribir datos de directorio.

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| C#/.NET |[WebApp-MSGraphAPI-DotNet](https://github.com/microsoftgraph/aspnet-snippets-sample) |Aplicación web que usa Microsoft Graph API para acceder a datos de directorio de Azure AD. |
| C#/.NET |[UWPApp-MSGraphAPI-DotNet](https://github.com/microsoftgraph/uwp-csharp-snippets-sample) |Esta aplicación de la plataforma universal de Windows muestra cómo acceder a varios recursos, incluidos Microsoft Azure Active Directory (AD) y las API de Office 365, mediante solicitudes a Microsoft Graph API en una aplicación de Windows 10. |

## <a name="calling-azure-ad-graph-api"></a>Llamada a Graph API de Azure AD
Estos ejemplos de código muestran cómo compilar aplicaciones que llaman a Graph API de Azure AD para leer y escribir datos de directorio.

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| Java |[WebApp-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Una aplicación web que usa Graph API para obtener acceso a datos de directorio de Azure AD. |
| PHP |[WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Una aplicación web que usa Graph API para obtener acceso a datos de directorio de Azure AD. |
| C#/.NET |[ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Una aplicación de consola que usa la consulta diferencial en Graph API para obtener cambios periódicos en objetos de usuario en un inquilino de Azure AD. |
| C#/.NET |[WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Una aplicación MVC que usa consultas de Graph API para generar un organigrama sencillo de la empresa. |
| PHP |[WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |Una aplicación PHP que llama a Graph API para registrar una extensión y luego leer, actualizar y eliminar valores en el atributo de extensión. |

## <a name="authorization"></a>Autorización
Estos ejemplos de código muestran cómo usar Azure AD para la autorización.

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| C#/.NET |[WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Realiza el control de acceso basado en roles (RBAC) mediante notificaciones de grupo de Azure Active Directory en una aplicación que está integrada con Azure AD. |
| C#/.NET |[WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Realiza el control de acceso basado en roles (RBAC) mediante roles de aplicación de Azure Active Directory en una aplicación que está integrada con Azure AD. |

## <a name="legacy-walkthroughs"></a>Tutoriales heredados
Estos tutoriales usan tecnología algo más antigua, pero también podrían ser de interés.

| Lenguaje/plataforma | Muestra | DESCRIPCIÓN |
| --- | --- | --- |
| C#/.NET |[Autorización basada en roles y basada en ACL en una aplicación de Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) |Realiza la autorización basada en roles (RBAC) y la autorización basada en ACL en una aplicación que se integra con Azure AD. |
| C#/.NET |[AAL: aplicación de la Tienda Windows para el servicio de REST (autenticación)](http://go.microsoft.com/fwlink/?LinkId=330605) |Use la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md) (antes AAL) para la Tienda Windows versión beta con objeto de agregar funciones de autenticación a una aplicación de la Tienda Windows. |
| C#/.NET |[ADAL: aplicación nativa para el servicio de REST (autenticación con AAD mediante el cuadro de diálogo del explorador)](http://go.microsoft.com/fwlink/?LinkId=259814) |Utilice la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md) para agregar capacidades de autenticación de usuarios a un cliente WPF. |
| C#/.NET |[ADAL: aplicación nativa para el servicio de REST (autenticación con ACS mediante el cuadro de diálogo del explorador)](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Utilice la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md) y [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) para agregar capacidades de autenticación de usuarios a un cliente WPF. |
| C#/.NET |[ADAL: autenticación de servidor a servidor](http://go.microsoft.com/fwlink/?LinkId=259816) |Use la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md) para proteger llamadas de servicio de un proceso de servidor a un servicio de REST de MVC4 Web API. |
| C#/.NET |[Incorporación del inicio de sesión a la aplicación web utilizando Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Configure una aplicación .NET para realizar un inicio de sesión único web en su directorio de empresa de Azure AD. |
| C#/.NET |[Desarrollo de aplicaciones web multiempresa con Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Use Azure AD para agregar funciones de inicio de sesión único y de acceso a directorio de una aplicación .NET para trabajar en varias organizaciones. |
| Java |[Aplicación de ejemplo de Java para Graph API de Azure AD](http://go.microsoft.com/fwlink/?LinkId=263969) |Use Graph API para acceder a los datos del directorio de Azure AD. |
| PHP |[Aplicación de ejemplo de PHP para Graph API de Azure AD](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |Use Graph API para acceder a los datos del directorio de Azure AD. |
| C#/.NET |[Aplicación de ejemplo para Graph API de Azure AD](http://go.microsoft.com/fwlink/?LinkID=262648) |Use Graph API para acceder a los datos del directorio de Azure AD. |
| C#/.NET |[Aplicación de ejemplo para consulta diferencial de Azure AD Graph](http://go.microsoft.com/fwlink/?LinkId=275398) |Usa la consulta diferencial en Graph API para obtener cambios periódicos en objetos de usuario en un inquilino de Azure AD. |
| C#/.NET |[Aplicación de ejemplo para integración de aplicaciones multiempresa en la nube con Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) |Integra una aplicación mutiempresa en Azure AD. |
| C#/.NET |[Protección de una aplicación de la Tienda Windows y un servicio web REST mediante Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Cree un recurso sencillo de API web y una aplicación cliente de la Tienda Windows mediante Azure AD y la [Biblioteca de autenticación de Azure AD (ADAL)](active-directory-authentication-libraries.md). |
| C#/.NET |[Uso de Graph API para realizar una consulta a Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Configure una aplicación de Microsoft .NET para usar Graph API de Azure AD para obtener acceso a los datos desde un directorio de inquilinos de Azure AD. |

## <a name="see-also"></a>Otras referencias
##### <a name="other-resources"></a>Otros recursos
[Guía del desarrollador de Azure Active Directory](active-directory-developers-guide.md)

[Conceptos y referencia de Graph API de Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca auxiliar de Graph API de Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
