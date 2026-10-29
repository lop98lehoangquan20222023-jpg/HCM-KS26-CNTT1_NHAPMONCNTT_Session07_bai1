## Phần 1 – Đọc và định vị tệp tin

### 1. In toàn bộ nội dung file log ra màn hình
```powershell
Get-Content logs\server.log
```
2. Giải thích vì sao mở file bằng Notepad dễ treo máy còn PowerShell lại mượt mà?
- Notepad: Khi mở file nặng (như file log 500 MB), Notepad cố gắng nạp toàn bộ nội dung file đó vào RAM cùng một lúc. Nếu file quá nặng mà máy tính không đủ RAM thì máy sẽ bị đơ hoặc treo.

- PowerShell: Dùng cách đọc tuần tự từng dòng (stream dữ liệu), chỉ lấy phần cần thiết xử lý ngay chứ không bắt RAM gánh hết một lúc nên chạy rất mượt và nhanh.
3. Lệnh tìm tất cả file đuôi .log trong toàn bộ project
```powershell
Get-ChildItem -Recurse -Filter *.log
```
## Phần 2 – Lọc dữ liệu và Pipe
4. Lệnh lọc ra các dòng chứa chữ ERROR 
```powershell
Get-Content logs\server.log | Select-String "ERROR"
```
5. Phân biệt tìm vị trí và tìm nội dung:

| Câu hỏi | lệnh powershell |
| :---: | :---: |
|File server.log đang nằm ở thư mục nào?|Get-ChildItem -Recurse -Filter server.log|
|Bên trong file này có dòng nào chứa lỗi 404 không?|Get-Content logs\server.log|

- Lệnh dùng Pipe (|) để đọc log rồi lọc lỗi 404:
```powershell
Get-Content logs\server.log | Select-String "404"
```
6. Lệnh tìm tất cả file .log rồi lọc những dòng không chứa chữ INFO:
```PowerShell
Get-ChildItem -Recurse -Filter *.log | Get-Content | Select-String "INFO" -NotMatch
```
## Phần 3 – Toán tử chuyển hướng > và >>
7. Lệnh đọc log, lọc 404 và ghi đè vào báo cáo
```powershell
Get-ChildItem -Recurse -Filter *.log | Get-Content | Select-String "INFO" -NotMatch
```
8. Thêm lỗi mới và giải thích toán tử
```powershell
Get-Content logs\server.log | Select-String "404" >> reports\daily_report.txt
```
## Phần 4 – Shell Script và lệnh echo
9. Lệnh thông báo và ghi tiêu đề file
```powershell
Write-Output "Dang loc log..."
"=== LOG REPORT ===" > reports\daily_report.txt
```
10. Nội dung hoàn chỉnh của file scripts\auto_report.ps1
```powershell
Write-Output "Dang loc log..."
"=== LOG REPORT ===" > ../reports/daily_report.txt
Get-Content ../logs/server.log | Select-String "404" >> ../reports/daily_report.txt
```