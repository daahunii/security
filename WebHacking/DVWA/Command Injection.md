# ⚜️Command Injection⚜️
### Command Injection이란 웹 애플리케이션에서 시스템 명령을 사용할 때, 세미콜론 혹은 &, && 를 사용하여 하나의 Command를 Injection 하여 두 개의 명령어가 실행되게 하는 공격입니다.

![R1280x0](https://user-images.githubusercontent.com/86994067/126741882-7181e5a5-db6e-48c9-a76e-94e1d4f3b4d6.png)

그림은 Command Injection 공격의 한 예를 보여주고 있습니다. 현재 웹 애플리케이션에서는 서버의 파일을 cat 명령어를 사용하여 사용자 입력과 함께 보여줄 수 있도록 만들어져 있습니다. 해커는 이 기능을 악용하여 리눅스에서 ① 두 개의 명령을 한 줄에 동시에 실행할 수 있도록 하는 연결자 인 “세미콜론”을 사용하여 ifconfig명령을 실행시키기 위해 웹 페이지에 요청을 보냅니다. ② 사용자로 부터의 요청을 아무런 검증없이 서버에 전송합니다. ③ 요청된 명령을 서버에서 실행하고, 서버의 민감정보를 그대로 노출합니다. 그 이후 여러 가지 명령어로 정보를 수집한 해커는 서버 내 라이브러리 중 원데이 취약점을 이용하여 리눅스 서버의 root 권한을 획득합니다. 

해당 웹 서버 개발자는 해커를 위한 웹셸을 만들어 둔 것이나 다름없습니다. 서버의 명렁을 실행하는 함수는 되도록이면 사용하지 말아야 하고, 사용하더라도 서버에 영향을 끼칠 수 있는 명령어는 사용이 불가능 하도록 하여야 합니다. 실 서버와 분리하여 운영하는 것도 좋은 방법입니다.


<br>

---

<br>

# Low Level

페이지를 살펴보면 IP주소를 입력받는 창이 있다. (IP 주소를 통해 ping 테스트를 하는 페이지인 것 같다.)
<img width="705" alt="스크린샷 2021-07-23 오후 3 36 17" src="https://user-images.githubusercontent.com/86994067/126745830-17fd9749-ae13-40e7-a024-46a8002b606a.png">

IP 주소 입력란에 ```; ls``` 다음과 같이 세미콜론 뒤 리눅스 명령어를 치면 디렉토리 내용을 추출할 수 있다.
<img width="706" alt="스크린샷 2021-07-23 오후 3 55 02" src="https://user-images.githubusercontent.com/86994067/126747217-918066d5-23c2-47c8-88b2-479412b859a5.png">

```; cat /etc/passwd```를 입력하면 호스트 사용자 목록이 출력되는 것을 확인할 수 있다.
<img width="696" alt="스크린샷 2021-07-23 오후 4 02 16" src="https://user-images.githubusercontent.com/86994067/126747908-fe39a49d-eaf2-47c7-a22c-6936b6507974.png">

마지막으로 ```; id```를 통해 root 권한을 획득할 수도 있다.
<img width="1032" alt="스크린샷 2021-07-23 오후 4 04 24" src="https://user-images.githubusercontent.com/86994067/126748057-a5aad9b3-d45b-42f2-852f-e8f6f20bddf1.png">


<br>

# Medium Level

Medium 단계에서는 Low 레벨처럼 세미콜론을 활용한 명령을 입력하면 안되는 것을 확인할 수 있다.

php 코드를 확인해보면

<img width="737" alt="스크린샷 2021-07-23 오후 4 12 18" src="https://user-images.githubusercontent.com/86994067/126748656-6e6e5868-69bc-43ec-b427-dc924cc9ad6d.png">
&&와 세미콜론을 잡아내고 있는 것을 확인할 수 있다. 하지만 이 밖에도 우회할 수 있는 명령어들이 많이 존재한다.

이번엔 ```& ls```을 injection 해주면 다음과 같이 우회되어 출력이 된다.
<img width="702" alt="스크린샷 2021-07-23 오후 4 17 54" src="https://user-images.githubusercontent.com/86994067/126749277-849c9058-23cc-45ce-a642-0aaf6be8b8ae.png">

"&" 외에도 "||"을 이용한 공격도 쉽게 우회되는 것을 확인할 수 있다.
<img width="683" alt="스크린샷 2021-07-23 오후 4 21 04" src="https://user-images.githubusercontent.com/86994067/126749920-e87be4c1-2928-463c-9f6f-5b32f8f3edc6.png">


<br>

# High Level

High 단계에서는 ```& ls```를 쳐보니 Medium 단계보다 더 검열이 심해졌음을 알 수 있다.

php 코드를 확인해보면

<img width="1081" alt="스크린샷 2021-07-23 오후 4 29 03" src="https://user-images.githubusercontent.com/86994067/126750295-cd95b567-1078-48e4-b76d-d3aa4fe70a0c.png">

``` &, ;, |, -, $, (, ), `, ||```가 검열되고 있음을 확인할 수 있다.

하지만 여기서 취약점은 "| " 다음 문자열에서 공백이 존재한다는 점...! (공백을 제외하고 붙여쓴다면...?)
<img width="684" alt="스크린샷 2021-07-23 오후 4 33 21" src="https://user-images.githubusercontent.com/86994067/126750769-b79c2118-8714-482d-98ea-282bcbf6393a.png">

붙여서 cat 명령어를 사용하니 뚫리는 것을 확인할 수 있다.

또 다음과 같이 "|(|" 명령어를 통해 우회하여 커맨드 인젝션 공격이 가능했다.
<img width="1033" alt="스크린샷 2021-07-23 오후 4 35 20" src="https://user-images.githubusercontent.com/86994067/126751076-08190d0e-0e47-4af1-8b08-fc8b55bcabc7.png">



