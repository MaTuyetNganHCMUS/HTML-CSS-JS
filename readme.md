## 1. Khởi tạo repo
- Tạo repository trực tiếp trên remote Github
    + Chú ý về public hay private
    + Mời thành viên (cần thành viên đó chấp nhận)
    + Có thể chọn `readme.md`, `.gitinore`, `LICSENSE` để tạo sẳn các file này, đồng thời sẽ có commit đầu tiên và tự có `branch main`

- `git init` 
    + Cd vào thư mục (dự án) trên máy cá nhân muốn sử dụng Git để quản lý, sau đó gõ câu lệnh `git init`
    + Thư mục này sẽ trở thành `local repository` mới trên máy tính cá nhân, hoàn toàn độc lập với Github hay bất kỳ remote nào
    + Nghĩa là trong thư mục này, Git sẽ tạo một thư mục .git ẩn chứa tất cả dữ liệu quản lý version (metadata, lịch sử commit, nhánh,...) -> để có thể sử dụng câu lệnh git quản lý project trên máy local
    + Khi mới git init, chưa có commit nào, nên Git chưa tạo nhánh main, dùng câu lệnh git branch sẽ không thấy nhánh nào hoặc Git sẽ gọi nhánh HEAD (no branch) tùy phiên bản Git
    + Thực hiện thêm câu lệnh này `git remote add origin <link-repo>` để cho biết local repo trên máy sẽ liên kết với remote repo ở server

- `git clone <link-repo> `
    + Sao chép một repo có sẳn trên server về máy local 
    + Git đã tự tạo .git ở thư mục hiện tại để cho phép sử dụng câu lệnh Git quản lý trên máy local
    + Khi clone về tức là đã có kết nối tới server repo đó luôn rồi
    + Nghĩa là tự mặc định tên đại diện cho remote repo là **origin** trên máy local dể trỏ (tham chiếu) tới remote repo trên server -> để biết push|pull lên remote repo nào
    + Tự động checkout tới nhánh mặc định -> thấy ngay code
    + Không cần `git init` để tạo local repo hay `git remote add origin` để liên kết tới server repo
- `git clone --no-checkout <link-repo> ten-thu-muc-chua-code-khi-repo`
    + Tương tự câu lệnh trên nhưng ở đây chỉ clone về máy mà không chuyển sang nhánh mặc định (thường là main hoặc master)
    + Trong thư mục hiện sẽ tạo tạo một ten-thu-muc-chua-code-khi-repo -> cần cd vào thư mục mới tạo đó
    + Nếu không đặt tên cho thư mục sẽ chứa remote repo -> tự lấy theo tên của remote repo
    + Cần `git checkout <ten-branch>` để chuyển tới nhánh đó và lấy file ra 

- Lưu ý về `branch main`:
    + Nhánh main dù là nhánh mặc định nhưng chỉ xuất hiện sau khi có commit đầu tiên, nghĩa là Git chỉ "hiện thực hóa" nó sau commit đầu tiên
    + Khi tạo repo mới trực tiếp trên Github nếu không chọn readme.md, .gitinore, LICSENSE -> repo trống -> không có commit nào -> không có nhánh nào kể cả nhánh main
    + Nếu tự tạo một nhánh != main, mà chưa tồn tại nhánh main thì nhánh != main đó sẽ thành default branch, sau này nếu tạo thêm một nhánh main và muốn nó trở thành default phải tự động cập nhật lại
    + Tương tự với `git init`, `git clone` 

## 2. Làm việc với commit
- Trạng thái file:
    + File có chữ **M**: file đã có sự thay đổi so với lần commit gần nhất
    + File có chữ **U**: file mới chưa được Git quản lý, chưa từng add/commit
    + File có chữ **A**: file đã được staged và sẽ bị commit trong lần commit tới
    + File có chữ **D**: File đã bị deleted và thay đổi này đang được staged
    + File có chữ **R**: file đã bị đổi tên 
    + File có chữ **C**: file đã bị copied

