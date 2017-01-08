> [Home](https://github.com/jjmean2/til) - [Git](https://github.com/jjmean2/til/tree/master/git) - [git 명령어에서 single dash(-) 의 의미](https://github.com/jjmean2/til/blob/master/git/git-to-checkout-previous-commit.md)

### git 명령어에서 single dash(-) 의 의미

`git checkout` 명령어에서 single dash (`-`)를 쓰면 직전에 체크아웃했던 commit을 체크아웃한다.
`-` 는 `@{-1}`와 동의어라고 한다. `-`는 `git merge`에서도 동일하게 직전에 체크아웃했던 커밋을 뜻한다.
(비슷하게 `cd -`는 직전에 있던 경로로 돌아간다.)
 
현재 체크아웃된 커밋이 브랜치가 아니라 detach된 커밋일 때, 브랜치로 돌아가 현재 커밋을 merge 하려고 할 때 활용하기 좋은 방법인 것 같다. 
 
 ```bash
 git checkout head~20
 touch added_file.txt
 git add added_file.txt
 git commit -m 'Add added_file.txt'
 git checkout master
 git merge -
 ```
