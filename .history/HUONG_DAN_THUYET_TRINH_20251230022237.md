# 📽️ HƯỚNG DẪN THUYẾT TRÌNH CHI TIẾT
# Mini-Project: Phân cụm khách hàng dựa trên luật kết hợp

> **🎯 Dành cho người KHÔNG BIẾT GÌ - Đọc đúng script này là ĂN ĐIỂM!**

---

# ⚠️ ĐỌC TRƯỚC KHI BẮT ĐẦU

## Bạn cần hiểu gì trước khi thuyết trình?

### 1. Bài toán này làm gì? (Giải thích như cho trẻ 10 tuổi)
```
Bạn mở cửa hàng bán đồ online. Bạn có 4000 khách hàng.
Mỗi người mua những thứ khác nhau.

BẠN MUỐN BIẾT:
- Khách nào GIỐNG nhau? (để gửi email giống nhau)
- Khách nào là VIP? (để chăm sóc đặc biệt)
- Khách nào sắp bỏ đi? (để níu kéo)

CÁCH LÀM:
1. Tìm ra "ai hay mua kèm cái gì" (ví dụ: mua áo thường mua quần)
2. Dựa vào đó, đánh dấu mỗi khách "có mua theo kiểu đó không"
3. Gom những khách có dấu hiệu giống nhau vào 1 nhóm
```

### 2. Các con số quan trọng cần NHỚ:
```
- 3,921 khách hàng (sau khi lọc)
- Top 50 luật mua kèm
- K = 10 nhóm (clusters)
- Silhouette = 0.99 (rất tốt, max là 1.0)
- Cluster 0 = 96.6% khách (đại trà)
- Cluster 1 = 2.5% khách (VIP, chi tiêu gấp 12 lần!)
```

### 3. Nếu cô hỏi gì mà không biết, trả lời:
```
"Dạ em xin phép kiểm tra lại trong notebook và trả lời sau ạ."
```

---

## 🎯 TỔNG QUAN: BẠN CẦN BAO NHIÊU SLIDE?

| STT | Nội dung | Số slide | Thời gian | Độ khó |
|-----|----------|----------|-----------|--------|
| 1 | Giới thiệu & Dữ liệu | 2 | 2 phút | Dễ |
| 2 | Bước 1: Tìm luật kết hợp | 3 | 3 phút | Trung bình |
| 3 | Bước 2: Tạo đặc trưng | 2 | 2 phút | Trung bình |
| 4 | Bước 3: Phân cụm K-Means | 3 | 3 phút | Trung bình |
| 5 | Kết quả: Phân tích từng cluster | 4-5 | 4 phút | ⭐ Quan trọng |
| 6 | Kết luận & Q&A | 2 | 1 phút | Dễ |
| **Tổng** | | **16-17 slide** | **15 phút** | |

---

# 📑 CHI TIẾT TỪNG SLIDE + SCRIPT NÓI

---

## SLIDE 1: TRANG BÌA

### Nội dung ghi trên slide:
```
MINI-PROJECT: PHÂN CỤM KHÁCH HÀNG DỰA TRÊN LUẬT KẾT HỢP

Môn: Data Mining
Sinh viên: [Tên của bạn]
MSSV: [Mã số]
Ngày: 30/12/2024
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Xin chào thầy/cô và các bạn. Em tên là [TÊN], mã số sinh viên [MSSV]. 
> Hôm nay em sẽ trình bày mini-project về đề tài Phân cụm khách hàng dựa trên luật kết hợp."

### Thời gian: 15 giây

---

## SLIDE 2: BÀI TOÁN ĐẶT RA

### Tiêu đề: "Chúng ta đang giải quyết vấn đề gì?"

### Nội dung ghi trên slide:
```
❓ VẤN ĐỀ:
• Cửa hàng có hàng nghìn khách hàng
• Mỗi người mua những sản phẩm khác nhau
• Làm sao biết khách nào GIỐNG nhau để marketing đúng cách?

💡 GIẢI PHÁP (3 bước):
• Bước 1: Tìm các luật "mua kèm" (Apriori/FP-Growth)
• Bước 2: Biến luật thành "dấu vân tay" của mỗi khách
• Bước 3: Dùng K-Means gom khách giống nhau vào 1 nhóm
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Trước hết, em xin trình bày bài toán mà chúng ta đang giải quyết.
>
> Giả sử bạn là chủ một cửa hàng online, bạn có hàng nghìn khách hàng. Mỗi người mua những sản phẩm khác nhau. 
>
> Câu hỏi đặt ra là: Làm sao để biết khách hàng nào GIỐNG nhau, để chúng ta có thể gửi email marketing phù hợp cho từng nhóm?
>
> Giải pháp của em gồm 3 bước:
> - Bước 1: Dùng thuật toán Apriori hoặc FP-Growth để tìm ra các luật mua kèm. Ví dụ: ai mua sản phẩm A thường mua thêm sản phẩm B.
> - Bước 2: Dựa vào các luật đó, em tạo ra "dấu vân tay" cho mỗi khách hàng - tức là đánh dấu xem khách đó có mua theo luật nào không.
> - Bước 3: Dùng thuật toán K-Means để gom những khách có dấu vân tay giống nhau vào cùng một nhóm."

### Thời gian: 1 phút

