git clone
git init 
git remote 
git push -u orgin ten-nhanh // đẩy code đã commit lên nhánh [ten-nhanh]
git reset --soft HEAD~1
git reset --hard HEAD~1

git branch // xem nhánh hiện tại đang làm việc
git checkout -b ten-nhanh // tạo nhánh mới 
git checkout ten-nhanh // chuyển sang nhánh khác (điều kiện: trong nhánh hiện tại những thay đổi đều đã commit hết rồi)
