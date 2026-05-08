# TÀI LIỆU THIẾT KẾ GAME (GAME DESIGN DOCUMENT) - IDLE TOWER

## 1. TỔNG QUAN DỰ ÁN (OVERVIEW)
- **Tên trò chơi:** Idle Tower
- **Thể loại:** Idle Tower Defense (Tháp phòng thủ rảnh tay) với yếu tố Roguelike.
- **Nền tảng mục tiêu:** Android, iOS.
- **Mô tả ngắn:** Người chơi điều khiển một tòa tháp duy nhất, nâng cấp chỉ số và trang bị các kỹ năng đặc biệt để chống lại hàng ngàn đợt quái vật. Game kết hợp giữa sự kiên nhẫn của dòng Idle và sự tùy biến chiến thuật của dòng Roguelike thông qua hệ thống thẻ bài.

---

## 2. CƠ CHẾ LỐI CHƠI (CORE GAMEPLAY)
### 2.1. Vòng lặp trò chơi (Core Loop)
1. **Phòng thủ (Battle):** Tháp tự động bắn quái trong phạm vi. Quái bị tiêu diệt rơi ra Coins và Scrap.
2. **Nâng cấp tức thời (In-game Upgrade):** Sử dụng **Scrap** để nâng cấp các chỉ số cơ bản ngay trong trận đấu (Damage, Health, Range...).
3. **Tiến hóa kỹ năng (Card Selection):** Cứ mỗi 2 Wave, người chơi chọn 1 trong 3 thẻ **Enhance Card** để nhận buff mạnh hoặc kỹ năng đặc biệt.
4. **Tích lũy vĩnh viễn (Meta-progression):** Khi tháp bị phá hủy, người chơi dùng **Coins** để nâng cấp chỉ số vĩnh viễn tại sảnh chính (Home).

### 2.2. Hệ thống Đợt quái (Wave System)
- **Cấu trúc:** Mỗi đợt quái có số lượng và tốc độ sinh (spawn rate) tăng dần.
- **Boss Wave:** Mỗi 5 Wave (5, 10, 15...) sẽ xuất hiện 1 Boss. Boss có lượng máu lớn và kỹ năng bắn đạn từ xa.
- **Độ khó (Tier):**
  - **Tier 1:** Độ khó cơ bản, kết thúc ở Wave 15 (Victory) để mở khóa Tier 2.
  - **Tier cao hơn:** Chỉ số quái tăng theo hệ số nhân (x2, x3...), phần thưởng Coins cũng tăng theo.

### 2.3. Hệ thống Hồi phục (Resume System)
- Trò chơi tự động lưu trạng thái (Wave, Máu tháp, Scrap, Buff đã chọn) sau mỗi Wave hoàn thành.
- Người chơi có thể thoát ra và tiếp tục trận đấu sau đó từ Wave đã lưu.

---

## 3. HỆ THỐNG THÁP (TOWER SYSTEM)
### 3.1. Chỉ số cơ bản & Công thức nâng cấp
Mọi chỉ số đều tuân theo cấu trúc `baseValue + (level * incrementPerLevel)`. Chi phí nâng cấp (Scrap/Coins) tăng theo `baseCost + (level * costIncrement)`.

| Chỉ số | Ý nghĩa | Thông số mặc định (Base/Inc) |
| :--- | :--- | :--- |
| **Damage** | Sát thương mỗi viên đạn | 10 / +2 |
| **Attack Speed** | Số phát bắn mỗi giây | 1.0 / +0.03 |
| **Range** | Bán kính tầm bắn (Max: 3.5) | 2.5 / +0.01 |
| **Health** | Lượng máu tối đa của tháp | 100 / +20 |
| **Health Regen** | Máu hồi mỗi giây (Max: 3) | 0 / +0.02 |
| **Defense %** | Giảm sát thương nhận vào % (Max: 75%) | 0 / +0.0015 |
| **Defense Absolute** | Trừ thẳng sát thương nhận vào | 0 / +1 |
| **Crit Chance** | Tỉ lệ chí mạng | 5% / +1% |
| **Crit Factor** | Hệ số sát thương chí mạng | 1.5x / +0.05x |
| **Damage Meter** | Tăng sát thương dựa trên khoảng cách | 0 / +0.1x |

### 3.2. Đạn đặc biệt (Specialty Bullets)
Các loại đạn này có hiệu ứng riêng và thời gian hồi chiêu (Cooldown):
- **Fire Bullet:** Gây hiệu ứng Burn (Đốt cháy), gây sát thương theo thời gian.
- **Ice Bullet:** Gây hiệu ứng Freeze (Đóng băng), làm quái đứng yên 1.5s.
- **Acid Bullet:** Đánh dấu quái (Mark), nhận thêm +70% sát thương từ mọi nguồn.
- **Slow Bullet:** Làm chậm (Slow) tốc độ di chuyển của quái.
- **Laser Bullet:** Bắn tia laser xuyên thấu mọi mục tiêu trên một đường thẳng.
- **Split Bullet:** Đạn vỡ ra thành nhiều viên nhỏ khi trúng mục tiêu.
- **Boomerang Bullet:** Đạn bay đi và quay trở lại, gây sát thương 2 lần.

