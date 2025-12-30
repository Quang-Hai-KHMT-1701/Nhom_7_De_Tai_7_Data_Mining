# 🛒 BÁO CÁO MINI PROJECT
# PHÂN CỤM KHÁCH HÀNG DỰA TRÊN LUẬT KẾT HỢP
### Môn: Data Mining | Ngày: 30/12/2025

---

# 📖 PHẦN 1: GIỚI THIỆU

## 1.1. Bài toán chúng ta đang giải quyết là gì?

**Tình huống thực tế**: Bạn là chủ một cửa hàng bán lẻ online. Bạn có hàng nghìn khách hàng, mỗi người mua những sản phẩm khác nhau. Bạn muốn biết:

> ❓ "Làm sao để biết khách hàng nào giống nhau để có chiến lược marketing phù hợp?"

**Giải pháp**: Dùng kỹ thuật **Phân cụm (Clustering)** để nhóm các khách hàng có hành vi mua hàng tương tự vào cùng một nhóm.

## 1.2. Dữ liệu chúng ta có là gì?

| Thông tin | Chi tiết |
|-----------|----------|
| **Dataset** | Online Retail Dataset (UK) |
| **Nguồn** | UCI Machine Learning Repository |
| **Nội dung** | Dữ liệu giao dịch của một cửa hàng bán lẻ trực tuyến tại Anh |
| **Thời gian** | 01/12/2010 - 09/12/2011 (khoảng 1 năm) |
| **Số giao dịch** | Hàng trăm nghìn transactions |
| **Số khách hàng** | 3,921 khách hàng (sau khi lọc) |

## 1.3. Pipeline (Quy trình) thực hiện

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  1. Dữ liệu     │───▶│  2. Khai phá    │───▶│  3. Tạo đặc     │
│     giao dịch   │    │     luật kết    │    │     trưng cho   │
│                 │    │     hợp         │    │     khách hàng  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                      │
                                                      ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  6. Đề xuất     │◀───│  5. Phân tích   │◀───│  4. Phân cụm    │
│     chiến lược  │    │     từng nhóm   │    │     K-Means     │
│     marketing   │    │     khách hàng  │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

---

# 📖 PHẦN 2: GIẢI THÍCH CÁC KHÁI NIỆM CƠ BẢN

## 2.1. Luật Kết Hợp (Association Rules) là gì?

**Ví dụ đời thường**: Khi bạn đi siêu thị, bạn có để ý là:
- Người mua **bánh mì** thường cũng mua **bơ**
- Người mua **bia** thường cũng mua **đồ nhắm**
- Người mua **tã em bé** thường cũng mua **bia** (câu chuyện nổi tiếng của Walmart!)

**Luật kết hợp** là cách máy tính tự động tìm ra các mối quan hệ như vậy từ dữ liệu.

### Cấu trúc một luật:

```
{Sản phẩm A} → {Sản phẩm B}
  Tiền đề       Kết quả
(Antecedent)  (Consequent)

Ví dụ: {Bánh mì} → {Bơ}
Nghĩa là: Người mua bánh mì có xu hướng mua bơ
```

### Ba chỉ số quan trọng của một luật:

| Chỉ số | Ý nghĩa dễ hiểu | Công thức | Ví dụ |
|--------|-----------------|-----------|-------|
| **Support** | "Phổ biến như thế nào?" | Số đơn có cả A và B / Tổng đơn | 5% nghĩa là cứ 100 đơn thì có 5 đơn mua cả bánh mì và bơ |
| **Confidence** | "Chắc chắn như thế nào?" | Số đơn có cả A và B / Số đơn có A | 80% nghĩa là 10 người mua bánh mì thì 8 người cũng mua bơ |
| **Lift** | "Bất ngờ như thế nào?" | Confidence / P(B) | Lift=3 nghĩa là mua bánh mì làm tăng gấp 3 lần khả năng mua bơ |

### Giải thích Lift chi tiết hơn:

- **Lift > 1**: Mua A làm TĂNG khả năng mua B ✅ (Luật có ý nghĩa)
- **Lift = 1**: A và B không liên quan ❌
- **Lift < 1**: Mua A làm GIẢM khả năng mua B ❌

## 2.2. Hai thuật toán khai phá luật: Apriori và FP-Growth