### Hình ảnh: Sơ đồ Pipeline (vẽ 3 hộp nối nhau bằng mũi tên)

---

## SLIDE 3: GIỚI THIỆU DỮ LIỆU

### Tiêu đề: "Dữ liệu Online Retail UK"

### Nội dung ghi trên slide:
```
| Thông tin | Giá trị |
|-----------|---------|
| Nguồn | UCI Machine Learning Repository |
| Thời gian | 01/12/2010 - 09/12/2011 (1 năm) |
| Số giao dịch | ~400,000 transactions |
| Số khách hàng | 3,921 (sau khi lọc UK) |
| Số sản phẩm | ~3,600 SKU |
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Dữ liệu em sử dụng là bộ Online Retail Dataset từ UCI Machine Learning Repository.
>
> Đây là dữ liệu thực tế của một cửa hàng bán lẻ trực tuyến tại Anh, trong khoảng thời gian 1 năm.
>
> Sau khi làm sạch và lọc chỉ lấy khách hàng UK, em còn lại khoảng 400 nghìn giao dịch của 3,921 khách hàng.
>
> Mỗi giao dịch gồm có: mã hóa đơn, mã sản phẩm, tên sản phẩm, số lượng, ngày mua, giá tiền, và mã khách hàng."

### Thời gian: 45 giây

### 📷 Hình ảnh cần có:
- Screenshot 5-10 dòng đầu của file `data/processed/cleaned_uk_data.csv`
- Mở file CSV, chụp màn hình, paste vào slide

---

## SLIDE 4: BƯỚC 1 - TÌM LUẬT KẾT HỢP

### Tiêu đề: "Bước 1: Tìm các luật MUA KÈM"

### Nội dung ghi trên slide:
```
🔍 LUẬT KẾT HỢP LÀ GÌ?
• "Ai mua A thường mua thêm B"
• Viết: {A} → {B}
• Ví dụ: {Bánh mì} → {Bơ}

📊 3 CHỈ SỐ QUAN TRỌNG:
• Support: Bao nhiêu % đơn hàng có cả A và B?
• Confidence: Trong số đơn có A, bao nhiêu % cũng có B?
• Lift: Mua A có làm TĂNG khả năng mua B không?

⚙️ THUẬT TOÁN: Apriori và FP-Growth
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Bước đầu tiên là tìm các luật mua kèm, hay còn gọi là Association Rules.
>
> Luật mua kèm là gì? Đơn giản là câu kiểu: Ai mua sản phẩm A thì thường mua thêm sản phẩm B.
>
> Ví dụ trong siêu thị: Ai mua bánh mì thì thường mua thêm bơ. Đó là một luật mua kèm.
>
> Để đánh giá một luật tốt hay không, chúng ta dùng 3 chỉ số:
>
> **Support** - trả lời câu hỏi: Luật này phổ biến không? Bao nhiêu phần trăm đơn hàng có cả A và B?
>
> **Confidence** - trả lời câu hỏi: Luật này chắc chắn không? Trong số những đơn có A, bao nhiêu phần trăm cũng có B?
>
> **Lift** - trả lời câu hỏi: Luật này có ý nghĩa không? Nếu Lift lớn hơn 1, nghĩa là mua A thực sự làm tăng khả năng mua B. Lift càng cao càng tốt.
>
> Em đã dùng 2 thuật toán là Apriori và FP-Growth để tìm luật. Cả 2 cho kết quả giống nhau."

### Thời gian: 1 phút 30 giây

### ⚠️ NẾU CÔ HỎI: "Apriori và FP-Growth khác nhau thế nào?"
> **Trả lời:** "Dạ, cả 2 thuật toán đều tìm ra cùng một tập luật. Sự khác biệt là cách triển khai: Apriori duyệt database nhiều lần nên chậm hơn, còn FP-Growth xây dựng cây FP-Tree nên nhanh hơn. Trong project này, em thấy kết quả 2 thuật toán giống hệt nhau ạ."

---

## SLIDE 5: CÁCH CHỌN LUẬT (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "Cách chọn luật kết hợp"

