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