| Thuật toán | Cách hoạt động | Ưu điểm | Nhược điểm |
|------------|---------------|---------|------------|
| **Apriori** | Duyệt qua database nhiều lần, tìm từ tập 1 item → 2 items → 3 items... | Đơn giản, dễ hiểu | Chậm với dữ liệu lớn |
| **FP-Growth** | Xây dựng cây FP-Tree, khai phá trực tiếp từ cây | Nhanh hơn nhiều | Phức tạp hơn |

> 💡 **Kết luận quan trọng**: Cả hai thuật toán cho **KẾT QUẢ GIỐNG NHAU** (cùng tập luật), chỉ khác nhau về tốc độ!

## 2.3. Phân cụm K-Means là gì?

**Ý tưởng đơn giản**: Chia khách hàng thành K nhóm sao cho:
- Khách hàng trong **cùng nhóm** thì **giống nhau** nhất có thể
- Khách hàng **khác nhóm** thì **khác nhau** nhất có thể

### Ví dụ minh họa:

```
Trước phân cụm:          Sau phân cụm (K=3):
     ● ○ ●                    🔴 🔴 🔴
   ○ ● ○ ●                  🔵 🔵 🔵 🔵
     ○ ● ○                    🟢 🟢 🟢
   ● ○ ●                    🔴 🔴 🔴
```

## 2.4. Silhouette Score - Đo chất lượng phân cụm

**Câu hỏi**: Làm sao biết phân cụm tốt hay không?

**Trả lời**: Dùng Silhouette Score!

| Giá trị | Ý nghĩa |
|---------|---------|
| **+1** | Hoàn hảo! Cluster rất chặt, tách biệt rõ ràng |
| **0.7 - 1.0** | Rất tốt ✅ |
| **0.5 - 0.7** | Khá tốt |
| **0.25 - 0.5** | Yếu |
| **< 0.25** | Không có cấu trúc cluster rõ ràng |
| **< 0** | Phân cụm sai! |

## 2.5. RFM - Cách đánh giá khách hàng

RFM là bộ 3 chỉ số kinh điển trong marketing:

| Chữ cái | Tên đầy đủ | Ý nghĩa | Cách tính | Giá trị tốt |
|---------|-----------|---------|-----------|-------------|
| **R** | Recency | "Mua gần đây không?" | Số ngày từ lần mua cuối | Càng NHỎ càng tốt |
| **F** | Frequency | "Mua thường xuyên không?" | Số đơn hàng | Càng LỚN càng tốt |
| **M** | Monetary | "Chi tiêu nhiều không?" | Tổng tiền đã chi | Càng LỚN càng tốt |

**Ví dụ**:
- Khách A: R=10, F=20, M=£5000 → Khách VIP! (mới mua, mua nhiều, chi nhiều)
- Khách B: R=300, F=1, M=£50 → Khách đã rời bỏ (lâu không mua, mua ít)

---

# 📖 PHẦN 3: KẾT QUẢ THÍ NGHIỆM

## 3.1. Thí nghiệm so sánh: Tìm cấu hình tốt nhất

### Chúng tôi thử nghiệm gì?

Chúng tôi thử **18 tổ hợp** khác nhau bằng cách thay đổi:

| Tham số | Các giá trị thử | Giải thích |
|---------|-----------------|------------|
| **Nguồn luật** | Apriori, FP-Growth | 2 thuật toán sinh luật |
| **Cách đánh trọng số** | none, lift, lift×confidence | 3 cách |
| **Số lượng luật** | 50, 100, 200 | 3 mức |

**Tổng cộng**: 2 × 3 × 3 = **18 thí nghiệm**

### Kết quả Top 5:

| Hạng | Thuật toán | Trọng số | Số luật | Số cluster | Silhouette |
|------|------------|----------|---------|------------|------------|
| 🥇 | Apriori | lift | 50 | 10 | **0.9933** |
| 🥇 | FP-Growth | lift | 50 | 10 | **0.9933** |
| 🥉 | Apriori | lift×conf | 50 | 10 | 0.9929 |
| 🥉 | FP-Growth | lift×conf | 50 | 10 | 0.9929 |
| 5 | Apriori | lift×conf | 100 | 2 | 0.9356 |

### Phát hiện quan trọng:

#### 🔍 Phát hiện 1: Apriori = FP-Growth
> Hai thuật toán cho **KẾT QUẢ HOÀN TOÀN GIỐNG NHAU**!
> 
> Điều này chứng minh cả hai thuật toán đều đúng. Sự khác biệt chỉ là tốc độ (FP-Growth nhanh hơn).

