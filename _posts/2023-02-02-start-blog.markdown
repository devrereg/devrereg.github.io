---
layout: post
title:  "깃헙블로그 시작하기 with 삽질"
date:   2023-02-02 02:56:00 +0900
categories: jekyll update
---

github 블로그를 시작하기위해 세팅을 했다.
세팅하는 과정과 중간에 있었던 문제들, 그 해결방법을 정리한 글이다.   
   

- 단계   
    1. github 계정 만들기
    2. 레파지토리 만들기
    3. 레파지토리 > settings 에서 Repository name을 "계정".github.io 로 세팅하기
    4. [테마선택](https://jekyllthemes.io/free) 마음에 드는 jeckyll 테마를 고르고, github 가서 소스를 다운받는다.
    5. 레파지토리를 local의 workspace에 clone 받는다.
    6. clone 한 디렉토리로 가서 gem install jekyll bundler (jeckyll 설치!) => error 발생!
    7. jekyll new ./  현재 디렉토리에 jeckyll 생성하기
    8. bundle install => 의존성 관리 모듈 설치? => 잘은 모르겠지만 ruby 언어의 의존성관리 모듈 인것같다.
    9. bundle exec jekyll serve (로컬에 테마가 적용된 프로젝트 띄우기)

위 단계를 모두 성공하면 로컬에서 작업 후에 메인에 푸쉬만 하면된다.   
그럼 github actions 에서 푸쉬를 감자해서 https://계정.github.io 에 배포를 해준다.   

## Trouble shooting

위단계 중 6번 단계에서 에러를 만났다.   
```shell
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```
맥을 사용하는데 로컬에 시스템 ruby가 설치 되어있어서 버전이 맞지않는 에러다.   
한참 해매다가 결국 ruby 공식문서에서 내 컴퓨터에 맞는 스팩을 찾아 따라하니까 되었다.   
ruby 버전 관련된 문제는 다른데서 찾지말고, 공식 홈페이지에 설치방법이 pc 사양별로 나와있으니 참고하길 바란다.   
[ruby 공홈](https://gorails.com/setup/macos/12-monterey)


ruby가 잘 설치되었으면 gem install bundler 명령어로 gem 을 설치해주면된다.   
그리고 다시   
gem install jekyll bundler   

실행은 성공했지만 build 할때 에러가 발생했다.
liquid 에러인데,,, Liquid 관련 의존성 모듈을 설치하면 된다.   
gem 으로 jekyll-include-cache 를 설치하고, _config.yml 파일에 아래와 같이 추가했더니 해결되었다.   
   
```yml
plugins:
  - jekyll-include-cache
```

그리고 다시 빌드하고 만난 에러는 sass 에러였다.   
sass 문법중에 현재버전의 sass 에서 지원하지 않는 문법이 있어서 발생한 에러였다.   
sass파일이 꽤나 많은데, 에러메세지에서 어디가 ㅇ러인지 잘 알려주니... 하나씩 해결하는수밖에!   
어느정도 해결하니까 잘 빌드 되었다.   
그리고 배포도 성공적!!   

세팅은 완료되었고, 이제부터 jeckyll 테마 사용법을 익히면서 블로그에 기술관련 포스팅을 정리해 나가면 된다.

블로그 세팅만해도... 너무 많은 시련을 겪었다...ㅠㅠ 


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