```bash
git status # kiểm tra trạng thái file: mới (U), mới sửa(M), đã staged sẳn
git add <ten-file-muon-commit> # thêm file vào stage để chuẩn bị commit
git commit -m "mo-ta-nhung-thay-doi" # commit, ghi chú, mô tả lại những việc đã thực hiện thay đổi
git reset --soft HEAD~N # xóa n commit gần nhất nhưng vẫn giữ lại những thay đổi trong file (còn lưu trong stage)
git reset --hard HEAD~N # xóa n commit gần nhất và bỏ luôn những thay đổi trong file
git log --oneline # xem nhanh lịch sử commit
git diff # xem chi tiết thay đổi giữa các commit hoặc so với HEAD
```

## 3. Làm việc với nhánh (branch)
- `git branch`
    - Xem nhánh hiện tại đang làm việc
- `git branch <ten-nhanh>` 
    - Tạo nhánh mới và chưa chuyển sang nhánh đó
- `git checkout -b <ten-nhanh>` 
    - Tạo nhánh mới và chuyển sang nhánh đó
- `git checkout <ten-nhanh>`   
    - Chuyển sang nhánh khác (điều kiện: trong nhánh hiện tại những thay đổi đều đã commit hết rồi)
- `git stash` 
    - Để lưu tạm những thay đổi chưa commit vào một stack đặc biệt và reset nhánh nguồn về trạng thái sạch (HEAD) 
    - Khi chuyển từ nhánh nguồn sang nhánh đích, ở nhánh đích vẫn có thể dùng `stash pop` để lấy lại những thay đổi đã lưu trên stack riêng biệt đó (stack này không dính gì với nhánh nguồn)
    - Nếu có xung đột với nhánh đích, sẽ có thông báo conflict
    - Nếu muốn giữ nguyên thay đổi trên nhánh nguồn
        - Không nên pop vào nhánh khác mà chỉ chuyển sang tạm thời rồi chuyển lại
        - Hoặc dùng `git stash apply` thay vì `git stash pop`, hoặc tạo nhánh mới từ stash
- `git stash apply` 
    - Áp dụng những thay đổi lên nhánh hiện tại mà không xóa stash
- `git stash pop` 
    - Áp dụng những thay đổi lên nhánh hiện tại và xóa stash

## 4. Cách pull request (so sánh và hợp nhất code từ nhánh nguồn vào nhánh đích)

- Khái niệm:
    + **Local repository** (kho lưu trữ cục bộ)
        + Là repository trên máy tính cá nhân
        + Mỗi thư mục chỉ có thể có 1 local repo (`git init` trong thư mục đã có local repo -> sẽ báo lỗi, `git clone` trong thư mục đã có local repo được -> vì nó tự tạo một thư mục mới chứa code)
        + Mỗi local repo có thể kết nối tới nhiều remote repo khác nhau
        + Trong local repo của thư mục, mỗi remote repo được kết nối tới cần đặt tên đại diện cho nó để phân biệt (tên đó sẽ tham chiếu tới URL của server repo)
    + **Remote repository** (kho lưu trữ từ xa)
        + Là repository trên server (Github, GitLab, ...) mà nhiều người có thể truy cập, dùng để chia sẻ code với nhau 
    + **Origin**: 
        + Là tên đại diện mặc định mà Git đặt cho **remote repository** đầu tiên mà clone về máy(`git clone \<link-repo>`). Lúc `git init` chưa kết nối tới remote repo sẽ không có tên nào hết kể cả origin. 
        + Có thể đổi lại tên đại diện bằng câu lệnh `git remote rename <ten-cu> <ten-moi>`
        + Khi tạo local repo trên máy -> lúc kết nối tới server repo -> có câu lệnh `git remote add <origin> <link-repo>` -> có thể đổi \<origin> thành tên khác

- Git dùng remote để:
    + Đẩy code từ local lên server: git push
    + Kéo code từ server về local: git pull/git fetch

