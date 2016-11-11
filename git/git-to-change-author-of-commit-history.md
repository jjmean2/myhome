### Ŀ�� �����丮���� �ۼ���(Author)�� Ŀ����(Committer)�� �ٲٰ� ���� �� 

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

`filter-branch`�� Ŀ�� �����丮�� �ٲٴ� ��ɾ� (��� Ŀ���� ���ο� Ŀ������ ��ü�Ѵ�.)�̹Ƿ� remote�� �̹� Ǫ���� ���, �̹� �ٸ� ����� Ŀ���� ������ ���� �����ؼ� �ؾ� �Ѵ�. 


### ��ó
http://stackoverflow.com/questions/4493936/could-i-change-my-name-and-surname-in-all-previous-commits
http://stackoverflow.com/questions/750172/change-the-author-and-committer-name-and-e-mail-of-multiple-commits-in-git
