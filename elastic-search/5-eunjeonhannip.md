# 설치는 항상 삽질을...

AWS EC2 instance에 설치 중인데, 삽질이 많아서 하나씩 남겨둔다.  
일단 C++ 컴파일러 설치가 필요하고, JDK 1.8 설치, 그리고 elasticsearch 설치가 필요하다.

```shell
sudo yum install gcc-c++

wget https://bitbucket.org/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.2.tar.gz
tar -zxvf mecab-0.996-ko-0.9.2.tar.gz
cd mecab-0.996-ko-0.9.2
./configure --with-charset=utf-8
make
make check
sudo make install
```

## mecab-ko-dic 설치

```shell
wget https://bitbucket.org/eunjeon/mecab-ko-dic/downloads/mecab-ko-dic-2.0.1-20150707.tar.gz
tar zxvf mecab-ko-dic-2.0.1-20150707.tar.gz
cd mecab-ko-dic-2.0.1-20150707
./configure --with-charset=utf-8
make
make install
```

이것도 설치하다가 에러가 난다. 그러면 아래처럼...

```shell
./autogen.sh
./configure --with-charset=utf-8
make
make install
```

## 테스트

`/usr/local/bin/mecab -d /usr/local/lib/mecab/dic/mecab-ko-dic`  
`mecab  SL,*,*,*,*...`

잘 나오면 성공!

삽질을 빨리 끝내도록 도움을 주신 자료들

- <http://nocode2k.blogspot.kr/2015/08/elasticsearch-mecab-ko.html>
- <https://bitbucket.org/eunjeon/mecab-ko-dic>
- Centos 설치기: <http://eunjeon.blogspot.kr/2013/02/cent-os-59-mecab-mecab-ko-dic.html>
- <https://kjune.com/korean_morphological_analysis_with_mecab/>