### Nội dung ghi trên slide:
```
📋 TIÊU CHÍ LỌC LUẬT:
• min_support = 0.01 (1%)
• min_confidence = 0.3 (30%)
• Lift > 1 (luật có ý nghĩa)

🏆 CÁCH CHỌN TOP-K:
• Sắp xếp theo LIFT giảm dần
• Lấy TOP 50 luật tốt nhất

📊 TẠI SAO TOP-50?
| Số luật | Silhouette Score |
|---------|------------------|
| Top-50 | 0.99 ✅ |
| Top-100 | 0.92 |
| Top-200 | 0.85 |
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Sau khi chạy thuật toán, em thu được rất nhiều luật. Vấn đề là không phải luật nào cũng tốt.
>
> Vì vậy em đã lọc luật theo các tiêu chí:
> - Support tối thiểu 1% - tức là luật phải xuất hiện trong ít nhất 1% số đơn hàng
> - Confidence tối thiểu 30% - tức là luật phải đúng ít nhất 30% số trường hợp
> - Lift phải lớn hơn 1 - tức là luật phải có ý nghĩa thống kê
>
> Sau đó, em sắp xếp các luật theo Lift từ cao xuống thấp, và lấy Top 50 luật tốt nhất.
>
> Tại sao lại là 50 mà không phải 100 hay 200? 
>
> Em đã thử nghiệm cả 3 trường hợp và đo bằng Silhouette Score. Kết quả cho thấy Top-50 cho Silhouette cao nhất là 0.99, trong khi Top-100 chỉ được 0.92, và Top-200 chỉ được 0.85.
>
> Điều này cho thấy: chọn ít luật nhưng chất lượng cao sẽ tốt hơn chọn nhiều luật nhưng có nhiễu."

### Thời gian: 1 phút

### ⚠️ NẾU CÔ HỎI: "Silhouette Score là gì?"
> **Trả lời:** "Dạ, Silhouette Score là chỉ số đánh giá chất lượng phân cụm, nằm trong khoảng từ -1 đến +1. Càng gần 1 thì phân cụm càng tốt, các cluster tách biệt rõ ràng. Score 0.99 của em là rất cao, gần như hoàn hảo ạ."

---

## SLIDE 6: 10 LUẬT TIÊU BIỂU (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "10 Luật mua kèm tiêu biểu"

### Nội dung ghi trên slide (bảng):
```
| # | Antecedent (Mua cái này...) | Consequent (...thường mua thêm) | Lift |
|---|-----------------------------|---------------------------------|------|
| 1 | PINK REGENCY TEACUP | ROSES REGENCY TEACUP | 85.2 |
| 2 | GREEN REGENCY TEACUP | ROSES REGENCY TEACUP | 78.4 |
| 3 | ALARM CLOCK BAKELIKE RED | ALARM CLOCK BAKELIKE GREEN | 72.1 |
| 4 | ALARM CLOCK BAKELIKE GREEN | ALARM CLOCK BAKELIKE RED | 72.1 |
| 5 | SET/6 RED SPOTTY PAPER CUPS | SET/6 RED SPOTTY PAPER PLATES | 65.3 |
| ... | ... | ... | ... |
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Đây là 10 luật mua kèm tiêu biểu nhất trong dataset của em.
>
> Nhìn vào luật số 1: Khách hàng mua PINK REGENCY TEACUP thì thường mua thêm ROSES REGENCY TEACUP.
>
> Lift bằng 85.2 nghĩa là gì? Nghĩa là khả năng mua ROSES TEACUP của người đã mua PINK TEACUP cao gấp 85 lần so với người bình thường! Đây là một luật rất mạnh.
>
> Các bạn có thể thấy pattern ở đây: Khách hàng thường mua theo BỘ SƯU TẬP. Ví dụ: mua cốc màu hồng thì mua thêm cốc màu hoa hồng, mua đồng hồ đỏ thì mua thêm đồng hồ xanh.
>
> Đây là insight quan trọng để đề xuất chiến lược marketing sau này."

### Thời gian: 1 phút

### 📷 Hình ảnh cần có:
- Bảng 10 luật từ file `data/processed/rules_apriori_filtered.csv`
- Mở file, sort theo lift giảm dần, chụp 10 dòng đầu

### ⚠️ NẾU CÔ HỎI: "Lift 85 có nghĩa gì?"
> **Trả lời:** "Dạ, Lift = 85 nghĩa là nếu một người đã mua sản phẩm A (PINK TEACUP), thì khả năng họ mua sản phẩm B (ROSES TEACUP) cao gấp 85 lần so với một người ngẫu nhiên. Lift càng cao thì mối liên hệ giữa 2 sản phẩm càng mạnh ạ."

---

## SLIDE 7: BƯỚC 2 - TẠO ĐẶC TRƯNG

### Tiêu đề: "Bước 2: Biến luật thành ĐẶC TRƯNG của khách"

### Nội dung ghi trên slide:
```
🎯 Ý TƯỞNG:
Mỗi khách hàng = 1 dãy số (50 số, vì có 50 luật)

📝 VÍ DỤ KHÁCH X:
• Luật 1 (TEACUP PINK → ROSES): 1 (có mua)
• Luật 2 (ALARM RED → GREEN): 0 (không mua)
• Luật 3: 1
• ...
→ Khách X = [1, 0, 1, 0, 1, ...] 

🔄 2 KIỂU ĐẶC TRƯNG:
• Kiểu 1: Binary 0/1
• Kiểu 2: Nhân với Lift (nếu có mua → ghi Lift thay vì ghi 1)
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Bước tiếp theo là biến các luật mua kèm thành đặc trưng của mỗi khách hàng.
>
> Ý tưởng rất đơn giản: Em có 50 luật, nên mỗi khách hàng sẽ được biểu diễn bằng một dãy 50 số.
>
> Với mỗi luật, em kiểm tra xem khách đó có mua theo luật đó không.
>
> Ví dụ: Luật 1 là 'Ai mua TEACUP PINK thì mua TEACUP ROSES'. Nếu khách X đã mua TEACUP PINK, thì em đánh dấu 1. Nếu không, em đánh dấu 0.
>
> Làm như vậy với cả 50 luật, em sẽ có một dãy số kiểu như [1, 0, 1, 0, 1, ...] cho mỗi khách.
>
> Dãy số này giống như 'dấu vân tay mua sắm' của khách hàng đó.
>
> Em đã thử 2 kiểu:
> - Kiểu 1: Đơn giản chỉ ghi 0 hoặc 1
> - Kiểu 2: Thay vì ghi 1, em ghi luôn giá trị Lift của luật đó. Ví dụ luật 1 có Lift = 85, thì em ghi 85 thay vì ghi 1.
>
> Kiểu 2 hay hơn vì nó phân biệt được luật quan trọng và luật ít quan trọng."

### Thời gian: 1 phút 15 giây

### 📷 Hình ảnh cần có:
- Bảng minh họa ma trận khách × luật (vẽ đơn giản):
```
         Luật1  Luật2  Luật3  Luật4  Luật5
