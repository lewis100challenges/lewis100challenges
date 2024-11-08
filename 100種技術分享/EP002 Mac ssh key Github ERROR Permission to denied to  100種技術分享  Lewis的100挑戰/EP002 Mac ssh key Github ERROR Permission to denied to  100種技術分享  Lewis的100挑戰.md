#### 障礙：

使用 git@github.com 方式推送，殘留舊帳號top-ow-dt，所以推送失敗

```bash
chenqingze@chenqingze-MBP lewis100challenges % git branch -M main
chenqingze@chenqingze-MBP lewis100challenges % git remote add origin git@github.com:lewis100challenges/lewis100challenges.git
chenqingze@chenqingze-MBP lewis100challenges % git push -u origin main
ERROR: Permission to lewis100challenges/lewis100challenges.git denied to top-ow-dt.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
```

#### 發現問題點：

官方連結：
https://docs.github.com/en/authentication/troubleshooting-ssh/error-key-already-in-use#finding-where-the-key-has-been-used

key綁定到Github top-ow-dt這個帳號上

```bash
chenqingze@chenqingze-MBP lewis100challenges % ssh -T -ai ~/.ssh/id_ed25519 git@github.com
Hi top-ow-dt! You've successfully authenticated, but GitHub does not provide shell access.
```

#### 解決：

登入Github top-ow-dt這個帳號刪除key，再將公鑰匯入到我的新帳號lewis100challenges

**Github top-ow-dt這個帳號，刪除key公鑰**

![](media/Pasted%20image%2020241108132533.png)

**Github lewis100challenges這個帳號，添加key公鑰**

![](media/Pasted%20image%2020241108132832.png)

再一次檢測，確定此公鑰已綁定在lewis100challenges這個帳號

```bash
chenqingze@chenqingze-MBP lewis100challenges % ssh -T -ai ~/.ssh/id_ed25519 git@github.com
Hi lewis100challenges! You've successfully authenticated, but GitHub does not provide shell access.

```

推送成功

```bash
chenqingze@chenqingze-MBP lewis100challenges % git add .
chenqingze@chenqingze-MBP lewis100challenges % git commit -m "second commit"
[main 1addc3f] second commit
 1 file changed, 1 deletion(-)
chenqingze@chenqingze-MBP lewis100challenges % git push -u origin main
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 10 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 273 bytes | 273.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:lewis100challenges/lewis100challenges.git
   80bbfb5..1addc3f  main -> main
branch 'main' set up to track 'origin/main'.
```

#### 使用Obsidian撰寫文章，直接推送至GitHub：

新增檔案

![](media/Pasted%20image%2020241108133243.png)

Git Commit all changes

![](media/Pasted%20image%2020241108133313.png)

Git Push

![](media/Pasted%20image%2020241108133337.png)

測試成功

![](media/Pasted%20image%2020241108133449.png)