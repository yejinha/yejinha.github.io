---
layout: post
title: Spring Email Sender
date:  2019-08-15 12:36:00 +0800
img: email.jpg # Add image post (optional)
categories: [study]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

# Spring Email Sender
스프링에서 제공하는 MailSender 로 쉽게 메일 전송하는 방법
[https://www.baeldung.com/spring-email](https://www.baeldung.com/spring-email)


**1) 필요한 library 받기** 
pom.xml 에 dependency 설정으로 받아도 되고, jar 파일 다운 받아서 톰캣 경로/lib 에 넣어줘도 된다. 

    <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>5.0.1.RELEASE</version>
    </dependency>
    
   jar 파일 받는 경로 -> [클릭](https://search.maven.org/classic/#search%7Cga%7C1%7Cspring-context-support) 

lib 내 인터페이스 정리
1.  **_MailSender_  interface**:  제일 상위 인터페이스 
2.  **_JavaMailSender_  interface**: MIME 메세지를 보내는데 도움이 되는 인터페이스로 주로 _MimeMessageHelper_   와 함께 사용한다.  
3.  **_JavaMailSenderImpl_  class**: implement 내용 정의된 클래스 
4.  **_SimpleMailMessage_  class**: 송수신 정보, 간단한 텍스트 내용, 참조 정보들 기본적인 메일 메시지  구현하는  클래스 
5.  **_MimeMessagePreparator_  interface**: MIME 메세지 구현 위한 인터페이스 
6.  **_MimeMessageHelper_  class**:  MIME 메세지 구현 

내가 해본 예제는 간단히 텍스트만 보내기에 JavaMailSender + SimpleMailMessage  조합이다. 

**2. JavaMailSenderImpl 의존 주입위해 spring context 파일 수정**
>  <bean id="mailSender"class="org.springframework.mail.javamail.JavaMailSenderImpl"/>
  
**3. MailSender 속성 세팅**

>     mailSender.setHost("smtp.gmail.com");
>     mailSender.setPort(587);
>     mailSender.setUsername("my.gmail@gmail.com");
>     mailSender.setPassword("password");
>     Properties props = mailSender.getJavaMailProperties();
>     props.put("mail.transport.protocol", "smtp");
>     props.put("mail.smtp.auth", "true");
>     props.put("mail.smtp.starttls.enable", "true");
>     props.put("mail.debug", "true");
속성 세팅을 위해 각 속성이 의미하는 바를 알아야한다.  
걍 뚝딱 세팅하니까 메일이 안가네...(당연한 말)  
 - [ ] setHost :  메일을 전송해주는 메일 서버
 - [ ] setPort: 포트 번호 세팅인데, 25, 587, 465 등이 있다.
>       25 : 옛날 표준 방식으로  요즘은 잘 쓰이지 않는다.  
>            relay (메일서버에서 다른 메일 서버로  수신 메일함 없으면 전달 > 스팸 메일의 원흉이 됨) 가 가능한 메일 서버에서  사용하던 포트로 요즘은 잘 쓰이지 않지만 레거시들이 남아있어서 사용된다. 
>       587: TLS 와 함께 사용되는 현재 표준 포트 번호이다. 어지간한 메일 서버의 포트번호 
>       465: IANA 가 발표했던 메일 표준 포트번호, SSL의 포트번호로 레거시들에 남아있음.
>   출처:  Which SMTP Port Should I Use? Understanding Ports 25, 465, & 587    [https://www.mailgun.com/blog/which-smtp-port-understanding-ports-25-465-587]
 - [ ] setUserName, Password : 메일 서버에 인증할 로그인 정보
   (서버에 올려서 같은 내부 서버 내에서 메일 전송할때는 인증 정보 쓸 필요 없다. ) 
 - [ ] getJavaMailProperties 세팅 :
   mail.smtp.starttls.enable > TLS 사용 여부

   mail.smtp.auth > 인증 사용 여부 

   mail.debug  > 메일 전송 내용 디버깅해 콘솔에 출력 

 
 **4. 메일 내용 세팅**
> 
>      @Autowired
>     public JavaMailSender emailSender;
>     
>     public  void sendSimpleMessage (String to, String subject, String text) {
	>      SimpleMailMessage message =new SimpleMailMessage();
	>      message.setTo(to);
>      message.setSubject(subject);
>      message.setText(text);
>      emailSender.send(message);
>     }


&nbsp;&nbsp;&nbsp;&nbsp;솔직히 너무 간단해서 할 말이 없다.. ㅠㅠ 
닉값 한다 진짜 simplemailmessage.. 진짜 simple.... 
파일 같은 걸 같이 보내고 싶을  땐 attachement 로 보내면 되는데 이것 또한 한 simple 하니 baeldung 참고해서 보내면 된다. 

&nbsp;&nbsp;&nbsp;&nbsp;근데 attachement 말고  전자 서명같은 인라인 첨부가 필요할 때가 있다. 
첨부 이미지긴한데 첨부파일 목록에 들어가지 않고 메일 본문에 나와야하는 경우. 
이때는 MimeMessageHelper.addAttachment() 사용하여 인라인 첨부에 CID 부여하여 사용한다. 

    @Service  
    public  class EmailService { 
    
    @Autowired  private JavaMailSender emailSender;
    
     public void sendSimpleMessage(Mail mail) throws MessagingException { 
     
       MimeMessage message = emailSender.createMimeMessage();
       
       MimeMessageHelper helper = new MimeMessageHelper(message,MimeMessageHelper.MULTIPART_MODE_MIXED_RELATED, StandardCharsets.UTF_8.name()); 
       
       helper.addAttachment("logo.png", new ClassPathResource("memorynotfound-logo.png")); // addAttachment(CID 값, path)
       
       String inlineImage = "<img src=\"cid:logo.png\"></img><br/>"; 
       
       helper.setText(inlineImage + mail.getContent(), true); 
       helper.setSubject(mail.getSubject()); helper.setTo(mail.getTo()); 
       helper.setFrom(mail.getFrom()); emailSender.send(message);
        }
    }
출처: [https://memorynotfound.com/spring-mail-sending-email-inline-attachment-example/](https://memorynotfound.com/spring-mail-sending-email-inline-attachment-example/)

배웠던 메일 전송  플로우랑 SMTP 에 대해서 조금 더 공부해서 정리해야겠다.
