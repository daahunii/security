bof5 solution

이전 bof 문제들과 마찬가지로 bof4에서 얻은 패스워드로 bof5 유저로 접속한 뒤
bat bof5.c 명령어를 통해 bof5.c 파일의 구성을 보면
<img width="744" alt="스크린샷 2021-07-15 오후 6 26 40" src="https://user-images.githubusercontent.com/86994067/125765175-8285971b-d652-4452-ab9d-673c025e9722.png">

여기서 KEY 값은 0x12345678로 little endian 방식임을 알 수 있다.
여기서 주목할 점은 이전 문제들과는 달리 system 함수에서 인자를 받는 것이다.

<img width="742" alt="스크린샷 2021-07-15 오후 6 28 04" src="https://user-images.githubusercontent.com/86994067/125765734-518b0add-3a0b-44a0-b425-72823b3d35d6.png">
gdb disas vuln 명령어를 통해 vuln의 구성을 확인하고

<img width="748" alt="스크린샷 2021-07-15 오후 6 28 43" src="https://user-images.githubusercontent.com/86994067/125766057-1fdef0e7-e5ee-452e-ab38-a495a20188cb.png">
버퍼오버플로우를 걸어주는데 이때 생각해야하는 것이
system 함수에서 buf인자를 받은 형태인 것을 감안하여

(python -c "print '/bin/sh\x00'+'x'*132+'\x78\x56\x34\x12'";cat) | ./bof5
다음 명령어를 통해서 '/bin/sh\x00'을 추가하고 little endian 방식의 \x78\x56\x34\x12 키값을
넣어주어야 한다.





bof6 solution


