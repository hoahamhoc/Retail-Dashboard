# Retail Performance Dashboard

Dashboard phân tích toàn diện hoạt động bán lẻ — từ doanh số, hiệu suất nhân viên & cửa hàng đến hành vi khách hàng theo mô hình RFM. Mục tiêu là giúp ban quản lý xác định đúng vấn đề và ra quyết định kịp thời.

## [Xem Interactive Dashboard tại đây](https://report.onhandbi.com/public/report?token=eyJhbGciOiJIUzI1NiJ9.eyJwdWJsaWNfbGlua19pZCI6NjU0LCJoYXNfcGFzc2NvZGUiOmZhbHNlLCJ0aW1lIjoxNzc5NjM3NzczfQ.HgQ9m0Nr7Y7STloQHi4Is-y1j-WthUfUCcMlKvD4iak)

---
## Lưu ý về dữ liệu

Dashboard sử dụng bộ dữ liệu mẫu Contoso Retail (Microsoft sample data). Các con số, tên nhân viên, tên cửa hàng chỉ mang tính minh họa cho cấu trúc và logic phân tích — không phản ánh số liệu thực tế của bất kỳ doanh nghiệp nào.

---

## Cấu trúc Dashboard

```
Homepage
   ├── Sales Overview            → Tổng quan doanh số & xu hướng
   ├── Sales Team & Store        → Hiệu suất nhân viên & cửa hàng
   └── Customer Segmentation     → Phân khúc khách hàng & RFM
```

---

## Tổng Quan Flow Phân Tích
 
Dashboard được thiết kế theo flow chẩn đoán từ tổng quan đến chi tiết. 3 trang kết nối với nhau để trả lời 1 câu hỏi duy nhất: **doanh số đang có vấn đề gì và vấn đề đến từ đâu?**
 
```mermaid
flowchart LR
    A([Bước 1\nSales Overview\nGiảm ở đâu?\nĐóng góp % ra sao?]) --> B([Bước 2\nSales Team & Store\nTraffic thấp?\nChuyển đổi kém?])
    B --> C([Bước 3\nCustomer & RFM\nKhách mới hay cũ?\nNhóm nào đang rời đi?])
 
    style A fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#a8d5c2,stroke:#5aab8e
```

---
 
## Hướng Dẫn Đọc Dashboard
 
### Bước 1 — Sales Overview: *"Chuyện gì đang xảy ra và xảy ra khi nào?"*
 
Trang Overview cung cấp 2 góc nhìn:
- **Đóng góp doanh số** theo thời gian, Category, Brand, Country — ai đang chiếm bao nhiêu % tổng revenue
- **So sánh** vs kỳ trước và vs Target — đang tăng/giảm bao nhiêu, đạt hay chưa đạt
Hai góc nhìn này kết hợp theo flow bên dưới:
 
```mermaid
flowchart LR
    A([Doanh số tăng/giảm\nvs kỳ trước?\nĐạt % Target?]) --> B([Revenue By Time\nGiảm tập trung\ntháng nào?])
    B --> C([Lọc giai đoạn đó:\nCategory / Brand / Country\nnào giảm? Đóng góp %?])
    C --> D([Ưu tiên:\nĐóng góp cao + giảm\n= xử lý trước])
 
    style A fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#a8d5c2,stroke:#5aab8e
    style D fill:#c9b8e8,stroke:#9b7fd4
```
 
**Logic đọc:** Xác định **khi nào** giảm → lọc giai đoạn đó → xem **ở đâu** giảm kèm **contribution %** → ưu tiên theo mức đóng góp. Dùng bộ lọc Time / Brand / Category / Country / Manager để thu hẹp trước khi sang Bước 2.
 
**Ví dụ:** Revenue $26.6M, giảm 31% vs năm ngoái, đạt 99.92% target. Xem Revenue By Time thấy giảm mạnh từ tháng 3 → lọc giai đoạn đó: Computers chiếm 36% tổng revenue nhưng giảm 35% — contribution cao nhất nên ưu tiên xử lý trước. NPD chỉ đóng góp $87K — quá nhỏ, chưa đủ bù đắp.
 
---
### Bước 2 — Sales Team & Store: *"Vấn đề đến từ đâu trong vận hành?"*

Khi đã biết doanh số giảm ở khu vực / danh mục nào, vào trang này để xác định vấn đề nằm ở nhân viên hay cửa hàng.

**Director/Manager nào không đạt target? GAP bao nhiêu?**

```mermaid
flowchart TD
    A([Manager khong dat target<br/>Kiem tra Traffic Foot]) --> B([Traffic THAP<br/>Khach khong vao cua hang])
    A --> C([Traffic CAO<br/>nhung Funnel duoi 18.1%<br/>Khach vao nhung khong chot duoc don])
    A --> D([Traffic CAO, don on<br/>nhung Revenue van thap])

    B --> B1([Co the do vi tri, nhan dien thuong hieu<br/>hoac marketing tai diem ban<br/>Can kiem tra them: tai sao luong khach thap?])
    C --> C1([Co the lien quan den quy trinh tu van<br/>hoac ho tro khach hang<br/>Can kiem tra them: chat luong tu van nhu the nao?])
    D --> D1([Khach mua o phan khuc gia thap<br/>Kiem tra Total Product By Price Group:<br/>Nhom Lowest & Low-Mid chiem ty trong lon?<br/>Can kiem tra them: co cau san pham co phu hop khong?])

    style A fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#f7d794,stroke:#e6b84a
    style D fill:#f7d794,stroke:#e6b84a
    style B1 fill:#a8d5c2,stroke:#5aab8e
    style C1 fill:#a8d5c2,stroke:#5aab8e
    style D1 fill:#a8d5c2,stroke:#5aab8e
```