---

## 4. HỆ THỐNG KẺ THÙ (ENEMIES)
Quái vật di chuyển về phía tháp và tấn công khi tiếp cận gần.

| Loại quái | Đặc điểm đặc biệt |
| :--- | :--- |
| **Normal** | Di chuyển cơ bản. |
| **Boss** | Có thanh máu thế giới (World HP Bar), bắn đạn tầm xa, máu tăng +50 mỗi lần xuất hiện. |
| **Splitter** | Khi chết sinh ra 2 quái con với 50% máu. |
| **Guardian** | Dừng lại ở khoảng cách 2m để làm lá chắn cho đồng đội. |
| **Healer** | Định kỳ 3s hồi 6 máu cho đồng đội trong bán kính 5m. |

---

## 5. HỆ THỐNG THẺ BÀI (BUFF CARDS)
Thẻ bài được chia làm 3 loại chính:
### 5.1. Skill Cards (Thẻ Kỹ Năng)
Thẻ vĩnh viễn, nâng cấp ở Home và trang bị vào tháp trước trận đấu.
- **Add Projectile:** Tăng số lượng đạn bắn ra mỗi lần (Multi-shot).
- **Bounce Shot:** Đạn nảy giữa các kẻ thù.
- **Add Missile:** Tên lửa tầm nhiệt gây sát thương diện rộng.
- **Energy Barrier:** Tạo khiên ảo bảo vệ tháp.

### 5.2. Enhance Cards (Thẻ Tăng Cường)
Thẻ tạm thời, xuất hiện ngẫu nhiên trong trận đấu (mỗi 2 wave).
- **Enhance Damage:** +70% sát thương (cộng dồn).
- **Nuclear Fallout:** Để lại vùng phóng xạ gây sát thương liên tục nơi quái chết.
- **Plasma Chain:** Tia điện nhảy giữa các quái vật (1 -> 2 -> 3 mục tiêu).
- **Hack Drive:** Cơ hội khiến quái bị "hack" và quay lại tấn công đồng đội.
- **Overload:** Mỗi phát bắn thứ X sẽ gây thêm sát thương nổ.

---

## 6. HỆ THỐNG TIỀN TỆ & KINH TẾ (ECONOMY)
- **Coins (Vàng):** Tiền tệ chính. Nhận được qua: `Wave + Tier - 1`. Dùng cho nâng cấp vĩnh viễn.
- **Scrap (Linh kiện):** Nhận được từ quái. Dùng để nâng cấp trong trận. Reset sau mỗi trận.
- **Diamonds (Kim cương):** Tiền tệ cao cấp. Dùng để mua gói Speed X3, X4, hoặc các gói nạp đặc biệt.
- **Interest (Lãi suất):** Nhận thêm Coins dựa trên lượng Coins hiện có vào cuối mỗi Wave.

---

## 7. CHẾ ĐỘ TRỰC TUYẾN (ONLINE MODE)
- **Lobby System:** Người chơi tạo phòng hoặc nhập ID để đấu chung.
- **Real-time Sync:** Đồng bộ vị trí quái thông qua `SyncedSeed`. Mọi người chơi thấy quái giống nhau.
- **Damage Ranking:** Hệ thống xếp hạng người chơi dựa trên tổng sát thương gây ra.
- **Spectating:** Khi tháp của bạn bị phá hủy, bạn có thể theo dõi tháp của đồng đội.
- **Network Authority:** Server (Host) chịu trách nhiệm tính toán logic quái, Client nhận dữ liệu và hiển thị.

---

## 8. ÂM THANH & HÌNH ẢNH (ART & AUDIO)
- **Art:** Sprite 2D, hiệu ứng nhấp nháy màu khi quái dính trạng thái (Cyan cho Freeze, Magenta cho Hack, Green cho Poison).
- **Audio:** `AudioManager.cs` quản lý nhạc nền (BGM) và hiệu ứng âm thanh (SFX) cho bắn đạn, nổ, nâng cấp và click UI.

---

## 9. CÁC GÓI VẬT PHẨM (PRICE PACKAGES)
- **Diamond Packs:** Từ 180 đến 7800 Kim cương (Tất cả đều là x2 cho lần mua đầu).
- **Remove Ads:** Xóa quảng cáo bắt buộc.
- **Starter Pack Combo:** 600 Kim cương + 1000 Coins + 1 Thẻ kỹ năng ngẫu nhiên.
- **Speed Packs:** Mở khóa tốc độ X3 (200 Kim cương), X4 (500 Kim cương) vĩnh viễn.
