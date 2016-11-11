### 커밋 히스토리에서 작성자(Author)나 커미터(Committer)를 바꾸고 싶을 때 

```
git filter-branch --commit-filter \
'if [ "$GIT_AUTHOR_NAME" = "Old Author Name" ]; then \
export GIT_AUTHOR_NAME="New Author Name";\
export GIT_AUTHOR_EMAIL="newAuthorEmail@example.com";\
export GIT_COMMITTER_NAME="New Committer Name";\
export GIT_COMMITTER_EMAIL="newCommitterEmail@example.com";\
fi;\
git commit-tree "$@"'
```

`filter-branch`는 커밋 히스토리를 바꾸는 명령어 (모든 커밋을 새로운 커밋으로 교체한다.)이므로 remote에 이미 푸시한 경우, 이미 다른 사람과 커밋이 공유된 경우는 조심해서 해야 한다. 


### 출처
http://stackoverflow.com/questions/4493936/could-i-change-my-name-and-surname-in-all-previous-commits
http://stackoverflow.com/questions/750172/change-the-author-and-committer-name-and-e-mail-of-multiple-commits-in-git
