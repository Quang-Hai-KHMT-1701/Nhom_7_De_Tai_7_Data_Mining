# 📽️ HƯỚNG DẪN LÀM SLIDE THUYẾT TRÌNH
# Mini-Project: Phân cụm khách hàng dựa trên luật kết hợp

> **Dành cho người không biết gì** - Cứ theo đúng khung này là đủ điểm!

---

## 🎯 TỔNG QUAN: BẠN CẦN BAO NHIÊU SLIDE?

| STT | Nội dung | Số slide | Thời gian |
|-----|----------|----------|-----------|
| 1 | Giới thiệu & Dữ liệu | 2 | 2 phút |
| 2 | Bước 1: Tìm luật kết hợp | 3 | 3 phút |
| 3 | Bước 2: Tạo đặc trưng | 2 | 2 phút |
| 4 | Bước 3: Phân cụm K-Means | 3 | 3 phút |
| 5 | Kết quả: Phân tích từng cluster | 4-5 | 4 phút |
| 6 | Kết luận & Q&A | 2 | 1 phút |
| **Tổng** | | **16-17 slide** | **15 phút** |

---

# 📑 CHI TIẾT TỪNG SLIDE

---

## SLIDE 1: TRANG BÌA

### Nội dung:
```
MINI-PROJECT: PHÂN CỤM KHÁCH HÀNG DỰA TRÊN LUẬT KẾT HỢP

Môn: Data Mining
Sinh viên: [Tên của bạn]
MSSV: [Mã số]
Ngày: 30/12/2024
```

### Hình ảnh: 
- Logo trường (nếu có)
- Hình minh họa shopping/customer segmentation

---

## SLIDE 2: BÀI TOÁN ĐẶT RA

### Tiêu đề: "Chúng ta đang giải quyết vấn đề gì?"

### Nội dung (bullet points):
```
❓ VẤN ĐỀ:
• Cửa hàng có hàng nghìn khách hàng
• Mỗi người mua những sản phẩm khác nhau
• Làm sao biết khách nào GIỐNG nhau để marketing đúng cách?

💡 GIẢI PHÁP:
• Bước 1: Tìm các luật "mua kèm" (Apriori/FP-Growth)
• Bước 2: Biến luật thành "dấu vân tay" của mỗi khách
• Bước 3: Dùng K-Means gom khách giống nhau vào 1 nhóm
```

### Hình ảnh:
- Sơ đồ Pipeline (có trong file miniproject.md - Phần 1.3)

---

## SLIDE 3: GIỚI THIỆU DỮ LIỆU

### Tiêu đề: "Dữ liệu Online Retail UK"

### Nội dung (dạng bảng):
```
| Thông tin | Giá trị |
|-----------|---------|
| Nguồn | UCI Machine Learning Repository |
| Thời gian | 01/12/2010 - 09/12/2011 |
| Số giao dịch | ~400,000 transactions |
| Số khách hàng | 3,921 (sau khi lọc UK) |
| Số sản phẩm | ~3,600 SKU |
```

### Hình ảnh:
- Screenshot bảng dữ liệu mẫu (5-10 dòng đầu)
- 📷 Lấy từ: `data/processed/cleaned_uk_data.csv`

---

## SLIDE 4: BƯỚC 1 - TÌM LUẬT KẾT HỢP

### Tiêu đề: "Bước 1: Tìm các luật MUA KÈM"

### Nội dung:
```
🔍 LUẬT KẾT HỢP LÀ GÌ?
• Ví dụ: "Ai mua Bánh mì thường mua thêm Bơ"
• Viết: {Bánh mì} → {Bơ}

📊 3 CHỈ SỐ QUAN TRỌNG:
• Support: Phổ biến như thế nào? (5% = cứ 100 đơn có 5 đơn)
• Confidence: Chắc chắn như thế nào? (80% = 10 người mua A thì 8 người mua B)
• Lift: Bất ngờ như thế nào? (Lift > 1 = luật có ý nghĩa)

⚙️ THUẬT TOÁN SỬ DỤNG:
• Apriori và FP-Growth (cả 2 cho kết quả giống nhau!)
```

### Hình ảnh:
- Sơ đồ minh họa {A} → {B} với mũi tên

---

## SLIDE 5: CÁCH CHỌN LUẬT (⭐ QUAN TRỌNG - CÔ CHẤM MẠNH)

### Tiêu đề: "Cách chọn luật kết hợp"

### Nội dung:
```
📋 TIÊU CHÍ LỌC LUẬT:
• min_support = 0.01 (1%)
• min_confidence = 0.3 (30%)
• Lift > 1 (luật có ý nghĩa)

🏆 CÁCH CHỌN TOP-K:
• Sắp xếp theo LIFT giảm dần
• Lấy TOP 50 luật tốt nhất
• Tại sao 50? → Thử nghiệm cho thấy Top-50 có Silhouette cao nhất (0.99)
```

### Hình ảnh:
- Bảng so sánh Top-50 vs Top-100 vs Top-200 (có trong miniproject.md)

---

## SLIDE 6: 10 LUẬT TIÊU BIỂU (⭐ QUAN TRỌNG)

### Tiêu đề: "10 Luật mua kèm tiêu biểu"

### Nội dung (dạng bảng):
```
| # | Luật (Antecedent → Consequent) | Lift | Confidence |
|---|-------------------------------|------|------------|
| 1 | {PINK REGENCY TEACUP} → {ROSES REGENCY TEACUP} | 85.2 | 0.65 |
| 2 | {GREEN REGENCY TEACUP} → {ROSES REGENCY TEACUP} | 78.4 | 0.58 |
| 3 | {ALARM CLOCK RED} → {ALARM CLOCK GREEN} | 72.1 | 0.52 |
| ... | ... | ... | ... |
```

### 📷 Lấy từ đâu:
- File: `data/processed/rules_apriori_filtered.csv`
- Sắp xếp theo lift giảm dần, lấy 10 dòng đầu

### Ghi chú khi nói:
> "Như các bạn thấy, luật đầu tiên có Lift = 85.2, nghĩa là người mua PINK REGENCY TEACUP có khả năng mua ROSES REGENCY TEACUP cao gấp 85 lần so với ngẫu nhiên!"

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
