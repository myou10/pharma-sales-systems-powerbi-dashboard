# Pharmaceutical Sales Analytics Dashboard

Dashboard Power BI phân tích dữ liệu bán hàng dược phẩm, tập trung vào doanh thu, VAT, số lượng bán, cơ cấu Brand Group và hiệu quả theo từng sản phẩm.

> **Lưu ý:** Toàn bộ giao dịch và số liệu trong dự án là dữ liệu giả lập phục vụ học tập, portfolio và trình diễn Power BI. Tên sản phẩm được dùng để minh họa và không đại diện cho kết quả kinh doanh thực tế của bất kỳ doanh nghiệp hoặc nhãn hàng nào.

## Live demo

[Mở dashboard trên On Hand BI](https://report.onhandbi.com/public/report?token=eyJhbGciOiJIUzI1NiJ9.eyJwdWJsaWNfbGlua19pZCI6NzM5LCJoYXNfcGFzc2NvZGUiOnRydWUsInRpbWUiOjE3OTI2ODcyNTZ9.uOhgjAfNDZDLgkvz-kJHtnTClRMSlE_IWJkjZWIFcXs)

Khi mở liên kết demo, mật khẩu truy cập sẽ được tự động điền theo cấu hình chia sẻ của On Hand BI.

## Tổng quan dự án

Dự án mô phỏng quy trình xây dựng một báo cáo bán hàng hoàn chỉnh:

1. Chuẩn hóa danh mục sản phẩm và dữ liệu giao dịch bằng Excel.
2. Nạp và biến đổi dữ liệu bằng Power Query.
3. Xây dựng mô hình dữ liệu theo hướng star schema.
4. Tạo các measure bằng DAX.
5. Thiết kế dashboard tương tác trong Power BI.
6. Chia sẻ báo cáo qua On Hand BI.

### Hồ sơ dữ liệu mẫu

| Chỉ tiêu | Giá trị |
|---|---:|
| Số dòng giao dịch | 10.000 |
| Số sản phẩm | 200 |
| Số Brand Name | 195 |
| Số Brand Group | 3 |
| Khoảng thời gian | 2024-01-01 đến 2026-10-08 |
| Tổng số lượng | 327.716 |
| Doanh số sau chiết khấu, chưa VAT | 56.353.729.900 |
| Tiền VAT | 2.817.750.300 |
| Tổng thanh toán | 59.171.480.200 |

Ba Brand Group được sử dụng trong dữ liệu mẫu:

- `VinaCare`
- `FHC`
- `Medica`

## Nội dung dashboard

Trang `Overview` cung cấp các thành phần chính:

- KPI tổng doanh thu chưa VAT.
- KPI tổng tiền VAT.
- KPI tổng thanh toán sau VAT.
- KPI tổng số lượng bán.
- Biểu đồ doanh thu, VAT và số lượng theo tháng.
- Biểu đồ kết hợp doanh thu và số lượng theo thời gian.
- Biểu đồ cơ cấu doanh thu theo Brand Group.
- Xếp hạng Top/Bottom 10 sản phẩm.
- Bảng chi tiết theo mã và tên sản phẩm.
- Thông tin thời điểm dữ liệu được cập nhật.

Các bộ lọc tương tác gồm:

- Năm, quý và tháng.
- Kỳ phân tích.
- Chỉ tiêu phân tích.
- Brand Group.
- Brand Name.
- Top hoặc Bottom sản phẩm.

## Mô hình dữ liệu

```mermaid
erDiagram
    DIM_PRODUCT ||--o{ FACT_SALES : "Mã"
    DIM_DATE ||--o{ FACT_SALES : "Ngày giao dịch"

    DIM_PRODUCT {
        text Ma PK
        text Ten_san_pham
        text Nhom_VTHH
        text Brand_Name
        text Brand_Group
    }

    FACT_SALES {
        int Nam
        int Thang
        int Ngay
        date Ngay_giao_dich FK
        text Ma FK
        text Ten_san_pham
        int So_luong
        number Doanh_so_sau_chiet_khau
        number Tien_VAT
        number Thanh_toan
    }

    DIM_DATE {
        date Date PK
        int Year
        int Quarter
        int Month
    }
```

Ngoài hai bảng dữ liệu chính, semantic model sử dụng:

- `Dim_Date` để phân tích theo năm, quý và tháng.
- `_Table of measures` để quản lý tập trung các DAX measure.
- `Metrics`, `Time Group` và `Dynamic Rank Selector` làm bảng lựa chọn cho các slicer động.

Quan hệ chính:

```text
DIM_PRODUCT[Mã]  (1) ───── (*)  FACT_SALES[Mã]
Dim_Date[Date]   (1) ───── (*)  FACT_SALES[Ngày giao dịch]
```

## Cấu trúc dữ liệu

### `DIM_PRODUCT.xlsx`

| Cột | Mô tả |
|---|---|
| `Mã` | Mã sản phẩm duy nhất |
| `Tên sản phẩm` | Tên sản phẩm dược phẩm |
| `Nhóm VTHH` | Nhóm vật tư, hàng hóa |
| `Brand Name` | Nhãn hàng của sản phẩm |
| `Brand Group` | Công ty chính quản lý sản phẩm |

### `FACT_SALES.xlsx`

| Cột | Mô tả |
|---|---|
| `Năm` | Năm giao dịch |
| `Tháng` | Tháng giao dịch |
| `Ngày` | Ngày trong tháng |
| `Ngày giao dịch` | Ngày Excel chuẩn, không chứa thành phần giờ |
| `Mã` | Mã sản phẩm, dùng để liên kết với `DIM_PRODUCT` |
| `Tên sản phẩm` | Tên sản phẩm tại thời điểm giao dịch |
| `Số lượng` | Số lượng bán |
| `Doanh số sau chiết khấu (-VAT)` | Doanh số sau chiết khấu, chưa gồm VAT |
| `Tiền VAT` | Thuế VAT |
| `Thanh toán` | Tổng giá trị thanh toán sau VAT |

## Các measure chính

```DAX
Doanh thu (-VAT) =
SUM ( FACT_SALES[Doanh số sau chiết khấu (-VAT)] )

Tiền VAT =
SUM ( FACT_SALES[Tiền VAT] )

Thành tiền (+VAT) =
SUM ( FACT_SALES[Thanh toán] )

Số lượng =
SUM ( FACT_SALES[Số lượng] )
```

Dashboard còn sử dụng measure hỗ trợ đổi chỉ tiêu, mô tả kỳ báo cáo, tô màu Min/Max và xếp hạng Top/Bottom sản phẩm.

## Công nghệ sử dụng

- Microsoft Power BI Desktop
- Power Query M
- DAX
- Microsoft Excel
- On Hand BI để chia sẻ báo cáo

## Cấu trúc repository đề xuất

```text
.
├── Dashboard Total All.pbix
├── DIM_PRODUCT.xlsx
├── FACT_SALES.xlsx
├── README.md
└── .gitignore
```

Không nên đưa các file `*_backup_*.xlsx`, file tạm của Excel hoặc dữ liệu thật có thông tin nhạy cảm lên GitHub.

## Hướng dẫn chạy dự án

### 1. Tải repository

```bash
git clone <repository-url>
cd <repository-folder>
```

### 2. Mở báo cáo

Mở file `Dashboard Total All.pbix` bằng Power BI Desktop.

### 3. Cập nhật đường dẫn nguồn dữ liệu

Trong Power BI Desktop:

1. Chọn **Home → Transform data**.
2. Mở query `DIM_PRODUCT` và `FACT_SALES`.
3. Thay đường dẫn trong `File.Contents(...)` bằng vị trí hai file Excel trên máy của bạn.
4. Chọn **Close & Apply**.

Ví dụ:

```powerquery
Source = Excel.Workbook(
    File.Contents("C:\duong-dan-du-an\FACT_SALES.xlsx"),
    null,
    true
)
```

Để repository dễ di chuyển giữa các máy, nên tạo parameter `DataFolder` trong Power Query và ghép tên file từ parameter này.

### 4. Kiểm tra quan hệ

Trong Model view, bảo đảm:

- `DIM_PRODUCT[Mã]` có quan hệ một-nhiều với `FACT_SALES[Mã]`.
- `Dim_Date[Date]` có quan hệ một-nhiều với `FACT_SALES[Ngày giao dịch]`.
- Hướng lọc mặc định đi từ dimension sang fact.

### 5. Làm mới dữ liệu

Chọn **Home → Refresh** và kiểm tra dashboard sau khi dữ liệu được nạp.

## Kiểm tra chất lượng dữ liệu

- `DIM_PRODUCT[Mã]` không được trùng hoặc để trống.
- Mọi `FACT_SALES[Mã]` phải tồn tại trong `DIM_PRODUCT`.
- `Ngày giao dịch` phải là ngày Excel hợp lệ và không chứa giờ.
- `Số lượng` phải là số nguyên không âm.
- `Thanh toán` phải bằng doanh số chưa VAT cộng tiền VAT, có xét sai số làm tròn nếu áp dụng.

## Hạn chế

- Dữ liệu trong dự án là dữ liệu giả lập.
- Đường dẫn nguồn Excel cần được cập nhật khi chuyển dự án sang máy khác.
- Link demo phụ thuộc vào trạng thái chia sẻ của On Hand BI.
- Passcode truy cập không được lưu trong mã nguồn hoặc lịch sử Git.

## Hướng phát triển

- Bổ sung chỉ tiêu tăng trưởng MoM và YoY.
- Thêm ngân sách hoặc chỉ tiêu doanh thu để phân tích Actual vs Target.
- Bổ sung phân tích theo khu vực, khách hàng và kênh bán hàng.
- Đưa dữ liệu vào PostgreSQL hoặc data warehouse thay cho file Excel.
- Thiết lập incremental refresh và tự động hóa quy trình cập nhật dữ liệu.

