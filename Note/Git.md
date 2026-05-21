**Basic of the git file system**

```
/some/path/git-project
	.git   ←——\> staging  area (chứa file cần theo dõi lưu trữ ) ⇒ khi  thay đổi file  ⇒ commit brand (new feature / master)
	từ 1 thu mục bth ⇒ thư mục git : git  init
	git -la ⇒ file .git là file db của thư mục
	git -la .git
```

- `rm -rf <repo_name>`

**Typical git repository listing**

```
commit_editmsg: chứa description mỗi lần thay đổi
head: tham chiếu đến nhánh hiện tai
config: chứa thông tin liên quan repo (user đang lv email, 
description: file name chứa mô tả dự án: hiển thị thông tin lên web
hook: chứa script setup cho path cho repo
index: chứa thông tin file
info: chứa item 
log: lưu lại activity
object: lưu trữ thông tin folder .git
refs: chứa file tham chiếu branch , tag
```

**Create a local git repository**

```
git init /path/to/directory
git init -bare /path/to/directory  (project lớn, nhiều branch)
```

**Using `git config`**

```
git config
git config --global [user.name](http://user.name/) "Taamne"
git config --global user.email "[thanhtam160904@gmail.com](mailto:thanhtam160904@gmail.com)"<br>
git config —list: thông tin liên quan project
man git-config
git add + file name: Đưa tệp vào khu vực chờ (staging area).
git add . : add toàn bộ file trong thư mục
git status
git rm <mention-page url="https://www.notion.so/36778b149d48800e8e79e6aa9f46d472">file + -f</mention-page> 
```

**Using `git status`**

```
git status:  Kiểm tra trạng thái các tệp (staged, unstaged).[**1**](https://drive.google.com/open?id=1TOQh0z_TJUG1UA1vKrOUCeM3YrFBc0QN)
git status -s: trạng thái
git status -v
man git-status
```

**Committing to git**

```
git commit: mở text editor 
git commit -m “commit message”
git commit -a -m “message”
man git-commit
```

**Ignoring certain file types**

```
pattern: chứa 1 nhóm kí tự đại diện cho file không muốn git
tạo file thuộc tính ẩn: vim .gitignor
```

**Using tag: đánh dấu các mốc quan trọng (phiên bản phần mềm)**

```
ls .git/refs/tags
git tag -a \<tagname\> -m \<message\>
git tag
git tag \<tagname\> -m \<message\>
git tag -d \<tagname\>: xóa tag
man git-tag
```

**Using branches**

```
git branch -a: liệt kê
git branch \<branch name\>: tạo branch
git checkout \<branch name\>: switch sang branch đã tồn tại
git checkout -b \<branchname\>: tạo + switch
```

```
HEAD
man git-branch
man git-checkout
```

**Merging branches: kết hợp các thay đổi từ hai nhánh lại với nhau**

```
git merge \<branch muốn gộp sang branch hiện tại đang đứng\>
git branch -d \<branch name\>: xóa branch
man git-merge
```

**Rebasing: dùng để áp dụng lại các thay đổi trên một nhánh khác**

```
git rebase:
man git-rebase
==== git pull
```

**Reverting a commit: quay lại commit trước đó**

```
git revert: 
man git-revert
```

**Using the `diff` command**

```
git diff: sự khác biệt giữa các commit 
man git-diff
```

**How Git garbage collection works: dọn rác**

```
git gc: Dọn dẹp các đối tượng cũ trong cơ sở dữ liệu mà không còn được tham chiếu tới nữa.
git gc -prune: mặc định sẽ dọn dẹp các đối tượng đã cũ hơn hai tuần.
man git-gc
```

**Using git logs**

```
git log: 
git log —graph
git log —stat
git log -pretty=format
man git-log
```

**Cloning local repositories**

```jsx
git clone <local repo>  <new repo>: sao chép một repo có sẵn sang một repo mới
man git-clone
```

**Cloning remote repositories**

- `git clone <link>`: sao chép một repo từ máy chủ từ xa về máy tính

**Tracking remote repositories**

- `git remote -v`: danh sách các máy chủ từ xa
- `git fetch`: tải thông tin commit mới từ máy chủ từ xa về nhưng **không** tự động commit hay thay đổi dữ liệu hiện tại
- `git remote add <server_mới>`
- `man git-fetch`
- `man git-remote`

**Pushing to remote repositories**

- `git push -u <remote> <local_branch>`
- `man git-push`

**GitLab**

- Tự triển khai trên máy chủ của riêng mình (self-hosted)
