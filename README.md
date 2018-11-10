# Elastic Beanstalk 자동화환경을 위한 개발환경 세팅

## 호스트에서 필요한 세팅

```text
Ubuntu:18.04
apt-get update
python 3.6.6
pyenv virtualenv
docker
postgresql postgresql-contrib
zsh

[pyenv 내부]
awsebcli
psql
```

## AWS 설정사항

- RDS 생성 (EC2/보안그룹 생성(허용 5432))
- S3 생성 (버킷 2개 생성)
- IAM 유저 생성 (AmazonS3FullAccess를 가진 유저 생성 후 액세스 키, 시크릿 키를 secret.json 내에 넣음(아래 참조))
- RDS 데이터베이스 생성(production용 DB, dev용 DB 두개 생성)
```bash
psql --user="" --host="엔드포인트" postgres
CREATE DATABASE "dev용 이름";
CREATE DATABASE "production용 이름";
```
- IAM 유저 생성(AWSElasticBeanstalkFullAccess, 인라인 정책(IAM -> CreateLinkedRole))을 가진 유저 생성 (액세스, 시크릿키를 ~/.aws/credential)안에 넣음
- EB 프로젝트 생성 (eb init --profile <credential 안에 넣은 태그 이름>)
- EB 환경 생성 (eb create --profile <credential 안에 넣은 태그 이름>, balancer 설정은 application)

## 설정 사항

- .secrets 폴더 생성
    - secret.json 생성
    ```json
    {
      "SECRET_KEY": "",
      "AWS_ACCESS_KEY_ID": "",
      "AWS_SECRET_ACCESS_KEY": ""
    }
    ```
    
    - production.json 생성
    ```json
{
  "AWS_STORAGE_BUCKET_NAME": "",
  "DATABASES" : {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "HOST": "엔드포인트",
        "NAME": "DBNAME",
        "USER": "",
        "PASSWORD": "",
        "PORT": "5432"
    }
  }
}
    ```
    
    - dev.json 생성
    ```json
{
  "AWS_STORAGE_BUCKET_NAME": "",
  "DATABASES" : {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "HOST": "엔드포인트",
        "NAME": "DBNAME",
        "USER": "",
        "PASSWORD": "",
        "PORT": "5432"
    }
  }
}
    ``
    
- .dev/bin eb-deploy의 profile 부분 변경
    
    
    
## 실행

    
## ETC