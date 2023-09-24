---
title: Git & GitHub
author: Rosie Yang
date: 2023-05-08
category: tool
layout: post
---

## 잘못된 Github 실수 만회하기
> 아찔한 잘못된 깃헙 실수를 발견할 때가 있습니다. 이를 다시 수정하기 위한 각각 상황별 방법을 기록해보고자 합니다.

<p style="text-align: center; margin: 10px 0">
    <img src="/assets/gitbook/post_images/git/git_meme.jpg" style="width:40%">
</p>

### 푸쉬한 내 커밋 메시지 수정하기
IntelliJ에서 취소를 했는데도 잘못된 깃헙 메시지가 푸쉬서 올라가는 경우가 있습니다. 이 경우 아래 방법들로 해결할 수 있습니다. 하지만 협업하는 팀 프로젝트에서는 각 커밋들이 강제로 push된다고 했을 때
다른 브런치와의 충돌이 있을 수 있기 때문에 한 브런치에서 작업한 작업물에 대한 commit 내역을 수정하는 경우에 권장됩니다. 그래서 ```feature```를 따로 작게 분류하거나 ```fork```하는 것이 좋습니다. 
~~엮이지 말자..~~

**IntelliJ에서 수정하기**
+ 간단하게 Git Log (```Alt+F9```)를 확인하고 해당 commit 내역을 ```Shift+F6``` 로 메시지 수정이 가능합니다.
+ 이 경우 그 commit을 기준으로 이후의 커밋들이 다시 쓰여지기 때문에 모두 ```Force Push``` 처리로 적용이 가능합니다.

**Git Bash를 이용한 수정방법**  
프로젝트 git 위치에서 수정할 커밋이 최근 커밋으로부터 몇 번째인지 확인하고 ```rebase```합니다. 
여기서 ```rebase```란, 기존 커밋 히스토리를 새로운 기준으로 재배치하는 기능입니다. 작업 중인 feature 브랜치에 다른 브랜치의 변경사항들을 반영시키기 위해서도 사용합니다.
이 이슈에서는 커밋 히스토리를 변경하기 위해 적용해보겠습니다.
```cmd
git rebase HEAD~10 -i
```
이후에 편집모드를 사용해서 ```pick```을 ```reword``` 상태로 수정 후 커밋메시지를 수정합니다. 그리고 저장 후 강제 푸쉬를 하면 적용된 것을 확인할 수 있습니다.
```cmd
git push --force
```

##### 실제 적용화면

![rebase_commit.png](/assets/gitbook/post_images/git/rebase_commit.png)


<br><br>

## Jekyll GitHub 블로그 로컬에서 관리하기
```깃헙계정 아이디```.github.io 로 된 repo를 생성합니다.   
jekyll에서 맘에 드는 테마를 가지고 오면 되는데 이 블로그에 사용된 테마는 [gitbook 테마](http://jekyllthemes.org/themes/gitbook/)입니다. 깃북으로 만들어도 되지만 더 마음대로 꾸밀 수 있고, 깃헙 파일구조도 깔끔하게 가지고 갈 수 있어서 jekyll 테마를 사용하게 되었습니다.  
설치될 OS에 적합한 <span style="background-color:#fff5b1">Ruby</span>를 설치하고 <span style="background-color:#fff5b1">Jekyll+Devkit</span> 설치를 진행합니다.    
Ruby가 설치되면 아래 명령어로 Ruby cmd에서 Jekyll과 Bundler를 설치합니다.
```file
gem install bundler jekyll 
```
이후 블로그 디렉토리로 이동해서 <span style="background-color:#fff5b1">Gemfile을 수정</span>해 필요한 플러그인이 설치될 수 있도록 합니다. 실제로 작성되어 있던 파일들 중에서 주석을 제거하고 버전을 맞춰주었는데 현재 이 블로그에 적용된 설정은 다음과 같습니다.
```file
source "https://rubygems.org"
gem "minima", "~> 2.5"
gem 'jekyll-include-cache'
gem "webrick", "~> 1.7"
gem "github-pages", group: :jekyll_plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
```
로컬 확인을 위해 <span style="background-color:#fff5b1">_config.yml 파일</span>의 url 부분을 ```https://localhost:4000```을 수정하고 Ruby cmd에서 실행하면, 로컬에서 확인이 가능합니다.
```file
bundle exec jekyll serve
```

<div style="padding:3px; margin:200px 0;"></div>   