#### 🔍 Phát hiện 2: Dùng Lift quan trọng hơn
> Khi dùng **lift** để đánh trọng số: Silhouette = **0.99** (xuất sắc!)
> 
> Khi **không dùng** trọng số (binary): Silhouette = **0.28-0.68** (kém hơn nhiều)
>
> **Giải thích**: Lift cho biết luật nào "đáng chú ý" hơn, giúp phân biệt khách hàng tốt hơn.

#### 🔍 Phát hiện 3: Ít luật hơn = Tốt hơn
> - Top **50 luật**: Silhouette = 0.99 ✅
> - Top 100 luật: Silhouette = 0.92-0.94
> - Top 200 luật: Silhouette = 0.85-0.89
>
> **Giải thích**: Chỉ giữ luật quan trọng nhất = ít nhiễu hơn = phân cụm tốt hơn.

## 3.2. Cấu hình tốt nhất được chọn

| Tham số | Giá trị được chọn |
|---------|-------------------|
| Thuật toán sinh luật | **Apriori** |
| Cách đánh trọng số | **lift** |
| Số lượng luật | **50 luật tốt nhất** |
| Số cluster K | **10** |
| Silhouette Score | **0.9933** (gần như hoàn hảo!) |

---

# 📖 PHẦN 4: PHÂN TÍCH 10 NHÓM KHÁCH HÀNG

## 4.1. Tổng quan phân bố

```
╔════════════════════════════════════════════════════════════════╗
║                    PHÂN BỐ 3,921 KHÁCH HÀNG                    ║
╠════════════════════════════════════════════════════════════════╣
║  Cluster 0: ████████████████████████████████████████ 96.6%     ║
║  Cluster 1: ██ 2.5%                                            ║
║  Cluster 2-9: █ 0.9% (tổng)                                    ║
╚════════════════════════════════════════════════════════════════╝
```

**Nhận xét**: Đây là hiện tượng **Long-tail** rất phổ biến trong retail:
- **1 nhóm lớn** (96.6%): Khách hàng phổ thông
- **Nhiều nhóm nhỏ**: Khách hàng đặc biệt (VIP, niche, outliers)

## 4.2. Bảng RFM chi tiết theo từng cluster

| Cluster | Số KH | % | Recency (ngày) | Frequency (đơn) | Monetary (£) |
|---------|-------|---|----------------|-----------------|--------------|
| **0** | 3,788 | 96.6% | 93 ngày | 4 đơn | £1,808 |
| **1** | 99 | 2.5% | 59 ngày | **25 đơn** ⭐ | **£21,284** ⭐ |
| **2** | 6 | 0.2% | **37 ngày** | 10 đơn | £3,484 |
| **3** | 7 | 0.2% | 82 ngày | 4 đơn | £1,321 |
| **4** | 3 | 0.1% | 92 ngày | 3 đơn | £521 |
| **5** | 7 | 0.2% | 73 ngày | 5 đơn | £1,538 |
| **6** | 4 | 0.1% | 78 ngày | 4 đơn | £1,242 |
| **7** | 3 | 0.1% | **28 ngày** | 11 đơn | £3,884 |
| **8** | 3 | 0.1% | **9 ngày** ⭐ | 7 đơn | £2,774 |
| **9** | 1 | 0.0% | **277 ngày** ❌ | 1 đơn | £392 |

## 4.3. Phân tích chi tiết và đặt tên từng cluster

### 🔵 CLUSTER 0: "Regular Customers" - Khách Hàng Bình Thường
**Tên tiếng Việt**: Khách Hàng Đại Trà

**Số lượng**: 3,788 khách (96.6%)

| Chỉ số | Giá trị | Nhận xét |
|--------|---------|----------|
| Recency | 93 ngày | Trung bình, khoảng 3 tháng trước |
| Frequency | 4 đơn | Mua thỉnh thoảng |
| Monetary | £1,808 | Chi tiêu vừa phải |

**Persona**: 👤 "Đây là nhóm khách hàng phổ thông, mua sắm không quá thường xuyên, chi tiêu ở mức trung bình. Họ cần được nhắc nhở và khuyến khích mua thêm."

**Chiến lược Marketing**:
- 📧 Email marketing định kỳ với các sản phẩm mới
- 🎁 Cross-sell: Gợi ý sản phẩm liên quan đến những gì họ đã mua
- 💰 Discount codes để kích thích mua hàng

---

### 🟠 CLUSTER 1: "VIP Champions" - Khách Hàng VIP
**Tên tiếng Việt**: Khách Hàng Thượng Đế

**Số lượng**: 99 khách (2.5%)

