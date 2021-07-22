# ⚜️XSS(Cross-Site Scripting)⚜️
### XSS(Cross-Site Scripting) 이란 웹 애플리케이션에서 일어나는 취약점으로 관리자가 아닌 권한이 없는 사용자가 웹 사이트에 스크립트를 삽입하는 공격 기법입니다.

<br>

## Persistent(or Stored) XSS

![R1280x0](https://user-images.githubusercontent.com/86994067/126596057-62b24d24-d51c-4b9c-a47c-5933d6610c35.png)

-> XSS 공격 종류 중 하나인 Persistent XSS 는 말 그대로 지속적으로 피해를 입히는 XSS 공격입니다. 위의 그림을 보면, 해커는 웹 애플리케이션에서 XSS 취약점이 있는 곳을 파악하고, 악성스크립트를 삽입합니다. 삽입된 스크립트는 데이터베이스에 저장이 되고, 저장된 악성스크립트가 있는 게시글 등을 열람한 사용자들은 악성스크립트가 작동하면서 쿠키를 탈취당한다던가, 혹은 다른 사이트로 리다이렉션 되는 공격을 받게 됩니다. 데이터베이스에 저장이 되어 지속적으로 공격한다고 하여 Persistent XSS 라고 부르며, 데이터베이스에 저장이 되므로 Stored XSS 공격이라고 부르기도 합니다. 한번의 공격으로 악성스크립트를 삽입하여 수많은 피해를 입힐 수 있다는 점이 특징입니다.

Persistent XSS로 가장 많이 공격이 되는 곳은 게시판이며, 굳이 게시판이 아니더라도 사용자가 입력한 값이 데이터베이스에 저장이 되고, 저장된 값이 그대로 프론트엔드 단에 보여주는 곳에 공격이 성공할 가능성이 큽니다. XSS공격도 마찬가지로 사용자의 입력에 대한 검증이 없기 때문에 발생합니다.

#### 공격구문 예시
```
<script>alert('XSS TEST')</script>
<SCRIPT>alert('XSS TEST')</SCRIPT>
<img src="" onerror="alert('XSS TEST')">
<script>alert(document.cookie)</script>
<img src=javascript:alert(“XSS alert!”)>
```


<br>

---

# Low Level

사이트의 구성을 보면 이름과 게시글을 올릴 수 있는 공간이 있다. (게시글을 올리면 아래쪽에 올라오는 방식)
<img width="711" alt="스크린샷 2021-07-22 오후 3 50 01" src="https://user-images.githubusercontent.com/86994067/126599997-84e93762-4094-41dd-961a-e849d3c0d429.png">

게시글에다가 다음 ```<script>alert("XSS")</script>``` 스크립트문을 넣어 공격할 수 있다.
<img width="705" alt="스크린샷 2021-07-22 오후 4 40 54" src="https://user-images.githubusercontent.com/86994067/126605099-11732c99-5709-4fe9-b144-e3d3f8ebf14d.png">

<img width="653" alt="스크린샷 2021-07-22 오후 4 41 11" src="https://user-images.githubusercontent.com/86994067/126605110-717cc441-3f3f-48b1-83be-c6832fa7d776.png">
공격에 성공하면 다음과 같은 팝업창이 뜬다.


<br>

# Medium Level

겉보기엔 Low단계와 같아보이지만 스크립트문을 작성해서 넣어보면 먹히지 않는 것을 볼 수 있다.
<img width="711" alt="스크린샷 2021-07-22 오후 4 54 22" src="https://user-images.githubusercontent.com/86994067/126606946-b7f9c4d3-ef82-48c3-b875-35ed2365081a.png">

이번엔 이미지 태그문을 활용한 XSS 공격을 시도해보았다. 다음 명령문을 삽입해주면
`<img src="#" onerror="alert('XSS TEST')">`

<img width="700" alt="스크린샷 2021-07-22 오후 5 57 09" src="https://user-images.githubusercontent.com/86994067/126614086-0fe7c82e-c0ae-421b-9a71-287aaa1bbf78.png">

<img width="351" alt="스크린샷 2021-07-22 오후 5 57 35" src="https://user-images.githubusercontent.com/86994067/126614168-d16c744b-219c-4749-9601-1c519aa072d3.png">
스크립트문을 적었을 때와 달리 이미지태그를 넣어서 그런지 Message 부분에는 아무것도 없음을 확인할 수 있다.


<br>

# High Level

Higt 단계에도 스크립트문을 작성해 넣으면 통하지 않는 것을 확인할 수 있다.
<img width="710" alt="스크린샷 2021-07-22 오후 6 04 20" src="https://user-images.githubusercontent.com/86994067/126614831-b58a9e07-53c0-4189-8b3d-b8b360b8f66c.png">

그렇다면 Medium 단계에서처럼 태그문을 활용한 XSS 공격을 시도해주면
<img width="699" alt="스크린샷 2021-07-22 오후 6 06 41" src="https://user-images.githubusercontent.com/86994067/126615036-c867c104-9e13-480d-84cd-51f472e1d3a4.png">

이전 단계처럼 Message 부분이 비어있는채로 게시되는 것을 확인할 수 있다.(공격 성공)