Khách A    1      0      1      0      0
Khách B    0      1      0      1      1
Khách C    1      1      0      0      0
```

---

## SLIDE 8: SO SÁNH 2 KIỂU ĐẶC TRƯNG (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "So sánh các kiểu tạo đặc trưng"

### Nội dung ghi trên slide:
```
| Kiểu đặc trưng | Silhouette Score | Kết luận |
|----------------|------------------|----------|
| Binary (0/1) | 0.28 - 0.68 | ❌ Kém |
| Lift weighting | 0.99 | ✅ Xuất sắc |
| Lift × Confidence | 0.99 | ✅ Xuất sắc |

🏆 CHỌN: Lift weighting
📝 LÝ DO: Lift giúp phân biệt luật quan trọng vs ít quan trọng
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Đây là bảng so sánh kết quả của 2 kiểu tạo đặc trưng.
>
> Khi dùng kiểu Binary đơn giản, chỉ ghi 0 hoặc 1, Silhouette Score chỉ đạt từ 0.28 đến 0.68. Đây là kết quả khá thấp.
>
> Nhưng khi em dùng kiểu Lift weighting, tức là nhân với giá trị Lift, Silhouette Score tăng vọt lên 0.99. Đây là kết quả gần như hoàn hảo!
>
> Tại sao lại có sự khác biệt lớn như vậy?
>
> Bởi vì khi dùng Lift, em cho thuật toán biết rằng: Luật có Lift cao (ví dụ 85) quan trọng hơn luật có Lift thấp (ví dụ 2). Nhờ đó, thuật toán phân biệt được khách hàng tốt hơn.
>
> Vì vậy, em chọn kiểu Lift weighting cho bước tiếp theo."

### Thời gian: 1 phút

### ⚠️ NẾU CÔ HỎI: "Tại sao Lift weighting lại tốt hơn?"
> **Trả lời:** "Dạ, vì kiểu Binary coi tất cả các luật đều bằng nhau, trong khi thực tế có luật quan trọng hơn luật khác. Ví dụ luật có Lift = 85 rõ ràng đáng chú ý hơn luật có Lift = 2. Khi nhân với Lift, em giữ được thông tin về độ quan trọng này, giúp thuật toán phân cụm chính xác hơn ạ."

---

## SLIDE 9: BƯỚC 3 - PHÂN CỤM K-MEANS

### Tiêu đề: "Bước 3: Phân cụm bằng K-Means"

### Nội dung ghi trên slide:
```
🔍 K-MEANS LÀ GÌ?
• Chia khách hàng thành K nhóm
• Khách trong CÙNG nhóm → giống nhau
• Khách KHÁC nhóm → khác nhau

⚙️ THUẬT TOÁN (4 bước):
1. Chọn K điểm ngẫu nhiên làm tâm cụm
2. Gán mỗi khách vào cụm có tâm gần nhất
3. Tính lại tâm cụm = trung bình các điểm trong cụm
4. Lặp lại bước 2-3 cho đến khi không đổi
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Bước cuối cùng là dùng thuật toán K-Means để phân cụm khách hàng.
>
> K-Means là thuật toán rất phổ biến. Mục tiêu của nó là chia dữ liệu thành K nhóm, sao cho các điểm trong cùng nhóm thì giống nhau nhất có thể, và các điểm khác nhóm thì khác nhau nhất có thể.
>
> Thuật toán hoạt động như sau:
> - Bước 1: Chọn ngẫu nhiên K điểm làm tâm cụm ban đầu.
> - Bước 2: Với mỗi khách hàng, tính khoảng cách đến từng tâm cụm, rồi gán khách đó vào cụm có tâm gần nhất.
> - Bước 3: Sau khi gán xong, tính lại tâm cụm mới bằng cách lấy trung bình tất cả các điểm trong cụm.
> - Bước 4: Lặp lại bước 2 và 3 cho đến khi các tâm cụm không thay đổi nữa.
>
> Đơn giản vậy thôi!"

### Thời gian: 1 phút

### 📷 Hình ảnh cần có:
- Minh họa K-Means: Vẽ các chấm màu và 3 tâm cụm (có thể tìm hình online "K-Means illustration")

---

## SLIDE 10: CHỌN SỐ CỤM K (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "Chọn số cụm K tối ưu"

### Nội dung ghi trên slide:
```
📊 CÁCH CHỌN K: Dùng Silhouette Score
• Thử K = 2, 3, 4, ..., 10
• Chọn K có Silhouette cao nhất

🏆 KẾT QUẢ:
| K | Silhouette |
|---|------------|
| 2 | 0.9723 |
| 5 | 0.9897 |
| 8 | 0.9919 |
| 10 | 0.9933 ✅ MAX |

