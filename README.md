# 재주 상회 clone

[프론트엔드 Github](https://github.com/AnSuebin/FE-CloneCode)

# 개발 기술
![JAVA](https://img.shields.io/badge/JAVA_17-blue?style=flat&logo=OpenJDK&logoColor=000000)
![Gradle](https://img.shields.io/badge/Gradle_7.6-02303A.svg?style=Plastic&logo=Gradle&logoColor=white)

![spring_Boot](https://img.shields.io/badge/spring_Boot_2.7.7-%236DB33F.svg?style=Plastic&logo=SpringBoot&logoColor=white)
![Spring_Web](https://img.shields.io/badge/Spring_Web-%236DB33F.svg?style=Plastic&logo=spring&logoColor=white)
![Spring_Data_Jpa](https://img.shields.io/badge/Spring_Data_Jpa-%236DB33F.svg?style=Plastic&logo=spring&logoColor=white)
![spring_Security](https://img.shields.io/badge/spring_Security-%236DB33F.svg?style=Plastic&logo=springsecurity&logoColor=white)

![Python](https://img.shields.io/badge/-python%203.8-%233776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/-Flask-%23000000?logo=flask&logoColor=white)

![MySQL](https://img.shields.io/badge/MySQL%208-%234479A1?logo=mysql&logoColor=white)

![JWT](https://img.shields.io/badge/JWT-black?style=Plastic&logo=JSON%20web%20tokens)

## 인프라
![AWS](https://img.shields.io/badge/EC2,RDS-%23FF9900.svg?style=Plastic&logo=amazon-aws&logoColor=white)
![GitHub_Actions](https://img.shields.io/badge/GitHub_Actions-blue.svg?style=Plastic&logo=GitHubActions&logoColor=white)

## 협업
![git](https://img.shields.io/badge/git-F05032?style=flat&logo=Git&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=Plastic&logo=github&logoColor=white)
![Slack](https://img.shields.io/badge/Slack-4A154B?style=Plastic&logo=slack&logoColor=white)
![Notion](https://img.shields.io/badge/Notion-000000?style=Plastic&logo=Notion&logoColor=white)


# 1. 팀 소개
- [이한주](https://github.com/yanJuicy)
- [김민호](https://github.com/minokim1080)
- [이진홍](https://github.com/sooni2)
- [황보석](https://github.com/seok6086)

# 2. 프로젝트 구조
## 백엔드 구조
![image](https://user-images.githubusercontent.com/43159295/209940959-39f92181-cb07-427f-b9e5-0e0cd211cb19.png)


## CI CD 구조
![image](https://user-images.githubusercontent.com/43159295/209932854-ff7fe326-884a-4fe0-a68d-46abc2847a42.png)


# 3. API 기능 명세서
[API 명세서 확인](https://caring-pin-8ac.notion.site/API-114dcca716cf81799098c91328c78510)

# 4. 개발 환경 설정
다음과 같이 설정 파일들을 작성
- `application-dev.properties` : RDS 접속 정보
- `applicatino-social.properties` : oauth 젒속 정보
- `security.properties` : jwt 정보

![image](https://user-images.githubusercontent.com/43159295/209933083-70c24436-7fd7-441c-8cda-d870f200ccb3.png)

# 5. 성능 개선

### 쿼리 줄이기
상품을 주문할 때 장바구니에 있던 상품을 주문하면 장바구니 목록에서 삭제하는 쿼리를 날렸다.

하지만 **기존 방식은 상품 하나당 하나의 쿼리**를 날려서 삭제하려는 상품의 수가 많아질수록 처리 속도가 급격하게 느려졌다.

그래서 **쿼리를 직접 작성하여 삭제 쿼리를 한번에 날릴 수 있도록 수정하였다**.

그 후 두 로직의 성능 차이를 위 AOP 메소드로 비교하였다.

테스트는 100개의 상품 데이터로 진행하였다.
![기존의 상품 구매 로직. for 문 안에서 상품 id 하나 하나에 개별적으로 삭제 쿼리를 날린다.](https://user-images.githubusercontent.com/43159295/209958696-be283c62-9e55-4b8f-a964-95c4089cc148.png)

기존의 상품 구매 로직. for 문 안에서 상품 id 하나 하나에 개별적으로 삭제 쿼리를 날린다.

![[100개 상품에 대한 기존 로직의 실행 시간. **8500ms 소요**](https://user-images.githubusercontent.com/43159295/209958806-4bd89d9c-7569-4469-9ab0-a25bc2bb68a1.png)

100개 상품에 대한 기존 로직의 실행 시간. **8500ms 소요**

이후 바꾼 로직과 걸린 시간은 다음과 같다.

![Repository에 직접 작성한 쿼리를 적용하는 메소드 추가](https://user-images.githubusercontent.com/43159295/209958856-b481abcb-92f4-47ca-b01e-b2067115d3bc.png)

Repository에 직접 작성한 쿼리를 적용하는 메소드 추가

![위의 메소드를 사용한 상품 주문 로직. ](https://user-images.githubusercontent.com/43159295/209958896-0e06418a-dd2c-4917-8749-3edac82e98af.png)

위의 메소드를 사용한 상품 주문 로직.

![100개 상품에 대한 신규 로직의 실행 시간. 4847ms 소요](https://user-images.githubusercontent.com/43159295/209958935-d3b49e2c-53c2-483e-a9a9-d316abe9565a.png)

100개 상품에 대한 신규 로직의 실행 시간. 4847ms 소요

약 40% API 속도 향상

