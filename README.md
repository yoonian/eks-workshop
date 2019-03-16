[![Build Status](https://travis-ci.org/awskrug/eks-workshop.svg?branch=master)](https://travis-ci.org/awskrug/eks-workshop)

# eksworkshop
https://awskrug.github.io/eks-workshop/

## 프로젝트 설정
### 리포지토리 클론하기:
```
git clone https://github.com/awskrug/eks-workshop.git
```

### theme submodule 클론하기:
```
cd eksworkshop
git submodule init
git submodule update
```

### Node.js 그리고 npm 설치:
링크 참고: https://www.npmjs.com/get-npm

### node 패키지 설치:
npm install

### 로컬 개발환경에서 실행하기:
`npm run server-kr` 명령어로 개발 서버를 띄우고 http://localhost:8081에서 확인 가능

`npm run build` 명령어로 `./public/` 경로에 정적 페이지 결과물이 빌드

### 페이지 수정하기:
개발 환경에서 페이지 수정하게 되면, 자동으로 변경된 페이지로 reload

### 자동 배포:
[travis-ci](https://travis-ci.org/awskrug/eks-workshop)를 이용한 자동 배포

참고 링크 : https://github.com/awskrug/eks-workshop/blob/master/.travis.yml 


### 한글화 작업중
참고 링크 : [한글화 작업 방법 Wiki Page](https://github.com/awskrug/eks-workshop/wiki/%ED%95%9C%EA%B8%80%ED%99%94-%EC%9E%91%EC%97%85-%EB%B0%A9%EB%B2%95)