→ CHỌN K = 10
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Câu hỏi quan trọng là: Nên chia thành bao nhiêu nhóm? Tức là K bằng bao nhiêu?
>
> Em đã thử K từ 2 đến 10, và với mỗi K, em tính Silhouette Score.
>
> Silhouette Score cho biết các cụm có tách biệt tốt không. Score càng gần 1 thì càng tốt.
>
> Nhìn vào bảng, các bạn thấy K = 10 cho Silhouette cao nhất là 0.9933. Đây là kết quả rất tốt, gần như hoàn hảo.
>
> Vì vậy em chọn K = 10, tức là chia khách hàng thành 10 nhóm."

### Thời gian: 45 giây

### 📷 HÌNH ẢNH BẮT BUỘC PHẢI CÓ:
- **Biểu đồ đường Silhouette theo K**
- Lấy từ: `data/processed/silhouette_plot.png`
- Trục X: K (2 đến 10)
- Trục Y: Silhouette Score

### ⚠️ NẾU CÔ HỎI: "Tại sao không dùng Elbow method?"
> **Trả lời:** "Dạ, em có thử cả Elbow method nhưng với dữ liệu này, điểm elbow không rõ ràng. Silhouette Score cho kết quả rõ ràng hơn vì nó cho điểm cụ thể từng K, dễ so sánh hơn ạ."

---

## SLIDE 11: TRỰC QUAN HÓA PCA (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "Trực quan hóa: Các cụm có tách biệt không?"

### Nội dung ghi trên slide:
```
📊 VẤN ĐỀ: Dữ liệu có 50 chiều (50 luật), không vẽ được!

💡 GIẢI PHÁP: Dùng PCA giảm về 2 chiều

📈 KẾT QUẢ PCA:
• PC1: giải thích 93.3% sự khác biệt
• PC2: giải thích 1.9% sự khác biệt
• Tổng: 95.2% thông tin được giữ lại

✅ NHẬN XÉT: Các cluster TÁCH BIỆT RÕ RÀNG!
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Để kiểm tra xem phân cụm có tốt không, em cần vẽ biểu đồ để nhìn trực quan.
>
> Nhưng vấn đề là dữ liệu của em có 50 chiều, tức 50 cột, không thể vẽ lên giấy được.
>
> Vì vậy em dùng kỹ thuật PCA - Principal Component Analysis - để giảm từ 50 chiều xuống còn 2 chiều.
>
> Kết quả cho thấy: Trục PC1 giải thích được 93.3% sự khác biệt giữa các khách hàng, trục PC2 giải thích thêm 1.9%. Tổng cộng 95.2% thông tin được giữ lại - đây là con số rất tốt.
>
> Nhìn vào biểu đồ, các bạn có thể thấy:
> - Cluster 0, là nhóm khách phổ thông, tập trung ở bên trái
> - Cluster 1, là nhóm VIP, nằm riêng ở bên phải
> - Các cluster khác cũng tách biệt rõ ràng, không bị chồng lấn
>
> Điều này chứng tỏ phân cụm của em thành công!"

### Thời gian: 1 phút

### 📷 HÌNH ẢNH BẮT BUỘC PHẢI CÓ:
- **Biểu đồ PCA 2D scatter plot**
- Lấy từ: `data/processed/pca_scatter.png`
- Mỗi màu = 1 cluster

### ⚠️ NẾU CÔ HỎI: "PCA là gì?"
> **Trả lời:** "Dạ, PCA là kỹ thuật giảm chiều dữ liệu. Nó tìm các trục mới (gọi là Principal Components) sao cho giữ lại được nhiều thông tin nhất có thể. Ở đây, 2 trục PC1 và PC2 giữ được 95% thông tin của 50 chiều ban đầu, nên biểu đồ 2D này vẫn phản ánh đúng cấu trúc dữ liệu gốc ạ."

---

## SLIDE 12: TỔNG QUAN KẾT QUẢ PHÂN CỤM

### Tiêu đề: "Kết quả: 10 nhóm khách hàng"

### Nội dung ghi trên slide (bảng):
```
| Cluster | Số KH | % | Recency | Frequency | Monetary |
|---------|-------|---|---------|-----------|----------|
| 0 | 3,788 | 96.6% | 93 ngày | 4 đơn | £1,808 |
| 1 | 99 | 2.5% | 59 ngày | 25 đơn | £21,284 |
| 2-9 | 34 | 0.9% | Đa dạng | Đa dạng | Đa dạng |
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Đây là kết quả phân cụm. Em chia được 3,921 khách hàng thành 10 nhóm.
>
> Điều thú vị nhất là PHÂN BỐ LONG-TAIL:
> - Cluster 0 chiếm đến 96.6%, tức gần như toàn bộ khách hàng. Đây là nhóm khách phổ thông.
> - Cluster 1 chỉ có 2.5%, nhưng đây là nhóm VIP cực kỳ quan trọng.
> - Các cluster còn lại rất nhỏ, mỗi nhóm chỉ vài người.
>
> Đây là hiện tượng rất bình thường trong bán lẻ! Theo quy luật Pareto: 20% khách hàng tạo ra 80% doanh thu. Ở đây còn cực đoan hơn: chỉ 2.5% khách VIP có giá trị gấp 12 lần khách thường!"

### Thời gian: 45 giây

