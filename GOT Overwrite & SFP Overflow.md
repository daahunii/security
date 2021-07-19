## ⚜️GOT Overwrite⚜️
GOT Overwrite란?

GOT Overwrite는 Dynamic Link방식으로 컴파일된 바이너리가 공유 라이브러리를 호출할 때 사용되는 PLT & GOT를 이용하는 공격 기법이다.

PTL는 GOT를 참조하고, GOT에는 함수의 실제 주소가 들어있는데, 이 GOT의 값을 원하는 함수의 실제 주소로 변조시킨다면, 원래의 함수가 아닌 변조한 함수가 호출된다. ex) printf("/bin/sh"); 
"/bin/sh"라는 문자열을 출력하는 함수가있으면 printf함수를 system함수로 변조하여 system("/bin/sh"); 가 되어 쉘을 실행시킨다.

GOT Overwrite의 예제를 살펴보면

<img width="324" alt="스크린샷 2021-07-19 오후 6 48 55" src="https://user-images.githubusercontent.com/86994067/126141175-51ff2869-e522-445c-828a-f62ca2fdb9d9.png">

gets와 puts 함수에서 각각 buf 인자를 받는 형식으로 이루어져 있다.(취약점 확인)
gdb를 통해 main 함수의 구성을 살펴보면

<img width="611" alt="스크린샷 2021-07-19 오후 6 47 37" src="https://user-images.githubusercontent.com/86994067/126141708-e46fe2e6-87f7-44bb-b5fb-13f3717c8f13.png">

다음과 같이 gets와 puts에서 PLT 주소를 확인할 수 있다. 이제 두 PLT 주소 중 하나를 다시
디스어셈블 해주면 GOT의 주소를 확인할 수 있다. (GOT 주소 *0x804c014 확인)

<img width="535" alt="스크린샷 2021-07-19 오후 6 56 57" src="https://user-images.githubusercontent.com/86994067/126142336-a5dd063a-044a-4dc9-9fd4-62a570eabdb2.png">

이제 system 함수의 주소를 구해 GOT을 system 함수의 주소로 바꾸어주면 셀을 획득할 수 있다.


system 함수의 주소를 확인해보면

<img width="638" alt="스크린샷 2021-07-19 오후 7 01 25" src="https://user-images.githubusercontent.com/86994067/126143088-fcd43217-3171-4d0a-8655-0069eb731917.png">

system 함수의 주소가 0xf7e1e420 인 것을 확인할 수 있다.

<img width="509" alt="스크린샷 2021-07-19 오후 7 02 37" src="https://user-images.githubusercontent.com/86994067/126143281-ffd17f09-eceb-41e4-9abb-a0980029a53d.png">

이렇게 주소를 바꿔주면서 셀을 얻을 수 있었다.

<br>

## ⚜️SFP Overflow⚜️

