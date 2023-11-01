---
layout: default
title: Swagger&CORS
parent: Web_Back
nav_order: 17
---

# Swagger&CORS

---

## Swagger

#### Swagger를 이용한 REST API 문서화

- 프로젝트 개발 시 일반적으로 FrontEnd 개발자와 BackEnd 개발자가 분리된다.
- FrontEnd 개발자의 경우 화면에 집중하고 BackEnd 개발자가 만든 문서 API를 보며 데이터를 처리한다.
- 이때 개발 상황의 변화에 따른 API의 추가 또는 변경할 때 마다 문서에 적용하는 불편함이 있다.
- 이 문제를 해결하기 위해 Swagger를 사용한다.

#### Swagger

- 기존 문서로 사용하던 문제를 해결하기 위해 Swagger 사용한다.
- 간단한 설정으로 프로젝트의 API 목록을 웹에서 확인 및 테스트 할수 있게 해주는 라이브러리다.
- Swagger를 사용하면 Controller에 정의되어 있는 모든 URL을 바로 확인할 수 있다.
- API 목록 뿐 아니라 API의 명세 및 설명도 볼 수 있으며 또한 API를 직접 테스트해 볼 수 있다.

## CORS

- 교차 출처 리소스 공유(Cross Origin Resource Sharing)는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 어플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제다.