### ⚠️ NẾU CÔ HỎI: "Tại sao Cluster 0 lại chiếm nhiều thế?"
> **Trả lời:** "Dạ, đây là hiện tượng Long-tail distribution, rất phổ biến trong bán lẻ. Đa số khách hàng có hành vi mua sắm giống nhau (mua ít, chi tiêu vừa phải), nên họ nằm cùng 1 cluster. Chỉ một số ít khách có hành vi đặc biệt (VIP, mua theo bộ sưu tập) thì tách riêng ra. Điều này hoàn toàn bình thường và phù hợp với thực tế kinh doanh ạ."

---

## SLIDE 13: CLUSTER 0 - KHÁCH HÀNG ĐẠI TRÀ (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "Cluster 0: Regular Customers - Khách Hàng Đại Trà"

### Nội dung ghi trên slide:
```
📊 THỐNG KÊ:
• Số lượng: 3,788 khách (96.6%)
• Recency: 93 ngày (mua cách đây ~3 tháng)
• Frequency: 4 đơn/năm
• Monetary: £1,808/năm

🎭 PERSONA (hình mẫu khách):
"Khách bình thường, mua sắm thỉnh thoảng, không follow pattern đặc biệt"

📣 CHIẾN LƯỢC MARKETING:
• Email marketing định kỳ
• Cross-sell sản phẩm liên quan
• Discount codes
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Cluster 0 là nhóm lớn nhất với gần 3,800 khách hàng.
>
> Hồ sơ nhóm này: Recency 93 ngày, tức là họ mua hàng cách đây khoảng 3 tháng. Frequency 4 đơn, tức trung bình mỗi quý mua 1 lần. Monetary khoảng 1,800 bảng Anh một năm.
>
> Em đặt tên nhóm này là 'Regular Customers' - Khách Hàng Đại Trà. Đây là những người mua sắm bình thường, không theo pattern đặc biệt nào.
>
> Chiến lược marketing cho nhóm này:
> - Gửi email marketing định kỳ với sản phẩm mới
> - Cross-sell: Khi họ mua 1 sản phẩm, gợi ý sản phẩm liên quan
> - Phát mã giảm giá để kích thích mua hàng
>
> Vì nhóm này đông nhất nên họ vẫn tạo ra phần lớn doanh thu cơ bản cho cửa hàng."

### Thời gian: 1 phút

### ⚠️ NẾU CÔ HỎI: "Recency, Frequency, Monetary là gì?"
> **Trả lời:** "Dạ, đây là 3 chỉ số RFM trong phân tích khách hàng:
> - Recency: Mua hàng gần đây nhất cách đây bao lâu (càng nhỏ càng tốt)
> - Frequency: Tổng số đơn hàng đã mua (càng nhiều càng tốt)
> - Monetary: Tổng tiền đã chi tiêu (càng cao càng tốt)
> 
> Ba chỉ số này giúp đánh giá giá trị của khách hàng ạ."

---

## SLIDE 14: CLUSTER 1 - KHÁCH VIP (⭐ CÔ CHẤM MẠNH)

### Tiêu đề: "Cluster 1: VIP Champions - Khách Hàng Thượng Đế"

### Nội dung ghi trên slide:
```
📊 THỐNG KÊ:
• Số lượng: 99 khách (chỉ 2.5%!)
• Recency: 59 ngày (mua GẦN ĐÂY!)
• Frequency: 25 đơn/năm (gấp 6 lần khách thường!)
• Monetary: £21,284/năm (gấp 12 lần!)

🎭 PERSONA:
"KHÁCH HÀNG VÀNG! 1 người VIP = 12 người thường!"

📣 CHIẾN LƯỢC (ĐẶC BIỆT):
• VIP Program riêng
• Personal Shopper
• Early Access sản phẩm mới
• Quà sinh nhật, lễ tết
• ⚠️ KHÔNG BAO GIỜ ĐỂ MẤT!
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Cluster 1 là nhóm quan trọng nhất - Khách VIP!
>
> Chỉ có 99 người, chiếm 2.5%, nhưng hãy nhìn số liệu:
> - Recency chỉ 59 ngày - họ mua hàng rất gần đây
> - Frequency 25 đơn - gấp 6 lần khách thường
> - Monetary hơn 21 ngàn bảng - gấp 12 lần khách thường!
>
> Nói cách khác: Một khách VIP có giá trị bằng 12 khách thường!
>
> Nhóm này thường mua theo bộ sưu tập: mua cốc màu hồng thì mua thêm cốc màu hoa hồng, mua đồng hồ đỏ thì mua thêm đồng hồ xanh.
>
> Chiến lược marketing phải ĐẶC BIỆT:
> - Tạo chương trình VIP riêng
> - Personal Shopper - nhân viên chăm sóc riêng
> - Cho họ được mua sản phẩm mới trước mọi người
> - Gửi quà sinh nhật, quà lễ tết
>
> Và quan trọng nhất: KHÔNG BAO GIỜ để mất nhóm này! Mất 1 VIP = mất doanh thu bằng 12 khách thường!"

### Thời gian: 1 phút 15 giây

### ⚠️ NẾU CÔ HỎI: "Làm sao biết họ thường mua theo bộ sưu tập?"
> **Trả lời:** "Dạ, em nhìn vào TOP luật của cluster này. Cluster 1 có giá trị cao nhất ở các luật liên quan đến TEACUP SETS, ALARM CLOCK SETS. Điều này cho thấy họ hay mua các sản phẩm theo bộ, theo màu sắc đồng bộ ạ."

