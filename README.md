# pharma-sales-systems-powerbi-dashboard

Dashboard Power BI trực quan dữ liệu từ 3 hệ thống bán hàng dược phẩm, giúp theo dõi hiệu quả kinh doanh, xu hướng doanh thu và đóng góp của từng nhãn hàng và sản phẩm theo 3 hệ thống chính.

> **Lưu ý:** Toàn bộ dữ liệu trong dashboard là dữ liệu minh họa.

## Live dashboard

[Mở dashboard trên On Hand BI](https://report.onhandbi.com/public/report?token=eyJhbGciOiJIUzI1NiJ9.eyJwdWJsaWNfbGlua19pZCI6NzM5LCJoYXNfcGFzc2NvZGUiOnRydWUsInRpbWUiOjE3OTI2ODcyNTZ9.uOhgjAfNDZDLgkvz-kJHtnTClRMSlE_IWJkjZWIFcXs)

**Mật khẩu:** `Nhập mật khẩu tại đây`

## Tổng quan dự án

### Ý nghĩa

Báo cáo dùng để hiển tình hình kinh doanh và doanh số bán hàng của 3 hệ thống bán hàng theo thời gian. Qua đó giúp theo dõi hiệu quả bán hàng, phân tích doanh thu theo từng chiều dữ liệu để có kế hoạch kinh doanh và ra quyết định kịp thời.

### Mục tiêu

- Theo dõi doanh thu trước VAT, tiền VAT, tổng thanh toán và số lượng bán.
- Phân tích xu hướng bán hàng theo năm, quý và tháng.
- So sánh hiệu quả giữa các Brand Group và Brand Name.
- Xác định các sản phẩm Top/Bottom theo chỉ tiêu được lựa chọn.
- Hỗ trợ ra quyết định dựa trên dữ liệu thông qua bộ lọc tương tác.

### Hướng dẫn sử dụng

1. Mở liên kết dashboard ở phần **Live dashboard**.
2. Nhập mật khẩu được cung cấp ở dòng **Mật khẩu**.
3. Chọn năm, quý, tháng hoặc kỳ phân tích cần xem.
4. Dùng bộ lọc Brand Group và Brand Name để thu hẹp phạm vi dữ liệu.
5. Chọn chỉ tiêu và chế độ Top/Bottom để xem thứ hạng sản phẩm.
6. Di chuột hoặc chọn một phần tử trên biểu đồ để xem chi tiết và lọc chéo toàn bộ báo cáo.

## Nội dung dashboard

Trang `Overview` bao gồm:

- KPI tổng doanh thu chưa VAT.
- KPI tổng tiền VAT.
- KPI tổng thanh toán sau VAT.
- KPI tổng số lượng bán.
- Biểu đồ doanh thu, VAT và số lượng theo tháng.
- Biểu đồ kết hợp doanh thu và số lượng theo thời gian.
- Biểu đồ cơ cấu doanh thu theo Brand Group.
- Xếp hạng Top/Bottom 10 sản phẩm.
- Bảng chi tiết theo mã và tên sản phẩm.
- Bộ lọc năm, quý, tháng, kỳ phân tích, chỉ tiêu, Brand Group và Brand Name.

## Cấu trúc dữ liệu

### `DIM_PRODUCT`

| Cột | Mô tả |
|---|---|
| `Mã` | Mã sản phẩm duy nhất |
| `Tên sản phẩm` | Tên sản phẩm dược phẩm |
| `Nhóm VTHH` | Nhóm vật tư, hàng hóa |
| `Brand Name` | Nhãn hàng của sản phẩm |
| `Brand Group` | Công ty chính quản lý sản phẩm |

### `FACT_SALES`

| Cột | Mô tả |
|---|---|
| `Năm` | Năm giao dịch |
| `Tháng` | Tháng giao dịch |
| `Ngày` | Ngày trong tháng |
| `Ngày giao dịch` | Ngày giao dịch không chứa thành phần giờ |
| `Mã` | Mã sản phẩm dùng để liên kết với `DIM_PRODUCT` |
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
- On Hand BI
