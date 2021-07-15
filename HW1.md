# security

bof2 solution

<img width="850" alt="스크린샷 2021-07-15 오후 4 02 04" src="https://user-images.githubusercontent.com/86994067/125743853-8a123dc7-c393-4dca-86c6-12b5d28f0060.png">
docker run -it ccss17/bof 명령어를 통해 bof 문제가 있는 컨테이너에 접속하여
이전 bof1에서 얻은 bof2의 패스워드를 입력하여 bof2에 접속한다.

<img width="614" alt="스크린샷 2021-07-15 오후 4 10 41" src="https://user-images.githubusercontent.com/86994067/125744972-232ccf87-f4d2-4096-8ef9-7394e7846138.png">

if(innocent == KEY)를 만족하면 system("/bin/sh");를 사용할 수 있으므로
<img width="890" alt="스크린샷 2021-07-15 오후 2 35 34" src="https://user-images.githubusercontent.com/86994067/125745306-238d6a0d-6dab-40c9-a434-cf7a1a827384.png">
cmp 명령어에서 innocnet와 KEY를 비교하는 것을 확인!
rbp-0x4가 innocent, 0x61616161이 KEY값
innocent 위치를 찾아서 해당 값을 KEY값으로 덮어 씌우면 되므로
<img width="895" alt="스크린샷 2021-07-15 오후 2 35 57" src="https://user-images.githubusercontent.com/86994067/125746300-2d6a0c99-b9e9-4bad-9e8d-6733971667ad.png">
시작 주소를 찾은 뒤 브레이크를 걸고 run을 시켜 두 값의 거리를 구한다.


최종적으로 두 값의 거리가 140인 것을 확인하고 쓰레기 값을 140개 채워준 뒤 키값은 0x61616161이므로 'aaaa'를 써준다.
(이때 main함수에서 인자를 입력 받으므로 ./bof `python -c "print 'a'*140 + 'xxxx'"` 이러한 꼴로 입력!)
<img width="896" alt="스크린샷 2021-07-15 오후 3 10 38" src="https://user-images.githubusercontent.com/86994067/125746748-3b24ea61-f698-4834-ba0d-e6990e94ddd8.png">
이렇게 최종적으로 bof3의 패스워드를 얻을 수 있다.


bof3 solution