---

## SLIDE 15: CÁC CLUSTER KHÁC

### Tiêu đề: "Các nhóm khách hàng đặc biệt khác"

### Nội dung ghi trên slide:
```
🟢 CLUSTER 2, 7, 8: Rising Stars - Ngôi Sao Đang Lên
• Recency thấp (9-37 ngày) → đang ACTIVE!
• Tiềm năng trở thành VIP
• → Chiến lược: Nurturing, chăm sóc đặc biệt

🔴 CLUSTER 9: Lost Customer - Đã Rời Bỏ  
• Recency: 277 ngày (gần 1 năm không mua!)
• → Chiến lược: Win-back email + Discount 20-30%

🟡 CLUSTER 3, 4, 5, 6: Niche Segments
• Các nhóm rất nhỏ, cần phân tích thêm
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Ngoài 2 cluster chính, em còn phát hiện một số nhóm đặc biệt.
>
> Cluster 2, 7, 8 - em đặt tên là Rising Stars - Ngôi Sao Đang Lên. Họ có Recency rất thấp, chỉ 9 đến 37 ngày, nghĩa là đang mua hàng rất thường xuyên. Đây là những khách có tiềm năng trở thành VIP nếu được chăm sóc đúng cách.
>
> Cluster 9 - Lost Customer - Khách Đã Rời Bỏ. Recency lên đến 277 ngày, gần 1 năm không mua hàng. Cần có chiến dịch win-back, ví dụ gửi email với mã giảm giá 20-30% để kéo họ quay lại.
>
> Các cluster còn lại rất nhỏ, cần phân tích thêm để hiểu rõ hơn."

### Thời gian: 45 giây

---

## SLIDE 16: SO SÁNH THUẬT TOÁN

### Tiêu đề: "So sánh các thuật toán Clustering"

### Nội dung ghi trên slide (bảng):
```
| Thuật toán | Silhouette | Số cluster | Ưu điểm | Nhược điểm |
|------------|------------|------------|---------|------------|
| K-Means | 0.9933 | 10 | Nhanh, dễ hiểu | Phải chọn K |
| Agglomerative | 0.9934 | 10 | Có dendrogram | Chậm |
| DBSCAN | 0.9973 | 7 + noise | Tự tìm K | Bỏ 99 outliers |

🏆 CHỌN: K-MEANS
📝 LÝ DO: Không bỏ sót khách hàng nào
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Em đã thử nghiệm 3 thuật toán clustering phổ biến để so sánh.
>
> K-Means cho Silhouette 0.9933, tạo ra 10 cluster như mong muốn.
>
> Agglomerative Clustering cho kết quả tương tự, Silhouette 0.9934. Thuật toán này có thể vẽ dendrogram để xem cấu trúc phân cấp, nhưng chạy chậm hơn.
>
> DBSCAN cho Silhouette cao nhất: 0.9973. Tuy nhiên, DBSCAN tự động phát hiện số cluster là 7, và quan trọng hơn, nó đánh dấu 99 khách hàng là outliers - noise - không thuộc cluster nào.
>
> Cuối cùng em chọn K-Means vì:
> - Silhouette rất cao, gần như bằng các thuật toán khác
> - Không bỏ sót khách hàng nào - quan trọng vì trong kinh doanh, mọi khách hàng đều có giá trị
> - Dễ giải thích cho team marketing"

### Thời gian: 1 phút

### 📷 HÌNH ẢNH (nếu có thời gian):
- Lấy từ: `data/processed/algorithm_comparison_visual.png`

### ⚠️ NẾU CÔ HỎI: "DBSCAN là gì? Tại sao nó bỏ outliers?"
> **Trả lời:** "Dạ, DBSCAN là thuật toán dựa trên mật độ. Nó nhóm các điểm gần nhau thành cluster, còn các điểm nằm rải rác xa thì đánh dấu là outliers. Ưu điểm là không cần chọn K trước, tự động phát hiện số cluster. Nhược điểm là với dữ liệu này, 99 khách bị đánh dấu là noise - mà trong kinh doanh em không muốn bỏ sót khách nào cả ạ."

---

## SLIDE 17: KẾT LUẬN

### Tiêu đề: "Kết luận"

### Nội dung ghi trên slide:
```
✅ VỀ KỸ THUẬT:
1. Apriori = FP-Growth (kết quả giống, FP-Growth nhanh hơn)
2. Lift weighting → Silhouette tăng từ 0.68 lên 0.99
3. Top-50 luật tốt hơn Top-200 (ít nhiễu)
4. K-Means phù hợp nhất

✅ VỀ KINH DOANH:
1. 96.6% khách phổ thông → Marketing đại chúng
2. 2.5% VIP = 12x giá trị → Chăm sóc đặc biệt
3. Có khách đang active → Cơ hội convert VIP
4. Có khách đã mất → Cần win-back campaign
```