| Chỉ số | Giá trị | So với Cluster 0 |
|--------|---------|------------------|
| Recency | 59 ngày | Gần hơn 34 ngày |
| Frequency | **25 đơn** | **Gấp 6 lần!** |
| Monetary | **£21,284** | **Gấp 12 lần!** |

**Persona**: 👑 "Đây là TOP CUSTOMERS! Họ mua rất thường xuyên (trung bình 2 đơn/tháng), chi tiêu cực kỳ cao. Mỗi khách này đáng giá bằng 12 khách thường!"

**Chiến lược Marketing**:
- 🌟 **VIP Program**: Tạo chương trình khách hàng thân thiết riêng
- 🎯 **Personal Shopper**: Nhân viên chăm sóc riêng
- 🚀 **Early Access**: Được mua trước khi ra mắt sản phẩm mới
- 🎁 **Exclusive Gifts**: Quà tặng đặc biệt vào dịp sinh nhật, lễ tết
- ⚠️ **Chú ý**: KHÔNG BAO GIỜ để mất những khách hàng này!

---

### 🟢 CLUSTER 2: "Rising Stars" - Ngôi Sao Đang Lên
**Tên tiếng Việt**: Khách Hàng Tiềm Năng

**Số lượng**: 6 khách (0.2%)

| Chỉ số | Giá trị | Nhận xét |
|--------|---------|----------|
| Recency | **37 ngày** | Rất gần đây! |
| Frequency | 10 đơn | Khá cao |
| Monetary | £3,484 | Gấp 2 lần Cluster 0 |

**Persona**: 🌱 "Khách hàng mới nhưng đang phát triển nhanh. Họ có tiềm năng trở thành VIP nếu được chăm sóc tốt."

**Chiến lược Marketing**:
- 🎯 **Nurturing Campaign**: Chăm sóc đặc biệt để convert thành VIP
- 📈 **Upsell**: Giới thiệu sản phẩm cao cấp hơn
- 🎁 **Loyalty Points**: Tích điểm để khuyến khích mua thêm

---

### 🟡 CLUSTER 7 & 8: "Active Buyers" - Khách Hàng Năng Động
**Tên tiếng Việt**: Khách Hàng Đang Hoạt Động

**Số lượng**: 6 khách (0.2%)

| Cluster | Recency | Frequency | Monetary |
|---------|---------|-----------|----------|
| **7** | 28 ngày | 11 đơn | £3,884 |
| **8** | **9 ngày** | 7 đơn | £2,774 |

**Persona**: 🔥 "Đây là những khách hàng đang RẤT ACTIVE. Cluster 8 đặc biệt mới mua chỉ 9 ngày trước!"

**Chiến lược Marketing**:
- 🎯 **Strike while hot**: Gửi offer ngay khi họ còn đang active
- 📦 **Bundle Deals**: Gợi ý mua combo với giá tốt hơn
- 🔔 **Push Notifications**: Thông báo ngay khi có deal mới

---

### ⚫ CLUSTER 9: "Lost Customer" - Khách Hàng Đã Mất
**Tên tiếng Việt**: Khách Hàng Đã Rời Bỏ

**Số lượng**: 1 khách

| Chỉ số | Giá trị | Nhận xét |
|--------|---------|----------|
| Recency | **277 ngày** | Gần 1 năm không mua! |
| Frequency | 1 đơn | Chỉ mua đúng 1 lần |
| Monetary | £392 | Rất ít |

**Persona**: 💀 "Khách hàng đã rời bỏ (churned). Có thể họ không hài lòng hoặc đã tìm được nơi mua khác."

**Chiến lược Marketing**:
- 📧 **Win-back Email**: "Chúng tôi nhớ bạn!" với discount lớn (20-30%)
- 📝 **Survey**: Hỏi lý do không quay lại để cải thiện
- ⚠️ **Lưu ý**: Xác suất win-back thấp, không nên đầu tư quá nhiều

---

### 🔘 CLUSTER 3, 4, 5, 6: "Niche Segments" - Các Nhóm Đặc Thù
**Tên tiếng Việt**: Khách Hàng Thị Trường Ngách

**Số lượng**: 21 khách (0.5%)

Đây là các nhóm nhỏ với hành vi mua hàng đặc thù, có thể là:
- Khách hàng mua theo mùa (seasonal buyers)
- Khách hàng mua cho doanh nghiệp (B2B)
- Khách hàng mua làm quà tặng

**Chiến lược**: Cần phân tích sâu hơn top rules của từng cluster để hiểu hành vi cụ thể.

