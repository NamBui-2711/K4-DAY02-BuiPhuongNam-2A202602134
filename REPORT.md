# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Bùi Phương Nam<br>
**MSSV:** 2A202602134<br>
**Hình thức:** Cá nhân — cá nhân hoặc theo cặp<br>
**Mã cặp:** Solo — ghi `SOLO` nếu làm cá nhân

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp:f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: drive_008, dirve_022, dirve_033, dirve_038
- Số vật thể thực tế: khoảng 150 ảnh 
- Mã SHA-256 của gói YOLO của bạn: f41cccf7ada719ed9777aaae7838511875279fa6b07c417cfa2b8cf5a90095d5
- Mã SHA-256 của gói CVAT gốc của bạn: 5b70c7196c1559d1233b1e7eb60986d87542be6010563cd12eb13ef4865e7ab0
- Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp: người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu: 
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: 

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:

Bài được thực hiện độc lập trên bộ ảnh được cấp. Các annotation được tạo trực tiếp trong CVAT theo class schema và quy tắc gán nhãn của bài, trước khi nhận hoặc sử dụng bộ dữ liệu đối chiếu từ bạn cùng cặp.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| Xe con màu đen | car | Kích thước nhỏ, dạng xe con, khoang hành khách thông thường | Gán car cho xe con thông thường |
| Xe van màu trắng | van | Thân xe dạng hộp, khoang chở người/hàng phía sau, lớn hơn xe con | Gán van cho xe dạng van |
| Xe tải | truck | Có phần cabin và thùng/khoang chở hàng riêng | Gán truck cho xe tải |
| Xe buýt màu vàng | bus | Thân xe lớn, dài, nhiều cửa/cửa sổ hành khách | Gán bus cho xe buýt |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

ví dụ 'car' là lớp của vật thể thể hiện rằng vật thể này là xe Ô tô con, taxi, SUV hoặc xe bán tải dùng như xe con. Còn thuộc tính (visibility, boundary, review_state) thể hiện mức độ nhìn thấy, vị trí so với mép ảnh, trạng thái rà soát

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Bounding box của một xe bao gồm một phần nền xung quanh | hình học| Kiểm tra trực quan và thấy box không bám sát biên vật thể | Thu nhỏ box để bám sát phần vật thể nhìn thấy, không lấy phần nền |

- Số hộp `needs_review` trước và sau khi kiểm: trước 9 - sau 6 
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: Một xe bị che khuất nhiều nên không đủ bằng chứng để xác định chắc chắn là car hay van. Tôi đánh dấu trường hợp này để xem xét và xin người hướng dẫn/bạn cùng cặp hỗ trợ thay vì tự suy đoán.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: row: [2, 0.384297, 0.724141, 0.437344, 0.349531]
- Tên lớp và tọa độ điểm ảnh `xyxy`: lớp 2 (bus), tọa độ điểm ảnh: xyxy: [106.0, 351.6, 385.9, 575.3]
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học? Vì đúng định dạng YOLO chỉ đảm bảo dòng có đủ các trường và đúng cấu trúc, không đảm bảo mã lớp được gán đúng, bounding box nằm đúng phạm vi vật thể hoặc tọa độ hình học chính xác.


## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện:
- Mã ảnh thẩm định:
- Mô tả một dự đoán trong `detect_result.jpg`:
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
- Minh chứng nào có thể bác bỏ nhận định của bạn?
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
 Vì chỉ đánh giá trên bốn ảnh nên số lượng mẫu quá ít và không đại diện cho dữ liệu thực tế. Kết quả chỉ dùng để kiểm tra quy trình gán nhãn, huấn luyện và đối chiếu trong bài thực hành, không đủ để kết luận mô hình hoạt động tốt khi triển khai thực tế.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 48
- IoU trung bình và trung vị: 0.863505 , 0.885177
- Mức đồng thuận lớp: 0.708333
- Số hộp phía bạn không ghép được: 61
- Số hộp phía đối chiếu không ghép được: 2
- Một điểm khác biệt cụ thể:
- Quy tắc hoặc hành động sửa phát sinh:
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?

Mức đồng thuận cao chỉ cho thấy hai bộ nhãn tương đồng, nhưng cả hai bên vẫn có thể cùng mắc lỗi giống nhau. Vì vậy cần kết hợp kiểm tra annotation và IoU để đánh giá chất lượng nhãn.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach: ở ảnh drive_008 mã xe Truck 31 em chắc chắn xe đó có lớp là truck nhưng theo trong file comparison_iou.csv thì lại có class là bus. 
