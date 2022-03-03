---
layout: post
title: 웹 보안에 대하여 2 -  JWT 와 구현 방법
date:  2022-03-03 17:00
img: websecurity.png # Add image post (optional)
categories: [study]
tags: [IT,  web, git] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

팀플하면서 JWT 공부하고 구현해본 내용 올려야겠다.  

# JWT (Json Web Token) 
- Json 객체를 통해서 관리되는 토큰 방식. 헤더, 페이로드, 서명으로 구성되어 있다.
- http header 에 담아서 서로 통신.
- 클라이언트에 저장하기에  서버에서 클라이언트의 토큰 조작할 수는 없다.  
                
 *헤더* : 토큰의 타입과  해싱 알고리즘 정보 들어있다. 나중에  토큰 검증할때 서명 부분에 사용된다. 

*페이로드* : 토큰에 담을 정보를 말하며, 이 때 정보 하나씩을 클레임 이라고 한다.  
  - 공개 클레임: 충돌을 방지하기 위한 정보로 URI 형식으로 지음
- 등록된 클레임:  토큰에 대한 정보를 담기 위해 이미 정해진 정보들.     
	Ex) 토큰 발급자,  만료 시간 등 . 고정 정보
	- 공개 클레임: 충돌을 방지하기 위한 정보로 URI 형식으로 지음
	- 비공개 클레임:  클라이언트- 서버 간 협의를 위해 만든 클레임.  (유저 이름 이런거) 

*서명* : 헤더의 인코딩 값과 페이로드의 인코딩 값을 다시 해시해서 만든다. 

이렇게 만든 데이터를 .  으로 조합해 다시 base64 인코딩 시켜서 보낸다. 

# JJWT :  
   자바에서 JWT 토큰 구현하는 라이브러리 중 하나 (https://github.com/jwtk/jjwt#jws).   
   JWT 처리를 위한 builder, parser  제공한다. Dependency 추가해서 사용 시작함. 

- 헤더: 토큰의 타입, 해시 알고리즘 방식 세팅할 수 있고,   
                세팅 안하면 토큰 타입은 JWT,  해시 알고리즘은 sha256으로 세팅된다. (위의 예시)     

-  페이로드 : 기본 정보인 발급자,  발급시간, 만료시간 등은 set  함수로 개별 구현되어있다.  
                        커스텀  내용들은 map<String, String >  으로 만든 후 setClaims ()로  넣는다. 
-  서명 : signWith() 메소드에 각자 secret key 로 사용할 키를 넣는다.    
                  기본 알고리즘은 sha256 이며 , .signWith(key,SignatureAlgorithm.HS512)  같이 알고리즘 변경 가능하다.   

# 소스 구현 
전체 소스는 [여기](https://github.com/yejinha/jwtStudy) 에서 볼 수 있다. 
구현시에는 jjwt 라이브러리 레포에서 보고 따라하면 사실 너무 쉽게 완성되긴하는데,  버전마다 달라지는 내용이 많아서 제대로 확인후에 구현해야한다. 
나는 버전 0.10.7 사용하였다.  

~~~java
//1. 서명에 사용할 키 생성 (실개발때는 프로퍼티로 관리)
    private String keyString = "Mauristinciduntpurustortoretfusceaaaabbbbeeeccc";  //랜덤 문장 생성기에서 랜덤 생성

    public String createToken(String userName, String role) {
        //2. 만료시간 설정
        Date now = new Date();
        Date expiration = new Date(now.getTime() + Duration.ofDays(1).toMillis()); // 만료기간 1일

        //3. JWT 생성
        Claims claims = Jwts.claims().setSubject(userName); // JWT payload 에 저장되는 정보단위
        claims.put("roles", role); // role 받아서 권한 설정해준다. 나중에는 이 권한 파싱해서 권한제어에 사용 . (admin -> 관리자 권한, user -> 일반사용자)

        String jws = Jwts.builder().setSubject(userName)  //토큰 발급 받은 사람 특정
                .setIssuer("testProduct")  //발급자
                .setIssuedAt(now).setExpiration(expiration) //발급시간,  만료시간
                .claim("roles",role)
                .setSubject(userName)
                .signWith(SignatureAlgorithm.HS256,keyString)
                .compact();

        return jws;

    }

    public Jws<Claims> isValid(String jws){
        //userName 뽑아내어 리턴  -> 추후에는 롤이나 다른 중요 값 뽑아서 검증해도 됨.
        String userName =null;
        Jws<Claims> parsedJwt =null;
        try {
            parsedJwt = Jwts.parser().setSigningKey(keyString).parseClaimsJws(jws);
        }catch (SignatureException e){ // 서명 인증 안되는 경우
            throw new SignatureException("Invalid token");
        }catch (ExpiredJwtException e){ //만료된 경우
            throw new ExpiredJwtException(parsedJwt.getHeader(),parsedJwt.getBody(),"Expired!");
        }
        return parsedJwt;
    }

    // jwt 파싱한 내용 확인
    public String getInfoByToken(Jws<Claims> parsedJwt) throws Exception {
        String jwtBody  =parsedJwt.getBody().toString();
        if (jwtBody == null){
            throw new Exception("jwtBody is empty");
        }
        return jwtBody;
    }

~~~

[1편](https://yejinha.github.io/posts/web-security-01/)에서 말했듯이 토큰은 헤더에 담아서 전달되기에 아래처럼 클라이언트- 서버간 전달할 수 있다. 

~~~java
//토큰 생성해 클라이언트에 보낼때 
response.setHeader("X-AUTH-TOKEN", token);  // response header 세팅

Cookie cookie = new Cookie("accesstoken", token); // 쿠키에 저장해 다음 요청때 쓸 수 있도록 함.
cookie.setPath("/");
cookie.setHttpOnly(true);
cookie.setSecure(true);
response.addCookie(cookie);
~~~

~~~java
// 클라이언트에서 토큰 받았을때
public ResponseEntity<?> loginActionWithToken( @RequestHeader Map<String, String> data) {
        String token = null;

        //header 에  accesstoken이란 값이 있다면 파싱
        if(data.get("accesstoken") !=null) {
            token =jwtcon.validToken(data.get("accesstoken"));
        }

        return new ResponseEntity<>(token, HttpStatus.OK);
    }
~~~