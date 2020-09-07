# OAuth-JWT-Bearer-kr

JSON Web Token (JWT) Bearer Token Profiles for OAuth 2.0를 공부하기 위함

# JSON Web Token (JWT) Bearer Token Profiles for OAuth 2.0

## Abstract

이 사양은 OAuth 2.0 액세스 토큰을 요청하고 클라이언트 인증 수단으로 사용하기위한 수단으로 JWT (JSON Web Token) Bearer 토큰의 사용을 정의합니다.

## Status of this Memo

This Internet-Draft is submitted in full conformance with the provisions of BCP 78 and BCP 79.

Internet-Drafts are working documents of the Internet Engineering Task Force (IETF). Note that other groups may also distribute working documents as Internet-Drafts. The list of current Internet- Drafts is at http://datatracker.ietf.org/drafts/current/.

Internet-Drafts are draft documents valid for a maximum of six months and may be updated, replaced, or obsoleted by other documents at any time. It is inappropriate to use Internet-Drafts as reference material or to cite them other than as "work in progress."

This Internet-Draft will expire on October 28, 2012.

## Copyright Notice

Copyright (c) 2012 IETF Trust and the persons identified as the document authors. All rights reserved.

This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents (http://trustee.ietf.org/license-info) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document. Code Components extracted from this document must include Simplified BSD License text as described in Section 4.e of the Trust Legal Provisions and are provided without warranty as

described in the Simplified BSD License.

# Table of Contents

- [1. 소개](#1-소개)
  - [1.1. Notational Conventions](#11-notational-conventions)
  - [1.2. Terminology](#12-terminology)
- [2. Assertions 전송을위한 HTTP 매개 변수 바인딩](#2-assertions-전송을위한-http-매개-변수-바인딩)
  - [2.1. JWT를 권한 부여로 사용](#21-jwt를-권한-부여로-사용)
  - [2.2. 클라이언트 인증에 JWT 사용](#22-클라이언트-인증에-jwt-사용)
- [3. JWT 형식 및 처리 요구 사항](#3-jwt-형식-및-처리-요구-사항)
  - [3.1. 권한 부여 처리](#31-권한-부여-처리)
  - [3.2. 클라이언트 권한 부여 처리](#32-클라이언트-권한-부여-처리)
- [4. 권한 부여 예](#4-권한-부여-예)
- [5. 보안 고려 사항](#5-보안-고려-사항)
- [6. IANA Considerations](#6-iana-considerations)
  - [6.1. Sub-Namespace Registration of](#61-sub-namespace-registration-of)
  - [6.2. Sub-Namespace Registration of](#62-sub-namespace-registration-of)
- [7. References](#7-references)
  - [7.1. Normative References](#71-normative-references)
  - [7.2. Informative References](#72-informative-references)
- [Appendix A. Acknowledgements](#appendix-a-acknowledgements)
- [Appendix B. Document History](#appendix-b-document-history)

<!-- /code_chunk_output -->

# 1. 소개

JWT (JSON Web Token) [JWT]는 보안 도메인간에 identity 및 보안 정보를 공유 할 수 있도록하는 JSON 기반 보안 토큰 인코딩입니다. JWT는 RFC 4627 [RFC4627]에 정의 된 JSON 데이터 구조를 사용합니다. 보안 토큰은 일반적으로 identity 공급자가 발급하고 보안 관련 목적으로 토큰의 주체를 식별하기 위해 콘텐츠에 의존하는 신뢰 당사자가 사용합니다.

OAuth 2.0 인증 프로토콜 [I-D.ietf-oauth-v2]은 액세스 토큰을 사용하여 리소스에 대해 인증된 HTTP 요청을 만드는 방법을 제공합니다. 액세스 토큰은 리소스 소유자의 (때때로 암시 적) 승인을받은 권한 부여 서버에 의해 타사 클라이언트에 발급됩니다. OAuth에서 권한 부여는 리소스 소유자 권한을 나타내는 중간 자격 증명을 설명하는 데 사용되는 추상적인 용어입니다. 클라이언트는 권한 부여를 사용하여 액세스 토큰을 얻습니다. 다양한 클라이언트 유형 및 사용자 경험을 지원하기 위해 여러 권한 부여 유형이 정의됩니다. OAuth는 또한 추가 클라이언트를 지원하거나 OAuth와 다른 신뢰 프레임 워크 사이의 다리를 제공하기 위해 새로운 확장 부여 유형의 정의를 허용합니다. 마지막으로 OAuth는 권한 부여 서버와 상호 작용할 때 클라이언트가 사용할 추가 권한 부여 메커니즘의 정의를 허용합니다.

OAuth 2.0 Assertion Profile [I-D.ietf-oauth-assertions]은 OAuth 2.0의 클라이언트 자격 증명 및/또는 권한 부여로 Assertion (일명 보안 토큰)을 사용하기위한 일반 프레임 워크를 제공하는 OAuth 2.0의 추상 확장입니다. 이 사양은 OAuth 2.0 Assertion Profile [I-D.ietf-oauth-assertions]에 JWT (JSON Web Token) Bearer 토큰을 사용하여 OAuth 2.0 액세스 토큰을 요청하고 클라이언트 자격 증명으로 사용하는 확장 권한 부여 유형을 정의합니다. 이 사양에 정의된 JWT의 형식 및 처리 규칙은 OAuth 2.0 용 SAML 2.0 Bearer Assertion Profiles [I-D.ietf-oauth-saml2-bearer]와 밀접하게 관련된 SAML 2.0 Bearer Assertion Profiles와 동일하지는 않지만 밀접하게 관련되어 있습니다.

이 문서는 클라이언트가 JWT(및 계산된 디지털 서명)을 통해 표현 된 기존 신뢰 관계를 사용하고자 할 때 JWT (JSON Web Token) Bearer 토큰을 사용하여 액세스 토큰을 요청하는 방법을 정의합니다. 권한 부여 서버에서 사용자 승인 단계. 또한 JWT를 클라이언트 인증 메커니즘으로 사용할 수 있는 방법도 정의합니다. 클라이언트 인증을 위한 보안 토큰의 사용은 독립적이며 보안 토큰을 권한 부여로 사용하는 것과 분리 할 수 ​​있으며 두 가지를 조합하여 또는 분리하여 사용할 수 있습니다.

클라이언트가 JWT를 권한 서버와 교환하거나 클라이언트 인증에 사용하기 전에 클라이언트가 JWT를 얻는 프로세스는 범위를 벗어납니다.

## 1.1. Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [RFC2119].

Unless otherwise noted, all the protocol parameter names and values are case sensitive.

## 1.2. Terminology

All terms are as defined in The OAuth 2.0 Authorization Protocol [I-D.ietf-oauth-v2], OAuth 2.0 Assertion Profile [I-D.ietf-oauth-assertions], and JSON Web Token (JWT) [JWT].

# 2. Assertions 전송을위한 HTTP 매개 변수 바인딩

OAuth 2.0 Assertion Profile [I-D.ietf-oauth-assertions]은 토큰 endpoint와 상호 작용하는 동안 Assertion (일명 보안 토큰)을 전송하기위한 일반 HTTP 매개 변수를 정의합니다. 이 섹션에서는 JWT Bearer 토큰과 함께 사용할 매개 변수 값을 정의합니다.

## 2.1. JWT를 권한 부여로 사용

JWT Bearer 토큰을 권한 부여로 사용하려면 다음 매개 변수 값 및 인코딩을 사용하십시오.

"grant_type"매개 변수의 값은 "urn:ietf:params:oauth:grant-type:jwt-bearer"여야합니다.

"assertion"매개 변수의 값은 단일 JWT를 포함해야합니다.

## 2.2. 클라이언트 인증에 JWT 사용

클라이언트 인증 부여에 JWT Bearer 토큰을 사용하려면 다음 매개 변수 값 및 인코딩을 사용하십시오.

"client_assertion_type"매개 변수의 값은 "urn:ietf:params:oauth:client-assertion-type:jwt-bearer"여야합니다.

"client_assertion"매개 변수의 값은 단일 JWT를 포함해야합니다.

# 3. JWT 형식 및 처리 요구 사항

OAuth 2.0 권한 부여 프로토콜 [I-D.ietf-oauth-v2]에 설명된대로 액세스 토큰 응답을 발행하거나 클라이언트 인증을 위해 JWT에 의존하기 위해 인증 서버는 아래 기준에 따라 JWT를 검증해야합니다. 추가 제한 및 정책의 적용은 권한 부여 서버의 재량입니다.

o JWT는 JWT를 발행 한 엔티티에 대한 고유 식별자를 포함하는 "iss"(발급자) claim을 포함해야합니다.

o JWT는 거래의 주체를 식별하는 "prn"(principal) claim을 포함해야합니다. principal은 액세스 토큰이 요청되는 리소스 소유자를 식별 할 수 있습니다. 클라이언트 인증의 경우 principal는 OAuth 클라이언트의 "client_id"여야합니다. JWT를 권한 부여로 사용할 때 principal는 액세스 토큰이 요청되는 권한이 있는 접근자를 식별해야합니다 (일반적으로 리소스 소유자 또는 권한이있는 대리인).

o JWT는 권한 부여 서버 또는 제어 도메인의 서비스 제공자 principal를 식별하는 URI 참조를 포함하는 "aud"(audience) claim을 포함해야합니다. 권한 부여 서버의 토큰 endpoint URL은 "aud"요소에 대해 허용되는 값으로 사용될 수 있습니다. 권한 부여 서버는 JWT를 대상으로하는 audience인지 확인해야합니다.

o JWT는 JWT를 사용할 수 있는 기간을 제한하는 "exp"(expiration) claim을 포함해야합니다. 권한 부여 서버는 시스템간 허용 가능한 clock skew에 따라 만료 시간이 지나지 않았는지 확인해야합니다. 권한 부여 서버는 "exp"claim 값이 불합리하게 먼 미래에있는 JWT를 거부 할 수 있습니다.

o JWT는 토큰이 처리를 위해 수락되어서는 안되는 시간을 식별하는 "nbf"(not before) claim을 포함 할 수 있습니다.

o JWT는 JWT가 발행된 시간을 식별하는 "iat"(issued at) claim을 포함 할 수 있습니다. 권한 부여 서버는 과거에 불합리하게 먼 "iat"claim 값을 가진 JWT를 거부 할 수 있습니다.

o JWT는 토큰에 대한 고유 식별자를 제공하는 "jti"(JWT ID) claim을 포함 할 수 있습니다. 권한 부여 서버는 적용 가능한 "exp"순간을 기반으로 JWT가 유효한 것으로 간주되는 시간 길이 동안 사용된 "jti"값 세트를 유지함으로써 JWT가 재생되지 않도록 할 수 있습니다.

o JWT에는 다른 claim이 포함될 수 있습니다.

o JWT는 발급자가 디지털 서명해야하며 권한 부여 서버는 서명을 확인해야합니다.

o 권한 부여 서버는 JWT (JSON Web Token) [JWT]에 따라 다른 모든 측면에서 유효한지 확인해야합니다.

## 3.1. 권한 부여 처리

존재하는 경우 권한 부여 서버는 클라이언트 자격 증명도 확인해야합니다.

JWT가 유효하지 않거나 현재 시간이 토큰의 유효한 시간 window 내에 있지 않은 경우 인증 서버는 OAuth 2.0 [I-D.ietf-oauth-v2]에 정의 된대로 오류 응답을 구성해야합니다. "error"매개 변수의 값은 "invalid_grant"오류 코드여야 합니다. 권한 부여 서버는 "error_description"또는 "error_uri"매개 변수를 사용하여 JWT가 유효하지 않은 것으로 간주 된 이유에 대한 추가 정보를 포함 할 수 있습니다.

    For example:
    HTTP/1.1 400 Bad Request
    Content-Type: application/json
    Cache-Control: no-store

    {
      "error":"invalid_grant",
      "error_description":"Audience validation failed"
    }

## 3.2. 클라이언트 권한 부여 처리

클라이언트 JWT가 유효하지 않거나 주체 확인 요구 사항을 충족 할 수 없는 경우 권한 부여 서버는 OAuth 2.0 [I-D.ietf-oauth-v2]에 정의 된대로 오류 응답을 구성해야합니다. "error"매개 변수의 값은 "invalid_client"오류 코드 여야합니다. 권한 부여 서버는 "error_description"또는 "error_uri"매개 변수를 사용하여 JWT가 유효하지 않은 것으로 간주 된 이유에 대한 추가 정보를 포함 할 수 있습니다.

# 4. 권한 부여 예

비 규범적이지만 다음 예제는 준수하는 JWT 및 액세스 토큰 요청이 어떻게 생겼는지 보여줍니다.

다음은 JWT에 대한 JWT 클레임 개체를 생성하기 위해 인코딩 할 수있는 JSON 개체의 예입니다.

    {"iss":"https://jwt-idp.example.com",
      "prn":"mailto:mike@example.com",
      "aud":"https://jwt-rp.example.net",
      "nbf":1300815780,
      "exp":1300819380,
      "http://claims.example.com/member":true}

JWT의 헤더로 사용되는 다음 예제 JSON 객체는 JWT가 ECDSA P-256 SHA-256 알고리즘으로 서명되었음을 선언합니다.

     {"alg":"ES256"}

예를 들어, 액세스 토큰 요청의 일부로 이전 예제에 표시된 클레임 및 헤더와 함께 JWT를 제공하기 위해 클라이언트는 다음 HTTPS 요청을 수행 할 수 있습니다 (표시 목적으로 만 줄 바꿈 포함):

    POST /token.oauth2 HTTP/1.1
    Host: authz.example.net
    Content-Type: application/x-www-form-urlencoded

    grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
    &assertion=eyJhbGciOiJFUzI1NiJ9.
    eyJpc3Mi[...omitted for brevity...].
    J9l-ZhwP_2n[...omitted for brevity...]

# 5. 보안 고려 사항

OAuth 2.0 권한 부여 프로토콜 [ID.ietf-oauth-v2], OAuth 2.0 Assertion Profile [ID.ietf-oauth-assertions] 및 JSON Web Token (JWT) [JWT] 사양에 설명 된 것 외에 추가 보안 고려 사항은 적용되지 않습니다.

# 6. IANA Considerations

## 6.1. Sub-Namespace Registration of

      urn:ietf:params:oauth:grant-type:jwt-bearer

This is a request to IANA to please register the value "grant-type:jwt-bearer" in the registry urn:ietf:params:oauth established in An IETF URN Sub-Namespace for OAuth [I-D.ietf-oauth-urn-sub-ns].

o URN: urn:ietf:params:oauth:grant-type:jwt-bearer

o Common Name: JWT Bearer Token Grant Type Profile for OAuth 2.0

o Change controller: IETF

o Description: [[this document]]

## 6.2. Sub-Namespace Registration of

      urn:ietf:params:oauth:client-assertion-type:jwt-bearer

This is a request to IANA to please register the value "client-assertion-type:jwt-bearer" in the registry urn:ietf:params:oauth established in An IETF URN Sub-Namespace for OAuth [I-D.ietf-oauth-urn-sub-ns].

o URN: urn:ietf:params:oauth:client-assertion-type:jwt-bearer

o Common Name: JWT Bearer Token Profile for OAuth 2.0 Client
Authentication

o Change controller: IETF

o Description: [[this document]]

# 7. References

## 7.1. Normative References

[I-D.ietf-oauth-assertions] Mortimore, C., Ed., Jones, M., Campbell, B., and Y. Goland, "OAuth 2.0 Assertion Profile", draft-ietf-oauth-assertions-02 (work in progress), April 2012.

[I-D.ietf-oauth-urn-sub-ns] Tschofenig, H., "An IETF URN Sub-Namespace for OAuth", draft-ietf-oauth-urn-sub-ns-02 (work in progress), January 2012.

[I-D.ietf-oauth-v2] Hammer-Lahav, E., Recordon, D., and D. Hardt, "The OAuth 2.0 Authorization Protocol", draft-ietf-oauth-v2-25 (work in progress), March 2012.

[JWT] Jones, M., Balfanz, D., Bradley, J., Goland, Y., Panzer, J., Sakimura, N., and P. Tarjan, "JSON Web Token (JWT)", March 2012.

[RFC2119] Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

[RFC4627] Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.

## 7.2. Informative References

[I-D.ietf-oauth-saml2-bearer] Campbell, B., Ed. and C. Mortimore, "SAML 2.0 Bearer Assertion Profiles for OAuth 2.0", draft-ietf-oauth-saml2-bearer-11 (work in progress), April 2012.

# Appendix A. Acknowledgements

This profile was derived from SAML 2.0 Bearer Assertion Profiles for OAuth 2.0 [I-D.ietf-oauth-saml2-bearer] by Brian Campbell and Chuck Mortimore.

# Appendix B. Document History

[[ to be removed by RFC editor before publication as an RFC ]]

-04

o Merged in changes between draft-ietf-oauth-saml2-bearer-09 and draft-ietf-oauth-saml2-bearer-11.

o Added the optional "iat" (issued at) claim, which was already present in the JWT spec.

-03

o Added the "jti" (JWT ID) claim to enable replay protection.

o Respect line length restrictions in examples.

-02

o Removed remaining vestiges of normative text talking about SAML that remained from the SAML Profile draft.

o Replaced all references where the reference is used as if it were part of the sentence (such as "defined by [I-D.whatever]") with ones where the specification name is used, followed by the reference (such as "defined by Whatever [I-D.whatever]").

-01

o Merged in changes from draft-ietf-oauth-saml2-bearer-09. In particular, this draft now uses draft-ietf-oauth-assertions, rather than being standalone. It also now defines how to use JWT bearer tokens both for Authorization Grants and for Client Authentication.

-00

o Initial draft.

Authors' Addresses

Michael B. Jones  
Microsoft

Email: mbj@microsoft.com  
URI: http://self-issued.info/

Brian Campbell  
Ping Identity Corp.

Email: brian.d.campbell@gmail.com

Chuck Mortimore  
Salesforce.com

Email: cmortimore@salesforce.com