**Xem Revenue By Store Legion By Category:**
- Store đó có contribution cao ở Category nào?
- Nếu Category chủ lực của store cũng giảm toàn thị trường (đã thấy ở Bước 1) → vấn đề mang tính thị trường chung, không phải lỗi riêng store
- Nếu store khác cùng Category vẫn tốt → vấn đề nằm riêng ở store này, cần kiểm tra thêm

**Ví dụ:** Mr. E có Revenue $776K, vượt target $10.8K nhưng Traffic Foot chỉ 5,634 — thấp hơn nhiều so với Mr. D (35,026). Khách không vào nhiều nhưng tỷ lệ chuyển đổi lại tốt → có thể không phải vấn đề tư vấn mà là lượng khách tiếp cận được đang thấp, cần kiểm tra thêm nguyên nhân tại khu vực đó.

---

### Bước 3 — Customer Segmentation & RFM: *"Khách hàng đang thay đổi như thế nào?"*

Sau khi hiểu doanh số và vận hành, bước này trả lời: giảm do khách mới hay khách cũ — và nếu khách cũ thì đang mất ở nhóm nào?

**3 chỉ số đọc trước:**
- **New Customer** — khách mua lần đầu trong kỳ → phản ánh sức hút đầu phễu
- **Returning Customer** — khách quay lại mua tiếp → phản ánh khả năng giữ chân
- **Loyal Customer** — khách mua liên tục không gián đoạn → nhóm giá trị nhất cần theo dõi

**Xem trend theo thời gian:** tháng doanh số giảm ở Bước 1 thì nhóm khách nào giảm theo?

```mermaid
flowchart TD
    A([New Customer tang hay giam?]) --> B([New GIAM<br/>Dau pheu co the dang yeu<br/>Can kiem tra: kenh acquisition nao dang kem?])
    A --> C([New TANG<br/>Dang thu hut duoc khach moi tot])
    D([Returning Customer tang hay giam?]) --> E([Returning GIAM<br/>Khach cu khong quay lai<br/>Can kiem tra them o RFM ben duoi])
    D --> F([Returning TANG nhung New GIAM<br/>Giu chan tot nhung khong mo rong duoc tep])

    style A fill:#f4a9a8,stroke:#e07b7b
    style D fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#a8d5c2,stroke:#5aab8e
    style E fill:#f7d794,stroke:#e6b84a
    style F fill:#f7d794,stroke:#e6b84a
```

**Nếu Returning giảm → đi sâu vào RFM để tìm nhóm nào đang rời đi:**

```mermaid
flowchart TD
    A([Xem tung chieu R F M rieng truoc]) --> B([Recency: nhom mua gan day chiem bao nhieu %?<br/>Nhom tren 12 thang lon = tep dang gia di])
    A --> C([Frequency: da so khach dang mua it hay nhieu lan?])
    A --> D([Monetary: khach dang chi nhieu hay it moi lan mua?])
    B --> E([Ghep lai xem Segment Treemap<br/>Nhom nao dang lon? Nhom nao dang thu hep?])
    C --> E
    D --> E
    E --> F([Xem trend tung nhom theo thoi gian])
    F --> G([Champions giam - dang mat khach VIP - dang lo nhat])
    F --> H([Loyal giam - chuyen sang At Risk - can theo doi])
    F --> I([At Risk giam - dang keo khach tro lai - tin hieu tot])

    style A fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#f7d794,stroke:#e6b84a
    style D fill:#f7d794,stroke:#e6b84a
    style E fill:#a8d5c2,stroke:#5aab8e
    style F fill:#a8d5c2,stroke:#5aab8e
    style G fill:#f4a9a8,stroke:#e07b7b
    style H fill:#f7d794,stroke:#e6b84a
    style I fill:#c9b8e8,stroke:#9b7fd4
```

**Ma trận Recency × Frequency × Monetary:** Xác định nhóm nào vừa có giá trị cao vừa còn active — đó là nhóm cần ưu tiên giữ chân nhất.

**Ví dụ:** Revenue giảm 31% → Computers giảm mạnh nhất (Bước 1). Sang Bước 3 thấy Returning Customer giảm → đi sâu vào RFM thấy nhóm Champions đang thu hẹp dần theo thời gian → không phải thiếu khách mới mà đang mất dần tệp khách VIP. Nhóm Loyal cũng giảm và dịch chuyển sang At Risk → tệp khách tốt đang có dấu hiệu rời đi, cần kiểm tra thêm.

---

## Công Nghệ Sử Dụng

- **Power BI** — Trực quan hóa & báo cáo tương tác
- **DAX** — Measures, KPI, time intelligence, RFM scoring
- **Star Schema** — Data modeling với Fact + Dimension tables
- **Power Query** — Data transformation & cleaning
