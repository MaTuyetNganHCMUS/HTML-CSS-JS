## 1. Khởi tạo repo
- git init 
+ tạo local repository mới trên máy tính (nghĩa là trong thư mục hiện tại sẽ có một thư mục .git ẩn chứa tát cả metadata, lịch sử commit,...) -> để có thể sử dụng câu lệnh git quản lý project trên máy local, hoàn toàn độc lập với Github hay bất kỳ remote vào
+ Khi mới git init, chưa có commit nào, nên Git chưa tạo nhánh main, dùng câu lệnh git brach sẽ không thấy nhánh nào hoặc Git sẽ gọi nhánh HEAD (no branch) tùy phiên bản Git
+ Nhánh main chỉ xuất hiện sau commit đầu tiên
+ Nhánh main là nhánh mặc định nhưng git chỉ "hiện thực hóa" nó sau commit đầu tiên

- git clone <link-repo> # không cần git init vì đã clone một repo có sẳn về máy rồi -> Git đã tự tạo .git và thêm remote origin rồi


## Làm việc với commit
- Trạng thái file:
+ File có chữ **M**: file đã có sự thay đổi so với lần commit gần nhất
+ File có chữ **U**: file mới chưa được Git quản lý, chưa từng add/commit

```bash
git status # kiểm tra trạng thái file: mới (U), mới sửa(M), đã staged sẳn
git add <ten-file-muon-commit> # thêm file vào stage để chuẩn bị commit
git commit -m "mo-ta-nhung-thay-doi" # commit - ghi chú, mô tả lại những việc đã thực hiện thay đổi
git reset --soft HEAD~N # xóa n commit gần nhất nhưng vẫn giữ lại những thay đổi trong file (còn lưu trong stage)
git reset --hard HEAD~N # xóa n commit gần nhất và bỏ luôn những thay đổi trong file
git log --oneline # xem nhanh lịch sử commit
git diff # xem chi tiết thay đổi giữa các commit hoặc so với HEAD
```

## Làm việc với nhánh (brach)
```bash
git branch # xem nhánh hiện tại đang làm việc
git branch <ten-nhanh> # tạo nhánh mới và chưa chuyển sang nhánh đó
git checkout -b <ten-nhanh> # tạo nhánh mới và chuyển sang nhánh đó
git checkout <ten-nhanh> # chuyển sang nhánh khác (điều kiện: trong nhánh hiện tại những thay đổi đều đã commit hết rồi)
git stash # để lưu tạm những thay đổi chưa commit vào một stack đặc biệt và reset nhánh hiện tại về trạng thái sạch (HEAD) -> để chuyển sang nhánh khác, ở nhánh này có thể dùng stash pop để lấy lại những thay đổi đã lưu trên stack riêng biệt đó (stack này không dính gì với nhánh trước khi chuyển)

# nếu có xung đột với nhánh đích, sẽ thông báo conflict 
# nếu muốn giữ nguyên thay đổi trên nhánh nguồn
# - không nên pop vào nhánh khác mà chỉ chuyển sang tạm thời rồi chuyển lại
# - hoặc dùng git stash apply thay vì pop, hoặc tạo nhánh mới từ stash
git stash apply # áp dụng những thay đổi lên nhánh hiện tại mà không xóa stash
git stash pop # để lấy lại thay đổi từ stash và xóa stash
```

## Cách pull request (so sánh và hợp nhất code từ nhánh phụ vào nhánh chỉnh)

- Khái niệm:
+ Local repository -> repo trên máy tính cá nhân
+ Remote repository -> repo trên server (Github, GitLab, ...) mà nhiều người có thể truy cập, dùng để chia sẻ code với nhau 

- Git dùng remote để:
+ Đẩy code từ local lên server: git push
+ Kéo code từ server về local: git pull/git fetch

- Các lệnh liên quan đến remote
```bash
git remote -v # xem danh sách các remote và URL của chúng (thường mặc định là origin)
git remote add origin <link-repo> # thêm remote mới tên là origin trỏ đến repo trên server
git push -u origin <ten-brach> # lần đầu đẩy nhánh hiện tại lên remote origin với tên nhánh, lần đầu dùng -u để Git nhớ, lần sau chỉ cần git push là được
git pull origin <branch> # kéo thay đổi từ remote về nhánh hiện tại
git fetch origin # lấy các commit mới từ remote nhưng không merge vào nhánh hiện tại
```

## Pull từ remote
- Cập nhật nhánh hiện tại từ remote
```bash
git checkout main # chuyển sang nhánh main
git pull origin main # kéo các commit mới từ remote về
```

- Nếu nhánh đang làm việc (test) đã tách ra từ main, muốn merge main vào nhánh test trước khi tạo PR
```bash
git checkout test
git pull origin main
```

- Nếu có xung đột, Git đã báo ngay trong terminal

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