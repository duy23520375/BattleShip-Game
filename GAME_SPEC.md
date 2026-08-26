# Battleship Corporate Game Show Specification (10x10)

## 1. Grid & Fleet Configuration
- **Grid Matrix:** 10x10 tác chiến (Hàng: A đến J, Cột: 1 đến 10). Tọa độ hiển thị to rõ theo chuẩn quân sự, viền neon phát sáng.
- **Tâm ngắm (Target Reticle):** Hiệu ứng con trỏ ngắm bắn `[ ]` màu xanh neon di chuyển theo ô chuột đang hover.
- **Cấu hình Hạm đội 5 Tàu (Tổng cộng 17 ô mục tiêu):**
  - **1 Tàu 5 ô (Rất dễ - Ngang):** Tọa độ `B5 - B6 - B7 - B8 - B9` | Thưởng hạ gục (Sunk Bonus): **+250 điểm**.
  - **1 Tàu 4 ô (Dễ - Ngang):** Tọa độ `D4 - D5 - D6 - D7` | Thưởng hạ gục (Sunk Bonus): **+200 điểm**.
  - **1 Tàu 3 ô - A (Trung bình - Dọc):** Tọa độ `F2 - G2 - H2` | Thưởng hạ gục (Sunk Bonus): **+150 điểm**.
  - **1 Tàu 3 ô - B (Trung bình - Ngang):** Tọa độ `H7 - H8 - H9` | Thưởng hạ gục (Sunk Bonus): **+150 điểm**.
  - **1 Tàu 2 ô (Khó / Về Công ty - Ngang):** Tọa độ `J4 - J5` | Thưởng hạ gục (Sunk Bonus): **+100 điểm**.
  - **83 ô còn lại:** Nước biển.

---

## 2. Teams & Scoreboard
- **4 Đội chơi:** Team 1, Team 2, Team 3, Team 4 (MC có thể click trực tiếp vào tên đội để chỉnh sửa tên thật theo phòng ban).
- **Banner Lượt Chơi:** Header hiển thị nổi bật đội đang đến lượt với hiệu ứng hào quang đèn LED Neon phát sáng.
- **Bảng điểm thời gian thực:** Mỗi đội hiển thị số điểm lớn và 2 nút điều chỉnh nhanh `+50` / `-50` điểm thủ công cho MC xử lý tình huống phát sinh.

---

## 3. Gameplay Flow & Actions (30s Timer & Steal Mechanics)

### Bước 1: Chọn Tọa Độ Bắn
- Đội đang đến lượt chọn 1 ô chưa mở trên bàn cờ:
  - **Bắn trúng Nước biển (Miss):** Cắm chốt trắng kim loại ⚪, phát âm thanh bõm nước + hiệu ứng sóng nước, tự động chuyển lượt sang đội kế tiếp theo vòng tròn (Team 1 → 2 → 3 → 4 → 1).
  - **Bắn trúng Tàu (Hit):** Ô chuyển sang nền đỏ rực 🔴, phát âm thanh đại bác nổ, mở ngay **Modal Câu hỏi Tác chiến**.

### Bước 2: Modal Câu hỏi 30s & Phán quyết của MC
- **Đồng hồ đếm ngược 30 giây:** Có thanh tiến trình gradient giảm dần. 5 giây cuối đổi màu đỏ cảnh báo + âm thanh tick-tock.
- **Kịch bản A - Team hiện tại trả lời ĐÚNG (trong vòng 30s):**
  - MC bấm nút **"Đúng (+100đ)"**.
  - Cắm chốt đỏ 🔴, cộng **+100 điểm** cho đội hiện tại, **giữ nguyên quyền bắn tiếp** cho đội đó.
- **Kịch bản B - Team hiện tại trả lời SAI hoặc HẾT 30s:**
  - Hết 30s: Phát âm thanh Buzzer cảnh báo hết giờ.
  - Modal chuyển sang giao diện **"QUYỀN CƯỚP ĐIỂM"** cho 3 đội còn lại.
  - **Nếu có 1 đội khác cướp điểm ĐÚNG:** MC bấm nút chọn tên đội đó $\rightarrow$ Cộng **+50 điểm**, cắm chốt đỏ 🔴, chuyển lượt chọn ô mới sang đội kế tiếp theo vòng tròn cố định.
  - **Nếu tất cả đều sai / Không ai cướp được:** MC bấm **"Hiện đáp án / 0đ"** $\rightarrow$ Cắm chốt đỏ 🔴, không cộng điểm câu hỏi, chuyển lượt chọn ô mới sang đội kế tiếp theo vòng tròn.

### Bước 3: Hạ Gục Tàu & Hiệu Ứng "SHIP SUNK!" Điện Ảnh
- Khi toàn bộ các ô của 1 con tàu bị bắn trúng:
  - **Kích hoạt Cinematic Cutscene (2.5s):** Màn hình hiển thị 2 dải đen điện ảnh + laser đỏ, dòng chữ 3D khổng lồ **`💥 SHIP SUNK! 💥`** / **`HẠ GỤC [TÊN TÀU]!`**, tiếng còi báo động hạm đội 🚨 + tiếng nổ bom rền vang 💣, rung lắc màn hình và pháo hoa ăn mừng.
  - **Cộng Sunk Bonus:** Cộng điểm thưởng hạ gục (+250đ / +200đ / +150đ / +100đ) cho đội giải quyết thành công ô cuối cùng.
  - **Hiện Thân Tàu Top-Down:** Trên bàn cờ 10x10, nguyên khối tàu chiến kim loại (Top-Down Blueprint SVG) với tháp pháo đại bác và buồng lái sẽ xuất hiện phủ kín các ô của con tàu kèm khung viền cảnh báo rực lửa.

---

## 4. UI Side Panels & MC Tools
- **Bảng Tình Trạng Hạm Đội (Fleet Status Panel):** Bên phải bàn cờ hiển thị danh sách 5 tàu (5 ô, 4 ô, 3 ô A, 3 ô B, 2 ô), số ô máu còn lại và huy hiệu `💥 ĐÃ CHÌM` khi bị hạ.
- **Bản Đồ Radar Mini (10x10 Mini Radar Map):** Hiển thị tổng quan toàn cảnh các vị trí đã bắn và tàu đã chìm.
- **Công cụ MC:**
  - Nút Bật/Tắt âm thanh (Mute/Unmute).
  - Nút Toàn màn hình (Fullscreen) để trình chiếu máy chiếu.
  - Nút Reset ván đấu mới (kèm xác nhận để tránh bấm nhầm).
  - **Ghost Mode (Soi map ngầm):** Ẩn hoàn toàn khỏi giao diện để chống lộ bài khi Share màn hình; chỉ kích hoạt khi MC **nhấn giữ phím `G`** trên bàn phím.

---

## 5. Game Conclusion & Sudden Death (Phân định Quán Quân)
- **Chiến Thắng:** Khi toàn bộ 17 ô tàu bị bắn chìm, hệ thống tự động vinh danh Đội Quán quân có tổng điểm cao nhất kèm pháo hoa toàn màn hình (Confetti) và nhạc khải hoàn (Victory Fanfare).
- **Phân định Hòa điểm (Sudden Death):** Nếu có 2 hoặc nhiều đội có điểm số bằng nhau ở ngôi đầu bảng, giao diện hiển thị nút **"🔥 Kích hoạt Sudden Death"** $\rightarrow$ Mở câu hỏi phụ về ngày sinh nhật CloudGO từ dữ liệu `tieBreaker` để phân định Quán quân.
