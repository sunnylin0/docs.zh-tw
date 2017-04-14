---
title: "&lt;defaultCertificate&gt; 項目 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f1ddf364-9a00-45d3-b989-ff381c154ce6
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# &lt;defaultCertificate&gt; 項目
指定服務或 STS 不透過交涉通訊協定提供憑證時要使用的 X.509 憑證。  
  
## 語法  
  
```  
  
<defaultCertificate findValue="String"   
storeLocation=" CurrentUser/LocalMachine"  
storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"   
x509FindType="FindByThumbPrint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialiNumber/FindByTimeValid/FindByTimeNotYetValid/FindByTimeExpired/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier" />  
```  
  
## 屬性和項目  
 下列各節說明屬性、子元素和父元素  
  
### 屬性  
  
|屬性|描述|  
|--------|--------|  
|findValue|字串。  要搜尋的值。|  
|x509FindType|列舉。  要搜尋的其中一個憑證欄位。|  
|storeLocation|列舉。  搜尋兩個系統存放位置中的一個。|  
|storeName|列舉。  搜尋的其中一個系統存放區。|  
  
## findValue 屬性  
  
|值|描述|  
|-------|--------|  
|String|這個值取決於要搜尋的欄位 \(由 X509FindType 屬性指定\)。  例如，如果搜尋指紋，則此值必須是十六進位數字字串。|  
  
## x509FindType 屬性  
  
|值|描述|  
|-------|--------|  
|列舉|值包括：FindByThumbprint、FindBySubjectName、FindBySubjectDistinguishedName、FindByIssuerName、FindByIssuerDistinguishedName、FindBySerialNumber、FindByTimeValid、FindByTimeNotYetValid、FindBySerialNumber、FindByTimeExpired、FindByTemplateName、FindByApplicationPolicy、FindByCertificatePolicy、FindByExtension、FindByKeyUsage、FindBySubjectKeyIdentifier。|  
  
## storeLocation 屬性  
  
|值|描述|  
|-------|--------|  
|列舉|CurrentUser 或 LocalMachine。|  
  
## storeName 屬性  
  
|值|描述|  
|-------|--------|  
|列舉|值包括：AddressBook、AuthRoot、CertificateAuthority、Disallowed、My、Root、TrustedPeople 和 TrustedPublisher。|  
  
### 子項目  
 無。  
  
### 父項目  
  
|項目|描述|  
|--------|--------|  
|[\<serviceCertificate\>](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md)|指定對用戶端驗證服務時所使用的憑證。|  
  
## 備註  
 對於使用以憑證為基礎之訊息安全性的繫結，這個組態項目指定的憑證會用來加密傳送給服務的訊息，而且預期會由服務用來簽署對用戶端的回覆。  它會儲存當服務未指定憑證時所要使用的單一憑證。  
  
## 範例  
 下列範例會為 URI 開頭為 http:\/\/www.contoso.com 的端點指定使用的憑證，並且為不執行憑證交涉的其他端點指定使用的憑證。  
  
```  
<serviceCertificate>  
  <defaultCertificate findValue="www.contoso.com"   
                      storeLocation="LocalMachine"  
                      storeName="TrustedPeople"   
                      x509FindType="FindByIssuerDistinguishedName" />  
  <scopedCertificates>  
     <add targetUri="http://www.contoso.com"   
          findValue="www.contoso.com" storeLocation="LocalMachine"  
                  storeName="Root" x509FindType="FindByIssuerName" />  
  </scopedCertificates>  
  <authentication revocationMode="Online"   
   trustedStoreLocation="LocalMachine" />  
</serviceCertificate>  
```  
  
## 請參閱  
 <xref:System.ServiceModel.Configuration.X509DefaultServiceCertificateElement>   
 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential>   
 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.DefaultCertificate%2A>   
 [使用憑證](../../../../../docs/framework/wcf/feature-details/working-with-certificates.md)   
 [\<authentication\>](../../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md)   
 [確保用戶端的安全](../../../../../docs/framework/wcf/securing-clients.md)   
 [確保服務與用戶端的安全](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)