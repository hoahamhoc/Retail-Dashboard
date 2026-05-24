# Retail Performance Dashboard

Dashboard phân tích toàn diện hoạt động bán lẻ — từ doanh số, hiệu suất nhân viên & cửa hàng đến hành vi khách hàng theo mô hình RFM. Mục tiêu là giúp ban quản lý xác định đúng vấn đề và ra quyết định kịp thời.

## [Xem Interactive Dashboard tại đây](https://report.onhandbi.com/public/report?token=eyJhbGciOiJIUzI1NiJ9.eyJwdWJsaWNfbGlua19pZCI6NjUzLCJoYXNfcGFzc2NvZGUiOmZhbHNlLCJ0aW1lIjoxNzc5NjMxNDg3fQ.VTH5d0Swd6UTp-59r10YrTo6W-8xcJJ6cJYerc2FpLA)

---

## Lưu ý về dữ liệu

Dashboard sử dụng dữ liệu nguồn từ bộ mẫu Contoso Retail (Microsoft sample data), có bổ sung thêm các cột về target doanh số và thông tin manager/nhân viên. Mục đích là để minh họa cấu trúc và logic phân tích — các con số, tên nhân viên, tên cửa hàng không phản ánh số liệu thực tế của bất kỳ doanh nghiệp nào.

---

## Cấu trúc Dashboard

```
Homepage
   ├── Sales Overview            → Tổng quan doanh số & xu hướng
   ├── Sales Team & Store        → Hiệu suất nhân viên & cửa hàng
   └── Customer Segmentation     → Phân khúc khách hàng & RFM
```

---

## Hướng Dẫn Đọc Dashboard

### Bước 1 — Sales Overview

Trang này trả lời câu hỏi đầu tiên: **Doanh số đang như thế nào và biến động ở đâu?**

- KPI tổng quan: Revenue, Quantity, ASP, % Revenue Achieved vs Target
- So sánh với Last Year để nắm được xu hướng dài hạn
- Phân tách doanh số theo Category / Brand / Country / NPD (sản phẩm mới)
- Xem biến động theo thời gian (ngày / tuần / tháng / quý)

**Ví dụ:** Revenue $26.6M, giảm 31% so với năm ngoái. Nhìn vào Revenue By Category thấy Computers ($9.6M) và Cell phones ($4.9M) chiếm tỷ trọng lớn nhất — lọc vào Computers thấy giảm 34.96% YoY, ASP cũng giảm 13%. Vừa bán ít hơn vừa bán rẻ hơn → cần đi sâu vào kênh phân phối ở Bước 2.

Dùng bộ lọc Time / Brand / Category / Country / Manager để thu hẹp phạm vi trước khi chuyển sang trang tiếp theo.

---

### Bước 2 — Sales Team & Store Performance

Khi đã xác định được doanh số giảm ở khu vực hoặc danh mục nào, vào trang này để tìm nguyên nhân từ phía **con người và cửa hàng**.

**2 góc nhìn chính:**

**Nhân viên — Ai đang kéo doanh số xuống?**
- So sánh Revenue vs Target → GAP âm hay dương?
- Traffic Foot (lượt vào cửa hàng) vs Quantity (số lượng bán) → tỷ lệ chuyển đổi từng nhân viên
- Funnel: 100% Traffic → 18.1% chuyển thành đơn hàng — nhân viên nào thấp hơn mức này cần follow up

**Cửa hàng — Store nào đang hoạt động hiệu quả?**
- Revenue/m² theo cửa hàng → store nào đang khai thác không gian tốt nhất?
- Revenue by Store Legion by Category → danh mục nào đang bán chạy ở từng khu vực?

```
flowchart TD
    A([Doanh so giam\nVan de o dau?]) --> B([Nhan vien\nGAP am / Traffic nhieu nhung don thap])
    A --> C([Cua hang\nRevenue/m2 thap])

    B --> D([Traffic cao nhung Quantity thap\nTy le chuyen doi kem])
    B --> E([Traffic cung thap\nKhong co khach vao])

    D --> D1[Review ky nang tu van / chot don]
    E --> E1[Review vi tri, marketing tai cua hang]

    C --> F([Category ban chay tai store khac\nnhung khong ban duoc o day])
    C --> G([Dien tich lon nhung revenue thap])

    F --> F1[Xem lai co cau hang hoa / display]
    G --> G1[Xem lai layout hoac staffing]

    style A fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#f7d794,stroke:#e6b84a
    style D fill:#a8d5c2,stroke:#5aab8e
    style E fill:#a8d5c2,stroke:#5aab8e
    style F fill:#c9b8e8,stroke:#9b7fd4
    style G fill:#c9b8e8,stroke:#9b7fd4
```

