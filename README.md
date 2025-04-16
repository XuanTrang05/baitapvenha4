# baitapvenha4
bai tap 4: (sql server)
yêu cầu bài toán:
 - Tạo csdl cho hệ thống TKB (đã nghe giảng, đã xem cách làm)
 - Nguồn dữ liệu: TMS.tnut.edu.vn
 - Tạo các bảng tuỳ ý (3nf)
 - Tạo được query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra.
   trả lời câu hỏi: trong khoảng thời gian từ datetime1 tới datetime2 thì có những gv nào đang bận giảng dạy.

các bước thực hiện:
1. Tạo github repo mới: đặt tên tuỳ ý (có liên quan đến bài tập này)
2. tạo file readme.md, edit online nó:
   paste những ảnh chụp màn hình
   gõ text mô tả cho ảnh đó

Gợi ý:
  sử dung tms => dữ liệu thô => tiền xử lý => dữ liệu như ý (3nf)
  tạo các bảng với struct phù hợp
  insert nhiều rows từ excel vào cửa sổ edit dữ liệu 1 table (quan sát thì sẽ làm đc)
  

deadline: 15/4/2025
## TẠO BẢNG GIẢNG VIÊN
![image](https://github.com/user-attachments/assets/d5ccd8f3-45a0-437e-a64f-16e34dbaf29e)
## BẢNG NULL CỦA BẢNG GIẢNG VIÊN
![image](https://github.com/user-attachments/assets/a587c5af-90fb-414e-8f6f-6a2f39258e1e)
## TẠO BẢNG MÔN HỌC
![image](https://github.com/user-attachments/assets/071ae106-c1d9-425f-af7c-ddcee7a79156)
## BẢNG NULL CỦA BẢNG MÔN HỌC
![image](https://github.com/user-attachments/assets/0b38290c-dfc1-4568-b8fd-22ad508e4361)
## TẠO BẢNG LỚP
![image](https://github.com/user-attachments/assets/a5bb6577-e8f8-4261-87a8-573b5fbec1ac)
## BẢNG NULL CỦA LỚP
![image](https://github.com/user-attachments/assets/a3498c66-400d-4e97-b806-71e4b3c8ebc3)
## TẠO BẢNG THỜI KHÓA BIỂU
![image](https://github.com/user-attachments/assets/80a526e3-113b-4842-9111-c03868a55815)
## BẢNG NULL CỦA THỜI KHÓA BIỂU
![image](https://github.com/user-attachments/assets/ecaf0444-4343-47b5-9a11-f0673f6dbdac)
### query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra.
--Trả lời câu hỏi : Từ khoảng thời gian từ datetime1 đến datetime2, những giảng viên đang bận giảng dạy là những người có lịch dạy (tiết học) được ghi nhận trong bảng thời khóa biểu (TKB) nằm trong khoảng thời gian đó. Những giảng viên đang bận giảng dạy trong khoảng thời gian từ datetime1 đến datetime2 là những giảng viên có mã giảng viên (magv) xuất hiện trong bảng thời khóa biểu (TKB), với ngày giảng dạy (ngay) nằm trong khoảng thời gian được chỉ định. Các giảng viên này đang đảm nhiệm các tiết học trong thời gian đó nên được coi là đang bận giảng dạy.
## CODE
* Khai báo chương trình
* Sử dụng tham số kiểu date : lọc ngày giảng dạy
  
![image](https://github.com/user-attachments/assets/f37cd339-f4d8-4dfd-9603-c665f235b293)

* nhập thời gian bắt đầu tiết học và thời gian kết thúc tiết học để đổi số tiết thành giờ cụ thể ( giờ vào- giờ ra)
![image](https://github.com/user-attachments/assets/23de293a-597d-4f0a-90c3-5b83d70ab30d)
* truy vấn các thông tin lịch giảng dạy của các giảng viên trong khoảng thời gian xác định bao gồm
  - tên giảng viên
  - môn dạy
  - giờ vào lớp( tính theo tiết bắt đầu)
  - giờ ra lớp( tính theo tiết kêt thúc)
  - ngày dạy
    
![image](https://github.com/user-attachments/assets/baeb775d-3cbe-4f64-a8c4-6c81ac60b1fc)

## Select các cột muốn hiện thị
- from TKB : bảng tkb chứa thông tin 
- JOIN  gv gv ON tkb.magv = gv.magv : kết nối với bảng gv để lấy thông tin giảng viên
- JOIN monhoc mh ON tkb.mamon = mh.mamon : lấy thông tin môn học
- JOIN TietGio tg1 ON tkb.tietbatdau = tg1.tiet : thời gian bắt đầu tiết học
- JOIN TietGio tg2 ON tkb.tietketthuc = tg2.tiet : Thời gian kết thúc tiết học
- WHERE 
    CAST(CONVERT(varchar, tkb.ngay, 23) + ' ' + tg2.GioRa AS DATETIME) > @datetime1 
    AND 
    CAST(CONVERT(varchar, tkb.ngay, 23) + ' ' + tg1.GioVao AS DATETIME) < @datetime2
  tìm các tiết học có thời gian diễn ra giao nhau với khoảng thời gian được truyền vào time 1 và time2

![image](https://github.com/user-attachments/assets/093096df-3f26-4424-abda-db4507fe9c6a)

- truyền dữ liệu giảng dạy muốn truy vấn
  
![image](https://github.com/user-attachments/assets/40acfcce-6177-455b-a428-a06d8327e6e9)
## TRUY VẤN THÔNG TIN HOÀN CHỈNH
![image](https://github.com/user-attachments/assets/7e19515b-e336-4343-b758-43ddc5bc30f8)
















