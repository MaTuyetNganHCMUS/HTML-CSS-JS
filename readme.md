#
git clone
git init 
git remote 
git push -u orgin ten-nhanh # đẩy code đã commit lên nhánh [ten-nhanh]
git reset --soft HEAD~1
git reset --hard HEAD~1

## Thực hiện commit như sau:
- File có chữ **M**: file đã có sự thay đổi so với lần commit gần nhất
- File có chữ **U**: file mới chưa được git quản lý, chưa từng add/commit

```bash
git add ten-file-muon-commit
git commit -m "mo-ta-nhung-thay-doi"
```

## Làm việc với nhánh (brach)
```bash
git branch # xem nhánh hiện tại đang làm việc
git checkout -b ten-nhanh # tạo nhánh mới và chuyển sang nhánh đó
git checkout ten-nhanh # chuyển sang nhánh khác (điều kiện: trong nhánh hiện tại những thay đổi đều đã commit hết rồi)
```
