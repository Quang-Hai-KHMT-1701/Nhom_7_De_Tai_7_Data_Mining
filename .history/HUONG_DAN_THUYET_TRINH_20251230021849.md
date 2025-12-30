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

### Nội dung:
```
🎯 Ý TƯỞNG:
Mỗi khách hàng = 1 dãy số thể hiện "có mua theo luật hay không"

📝 VÍ DỤ KHÁCH X:
• Luật 1 (TEACUP → ROSES): 1 (có mua)
• Luật 2 (ALARM RED → GREEN): 0 (không mua)
• Luật 3 (...): 1
• ...
→ Khách X = [1, 0, 1, 0, 1, ...] (50 số)

🔄 2 KIỂU ĐẶC TRƯNG ĐÃ THỬ:
• Kiểu 1 (Baseline): 0/1 đơn giản
• Kiểu 2 (Nâng cao): Nhân với trọng số LIFT
```

### Hình ảnh:
- Bảng ma trận khách hàng × luật (ví dụ 5 khách × 5 luật)

---

## SLIDE 8: SO SÁNH 2 KIỂU ĐẶC TRƯNG (⭐ QUAN TRỌNG)

### Tiêu đề: "So sánh các kiểu tạo đặc trưng"

### Nội dung (bảng):
```
| Kiểu đặc trưng | Silhouette Score | Nhận xét |
|----------------|------------------|----------|
| Binary (0/1) | 0.28 - 0.68 | ❌ Kém |
| Lift weighting | 0.99 | ✅ Xuất sắc |
| Lift × Confidence | 0.99 | ✅ Xuất sắc |
```

### Kết luận:
```
🏆 CHỌN: Lift weighting
📝 LÝ DO: Lift cho biết luật nào "quan trọng" hơn, giúp phân biệt khách tốt hơn
```

### Hình ảnh:
- Biểu đồ cột so sánh Silhouette Score

---

## SLIDE 9: BƯỚC 3 - PHÂN CỤM K-MEANS

### Tiêu đề: "Bước 3: Phân cụm bằng K-Means"

### Nội dung:
```
🔍 K-MEANS LÀ GÌ?
• Chia khách hàng thành K nhóm
• Khách trong CÙNG nhóm → giống nhau
• Khách KHÁC nhóm → khác nhau

⚙️ THUẬT TOÁN:
1. Chọn K tâm cụm ngẫu nhiên
2. Gán mỗi khách vào cụm gần nhất
3. Cập nhật tâm cụm = trung bình các điểm
4. Lặp lại đến khi hội tụ
```

### Hình ảnh:
- Minh họa K-Means với 3 bước (có thể vẽ hoặc tìm online)

---

## SLIDE 10: CHỌN SỐ CỤM K (⭐ QUAN TRỌNG)

### Tiêu đề: "Chọn số cụm K tối ưu"

### Nội dung:
```
📊 PHƯƠNG PHÁP: Silhouette Score
• Thử K = 2, 3, 4, ..., 10
• Silhouette càng gần 1 càng tốt

🏆 KẾT QUẢ:
• K = 10 cho Silhouette = 0.9933 (cao nhất!)
• Các cluster tách biệt gần như hoàn hảo
```

### 📷 HÌNH ẢNH CẦN CÓ:
- **Biểu đồ Silhouette theo K** 
- Lấy từ: `data/processed/silhouette_plot.png`

---

## SLIDE 11: TRỰC QUAN HÓA PCA (⭐ QUAN TRỌNG)

### Tiêu đề: "Trực quan hóa: Các cụm có tách biệt không?"

### Nội dung:
```
📊 PHƯƠNG PHÁP: PCA giảm chiều về 2D
• PC1: giải thích 93.3% sự khác biệt
• PC2: giải thích 1.9% sự khác biệt
• Tổng: 95.2% thông tin được giữ lại

✅ NHẬN XÉT:
• Cluster 0 (mainstream) tập trung bên TRÁI
• Cluster 1 (VIP) nằm bên PHẢI
• Các cluster TÁCH BIỆT RÕ RÀNG → Phân cụm tốt!
```

### 📷 HÌNH ẢNH CẦN CÓ:
- **Biểu đồ PCA 2D scatter plot**
- Lấy từ: `data/processed/pca_scatter.png`

---

## SLIDE 12: TỔNG QUAN KẾT QUẢ PHÂN CỤM

### Tiêu đề: "Kết quả: 10 nhóm khách hàng"

### Nội dung (bảng):
```
| Cluster | Số KH | % | Recency | Frequency | Monetary |
|---------|-------|---|---------|-----------|----------|
| 0 | 3,788 | 96.6% | 93 ngày | 4 đơn | £1,808 |
| 1 | 99 | 2.5% | 59 ngày | 25 đơn | £21,284 |
| 2 | 6 | 0.2% | 37 ngày | 10 đơn | £3,484 |
| ... | ... | ... | ... | ... | ... |
```

