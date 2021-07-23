# ⚜️XSS(Cross-Site Scripting)⚜️
### XSS(Cross-Site Scripting) 이란 웹 애플리케이션에서 일어나는 취약점으로 관리자가 아닌 권한이 없는 사용자가 웹 사이트에 스크립트를 삽입하는 공격 기법입니다.


<br>

## DOM(Document Object Model) based XSS

![R1280x0](https://user-images.githubusercontent.com/86994067/126744671-1d6141b6-ff6a-4b26-bbf1-c0ea5493005b.png)

-> DOM based XSS 는 악의적인 스크립트가 포함 된 URL을 사용자가 요청하게 되어 브라우저를 해석하는 단계에 발생하는 공격입니다. 악의적인 스크립트로 인해서 클라이언트 측 코드가 원래 의도와는 다르게 실행됩니다. DOM based XSS 공격은 다른 XSS 공격과는 다르게 서버 측에서 탐지가 어렵다는 점입니다. 위의 그림을 보면 해커는 ```http://www.some.site/page.html``` URL 과 함께 # 이라는 특수문자를 사용하고 있습니다. 이 특수문자는 # 이후의 값은 서버로 전송되지 않는 기능을 가지고 있습니다.

![R1280x0](https://user-images.githubusercontent.com/86994067/126745294-2dd28cb1-25cb-40b5-a9e7-6f98a1c6f916.png)

DOM based XSS 공격 위치

사용자의 요청에 따라 HTML을 다르게 해석하는 부분에 공격이 가능합니다.

<br>

---

<br>

# Low Level



<br>

# Medium Level



<br>

# High Level