**Ví dụ:** Mr. E có Revenue $776K, vượt target 10.810 nhưng Traffic Foot chỉ 5,634 — thấp hơn Mr. D (35,026). Không phải kém về chốt đơn mà đang thiếu lượng khách vào → cần review marketing tại khu vực Mr. E phụ trách.

---

### Bước 3 — Customer Segmentation & RFM

Sau khi hiểu doanh số và hiệu suất vận hành, bước này trả lời: **Khách hàng là ai và cần ưu tiên nhóm nào?**

**3 chỉ số nền tảng:**
- **New Customer** — khách mua lần đầu trong kỳ → phản ánh sức hút của marketing
- **Returning Customer** — khách đã mua trước đó, quay lại → phản ánh sức giữ chân
- **Loyal Customer** — khách mua liên tục, không bỏ ngày nào → nhóm cần bảo vệ nhất

**Đọc RFM như thế nào?**

| Chiều | Ý nghĩa | Hành động |
|---|---|---|
| Recency (Dưới 3 tháng) | Mua gần đây → còn đang active | Upsell / Cross-sell ngay |
| Recency (Trên 12 tháng) | Lâu không mua → có nguy cơ mất | Winback campaign |
| Frequency (5–9 đơn) | Nhóm lớn nhất (34.85%) — mua đều nhưng chưa nhiều | Loyalty program để tăng tần suất |
| Monetary (Dưới 3.000đ) | Nhóm chi tiêu thấp nhất | Bundle deals để tăng giá trị đơn |

**Đọc Segment Treemap:**
- **Khách hàng xuất sắc nhất** (Champions) → doanh thu cao, mua thường xuyên, mua gần đây → đây là nhóm VIP, cần chăm sóc đặc biệt
- **Khách hàng trung thành** (Loyal) → mua nhiều nhưng recency đang giảm → cần re-engage trước khi họ chuyển sang "Cần được chú ý"
- **Sắp mất** (At Risk) → từng là khách tốt nhưng lâu không mua → cần winback campaign gấp
- **Khách hàng mới** → số lượng thấp trong kỳ (-55% YoY) → marketing đang yếu ở đầu phễu

**Ma trận RFM — Recency × Frequency × Monetary:**

Đây là công cụ để ưu tiên nguồn lực. Ví dụ: nhóm "Dưới 3 tháng × Trên 15.000đ × 20–24 đơn" chỉ có 30 khách nhưng giá trị cực cao → ưu tiên giữ chân nhóm này bằng chương trình VIP.

```
flowchart TD
    A([Phan khuc khach hang]) --> B([New Customer giam\n-55% YoY])
    A --> C([Returning tang\n+38% YoY])
    A --> D([Loyal: on dinh?])

    B --> B1([Marketing dau pheu dang yeu\nKiem tra Ads / Brand awareness])

    C --> C1([Gio hang tot\nKhach cu quay lai nhieu hon moi])
    C --> C2([Kiem tra: ho thuc su trung thanh\nHay chi mua vi khuyen mai?])
    C2 --> C3[Xem Recency: neu nhieu nhom 3-6 thang\nthi dang phu thuoc khuyen mai]

    D --> D1([Loyal thap\nChua co chuong trinh giu chan hieu qua])
    D --> D2([Loyal cao\nBao ve bang chuong trinh VIP])

    style A fill:#f4a9a8,stroke:#e07b7b
    style B fill:#f7d794,stroke:#e6b84a
    style C fill:#a8d5c2,stroke:#5aab8e
    style D fill:#c9b8e8,stroke:#9b7fd4
    style B1 fill:#f4a9a8,stroke:#e07b7b
    style C2 fill:#f7d794,stroke:#e6b84a
    style D1 fill:#f4a9a8,stroke:#e07b7b
    style D2 fill:#a8d5c2,stroke:#5aab8e
```

**Ví dụ:** New Customer giảm 55% trong khi Returning tăng 38% → tệp khách cũ đang quay lại tốt nhưng không đang thu hút được khách mới. Nhìn vào By Age Group thấy nhóm 18–24 có New Customer chỉ 202 — thấp nhất tất cả các nhóm tuổi → đây là khoảng trống cần marketing nhắm đến.

---

## Công Nghệ Sử Dụng

- **Power BI** — Trực quan hóa & báo cáo tương tác
- **DAX** — Measures, KPI, time intelligence, RFM scoring
- **Star Schema** — Data modeling với Fact + Dimension tables
- **Power Query** — Data transformation & cleaning
