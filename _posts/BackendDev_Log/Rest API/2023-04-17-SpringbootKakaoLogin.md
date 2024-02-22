---
layout: post
related_posts:
  - /backdev-log/restapi/restapi-4/
title: 카카오 로그인 API
categories: 
  - backdev-log
  - Api
---
### 카카오 로그인 API 환경 및 문서 참고
<https://developers.kakao.com/>

**_callback 경로를 구현해준다._**

<img width="1500" alt="카카오로그린" src="https://user-images.githubusercontent.com/107549149/232360806-bc8e7876-df7b-4bb8-a2d7-6b2ca4796e6d.png">

#### LoginPage

* 카카오 로그인 버튼 

```jsp
<a href="https://kauth.kakao.com/oauth/authorize?client_id=77ccaa86becdc3e0d53eb721fe3db52b
		&redirect_uri=http://localhost:8000/auth/kakao/callback&response_type=code">
		<img height="38px;" src="/image/kakao_login_button.png" /></a>
```
#### UserController 클래스에 함수를 구현한다. 

```java
	@GetMapping("/auth/kakao/callback")
		public  String kakaoCallback(String code) { //Data를 리턴해주는 컨트롤 함수
return"카카오 인증:"+code;
    }
```

**_실행결과_**

<img width="2000" alt="카카오 인증" src="https://user-images.githubusercontent.com/107549149/232360215-6b0ae9d6-a8b2-41d0-b805-219b358af021.png">


**_카카오 로그인을 완료하기 위해서는 토큰을 받아야 하는데,그 방법은 다음과 같습니다._**

