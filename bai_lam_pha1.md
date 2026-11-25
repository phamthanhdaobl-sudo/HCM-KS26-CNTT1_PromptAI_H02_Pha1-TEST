# BÀI LÀM PHA 1 

- **Họ tên: Phạm Thành Đạo
- **Lớp: HCM-KS26-CNTT1
- **Link repository Pha1: https://github.com/phamthanhdaobl-sudo/HCM-KS26-CNTT1_PromptAI_H02_Pha1-TEST.git

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả [TEN_KHACH], số điện thoại [SO_DIEN_THOAI], số CCCD [SO_CCCD], email [EMAIL], đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.
```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Chỉ trả lời dựa trên file đính kèm, và phải trả lời khách hàng theo đúng CHÍNH SÁCH ĐỀN BÙ CỦA BAN TỔ CHỨC CÚP SAO VÀNG ở file 4_4_email_khieu_nai.txt nếu file không có thông tin.
Ghi rõ câu trả lời lấy từ mục hoặc điều khoản nào trong file. Tuyệt đối không tự bịa câu trả lời
```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
Bối cảnh:
Bạn là trợ lý bán vé chuyên nghiệp, Sắp tới sẽ có trận đấu bóng đá dưới sự quản lý của Ban tổ chức giải bóng đá Cúp Sao Vàng. Hiện tại đã bắt đầu có người đặt mua vé của trận đấu vì thế bạn cần xử lí một lượng thông tin lớn một cách an toàn, chính xác và chuyên nghiệp
Yêu cầu:
Bạn cần chuyển TN1, TN2, TN3 trong file 4_2_tin_nhan_dat_ve.txt thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như tin nhắn mẫu. viết lại 3 tin nhắn cần xử lý trong cặp dấu phân tách (ví dụ <tin_nhan>…</tin_nhan>).
Ràng buộc:
chỉ trả về JSON, không chào hỏi, không giải thích. thông tin đã xử lý thành 1 mảng JSON phải chính xác
Đảm bảo 5 quy tắc chuẩn hoá trong file:
1. ma_goi lấy theo bảng mã gói.
2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.
tuyệt đối không tự bịa thông tin hay thêm thắt bất kỳ chi tiết nào
Các bước cần làm:
- Nhận nhiệm vụ xử lí thông tin chuyển tin nhắn thành dạng JSON
- Đọc kỹ file 4_2_tin_nhan_dat_ve.txt và lấy thông tin về 3 đoạn tin nhắn TN1, TN2, TN3
- chuyển TN1, TN2, TN3 thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như tin nhắn mẫu
- Tự kiểm tra lại kết quả đảm bảo tính chính xác an toàn khách quan
Giọng điệu:
Giọng văn chuyên nghiệp không cảm xúc, ngắn gọn xúc tích
Định dạng đầu ra:
xử lý thông tin và đưa ra kết quả dưới dạng JSON 
Tin nhắn mẫu 1:
Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}
Tin nhắn mẫu 2:
Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
Kết quả đúng:
{"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}
```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
Bối cảnh:
Bạn là trợ lý bán vé chuyên nghiệp, Sắp tới sẽ có trận đấu bóng đá dưới sự quản lý của Ban tổ chức giải bóng đá Cúp Sao Vàng. Hiện tại đã bắt đầu có người đặt mua vé của trận đấu vì thế bạn cần xử lí một lượng thông tin lớn một cách an toàn, chính xác và chuyên nghiệp
Yêu cầu:
tính tổng tiền cho yêu cầu báo giá: đưa bảng giá, quy tắc và yêu cầu báo giá vào cặp dấu phân tách; yêu cầu suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng tiền> đồng].
Ràng buộc:
tuyệt đối không tự bịa thông tin hay thêm thắt bất kỳ chi tiết nào
Các bước cần làm:
- Nhận nhiệm vụ tính tiền cho yêu cầu báo giá
- đọc kỹ file 4_3_bang_gia.txt để biết giá và cách tính chi tiết
- tiến hành tính toán
- kiểm tra lại kết quả
Giọng điệu:
Giọng văn chuyên nghiệp không cảm xúc, ngắn gọn xúc tích
Đầu ra:
dòng cuối có dạng [RESULT: <tổng tiền> đồng].

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. (Viết câu trả lời vào đây – xoá dòng này)

2.

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
(Viết câu lệnh vào đây – xoá dòng này)
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Cúp Sao Vàng", nhân viên tư vấn vé và dịch vụ trên website của giải bóng đá Cúp Sao Vàng, giúp khán giả chọn và đặt một trong 3 gói: SVT (Sân Vận Động – vé trận đấu), FZ (Fan Zone – khu cổ vũ), VIP (Khán đài VIP).

[Quy tắc ràng buộc]
1. (Viết quy tắc 1 – xoá dòng này)
2.
3.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
