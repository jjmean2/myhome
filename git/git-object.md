> [Home](https://github.com/jjmean2/til) - [Git](https://github.com/jjmean2/til/tree/master/git) - [Git Objects](https://github.com/jjmean2/til/blob/master/git/git-object.md)


## Git Objects

https://git-scm.com/book/en/v2/Git-Internals-Git-Objects 의 내용 나름의 정리.. 

#### git hash-object

컨텐츠를 git object로 변환하여 저장한다.

```bash
$ echo 'test content' | git hash-object -w --stdin
```

`git hash-object` 명령어로 입력된 컨텐츠에 대한 SHA-1 해시값을 확인할 수 있다. 이 해시값은 git에서 이 컨텐츠를 저장하는 키값이 된다. git에서는 이 컨텐츠를 `.git/objects` 디렉토리에 SHA-1 해시값을 디렉토리/파일명에 적용하여 저장한다. 즉, 이렇게 저장된 컨텐츠들은 git의 objects로 여겨진다. 위의 명령어의 결과 SHA-1 해시값은 `d670460b4b4aece5915caf5c68d12f560a9fe3e4` 이다.

#### git cat-file

git object로부터 저장된 컨텐츠를 뽑아낸다.

objects는 *blob*, *tree*, *commit*, *tag*가 될 수 있다. `git cat-file` 은 이 object를 지정하여 담고 있는 컨텐츠를 출력할 때 쓸 수 있는 명령어이다. `-p` 옵션은 objects를 타입에 따라 읽을 수 있는 모양으로 출력해준다. 

```bash
git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4
```

`d670460b4b4aece5915caf5c68d12f560a9fe3e4` 이 어떤 특정 파일 또는 컨텐츠(*blob*)를 담고 있는 objects라면 위 명령어는 해당 파일(컨텐츠)내용을 출력한다.

`-t` 옵션은 objects의 타입을 보여준다.

```bash
$ git cat-file -t d670460b4b4aece5915caf5c68d12f560a9fe3e4
blob
```

#### Tree Object
tree object는 git에 저장된 컨텐츠들의 계층구조를 저장할 수 있다. tree는 Unix 파일시스템의 디렉토리와 비슷하고, blob은 파일과 비슷하다. tree는 여러 개의 blob과 하위 tree들로 구성될 수 있다. blob은 파일의 내용만을 저장하는데 파일의 이름과 경로와 같은 메타정보는 tree에 저장된다.

```bash
$ git cat-file -p master^{tree}
100644 blob a906cb2a4a904a152e80877d4088654daad0c859      README
100644 blob 8f94139338f9404f26296befa88755fc2598c289      Rakefile
040000 tree 99f1a6d12cb4b6f19c8655fca46c3ecf317074e0      lib
```

`master^{tree}`는 master 브랜치의 최신 커밋이 가리키는 tree object를 뜻한다.

#### Commit Object
commit object는 어떤 특정한 시점의 tree를 시간과 저장한 사람과 저장한 이유 등의 메터정보와 함께 저장하고 있는 object이다. commit object를 만들 때에는 하나의 tree와 부모 commit (바로 직전의 스냅샷)을 지정한다.




