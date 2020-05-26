확장자가 .deb 인 파일을 받으면 아래와 같이 설치할 수 있습니다.

##### deb(데비안) 파일 설치
  
```bash
 sudo dpkg -i <deb_file>
```

##### deb(데비안) 파일 제거  
```bash
sudo dpkg -r <deb_file>
```

##### dpkg 키워드를 이용한 설치 패키지 확인
```bash
sudo dpkg -l
```
- 터미널에서 `sudo dpkg -l` 을 실행하면 `apt install`로 설치한 패키지도 같이 출력됩니다.
