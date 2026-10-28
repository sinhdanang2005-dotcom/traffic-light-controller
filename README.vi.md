# Bộ điều khiển đèn giao thông — FSM & Proteus

Đồ án nhóm thiết kế bộ điều khiển đèn giao thông bằng mạch logic số, kết hợp FSM 3 trạng thái và bộ đếm lùi BCD hiển thị trên LED 7 đoạn.

**Người trình bày portfolio:** Thái Trung Sinh

[English](README.md) · [Chi tiết thiết kế](docs/design.md) · [Slide kỹ thuật](docs/technical-slides.pdf) · [Mạch Proteus](proteus/TrafficLight.pdsprj)

![Mạch đèn giao thông trích từ slide 25](assets/proteus-schematic.png)

*Ảnh mạch được trích nguyên bản từ slide 25 của đồ án.*

## Phần công việc tôi thực hiện

Theo phần đóng góp cá nhân trong CV:

- Lập bảng trạng thái, thiết kế logic và mô phỏng bộ điều khiển đèn giao thông bằng Proteus.
- Thiết kế FSM 3 trạng thái và rút gọn hàm logic bằng bảng Karnaugh.
- Kết hợp bộ đếm lùi BCD 74192 với LED 7 đoạn để hiển thị thời gian đếm ngược.

Đây là **đồ án nhóm**. Slide và toàn bộ mạch là sản phẩm chung; repository này trình bày rõ phần công việc cá nhân của tôi trong đồ án.

## Nguyên lý hoạt động

Hệ thống chuyển pha theo thứ tự **Xanh → Vàng → Đỏ → Xanh**. Hai IC 74192 tạo nhánh đếm hai chữ số dùng cho pha xanh và đỏ; một IC 74192 tạo nhánh đếm một chữ số cho pha vàng. FSM điều khiển tín hiệu cho phép đếm và các đèn báo.

![Sơ đồ khối gốc](assets/block-diagram.png)

| Khối | Chức năng |
| --- | --- |
| FSM 3 trạng thái | Chọn pha và điều khiển các tín hiệu enable |
| Bộ đếm 74192 | Tạo giá trị BCD đếm lùi |
| Bộ giải mã 74LS247 | Giải mã BCD để điều khiển LED 7 đoạn |
| LED 7 đoạn | Hiển thị giá trị đếm |
| Flip-flop 4013 và cổng logic | Lưu trạng thái và thực hiện logic điều khiển |

Tên linh kiện trên được đối chiếu trong file Proteus. Slide cũng giới thiệu các bộ giải mã họ 7447/74LS47.

## Máy trạng thái

Quy ước mã trạng thái theo thứ tự `s0 s1` trong slide. `C1=1` khi nhánh đếm xanh/đỏ kết thúc; `C2=1` khi nhánh đếm vàng kết thúc.

| Trạng thái | Mã | Đèn sáng | `en1` | `en2` | Điều kiện chuyển |
| --- | --- | --- | --- | --- | --- |
| A | `00` | Xanh | 1 | 0 | `C1=0`: giữ A; `C1=1`: sang B |
| B | `01` | Vàng | 0 | 1 | `C2=0`: giữ B; `C2=1`: sang C |
| C | `10` | Đỏ | 1 | 0 | `C1=0`: giữ C; `C1=1`: sang A |

![Sơ đồ FSM gốc](assets/fsm-diagram.png)

Các bảng Karnaugh và phương trình điều khiển được trình bày trong [tài liệu thiết kế](docs/design.md).

## Mở và mô phỏng

1. Tải repository về máy.
2. Mở [`proteus/TrafficLight.pdsprj`](proteus/TrafficLight.pdsprj) bằng phiên bản Proteus tương thích và có các mô hình linh kiện cần thiết.
3. Kiểm tra nguồn clock, dữ liệu nạp trước của các bộ đếm và trạng thái khởi tạo FSM.
4. Chạy mô phỏng, quan sát thứ tự sáng đèn và giá trị đếm trên LED 7 đoạn.

File Proteus được giữ nguyên. Việc chuẩn bị repository chưa bao gồm chạy lại mô phỏng; tài liệu không khẳng định tần số clock hay thời lượng cụ thể của từng pha.

## Tài liệu trong repository

| Đường dẫn | Nội dung |
| --- | --- |
| [`proteus/TrafficLight.pdsprj`](proteus/TrafficLight.pdsprj) | File mạch gốc |
| [`assets/`](assets/) | Hình mạch, FSM, bảng trạng thái và Karnaugh trích từ slide |
| [`docs/design.md`](docs/design.md) | Chi tiết logic và nguồn hình |
| [`docs/technical-slides.pdf`](docs/technical-slides.pdf) | Sáu trang kỹ thuật trích từ slide gốc (trang 5 và 21–25) |

## Kỹ năng thể hiện

Thiết kế FSM, mã hóa trạng thái, lập bảng trạng thái, rút gọn Karnaugh, bộ đếm BCD, giải mã LED 7 đoạn và mô phỏng mạch số bằng Proteus.

## Bối cảnh đồ án

Dự án được thực hiện trong khuôn khổ đồ án nhóm 2 tại trường đại học. Repository trình bày phần đóng góp cá nhân của tôi trong sản phẩm chung.