---

# 📖 PHẦN 5: SO SÁNH THUẬT TOÁN CLUSTERING (NÂNG CAO)

## 5.1. Các thuật toán được so sánh

| Thuật toán | Loại | Đặc điểm |
|------------|------|----------|
| **K-Means** | Centroid-based | Chia thành K nhóm dựa trên tâm cụm |
| **Agglomerative** | Hierarchical | Ghép dần các điểm gần nhau thành cluster |
| **DBSCAN** | Density-based | Tìm vùng có mật độ cao, tự phát hiện outliers |

## 5.2. Kết quả so sánh

| Thuật toán | Silhouette | Davies-Bouldin | Calinski-Harabasz | Noise |
|------------|------------|----------------|-------------------|-------|
| **DBSCAN (eps=0.5)** | **0.9973** ⭐ | **0.0027** ⭐ | **9,995,118** ⭐ | 69 |
| DBSCAN (eps=1.0) | 0.9971 | 0.0037 | 5,634,274 | 35 |
| Agglomerative (ward) | 0.9934 | 0.5641 | 64,460 | 0 |
| **K-Means** | 0.9933 | 0.3306 | 62,506 | 0 |

### Giải thích các metrics:
- **Silhouette**: Cao hơn = cluster tốt hơn (max = 1)
- **Davies-Bouldin**: Thấp hơn = cluster tốt hơn (min = 0)
- **Calinski-Harabasz**: Cao hơn = cluster tốt hơn
- **Noise**: Số điểm bị coi là outlier (chỉ DBSCAN)

## 5.3. Kết luận so sánh thuật toán

| Thuật toán | Ưu điểm | Nhược điểm | Phù hợp khi |
|------------|---------|------------|-------------|
| **K-Means** | Nhanh, dễ hiểu, không bỏ khách hàng nào | Phải chọn K trước | Cần phân khúc rõ ràng ✅ |
| **Agglomerative** | Không cần K, có dendrogram | Chậm với dữ liệu lớn | Muốn xem cấu trúc phân cấp |
| **DBSCAN** | Tự tìm K, phát hiện outliers | Bỏ qua một số khách hàng | Dữ liệu có nhiều outliers |

> 🎯 **Kết luận**: Với bài toán phân khúc khách hàng, **K-Means là lựa chọn tốt nhất** vì:
> - Silhouette rất cao (0.9933)
> - Không bỏ sót khách hàng nào
> - Dễ giải thích cho bộ phận marketing

---

# 📖 PHẦN 6: TRỰC QUAN HÓA

## 6.1. Biểu đồ Silhouette Score theo K

```
Silhouette Score
     ^
0.99 │            ●─────●─────●
     │        ●──●
0.98 │    ●──●
     │ ●
0.97 │
     │
     └──────────────────────────────▶ K
       2   3   4   5   6   7   8   9  10
       
Nhận xét: Score tăng dần và đạt MAX tại K=10
```

## 6.2. Biểu đồ PCA 2D

**PCA là gì?**: Là kỹ thuật giảm chiều dữ liệu để có thể vẽ trên mặt phẳng 2D.

**Kết quả PCA của chúng tôi**:
- **PC1** (trục X): Giải thích **93.3%** sự khác biệt giữa các khách hàng
- **PC2** (trục Y): Giải thích **1.9%** sự khác biệt
- **Tổng**: **95.2%** thông tin được giữ lại

**Ý nghĩa của biểu đồ**:
- Cluster 0 (mainstream) tập trung ở **bên trái**
- Cluster 1 (VIP) nằm ở **bên phải**
- Các cluster nhỏ (2-9) **phân tán rõ ràng**, không chồng lấn

> 💡 Việc các cluster tách biệt rõ ràng chứng minh Silhouette = 0.99 là chính xác!

---

# 📖 PHẦN 7: KẾT LUẬN VÀ ĐỀ XUẤT

## 7.1. Kết luận chính

### ✅ Về kỹ thuật:
1. **Apriori và FP-Growth cho kết quả giống nhau** - Chọn FP-Growth nếu dữ liệu lớn (nhanh hơn)
2. **Dùng Lift làm trọng số là quan trọng** - Tăng Silhouette từ 0.68 lên 0.99
3. **Chọn Top-50 luật tốt hơn Top-200** - Ít hơn nhưng chất lượng hơn
4. **K-Means là thuật toán phù hợp nhất** cho bài toán phân khúc khách hàng

