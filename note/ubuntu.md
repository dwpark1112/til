# Ubuntu

우분투에서의 삽질

## How to set the timezone for crontab

우분투에서 crontab에 몇개의 스케쥴 작업을 등록해두었는데, 이게 항상 UTC 시간으로 동작했다. 분명 date로 확인하면 KST 시간으로 나오는데 스케쥴 작업 기록은 UTC;;

ubuntu에서 region/country를 설정할 수 있는 커맨드가 있길래 해보니깐 원하는 timezone의 시간대에 맞춰 동작하는걸 확인했다.

`sudo dpkg-reconfigure tzdata`