![image](https://user-images.githubusercontent.com/107549149/232361132-75abd2b6-7162-4b16-a8cb-bd4dec6c8137.png)

**_위에있는 규칙에 따라 데이터를 요청합니다._**


### HttpHeader 오브젝트 생성 

**_RestTemplate이란 스프링 3.0부터 지원하는 http 통신을 위한 템플릿으로 Rest API 호출 이후 응답을 받을 때까지 기다리는 동기 방식의 템플릿입니다._**

```java
	// HttpHeader 오브젝트 생성
			RestTemplate rt=new RestTemplate();
```
**_HttpHeaders 객체를 통해 요청하는 데이터의 타입을 지정_**

```java
HttpHeaders headers = new HttpHeaders();
			headers.add("Content-Type","application/x-www-form-urlencoded;charset=utf-8" );
```
**_데이터 요청에 필요한 정보들을 하나의 객체에 담아 줍니다. 여기서 들어간 정보들은 위에서 언급된 규칙들에 의해 설정합니다._**

```java
//HttpBody 오브젝트 생성 
			MultiValueMap<String, String>params = new LinkedMultiValueMap<>();
			params.add("grant_type", "authorization_code");
			params.add("client_id", "77ccaa86becdc3e0d53eb721fe3db52b");
			params.add("redirect_uri","http://localhost:8000/auth/kakao/callback");
			params.add("code", code);
```

**_HttpEntiy는 HttpHeader 와 HttpBody를 하나의 오브젝트에 담기_**

```java
HttpEntity<MultiValueMap<String, String>>kakaoTokenRequest=
					new HttpEntity<>(params,headers);
```
**_해당 객체를 통해 카카오쪽으로 _Http요청을 보내고 - post방식으로  response 변수의 응답 받는다._**


```java

			ResponseEntity<String>response=rt.exchange(
					"https://kauth.kakao.com/oauth/token",
					HttpMethod.POST,
					kakaoTokenRequest,
					String.class
					);
```
<br>

**_반환된 객체를 페이지를 통해 출력시키고 그 결과를 Json Parser를 통해 확인_**

<http://json.parser.online.fr/>

<img width="1500" alt="토큰확인" src="https://user-images.githubusercontent.com/107549149/232362442-283faed5-1cf5-4a55-9e79-fd5d026e8039.png">

**access_token을 받아왔으며 해당 토큰을 통해서 카카오쪽 서버에 있는 회원 정보에 접근할 수 있는 권한을 얻게되었다.**

**_사용자 프로필을 받아서 자동 로그인 및 회원가입하는 기능을 구현해보도록 하겠습니다._**

<img width="641" alt="토큰 요청" src="https://user-images.githubusercontent.com/107549149/232364071-51e31aa4-ea4b-4607-b44d-e8ef19b97abe.png">


### OAuthToken 

* 우선 받아온 토큰 정보를 객체로 저장하기 위해 토큰 클래스를 생성해줍니다.

```java
package com.cos.blog.model;

import lombok.Data;

@Data
public class OAuthToken {

	private String access_token;
	private String token_type;
	private String refresh_token;
	private String id_token;
	private int expires_in;
	private String scope;
	private int refresh_token_expires_in;
	
}

```
**_위 클래스를 통해 토큰 정보를 객체로 저장합니다._**

```java
ObjectMapper objectMapper = new ObjectMapper();
			OAuthToken oauthToken=null;
			
			try {
			 oauthToken = objectMapper.readValue(response.getBody(), OAuthToken.class);
			} catch (JsonMappingException e) {
				
				e.printStackTrace();
			} catch (JsonProcessingException e) {
				
				e.printStackTrace();
			}
			
			System.out.println("카카오엑세스 토큰"+oauthToken.getAccess_token());
```
**_사용자 프로필을 요청한다._**

```java
// 객체로 저장한 토큰 정보(access_token)을 통해 사용자 프로필을 요청한다. 
			RestTemplate rt2 = new RestTemplate();
			
			// HttpHeader 오브젝트 생성
			HttpHeaders headers2 = new HttpHeaders();
			headers2.add("Authorization", "Bearer "+oauthToken.getAccess_token());
			headers2.add("Content-type", "application/x-www-form-urlencoded;charset=utf-8");
			
			// HttpHeader와 HttpBody를 하나의 오브젝트에 담기
			HttpEntity<MultiValueMap<String, String>> kakaoProfileRequest2 = 
					new HttpEntity<>(headers2);
			
			// Http 요청하기 - Post방식으로 - 그리고 response 변수의 응답 받음.
			ResponseEntity<String> response2 = rt2.exchange(
					"https://kapi.kakao.com/v2/user/me",
					HttpMethod.POST,
					kakaoProfileRequest2,
					String.class
			);
```
**_사용자 프로필 정보를 객체로 저장하기 위해 아래와 같은 클래스를 생성_**

* <https://www.jsonschema2pojo.org/> 이용하여 손쉽게 클래스를 만들수 있다.
* 콘솔에 나온 정보를 드레그 하여 붙여넣는다. 

<img width="763" alt="ss" src="https://user-images.githubusercontent.com/107549149/232365364-a0df0273-21a9-4718-a910-7c7fe4ffe904.png">

```java
package com.cos.blog.model;

import lombok.Data;

@Data
public class KakaoProfile {
	public Long id;
	public String connected_at;
	public Properties properties;
	public KakaoAccount kakao_account;

	@Data
	public class Properties {
		public String nickname;
		public String profile_image;
		public String thumbnail_image;
	}

	@Data
	public class KakaoAccount {
		public Boolean profile_nickname_needs_agreement;
		public Profile profile;
		public Boolean has_email;
		public Boolean profile_image_needs_agreement;
		public Boolean is_email_valid;
		public Boolean is_email_verified;
		public Boolean email_needs_agreement;
		public String email;

		@Data
		public class Profile {
			public String nickname;
			public String thumbnail_image_url;
			public String profile_image_url;
			public String is_default_image;
		}
	}
}
```
### 사용자 객체를 저장한다.

```java

ObjectMapper objectMapper2 = new ObjectMapper();
			KakaoProfile kakaoProfile=null;
			
			try {
				kakaoProfile = objectMapper2.readValue(response2.getBody(), KakaoProfile.class);
			} catch (JsonMappingException e) {
				
				e.printStackTrace();
			} catch (JsonProcessingException e) {
				
				e.printStackTrace();
			}
				
			//User 오브젝트 : username, password,email
			System.out.println("카카오아이디번호:"+kakaoProfile.getId());
			System.out.println("카카오이메일:"+kakaoProfile.getKakao_account().getEmail());
			
			System.out.println("블로그서버 유저네임:"+kakaoProfile.getKakao_account().getEmail()+"_"+kakaoProfile.getId());
			System.out.println("블로그서버 이메일:"+kakaoProfile.getKakao_account().getEmail());
			
			// UUID 중복되지 않는 어떤 특정 값을 만들떄 사용한다. 
			//UUID garbagePassword=UUID.randomUUID();
			System.out.println("블로그 서버 패스워드:"+cosKey);
			
			User kakaoUser = User.builder()
					.username(kakaoProfile.getKakao_account().getEmail()+"_"+kakaoProfile.getId())
					.email(kakaoProfile.getKakao_account().getEmail())
					.oauth("kakao")
					.password(cosKey)
					.build();
```

<img width="366" alt="카카오 유저 정보2" src="https://user-images.githubusercontent.com/107549149/232369138-49338c0d-1812-47bd-90ab-3d0cb9353e54.png">



  * 여기서 cosKey란 카카오 로그인을 통해 로그인한 회원들에게 공통적으로 부여되는 
    비밀번호로써 모두가 공유하는 비밀번호이기 때문에 해당 비밀번호는 
    절대 외부에 알려져서는 안되며 이러한 비밀번호는 아래와 같이 설정할 수 있습니다.
			
**application.yml 파일에 다음과 같이 코드를 추가해줍니다.**

<img width="400" alt="s" src="https://user-images.githubusercontent.com/107549149/232366577-586731a4-86a7-4a18-9f1b-ee0c272dc37e.png">

### UserController 클래스에 다음과 같은 필드를 추가해줍니다.

<img width="400" alt="a" src="https://user-images.githubusercontent.com/107549149/232366717-bfdad3e0-572e-4236-8ffc-17a8e5850a9e.png">

* 카카오 로그인을 수행할 때 위의 비밀번호를 통해 자동 로그인을 수행될 것입니다.  
  따라서 해당 비밀번호는 사용자에 의해 변경될 수 없으므로 기존의 코드를 조금 수정합니다.

### User클래스 에 oauth 필드를 추가한다. 
* 해당 유저가 어떤 방식으로 로그인하였는지를 확인하기 위해서 필드를 추가합니다.

<img width="278" alt="as" src="https://user-images.githubusercontent.com/107549149/232367490-0f1522cf-3c64-49f6-b65b-fdc97d8136e4.png">

### updateForm.jsp 을 수정합니다

**_일반 로그인일 경우 null 값이, 카카오 로그인일 경우 "kakao"가 들어갈 것입니다._**

<img width="891" alt="d" src="https://user-images.githubusercontent.com/107549149/232367765-a6190296-8288-4c84-999e-6afe58128e8b.png">


<br>

* 위 코드의 의미는 해당 유저의 로그인 방식이 OAuth 방식이라면   
비밀번호와 이메일을 변경할 수 없도록 각각을 숨기거나 readonly 속성을 사용하여     
수정하지 못하도록 설정한 것입니다.

**_해당 회원이 기존 회원인지 아닌지를 판별하여 자동 회원가입 후 로그인을 수행하도록 구현해줄 것인데,이를 위해 먼저 UserService 클래스에 회원을 찾아주는 새로운 함수를 구현해주었습니다._**

```java
// 회원찾기
	@Transactional(readOnly = true)
	public User 회원찾기(String username) {
		User user = userRepository.findByUsername(username).orElseGet(()->{
			return new User();
		});
		return user;
	}
```
* 위 함수는 파라미터로 username을 받아 해당되는 회원이 존재하는지 확인한 후, 존재한다면 해당 회원을 반환할 것이고 존재하지 않는다면 빈 회원 객체를 반환할 것입니다.

* 그 후 UserController 클래스에서는 위 함수를 사용하여 회원인지 비회원인지를 판별한 뒤 그 결과에 따라 작업을 수행하도록 구현해줍니다.

```java
// 가입자 혹은 비가입자 체크 해서 처리
			User originUser = userService.회원찾기(kakaoUser.getUsername());
            System.out.println("가입자 찾기 Controller  확인");
			
			if(originUser.getUsername() == null) {
				System.out.println("기존 회원이 아니기에 자동 회원가입을 진행합니다");
				userService.회원가입(kakaoUser);
			}
			System.out.println("자동 로그인을 진행합니다.");
			
			// 로그인 처리
			Authentication authentication = authenticationManager.authenticate(new UsernamePasswordAuthenticationToken(kakaoUser.getUsername(), cosKey));
			SecurityContextHolder.getContext().setAuthentication(authentication);
			
			return "redirect:/";
		}

```
<img width="610" alt="제목 없음" src="https://user-images.githubusercontent.com/107549149/232369356-61062436-898c-4390-b645-465515055e10.png">

* 추가로 반환 값을 통해 redirect 처리해주었으므로 해당 함수의 반환 타입에 붙어있던 @RequestBody 어노테이션을 삭제해주었습니다.