### ✅ Về kinh doanh:
1. **96.6% khách hàng là phổ thông** - Cần chiến lược đại chúng
2. **2.5% là VIP, đóng góp doanh thu cực lớn** - Ưu tiên chăm sóc đặc biệt
3. **Có khách hàng đang active (Cluster 7, 8)** - Cơ hội convert thành VIP
4. **Có khách hàng đã mất (Cluster 9)** - Cần win-back campaign

## 7.2. Đề xuất chiến lược Marketing

| Cluster | Tên | Chiến lược | Hành động cụ thể |
|---------|-----|------------|------------------|
| **0** | Regular | Retention & Growth | Email marketing, cross-sell, discount codes |
| **1** | VIP | Premium Care | VIP program, personal shopper, exclusive offers |
| **2, 7, 8** | Rising Stars | Nurturing | Upsell, loyalty program, personalized offers |
| **3, 4, 5, 6** | Niche | Targeted | Phân tích thêm, campaign theo mùa |
| **9** | Churned | Win-back | "We miss you" email, big discount, survey |

## 7.3. Files đã xuất

| File | Nội dung |
|------|----------|
| `clustering_comparison.csv` | Bảng so sánh 18 thí nghiệm |
| `customer_cluster_labels.csv` | Label cluster cho 3,921 khách hàng |
| `cluster_profiles_detailed.csv` | RFM và top rules của từng cluster |
| `silhouette_table.csv` | Silhouette score theo K |
| `algorithm_comparison.csv` | So sánh K-Means, Agglomerative, DBSCAN |
| `silhouette_plot.png` | Biểu đồ Silhouette |
| `pca_scatter.png` | Biểu đồ PCA 2D |
| `algorithm_comparison_visual.png` | So sánh trực quan các thuật toán |

---

# 📖 PHẦN 8: PHỤ LỤC - TỪ ĐIỂN THUẬT NGỮ

| Thuật ngữ | Tiếng Việt | Giải thích đơn giản |
|-----------|------------|---------------------|
| **Association Rules** | Luật Kết Hợp | Các quy tắc "nếu mua A thì thường mua B" |
| **Support** | Độ Hỗ Trợ | Độ phổ biến của một luật |
| **Confidence** | Độ Tin Cậy | Độ chắc chắn của một luật |
| **Lift** | Độ Nâng | Độ "bất ngờ" của một luật |
| **Clustering** | Phân Cụm | Nhóm các đối tượng giống nhau lại với nhau |
| **K-Means** | K-Means | Thuật toán phân cụm dựa trên tâm (centroid) |
| **Silhouette Score** | Điểm Silhouette | Điểm đánh giá chất lượng phân cụm (-1 đến +1) |
| **RFM** | RFM | 3 chỉ số: Recency, Frequency, Monetary |
| **PCA** | Phân Tích Thành Phần Chính | Kỹ thuật giảm chiều để trực quan hóa |
| **Centroid** | Tâm Cụm | Tâm của một cluster |
| **Outlier** | Điểm Ngoại Lệ | Điểm dữ liệu bất thường |
| **Agglomerative** | Cụm Kết Tụ | Thuật toán ghép dần các điểm gần nhau |
| **DBSCAN** | DBSCAN | Thuật toán phân cụm dựa trên mật độ |

---

# 🙏 CẢM ƠN ĐÃ LẮNG NGHE!

## Q&A - Câu hỏi thường gặp

**Q: Tại sao chọn K=10 mà không phải K=5?**
> A: Vì Silhouette score cao nhất tại K=10 (0.9933). Các nhóm nhỏ (cluster 2-9) tuy ít khách nhưng có hành vi rất đặc trưng.

**Q: Tại sao Apriori và FP-Growth cho kết quả giống nhau?**
> A: Vì cả hai đều tìm ra đúng các luật thỏa mãn min_support và min_confidence. Sự khác biệt chỉ là cách triển khai (tốc độ).

**Q: Silhouette 0.99 có cao không?**
> A: RẤT CAO! Giá trị tốt nhất là 1.0. Score 0.99 nghĩa là các cluster tách biệt gần như hoàn hảo.

**Q: Cluster 0 chiếm 96.6% có phải vấn đề không?**
> A: Không. Đây là hiện tượng Long-tail phổ biến trong retail. Đa số khách hàng có hành vi tương tự, chỉ một số ít có hành vi đặc biệt.

---

*Báo cáo này được tạo từ notebook `comprehensive_clustering_analysis.ipynb`*

*Ngày tạo: 30/12/2024*