- Các lệnh liên quan đến remote
    - `git remote -v` 
        - Xem danh sách các remote và URL của chúng (thường mặc định là origin)
    - `git remote add <origin> <link-repo>` 
        - Kết nối tới URL (repo trên server) trên máy cá nhân bằng cách sử dụng tên đại diện cho URL đó là <oridin> để tiện quản lý khi có nhiều kết nối
        - Có thể thay <origin> bằng tên khác
    - `git remote rename <ten-cu> <ten-moi>`
        - Đổi tên đại diện cho URL (remote repo mà local repo kết nối tới) bằng tên khác
    - `git push -u <origin> <local-brach>:<remote-branch>` 
        - Lần đầu đẩy những thay đổi đã commit từ nhánh local (tên A) vào nhánh server (tên B)
        - Nếu nhánh trên server (chưa tốn tại nhánh tên B) -> Git sẽ tạo nhánh tương ứng tên B trên server và đẩy code lên
        - Lần đầu đẩy nhánh hiện tại lên remote sẽ sử dụng -u để Git ghi nhớ (để biết nhánh local này liên kết với nhánh nào ở server, có thể 2 nhánh tên khác nhau, nhưng thường nên đặt tên giống nhau)
    - `git push -u <orign> <branch>`
        + Tương tự câu lệnh trên nhưng ở đây tên nhánh ở local và server là giống nhau
        + Git sẽ tạo một nhánh trùng tên với local branch ở trên server nếu trên server chưa có nhánh tên tương ứng
    - `git push <origin>` hoặc `git push`
        - Sử dụng `git push` nếu local repo chỉ kết nối tới duy nhất remote repo 
        - Không cần -u nếu trước đó local branch hiện tại đã thực hiện push lên server branch
        - Vì đã biết nhánh hiện tại ở local liên kết với nhánh nào trên server
    - `git pull <origin> <branch>` hoặc `git pull`
        - `git pull` = `git fetch` + `git merge`
        - **\<branch>**: tên nhánh trên remote muốn merge vào nhánh local hiện tại
        - Nghĩa là kéo toàn bộ commit mới (mà trên nhánh local hiện tại không có) từ một nhánh nào đó ở remote về nhánh local hiện tại
        - Nếu có xung đột Git sẽ báo để kiểm tra và sửa rồi commit lại
        - Nếu nhánh local đã được thiết lập -u khi `push` lần đầu, chỉ cần sài `git pull` nếu nhánh mới merge về tương ứng, nhưng nên ghi rõ ra vì không biết nhánh trên server muốn lấy về có tương ứng với nhánh trên local không (lấy commit mới nhất từ main ở remote về branch phụ ở local)
    - `git fetch <origin>` 
        - lấy các commit mới từ remote nhưng không merge vào nhánh hiện tại

## 5. Hướng dẫn cách làm việc nhóm
- Mỗi task được giao sẽ làm ở branch phụ ở local
- Không nhất thiết phải update main local thường xuyên 
- Quy trình làm
    - Làm xong task được giao
    - Khâu chuẩn bị để tạo PR (pull request)
        + Đảm bảo branch phụ ở local đã cập nhật những commit mới nhất từ main ở remote
        + `git pull origin main` ngay trong branch phụ ở local 
        + Resolve conflict (fix lỗi conflict do nhiều người làm việc, ở branch phụ trên local chưa cập nhật được những commit mới nhất ở main remote)
        + Push branch phụ lên remote (update branch phụ)
    - Tạo PR từ branch phụ -> main
    - Nếu báo conflict thì sửa conflict ở Github (sẽ không vì ở branch phụ đã chứa đầy đủ commit mới nhất từ main ở remote rồi)


## Xử lý conflict trên vscode 
- VSCode tự nhận biết conflict và hiển thị các tùy chọn
+ Accept Current Change -> Giữ code nhánh hiện tại
+ Accept Incoming Change -> Giữ code nhánh đang merge vào
+ Accept Both Changes -> Giữ cả hai đoạn
+ Compare Changes -> Xem chi tiết so sánh
- Sau khi chỉnh sửa xong, stage và commit
```bash
git add .
git commit -m "Giải quyết conflic"
```

## Merge nhánh
- Sau khi conflict được giải quyết, merge nhánh phụ vào nhánh chính
```bash
git checkout main
git merge test
git push origin main
```
