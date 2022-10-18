
# 깃허브 블로그 만들기

깃허브 블로그를 만들면서 발생한 오류 정리.

## 에러 1

```tsx
Bundler::GemNotFound: Could not find rake-10.3.2 in any of the sources
~/.rvm/gems/ruby-2.0.0-p451/gems/bundler-1.6.2/lib/bundler/spec_set.rb:92:in `block in materialize'
~/.rvm/gems/ruby-2.0.0-p451/gems/bundler-1.6.2/lib/bundler/spec_set.rb:85:in `map!'
~/.rvm/gems/ruby-2.0.0-p451/gems/bundler-1.6.2/lib/bundler/spec_set.rb:85:in `materialize'
~/.rvm/gems/ruby-2.0.0-p451/gems/bundler-1.6.2/lib/bundler/definition.rb:133:in `specs'
~/.rvm/gems/ruby-2.0.0-p451/gems/bundler-1.6.2/lib/bundler/definition.rb:178:in `specs_for'
Show 28 more lines
```

`gem install rake && bundle install`  
bundle install 시 에러 발생  
번들러 재설치 후 해결

## 에러 2

could not find expected ‘:’ while scanning a simple key at

: 뒤에 띄어쓰기 하지 않아서 발생한 오류, 띄어쓰기 해주면 사라짐. prettier가 자동 적용 돼서 띄어쓰기 하면 주석이 붙여지길래 삭제함

## 에러라고 생각했던 것 3

_config.yml은 지킬 동작에 대한 설정 내용을 모두 담고 있다 이 파일을 수정하면 자동 반영되지 않으며 지킬 서비스를 중단하고 다시 사이트가 빌드되어야 적용된다. 여러 환경변수가 담겨 있는데 사이트가 빌드될 때 한번만 읽어들이기 때문이다.

수정 후 재시작하는 명령어를 입력하면 된다.

```tsx
bundle exec jekyll serve
```