### Nhận xét:
```
📊 PHÂN BỐ LONG-TAIL:
• 96.6% là khách phổ thông (Cluster 0)
• 2.5% là VIP siêu chi tiêu (Cluster 1)
• Còn lại là các nhóm đặc thù
```

---

## SLIDE 13: CLUSTER 0 - KHÁCH HÀNG ĐẠI TRÀ (⭐ QUAN TRỌNG)

### Tiêu đề: "Cluster 0: Regular Customers - Khách Hàng Đại Trà"

### Nội dung:
```
📊 THỐNG KÊ:
• Số lượng: 3,788 khách (96.6%)
• Recency: 93 ngày (mua cách đây ~3 tháng)
• Frequency: 4 đơn/năm
• Monetary: £1,808/năm

🎭 PERSONA:
"Khách hàng bình thường, mua sắm thỉnh thoảng, chi tiêu vừa phải"

🎯 TOP LUẬT CỦA CLUSTER:
• Thường mua các sản phẩm phổ thông
• Không theo pattern đặc biệt nào

📣 CHIẾN LƯỢC MARKETING:
• Email marketing định kỳ với sản phẩm mới
• Cross-sell: Gợi ý sản phẩm liên quan
• Discount codes để kích thích mua hàng
```

---

## SLIDE 14: CLUSTER 1 - KHÁCH VIP (⭐ QUAN TRỌNG)

### Tiêu đề: "Cluster 1: VIP Champions - Khách Hàng Thượng Đế"

### Nội dung:
```
📊 THỐNG KÊ:
• Số lượng: 99 khách (2.5%)
• Recency: 59 ngày (mua gần đây!)
• Frequency: 25 đơn/năm (gấp 6 lần!)
• Monetary: £21,284/năm (gấp 12 lần!)

🎭 PERSONA:
"TOP CUSTOMERS! Mua rất thường xuyên, chi tiêu cực cao. 
1 khách VIP = 12 khách thường!"

🎯 TOP LUẬT CỦA CLUSTER:
• Hay mua bộ sưu tập (TEACUP sets, ALARM CLOCK sets)
• Mua theo combo có lift cao

📣 CHIẾN LƯỢC MARKETING:
• 🌟 VIP Program riêng
• 🎯 Personal Shopper
• 🚀 Early Access sản phẩm mới
• 🎁 Quà sinh nhật, lễ tết
• ⚠️ KHÔNG BAO GIỜ để mất nhóm này!
```

---

## SLIDE 15: CÁC CLUSTER KHÁC (tùy chọn)

### Tiêu đề: "Các nhóm khách hàng khác"

### Nội dung:
```
🟢 CLUSTER 2, 7, 8: Rising Stars - Ngôi Sao Đang Lên
• Recency thấp (9-37 ngày) → đang active!
• Chiến lược: Nurturing để convert thành VIP

🔴 CLUSTER 9: Lost Customer - Đã Rời Bỏ
• Recency: 277 ngày (gần 1 năm!)
• Chiến lược: Win-back email với discount 20-30%

🟡 CLUSTER 3, 4, 5, 6: Niche Segments
• Các nhóm đặc thù cần phân tích thêm
```

---

## SLIDE 16: SO SÁNH THUẬT TOÁN (tùy chọn)

### Tiêu đề: "So sánh các thuật toán Clustering"

### Nội dung (bảng):
```
| Thuật toán | Silhouette | Ưu điểm | Nhược điểm |
|------------|------------|---------|------------|
| K-Means | 0.9933 | Nhanh, dễ hiểu | Phải chọn K |
| Agglomerative | 0.9934 | Có dendrogram | Chậm |
| DBSCAN | 0.9973 | Tự tìm K | Bỏ outliers |
```

### Kết luận:
```
🏆 CHỌN K-MEANS vì:
• Silhouette rất cao
• Không bỏ sót khách hàng nào
• Dễ giải thích cho marketing
```

### 📷 HÌNH ẢNH CẦN CÓ:
- Biểu đồ so sánh (nếu có)
- Lấy từ: `data/processed/algorithm_comparison_visual.png`

---

## SLIDE 17: KẾT LUẬN

### Tiêu đề: "Kết luận"

### Nội dung:
```
✅ VỀ KỸ THUẬT:
1. Apriori = FP-Growth (cùng kết quả, FP-Growth nhanh hơn)
2. Dùng LIFT làm trọng số → Silhouette tăng từ 0.68 lên 0.99
3. Top-50 luật tốt hơn Top-200 (ít nhiễu hơn)
4. K-Means phù hợp nhất cho bài toán này

✅ VỀ KINH DOANH:
1. 96.6% khách là phổ thông → Marketing đại chúng
2. 2.5% là VIP → Chăm sóc đặc biệt (giá trị gấp 12 lần!)
3. Có khách đang active → Cơ hội convert VIP
4. Có khách đã mất → Cần win-back campaign
```

---

## SLIDE 18: CẢM ƠN & Q&A

### Tiêu đề: "Cảm ơn đã lắng nghe!"

### Nội dung:
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
