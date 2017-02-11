# AWS S3 CLI

reference <http://docs.aws.amazon.com/cli/latest/reference/s3/sync.html>

윈도우에서는 s3 browser가 있어서 편했는데, mac에서는 s3 browser가 설치해도 실행이 제대로 안된다. 생각해보니 mac은 터미널이 잘되어 있는데, 굳이 s3 browser가 필요할까 싶더라;; 그래서 그냥 cli를 설치해서 사용하기로...

## Mac에 AWS CLI 설치

```
brew install awscli
echo 'complete -C aws_completer aws' >> ~/.bashrc
echo 'source /usr/local/share/zsh/site-functions/_aws' >> ~/.zshrc

# AWS IAM로 S3 access 설정
aws configure
```

## sync

로컬 파일을 s3로 동기화  
로컬에서 삭제한 것 까지 s3에서 같이 동기화되도록 하려면 --delete option을 넣어줘야 한다.

```
aws s3 sync . s3://mybucket
aws s3 sync . s3://mybucket --delete
```

s3파일을 로컬로 동기화  
이때는 위에서 한 것 반대로 하면 된다.

```
aws s3 sync s3://mybucket .
```

그 외에 것들은 그냥 나열... cli 자체만 읽어도 의미가 파악된다.

```
aws s3 sync s3://mybucket/ . --exclude "*another/*"
aws s3 sync . s3://mybucket --exclude "*.jpg"
```

