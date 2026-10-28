# Bộ điều khiển đèn giao thông

**Thái Trung Sinh** · Mạch logic số · FSM · Proteus

[English](README.md) · [Chi tiết thiết kế](docs/design.md) · [Slide đồ án](docs/technical-slides.pdf) · [Mạch Proteus](proteus/TrafficLight.pdsprj)

Đây là đồ án nhóm về bộ điều khiển đèn giao thông có hiển thị đếm ngược. Trong đồ án, tôi phụ trách lập bảng trạng thái, thiết kế logic FSM, rút gọn Karnaugh, kết hợp mạch đếm lùi BCD với LED 7 đoạn và mô phỏng trên Proteus.

## Phần tôi thực hiện

- Lập bảng trạng thái cho ba pha xanh, vàng và đỏ.
- Thiết kế FSM 3 trạng thái, rút gọn hàm điều khiển bằng bảng Karnaugh.
- Kết nối bộ đếm lùi BCD 74192 với LED 7 đoạn.
- Thiết kế và mô phỏng mạch điều khiển trên Proteus.

Mạch và bài trình bày là sản phẩm của nhóm. Các công việc liệt kê trên là phần đóng góp của tôi.

## Mạch điều khiển

![Mạch đèn giao thông trên Proteus](assets/proteus-schematic.png)

*Hình mạch trong bài trình bày của nhóm.*

Mạch chuyển pha theo thứ tự **Xanh → Vàng → Đỏ → Xanh**. Hai IC 74192 tạo bộ đếm hai chữ số dùng cho pha xanh và đỏ. IC 74192 còn lại dùng cho pha vàng. FSM chọn nhánh đếm đang hoạt động và điều khiển các đèn.

| Linh kiện | Vai trò trong mạch |
| --- | --- |
| 74192 | Đếm lùi BCD |
| 74LS247 | Giải mã BCD sang LED 7 đoạn |
| LED 7 đoạn | Hiển thị giá trị đếm |
| Flip-flop 4013 | Lưu trạng thái FSM |
| Cổng logic | Tạo logic trạng thái kế tiếp, tín hiệu enable và đầu ra đèn |

![Sơ đồ khối](assets/block-diagram.png)

## Thiết kế FSM

Tôi sử dụng ba trạng thái: A ứng với đèn xanh, B ứng với đèn vàng và C ứng với đèn đỏ. Mã trạng thái được viết theo thứ tự `s0 s1`.

- `C1 = 1`: nhánh đếm xanh/đỏ đã kết thúc.
- `C2 = 1`: nhánh đếm vàng đã kết thúc.
- `en1`: cho phép nhánh đếm xanh/đỏ hoạt động.
- `en2`: cho phép nhánh đếm vàng hoạt động.

| Trạng thái | Mã | Đèn sáng | `en1` | `en2` | Trạng thái kế tiếp |
| --- | --- | --- | --- | --- | --- |
| A | `00` | Xanh | 1 | 0 | Giữ A khi `C1=0`; sang B khi `C1=1` |
| B | `01` | Vàng | 0 | 1 | Giữ B khi `C2=0`; sang C khi `C2=1` |
| C | `10` | Đỏ | 1 | 0 | Giữ C khi `C1=0`; sang A khi `C1=1` |

![FSM ba trạng thái](assets/fsm-diagram.png)

Bảng trạng thái đầy đủ, bảng Karnaugh và các phương trình rút gọn nằm trong [tài liệu thiết kế](docs/design.md).

## Chạy mạch trên Proteus

1. Tải hoặc clone repository về máy.
2. Mở file [`TrafficLight.pdsprj`](proteus/TrafficLight.pdsprj) bằng Proteus.
3. Kiểm tra nguồn clock, giá trị nạp trước của bộ đếm và trạng thái khởi tạo FSM.
4. Chạy mô phỏng, quan sát LED đếm ngược và quá trình chuyển đèn.

Thời gian từng pha phụ thuộc vào nguồn clock và giá trị nạp trong mạch. Các hình minh họa trong repository được lấy từ slide đồ án.

## Các file chính

| Đường dẫn | Nội dung |
| --- | --- |
| [`proteus/TrafficLight.pdsprj`](proteus/TrafficLight.pdsprj) | File mạch Proteus |
| [`docs/design.md`](docs/design.md) | Bảng trạng thái, Karnaugh và phương trình |
| [`docs/technical-slides.pdf`](docs/technical-slides.pdf) | Các trang thiết kế trong bài trình bày nhóm |
| [`assets/`](assets/) | Hình mạch và hình thiết kế |