### 🎤 SCRIPT NÓI (đọc y nguyên):
> "Tổng kết lại project của em.
>
> Về mặt kỹ thuật:
> - Apriori và FP-Growth cho kết quả giống nhau, nhưng FP-Growth chạy nhanh hơn
> - Dùng Lift làm trọng số giúp Silhouette tăng từ 0.68 lên 0.99 - một cải thiện rất lớn
> - Chọn Top-50 luật tốt hơn Top-200 vì ít nhiễu hơn
> - K-Means là thuật toán phù hợp nhất vì không bỏ sót khách hàng
>
> Về mặt kinh doanh:
> - 96.6% là khách phổ thông, cần marketing đại chúng
> - 2.5% là VIP, có giá trị gấp 12 lần, cần chăm sóc đặc biệt
> - Phát hiện được nhóm khách đang active có tiềm năng thành VIP
> - Cũng phát hiện nhóm khách đã mất cần có chiến dịch win-back
>
> Cảm ơn cô và các bạn đã lắng nghe!"

### Thời gian: 1 phút

---

## SLIDE 18: CẢM ƠN & Q&A

### Tiêu đề: "Cảm ơn đã lắng nghe!"

### Nội dung ghi trên slide:
```
📁 FILES ĐÃ NỘP:
• Notebook: comprehensive_clustering_analysis.ipynb
• Report: miniproject.md
• Data: customer_cluster_labels.csv

❓ Q&A - Câu hỏi thường gặp:
• Q: Tại sao K=10? → Silhouette cao nhất
• Q: Tại sao Cluster 0 chiếm 96.6%? → Long-tail, bình thường trong retail
• Q: Silhouette 0.99 có cao không? → RẤT CAO (max = 1.0)
```

---

# 📷 DANH SÁCH HÌNH ẢNH CẦN CÓ

| # | Hình ảnh | File nguồn | Slide sử dụng |
|---|----------|------------|---------------|
| 1 | Biểu đồ Silhouette theo K | `data/processed/silhouette_plot.png` | Slide 10 |
| 2 | Biểu đồ PCA 2D | `data/processed/pca_scatter.png` | Slide 11 |
| 3 | So sánh thuật toán | `data/processed/algorithm_comparison_visual.png` | Slide 16 |
| 4 | Screenshot dữ liệu mẫu | Chụp từ CSV | Slide 3 |
| 5 | Bảng 10 luật top | Chụp từ CSV | Slide 6 |

---

# 💬 SCRIPT NÓI KHI THUYẾT TRÌNH

## Mở đầu (Slide 1-2):
> "Xin chào thầy cô và các bạn. Hôm nay em sẽ trình bày về mini-project Phân cụm khách hàng dựa trên luật kết hợp.
> 
> Bài toán rất đơn giản: Chúng ta có dữ liệu mua hàng của khách, và muốn chia họ thành các nhóm để marketing phù hợp."

## Khi nói về Luật kết hợp (Slide 4-6):
> "Bước đầu tiên là tìm các luật mua kèm. Ví dụ như 'Ai mua đồng hồ báo thức đỏ thường mua thêm đồng hồ xanh'.
>
> Em đã lọc lấy Top-50 luật có Lift cao nhất. Lift = 85 có nghĩa là khả năng mua kèm cao gấp 85 lần so với ngẫu nhiên!"

## Khi nói về Đặc trưng (Slide 7-8):
> "Tiếp theo, em biến mỗi khách thành một dãy số. Nếu khách mua theo luật nào thì đánh 1, không thì đánh 0.
>
> Em đã thử 2 kiểu: kiểu đơn giản 0/1 và kiểu có trọng số. Kết quả cho thấy dùng trọng số Lift cho Silhouette cao hơn nhiều."

## Khi nói về K-Means (Slide 9-11):
> "K-Means là thuật toán chia nhóm. Em thử K từ 2 đến 10, và K=10 cho kết quả tốt nhất với Silhouette = 0.99.
>
> Nhìn vào biểu đồ PCA, các cluster tách biệt rất rõ ràng, chứng tỏ phân cụm thành công."

## Khi nói về Kết quả (Slide 12-15):
> "Kết quả cho thấy 96.6% khách hàng là phổ thông, nhưng có 2.5% là VIP chi tiêu gấp 12 lần!
>
> Với nhóm VIP, em đề xuất tạo chương trình thành viên riêng, gửi quà sinh nhật, cho mua sớm sản phẩm mới.
>
> Với nhóm đại trà, em đề xuất email marketing và discount codes định kỳ."

## Kết thúc (Slide 17-18):
> "Tóm lại, project này đã thành công trong việc chia khách hàng thành các nhóm có ý nghĩa và đề xuất chiến lược marketing cụ thể cho từng nhóm.
>
> Em xin cảm ơn thầy cô và các bạn đã lắng nghe. Em sẵn sàng trả lời câu hỏi ạ."

---

# ✅ CHECKLIST TRƯỚC KHI NỘP

- [ ] Có đủ 16-18 slide
- [ ] Slide 6: Có bảng 10 luật tiêu biểu
- [ ] Slide 8: Có so sánh 2 kiểu đặc trưng
- [ ] Slide 10: Có biểu đồ Silhouette + giải thích chọn K
- [ ] Slide 11: Có biểu đồ PCA 2D
- [ ] Slide 13-15: Mỗi cluster có đủ: Số lượng, Top luật, Tên (EN+VI), Persona, Marketing
- [ ] Font chữ đủ lớn (tối thiểu 24pt cho nội dung)
- [ ] Không quá 6 bullet points mỗi slide
- [ ] Đã chạy thử thuyết trình 1 lần
