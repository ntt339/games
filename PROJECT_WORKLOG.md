# CAROM BILLIARDS (iOS) - TÀI LIỆU DỰ ÁN & NHẬT KÝ CÔNG VIỆC (PROJECT WORKLOG)

> **Dự án**: Carom Billiards (Bida Phăng 3D & Multiplayer)  
> **Nền tảng**: iOS 16.0+ (Tối ưu hóa iPhone & iPad Landscape)  
> **Ngôn ngữ & Công nghệ**: Swift 5.9, SwiftUI, SceneKit 3D, MultipeerConnectivity (Bluetooth/P2P), GameKit, AVFoundation  
> **Cập nhật lần cuối**: Tháng 10/2026  

---

## 📋 MỤC LỤC
1. [Tổng Quan Dự Án](#1-tổng-quan-dự-án)
2. [Kiến Trúc Kỹ Thuật & Cấu Trúc File](#2-kiến-trúc-kỹ-thuật--cấu-trúc-file)
3. [Nhật Ký Các Yêu Cầu & Tính Năng Đã Thực Hiện](#3-nhật-ký-các-yêu-cầu--tính-năng-đã-thực-hiện)
4. [Chi Tiết Các Giải Pháp Kỹ Thuật Đã Áp Dụng](#4-chi-tiết-các-giải-pháp-kỹ-thuật-đã-áp-dụng)
5. [Hướng Dẫn Biên Dịch & Kiểm Thử](#5-hướng-dẫn-biên-dịch--kiểm-thử)
6. [Kế Hoạch & Đề Xuất Phát Triển Tiếp Theo](#6-kế-hoạch--đề-xuất-phát-triển-tiếp-theo)

---

## 1. TỔNG QUAN DỰ ÁN
Carom Billiards là ứng dụng trò chơi thể thao mô phỏng bida Carom (Bida Phăng) 3D chất lượng cao dành cho hệ điều hành iOS. Trò chơi tái hiện chân thực cảm giác thi đấu carom chuyên nghiệp với các cơ chế:
- **3 Thể loại thi đấu chuẩn quốc tế**: Bida Tự Do (*Libre*), Bida 1 Băng (*1-Cushion*), và Bida 3 Băng (*3-Cushion*).
- **Vật lý bida chính xác (Physics Engine)**: Ma sát mặt nỉ Simonis, độ nảy băng cao su, hiệu ứng xoáy áp-phê (trô, cule, né bi, ép-phê thuận/nghịch).
- **Trí tuệ nhân tạo (AI Engine)**: 3 cấp độ thông minh (Dễ, Trung bình - mặc định, Khó) với thuật toán quét tìm đường cơ tối ưu.
- **Chế độ Luyện tập (Training Drills)**: Thư viện bài tập đa dạng, hiển thị bi bóng ma (ghost ball), đường dẫn hướng và gợi ý điểm chạm cơ.
- **Chơi đối kháng đa người chơi (Multiplayer)**:
  - **Bluetooth / Local P2P**: Kết nối trực tiếp giữa 2 thiết bị không cần mạng Internet/Wi-Fi.
  - **Game Center**: Thi đấu trực tuyến qua tài khoản Apple ID.
- **Hệ thống Replay & Video Highlight**: Ghi lại toàn bộ diễn biến cú đánh, cho phép tua chậm, tua lại và xuất video chia sẻ.
- **Giao diện hiện đại (Modern Glassmorphism HUD)**: Tối ưu hoá đặc biệt cho màn hình tai thỏ, Dynamic Island trên iPhone và màn hình lớn iPad.

---

## 2. KIẾN TRÚC KỸ THUẬT & CẤU TRÚC FILE

```
Carom Mobile/
├── Carom Billiards.xcodeproj/               # Project cấu hình Xcode
├── Carom Billiards/
│   ├── CaromBilliardsApp.swift              # Điểm khởi chạy ứng dụng (@main SwiftUI)
│   ├── TableGeometry.swift                  # Thông số kích thước bàn chuẩn (2.84m x 1.42m), toạ độ các điểm đặt bi
│   ├── TableSceneBuilder.swift              # Khởi tạo SceneKit 3D: Mặt bàn, thành băng, nút số (diamonds), ánh sáng, gậy cơ
│   ├── CaromPhysicsEngine.swift             # Động cơ vật lý: Va chạm đàn hồi giữa các bi, nảy băng, độ võng áp-phê (swerve/curve)
│   ├── CaromRules.swift                     # Trọng tài số: Kiểm tra tính hợp lệ của cú đánh Libre, 1 Băng, 3 Băng & tính sê-ri
│   ├── GameSessionViewModel.swift           # Quản lý toàn bộ State trò chơi, lượt đánh, camera, đồng hồ shot clock, đồng bộ mạng
│   ├── CaromAIController.swift              # Trí tuệ nhân tạo (AI): Quét góc đánh, tính toán lực và áp-phê theo cấp độ khó
│   ├── CaromDrill.swift                     # Thư viện bài tập thế bi, gợi ý áp-phê, ghost ball
│   ├── CaromCareerStats.swift               # Lưu trữ thành tích cá nhân, tỉ lệ thắng, sê-ri cao nhất, hệ thống cấp bậc (Rank)
│   ├── CaromBluetoothSession.swift          # Kết nối mạng P2P ngoại tuyến (Offline MultipeerConnectivity qua Bluetooth/Wi-Fi Direct)
│   ├── CaromGameCenterManager.swift         # Kết nối mạng trực tuyến qua Apple Game Center
│   ├── CaromSoundManager.swift              # Quản lý âm thanh đa luồng: Tiếng va chạm bi, tiếng cơ, tiếng băng, BGM lounge
│   ├── CaromReplayExporter.swift            # Lưu trữ frame cú đánh, phát lại tua chậm và xuất video highlight (AVAssetWriter)
│   ├── CaromGameView.swift                  # Giao diện chính SwiftUI: Màn chơi 3D, HUD tỉ số, thanh chỉnh lực, áp-phê pad, modal menus
│   ├── CaromMultiplayerAndStatsView.swift   # Giao diện Sảnh chờ Bluetooth, Game Center, Cài đặt & Bảng thành tích
│   ├── Audio/                               # Kho tài nguyên âm thanh .wav nén chất lượng cao
│   ├── Assets.xcassets/                     # App icon, màu sắc, textures
│   └── Info.plist                           # Quyền Bluetooth (NSBluetoothAlwaysUsageDescription), Game Center
├── docs/                                    # Tài nguyên GitHub Pages: Privacy Policy, Terms, Support
├── APP_STORE_SUBMISSION_GUIDE.md            # Hướng dẫn chi tiết quy trình chuẩn bị và nộp ứng dụng lên App Store
├── README.md                                # Tài liệu giới thiệu nhanh dự án
└── PROJECT_WORKLOG.md                       # Nhật ký phát triển và giải pháp kỹ thuật (File hiện tại)
```

---

## 3. NHẬT KÝ CÁC YÊU CẦU & TÍNH NĂNG ĐÃ THỰC HIỆN

### Giai đoạn 1: Khởi tạo Core Game & Vật lý Bida 3D
- Xây dựng mô hình 3D bàn bida Carom chuẩn tỷ lệ quốc tế với SceneKit.
- Hiện thực hoá công thức chuyển động vật lý bi: Lực ma sát lăn, trượt, va chạm đàn hồi 2 vật thể tròn, phản xạ thành băng cao su.
- Hệ thống áp-phê (Cue Spin Pad): Cho phép người chơi đặt mũi cơ trên mặt bi chủ để tạo xoáy (Topspin, Backspin, English/Sidespin).
- Trọng tài phân giải luật thi đấu chính thức: Bida Libre, Bida 1 Băng, Bida 3 Băng.
- Camera 3 chế độ: Góc nhìn sau gậy cơ (Behind Cue), Góc nhìn trên cao (Top-down 2D), và Góc nhìn bám đuổi (Dynamic Follow).

### Giai đoạn 2: Trí tuệ nhân tạo (AI) & Bài tập luyện (Drills)
- Tích hợp AI thông minh với 3 cấp độ:
  - **Easy**: Đánh cơ bản, độ lệch góc ngẫu nhiên cao.
  - **Medium**: Tính toán góc 1-2 băng, kiểm soát lực tốt (đã đặt làm **mặc định**).
  - **Hard**: Tự động tính toán đường 3 băng chuẩn xác theo hệ thống nút số Diamond system, độ chính xác gần như tuyệt đối.
- Xây dựng chế độ Huấn luyện (Drills) với các thế bi thực tế: Cule gom bi, trô bi kéo lùi, đảo bi 3 băng, kèm hình ảnh minh hoạ đường ngắm mẫu và ghost ball.

### Giai đoạn 3: Hệ thống Replay & Xuất Video Highlight
- Thu thập vị trí bi và chuyển động gậy cơ theo từng frame (60 FPS).
- Tính năng xem lại cú đánh vừa thực hiện ngay trên bàn với thanh tua thời gian (scrubber) và tốc độ phát lại (0.5x, 1x, 2x).
- Khả năng xuất video MP4 chất lượng cao bằng `AVAssetWriter` và mở bảng chia sẻ iOS (UIActivityViewController).

### Giai đoạn 4: Chơi đối kháng Bluetooth & Game Center
- **Chế độ Bluetooth Offline**:
  - Tự động quét và phát hiện thiết bị lân cận (không cần Wi-Fi hay Internet).
  - Đồng bộ hoá các cú đánh, góc ngắm, lực, và trạng thái bàn cờ giữa Host và Client.
  - Tự động kích hoạt trận đấu cho Client khi Host bấm Bắt đầu (không yêu cầu Client phải bấm start lần 2).
  - Xử lý mất kết nối: Đếm ngược 5 giây, tự động xử thắng cho người còn lại và đưa về màn hình chính gọn gàng.
- **Chế độ Game Center**: Ghép phòng trực tuyến ngẫu nhiên hoặc mời bạn bè trong danh bạ Game Center.

### Giai đoạn 5: Tối ưu UI/UX, Hiển thị & Âm thanh
- **Tránh che khuất bàn bida**: Tinh chỉnh vị trí HUD điểm số, đặt gọn gàng ở dải gỗ trên thành băng, không xâm phạm mặt vải nỉ thi đấu.
- **Tối ưu màn hình Dynamic Island / Notch**: Bổ sung tính toán safe area tự động theo kích thước màn hình và hướng xoay ngang landscape.
- **Tăng thời gian shot clock**: Điều chỉnh thời gian chờ đánh từ 30s lên 60s để người chơi có đủ thời gian ngắm nghía và tính nút số.
- **Giữ sáng màn hình trong trận đấu**: Tự động kích hoạt `isIdleTimerDisabled = true` khi trận đấu diễn ra, tránh việc màn hình bị mờ hoặc khóa làm đứt kết nối Bluetooth.
- **Chức năng Bật/Tắt âm thanh thông minh**:
  - Cho phép tắt nhạc nền (BGM) nhưng vẫn giữ nguyên âm thanh đánh bi, va chạm băng và tiếng cơ.
  - Lưu cài đặt này vào `UserDefaults` để tự động khôi phục trong các phiên chơi sau.
- **Thu gọn Menu "Game Modes & Settings"**:
  - Thay thế action sheet mặc định bằng cửa sổ nhỏ gọn (Compact Glassmorphism Modal) cân đối tỷ lệ ngang.
  - Bổ sung nút [X] đóng nhanh cho tất cả các pop-up (Game Modes, Chọn đối thủ, Danh sách bài tập, Hướng dẫn bài tập, Sảnh Multiplayer) mà **không làm gián đoạn trận đấu hoặc bài tập đang chơi**.
- **Khắc phục lỗi phản hồi nút BGM**:
  - Kết nối `CaromSoundManager` với `CaromGameView` thông qua `@ObservedObject` giúp nút bấm cập nhật tức thì trạng thái ON (Xanh lá) và OFF (Đỏ).
- **Ẩn Line Ngắm Bi & Gậy Cơ Trong Chế Độ Multiplayer Khi Chưa Tới Lượt**:
  - Khi thi đấu qua Bluetooth hoặc Game Center, nếu không phải lượt đánh của người chơi (`!isLocalShooter`), đường line ngắm bi (aim guideline) và gậy cơ sẽ được tự động ẩn hoàn toàn.
  - Khi đến lượt đánh của mình hoặc đối thủ đánh hụt/hết giờ, line và cơ sẽ tự động xuất hiện lại ngay tại vị trí bi chủ của người chơi.

### Giai đoạn 6: Hoàn Thiện Quy Trình Mất Kết Nối & Giải Quyết Lỗi Cả 2 Cùng Thắng
- **Thời gian ân hạn Reconnection (Grace Period) & Background Keepalive**:
  - Trước đây, khi người chơi chỉ vuốt mở Control Center hoặc chuyển app trong 1-2 giây, hệ thống đã vội vàng gửi lệnh ngắt kết nối (`peerLeft`).
  - Đã chuyển từ `willResignActiveNotification` sang `didEnterBackgroundNotification`, kết hợp với `UIApplication.shared.beginBackgroundTask` để duy trì kết nối nền.
  - Cung cấp thời gian ân hạn 10-12 giây: nếu người chơi quay lại app trong thời gian này, đồng hồ đếm ngược lập tức bị hủy, gửi ping khôi phục và tiếp tục trận đấu bình thường mà không xử thua.
- **Giải quyết triệt để lỗi "Cả 2 người chơi cùng thắng" (Bounce-back PeerLeft Bug)**:
  - Trước đây, khi Người B thắng do Người A thoát app, hàm `disconnect()` của Người B lại tự động gửi ngược gói tin `peerLeft` về cho Người A. Người A nhận được gói tin này cũng tự nhận định Người B đã thoát và cả 2 bên đều được ghi nhận chiến thắng (`isWin = true`).
  - Khắc phục: Phân tách rõ ràng luồng Xử thua (`handleLocalPlayerForfeit`) với `isWin = false` cho bên bỏ cuộc, và Xử thắng (`finalizeDisconnectVictoryAndReturnHome`) với `isWin = true` cho bên ở lại.
  - Hàm `disconnect(broadcastPeerLeft: Bool = false)` mặc định không bắn ngược gói tin `peerLeft` khi dọn dẹp sau trận đấu.

### Giai đoạn 7: Mở Rộng Thi Đấu Đa Người Chơi (Multiplayer Up To 8 Players: 2vs2, 4vs4 & Vòng Tròn Độc Lập)
- **Hỗ trợ tối đa 8 người chơi (Bluetooth) và 4 người chơi (Game Center)**:
  - Nâng cấp `CaromBluetoothSession` quản lý danh sách `connectedPeers` động lên tới 7 thiết bị khách + 1 máy chủ = 8 người chơi cùng lúc.
  - Tự động duy trì việc mở nhận kết nối (`startAdvertisingPeer`) cho đến khi đủ số lượng người chơi theo thể thức hoặc chủ phòng bắt đầu trận đấu.
- **4 Thể thức thi đấu đa dạng (`MultiplayerFormat`)**:
  - `oneVsOne`: Đấu đơn kinh điển (2 người chơi, Bi Trắng vs Bi Vàng).
  - `twoVsTwo`: Đấu đôi (4 người chơi: Đội Trắng [P1 & P3] vs Đội Vàng [P2 & P4], xoay vòng lượt đánh luân phiên giữa các đồng đội).
  - `fourVsFour`: Đấu đội quy mô lớn (8 người chơi: Đội Trắng 4 người vs Đội Vàng 4 người, phù hợp chơi nhóm offline qua Bluetooth).
  - `freeForAll`: Vòng tròn độc lập (Từ 3 đến 8 người chơi: Mỗi người thi đấu tính điểm cá nhân riêng biệt, cạnh tranh về đích với Race To).
- **Hệ thống Đồng bộ Lượt đánh & Phân bổ Bi Chủ**:
  - Chế độ Đấu Đội (Team Mode): Đội Trắng dùng Bi Trắng, Đội Vàng dùng Bi Vàng. Điểm số của từng thành viên được cộng dồn trực tiếp vào điểm tổng của Đội.
  - Chế độ Vòng tròn Độc lập (Free-For-All): Người đang tới lượt đánh luôn cầm Bi Trắng, người kế tiếp cầm Bi Vàng, bi còn lại và bi đỏ là bi mục tiêu; lượt xoay vòng tuần tự qua tất cả thành viên trong phòng.
- **Giao diện Bảng Điểm Thích Ứng (Adaptive Top HUD Scoreboard)**:
  - *Team Mode*: Bố cục 2 khối đối xứng `⚪ ĐỘI TRẮNG` và `🟡 ĐỘI VÀNG`, hiển thị điểm tổng và danh sách thẻ tên từng cơ thủ. Cơ thủ đang đánh được làm nổi bật với viền sáng Neon và biểu tượng hồng tâm 🎯.
  - *Free-For-All Mode*: Dải thanh điểm nằm ngang (Score Ribbon Bar) với các chip thông tin nhỏ gọn hiển thị tên, điểm, bi chủ và trạng thái kết nối của từng người. Kèm nút bấm nhanh `[📊 BXH]` để mở bảng xếp hạng chi tiết.
- **Bảng Xếp Hạng Trực Tiếp Trong Trận (`MultiplayerLeaderboardModalView`)**:
  - Hiển thị thứ hạng (Rank 1, 2, 3...), tên người chơi/đội, điểm số hiện tại so với mục tiêu (Score / Race), điểm sê-ri cao nhất (High Run), và trạng thái kết nối trực tuyến/ngoại tuyến.
- **Xử lý Ngắt Kết Nối Linh Hoạt Trong Phòng Nhiều Người**:
  - Trong phòng 3-8 người, khi một người chơi mất kết nối, hệ thống không hủy cả trận đấu mà đánh dấu trạng thái người đó là mất kết nối (`isConnected = false`), tự động chuyển lượt sang cơ thủ kế tiếp nếu đang tới lượt họ, cho phép các cơ thủ còn lại tiếp tục tranh tài bình thường.

### Giai đoạn 8: Chuẩn Hóa Ngôn Ngữ Giao Diện Sang Tiếng Anh Toàn Diện (Full English Localization)
- **Chuẩn hóa 100% văn bản giao diện người dùng (UI) sang tiếng Anh quốc tế**:
  - **Bảng điểm thích ứng HUD**: `TEAM WHITE`, `TEAM YELLOW`, `Format: ...`, `Target: ... pts`.
  - **Sảnh thi đấu (Lobby)**: `MATCH FORMAT` (1 vs 1 Single, 2 vs 2 Doubles, 4 vs 4 Team, Free-For-All), `ROOM ROSTER`, `⚪ Team White vs 🟡 Team Yellow`, `Team White • Host`, `Player 1 • Host`.
  - **Bảng xếp hạng trực tiếp (`Live Standings`)**: `LIVE STANDINGS`, `(You)`, `🎯 Shooting`, `High Run: ...`, `• Disconnected`.
  - **Cài đặt & Âm thanh**: `Background Music (BGM)`, `Toggle ambient music while preserving all game sound effects`.
  - **Thông báo thắng trận & phản hồi cú đánh**: `🏆 Team White Won the Match!`, `🏆 [Winner] is the Champion!`.
  - **Quyền hệ thống (`Info.plist`)**: Cập nhật `NSLocalNetworkUsageDescription` sang tiếng Anh chuẩn hỗ trợ multi-player ("...for multiplayer matches").

---

## 4. CHI TIẾT CÁC GIẢI PHÁP KỸ THUẬT ĐÃ ÁP DỤNG

### 4.1. Kết nối Bluetooth Ngoại tuyến (Offline P2P Multipeer Connectivity)
* **Framework**: `MultipeerConnectivity` của Apple.
* **Service Type**: `"carom-game"`
* **Cơ chế**:
  - Thiết bị làm Host sẽ kích hoạt `MCNearbyServiceAdvertiser`.
  - Thiết bị làm Client kích hoạt `MCNearbyServiceBrowser`.
  - Kết nối sử dụng mã hoá `MCEncryptionPreference.required`, truyền dữ liệu qua giao thức tin cậy `MCSessionSendDataMode.reliable`.
  - Giao thức hoạt động trực tiếp qua Bluetooth LE kết hợp Wi-Fi Peer-to-Peer nội bộ của chip Apple, **hoàn toàn không cần kết nối mạng Internet hay Router Wi-Fi**.

### 4.2. Xử lý Trạng thái Reactive trong SwiftUI (`@ObservedObject`)
* Trước đây, view đọc trạng thái từ singleton `CaromSoundManager.shared.isMusicEnabled` nhưng không khai báo là `@ObservedObject`. Khi giá trị thay đổi, `AVAudioPlayer` dừng nhạc nhưng SwiftUI không kích hoạt render lại giao diện.
* **Giải pháp**: Khai báo `@ObservedObject private var soundManager = CaromSoundManager.shared` trong `CaromGameView`. Đảm bảo mọi thay đổi của âm thanh đều lập tức làm mới giao diện và đổi màu nút (Xanh lá / Đỏ) đồng bộ.

### 4.3. Bố trí HUD tránh che khuất mặt bàn & Xử lý Dynamic Island
* Bàn bida được render trực giao/phối cảnh ở vùng trung tâm.
* Bảng điểm HUD được định vị với `.padding(.top, topHUDPadding(geometry: geometry))` và giới hạn `maxWidth`, đảm bảo nằm gọn trong dải viền gỗ mahogany phía trên, giải phóng 100% không gian góc nảy và bi nằm sát băng trên.

### 4.4. Giữ Màn hình Luôn Sáng (Keep Screen Awake)
* Khi bắt đầu trận đấu, thiết lập `UIApplication.shared.isIdleTimerDisabled = true`.
* Khi trận đấu kết thúc hoặc thoát ra màn hình chính, khôi phục `UIApplication.shared.isIdleTimerDisabled = false` để tiết kiệm pin.

### 4.5. Đóng Menu Không Làm Gián Đoạn Trận Đấu
* Các nút bấm [X] chỉ điều khiển biến hiển thị của modal (`showModeSelector = false`, `showOpponentSelector = false`,...), hoàn toàn không gọi `resetGame()` hay tác động vào `GameSessionViewModel`. Trạng thái vật lý của các bi, điểm số, lượt cơ được giữ nguyên trạng thái ban đầu.

### 4.6. Cơ Chế Xử Lý Mất Kết Nối & Tự Động Phục Hồi (Fault-Tolerant Reconnection)
* **Background Keepalive Task**: Khi người chơi rời app tạm thời, hệ thống xin iOS cấp `UIBackgroundTaskIdentifier` để giữ socket Bluetooth/Game Center không bị ngắt ngay lập tức.
* **10-12s Grace Period**: Bắt đầu bộ đếm thời gian ân hạn. Nếu người chơi kích hoạt lại app (`didBecomeActiveNotification`), bộ đếm hủy bỏ, gửi gói tin Ping/Resume, hiển thị thông báo "🟢 Opponent reconnected! Match continues." và tiếp tục trận đấu trơn tru.
* **Đảm bảo tính duy nhất người thắng cuộc**: Chỉ người chơi duy trì kết nối trong phòng mới được ghi nhận thắng (`isWin = true`). Người rời app quá thời gian ân hạn sẽ bị ghi nhận thua phạt (`isWin = false`) vào `CareerStatsManager`. Cờ `broadcastPeerLeft: false` ngăn chặn hoàn toàn hiện tượng phản hồi ngược gói tin gây thắng kép.

### 4.7. Kiến Trúc Mạng Đa Điểm P2P Bluetooth 8 Người & Bảng Điểm Thích Ứng (Adaptive HUD Scoreboard)
* **Mạng Đa Điểm Star Topology trên MultipeerConnectivity**:
  - Máy Chủ (Host) đóng vai trò trung tâm điều phối (Coordinator), quản lý mảng `connectedPeers: [MCPeerID]`.
  - Gói tin định tuyến đa người chơi:
    - `.lobbyRosterSync`: Máy chủ liên tục phát sóng danh sách người chơi, gán số thứ tự slot và đội tuyển (Team White / Team Yellow) cho từng thiết bị khách khi họ tham gia phòng.
    - `.multiMatchSetup`: Đồng bộ thể thức thi đấu (`MultiplayerFormat`), danh sách cơ thủ `CaromPlayerSlotData`, và cấu hình trận đấu cho toàn bộ phòng khi chủ phòng bấm Bắt đầu.
    - `.multiTurnSync`: Đồng bộ chỉ số cơ thủ đang tới lượt đánh (`activePlayerIndex`), điểm số các đội (`teamWhiteScore`, `teamYellowScore`) và danh sách điểm cá nhân sau mỗi cú đánh hoặc khi chuyển lượt.
* **Cơ Chế Chuyển Tiếp Gói Tin Star-Topology (Host Packet Relay)**:
  - Trong mạng hình sao của MultipeerConnectivity, các máy khách chỉ kết nối trực tiếp với Máy Chủ (Host). Khi một cơ thủ khách thực hiện cú đánh hoặc bắn emote, Máy Chủ tự động nhận diện và chuyển tiếp (Relay) gói tin `.shot`, `.emote`, `.tableSync` tới toàn bộ các thiết bị khách còn lại trong phòng. Nhờ vậy, tất cả 8 thiết bị đều quan sát được chuyển động cơ và bi của nhau theo thời gian thực mượt mà.
* **Cơ Chế Phân Bổ Bi & Xoay Vòng Lượt Đánh Trong Free-For-All**:
  - Với thể thức vòng tròn độc lập nhiều người, để giữ vững tính công bằng và cảm giác chơi tự nhiên của Bida Carom, cơ thủ ở lượt hiện tại luôn điều khiển Bi Trắng, người đánh lượt kế tiếp sở hữu Bi Vàng (làm bi đích thứ 1), và Bi Đỏ là bi đích thứ 2. Khi hết lượt (đánh hụt), lượt đánh xoay vòng (`(activePlayerIndex + 1) % players.count`), bi chủ được tráo đổi liền mạch trong SceneKit.
* **Giao Diện Bảng Điểm Thích Ứng Không Che Khuất Mặt Vải Nỉ**:
  - Router giao diện `adaptiveScoreHeader` tự động nhận diện chế độ chơi để kích hoạt:
    - `standardScoreHeader` cho chế độ 1vs1 truyền thống.
    - `teamScoreHeader` cho 2vs2 và 4vs4 với 2 Team Pods thanh lịch.
    - `freeForAllRibbonHeader` cho vòng tròn độc lập nhiều người.
  - Toàn bộ các thanh điểm đều được giới hạn chiều cao nghiêm ngặt, bám sát mép viền gỗ trên thành băng (Top Mahogany Rail), đảm bảo 100% diện tích mặt nỉ Simonis luôn thông thoáng.

### 4.8. Chuẩn Hóa Ngôn Ngữ Ứng Dụng Sang Tiếng Anh Toàn Diện (Full English App UI Localization)
* **Chuẩn hóa toàn bộ chuỗi văn bản**:
  - Giao diện sảnh chính, chế độ chơi, tùy chọn độ khó AI, hướng dẫn luyện tập, các thông điệp cảnh báo mất kết nối, kết quả trận đấu, thông số thẻ chia sẻ Highlight và menu Pause/Setting đều được chuyển thể sang tiếng Anh tự nhiên, chuẩn mực thuật ngữ Billiards quốc tế.

### 4.9. Nâng Cấp Xuất Video Trận Đấu, Tinh Chỉnh Độ Khó AI & Tối Ưu Vật Lý Bi Lăn (Replay Video Cue Strike, Clean Cloth & Accessible Physics)
* **Hoạt Ảnh Đánh Cơ Trong Video Xuất (Cue Stick Strike Animation)**:
  - Bổ sung chuỗi khung hình tiền đề (Pre-roll Cue Stroke Sequence - 26 frames @ 60fps) trong `CaromReplayExporter.swift`: Gậy cơ xuất hiện phía sau bi chủ theo đúng góc ngắm `aimAngle`, thực hiện động tác kéo cơ lấy đà (smooth pullback), lao nhanh về phía trước chạm bi chủ (accelerated thrust), theo đà nhẹ và tan mờ dần khi bi bắt đầu chuyển động.
* **Loại Bỏ Vạch Hướng Dẫn/Đường Bi Chạy Trong Video**:
  - Tự động ẩn toàn bộ các node vạch đường bi (`trajectory_root`, `aimLineRootNode`, `drillVisualsRoot`) trong suốt quá trình render video MP4 để giữ trọn vẻ đẹp chân thực, sang trọng của bàn bida 3D như một góc máy truyền hình trực tiếp. Khôi phục nguyên vẹn trạng thái sau khi xuất xong.
* **Độ Khó Mặc Định Dễ Dàng Tiếp Cận**:
  - Chuyển độ khó AI mặc định khi mở game thành **Easy** (`.easy`) để người chơi mới dễ làm quen và trải nghiệm chiến thắng phấn khích ban đầu.
* **Cân Chỉnh Vật Lý & Lực Đánh Thân Thiện Hơn**:
  - Nâng lực đánh mặc định ban đầu lên `4.8 m/s` (từ `3.5 m/s`).
  - Giảm hệ số ma sát lăn `rollingFriction = 0.0095` (từ `0.012`) giúp bi lăn êm ái và xa hơn.
  - Tăng độ đàn hồi băng cao su `cushionRestitution = 0.87` (từ `0.82`) và giảm ma sát băng `cushionFriction = 0.18` (từ `0.20`), giúp bi nảy thoát băng sống động, tạo điều kiện thuận lợi cho các cú chạm 3 băng (3-Cushion Carom).
  - Tăng hệ số tác động xoáy trô/cule (`verticalSpinFactor = 44.0`, `sideSpinFactor = 38.0`) cho phép người chơi tạo những đường bi cong nghệ thuật dễ dàng hơn.

### 4.10. Tối Ưu & Hoàn Thiện Chuẩn Bị Phát Hành App Store Chính Thức (App Store Production Readiness & Optimizations)
* **Khai Báo Quyền Truy Cập Quyền Riêng Tư (Privacy & Info.plist)**:
  - Bổ sung `NSPhotoLibraryAddUsageDescription` và `NSPhotoLibraryUsageDescription`: Đảm bảo khi người chơi lưu video highlight MP4 hoặc hình ảnh bàn bida 3D vào thư viện ảnh hệ thống (Photos / Camera Roll) qua `UIActivityViewController` hoạt động trơn tru 100%, không bị crash hoặc từ chối duyệt App Store.
  - Cấu hình `ITSAppUsesNonExemptEncryption = false`: Tự động bỏ qua bước khai báo tuân thủ mã hóa xuất khẩu (Export Compliance) khi tải file build `.ipa` lên App Store Connect và TestFlight.
* **Tích Hợp Xin Đánh Giá Xếp Hạng App Store Thông Minh (In-App Store Review)**:
  - Tích hợp `SKStoreReviewController.requestReview(in:)` trong `CaromCareerStats.swift`. Hệ thống sẽ chỉ kích hoạt sau các cột mốc chiến thắng xứng đáng (khi đạt 3 trận thắng, 8 trận thắng, 20 trận thắng) khi tâm lý người chơi đang phấn khích nhất, hoàn toàn tuân thủ chặt chẽ Apple Human Interface Guidelines (HIG).
* **Bảo Vệ Tài Nguyên & Chống Treo Vòng Lặp Video Export**:
  - Bổ sung kiểm tra trạng thái luồng ghi `AVAssetWriter.status` (`.failed` / `.cancelled`) trong toàn bộ vòng lặp xử lý frame của `CaromReplayExporter.swift`, ngăn chặn khả năng đóng băng luồng khi thiết bị cạn bộ nhớ lưu trữ hoặc người dùng hủy giữa chừng.
* **Kiểm Tra Toàn Diện Hệ Thống**:
  - Đã xác thực toàn bộ tài nguyên: AppIcon 1024x1024 Universal, cấu hình Game Center Entitlements, âm thanh đa kênh Audio Session `.ambient` (hỗ trợ nghe nhạc song song), chế độ chơi ngoại tuyến hoàn toàn khi không có mạng (Offline VS AI & Practice), và ngôn ngữ giao diện tiếng Anh chuẩn quốc tế.

### 4.11. Xử Lý Triệt Để Vạch Đường Bi Chạy & Tái Hiện Cây Cơ Thụt Bi Sắc Nét Trong Replay / Video
* **Xóa Bỏ Triệt Để 100% Vạch Đường Bi Lăn (Zero Artifact Line Rendering)**:
  - Khắc phục đặc tính của SceneKit Offscreen Renderer (bỏ qua cờ `isHidden` trên các node container): Áp dụng cơ chế tách rời vật lý (`removeFromParentNode`) toàn bộ cây node vạch đường bi (`trajectory_root`, các đốt trụ `trajectory_segment`, `aimLineRootNode`) khỏi Scene Graph trong suốt quá trình quay video hoặc phát Replay. Sau khi kết thúc, node được đính kèm lại nguyên vẹn.
* **Cây Cơ Thi Đấu Siêu Nét (High-Visibility Tournament Cue Stick)**:
  - Thiết kế module cơ 5 thành phần riêng biệt `TableSceneBuilder.buildHighVisibilityCueStickNode()`:
    1. Đầu lơ xanh (Blue Chalk Tip - bán kính 9mm, đường kính 18mm) tiếp xúc bóng nổi bật.
    2. Bọc phíp ngà trắng (Ivory White Ferrule - 22mm) tương phản cao.
    3. Ngọn cơ gỗ thích Canada (Canadian Maple Shaft - 82cm) bóng vàng ấm áp.
    4. Vòng ren khớp nối mạ vàng kim loại phản chiếu ánh sáng (Metallic Gold Joint Ring).
    5. Cán cơ gỗ mun đen bọc chỉ Linen (Midnight Ebony Butt - 58cm, đường kính 42mm).
* **Góc Máy Quay Truyền Hình 3D Độc Quyền Cho Video (Broadcast Video Camera)**:
  - Khởi tạo camera riêng biệt `videoCameraNode` (vị trí `(0, 4.8, 2.2)`, góc nhìn 36° FOV hướng về trung tâm bàn) dành riêng cho Video Replay. Nhờ đó, bất kể người chơi đang để camera 2D từ trên trần nhìn xuống hay zoom sát bi, video xuất ra luôn có góc máy truyền hình thể thao sống động, thấy rõ toàn bộ cây cơ và 4 bờ băng.
* **Chuỗi Hoạt Ảnh Thụt Cơ Điện Ảnh (64-Frame Cinematic Stroke Sequence)**:
  - Tăng thời lượng thụt cơ lên 1.06 giây (64 khung hình @ 60fps) với đầy đủ 4 giai đoạn chuẩn bida:
    - *Giai đoạn 1 (0-0.27s)*: Đặt cơ cân chỉnh hướng bi ngắm.
    - *Giai đoạn 2 (0.27-0.70s)*: Kéo cơ lấy đà ra phía sau mượt mà (smooth pullback 14-24cm).
    - *Giai đoạn 3 (0.70-0.87s)*: Vung cơ gia tốc mạnh mẽ về phía trước (accelerated forward thrust).
    - *Giai đoạn 4 (0.87-1.06s)*: Chạm đầu lơ vào bi chủ kèm âm thanh thụt cơ giòn tan, sau đó tan mờ dần khi 3 viên bi bắt đầu lăn tự do trên mặt nỉ sạch bóng.
* **Đồng Bộ Hoàn Hảo Cho Cả Replay Trực Tiếp Trong Game (`GameSessionViewModel.swift`)**:
  - Khi người chơi bấm nút **Replay** trong game: vạch bi xanh cũng tự động ẩn đi hoàn toàn, gậy cơ xuất hiện thực hiện cú đánh với hoạt ảnh `SCNAction` mượt mà trước khi các bi chuyển động.

### 4.12. Tối Ưu Độ Nhạy & Tốc Độ Xoay Hướng Ngắm Bi (Precision Aiming Curve & Anti-Overshoot)
* **Đường Cong Độ Nhạy Đa Tầng (Dynamic Aiming Sensitivity Curve)**:
  - Khắc phục tình trạng xoay hướng bị vọt quá nhanh (*slipping/overshooting*): Thay thế hệ số cố định `0.005` bằng thuật toán kiểm soát độ nhạy thích ứng theo vận tốc ngón tay:
    - *Vi chỉnh chính xác (Micro-drags < 3 pt)*: Độ nhạy giảm xuống `0.0010` (giảm 80%), giúp người chơi nhích từng li góc ngắm, dễ dàng căn chuẩn các độ dày bi mỏng như 1/4 hay 1/8 hoặc điểm chạm băng chính xác tuyệt đối mà không sợ bị trượt qua mục tiêu.
    - *Theo dõi thông thường (Standard tracking 3 - 10 pt)*: Độ nhạy duy trì ở `0.0016` cho cảm giác rê tay đầm chắc, mượt mà và kiểm soát tốt.
    - *Chuyển hướng nhanh (Fast swipes > 10 pt)*: Tối đa `0.0022` giúp đổi hướng nhanh chóng khi cần ngắm sang góc bàn đối diện mà không bị quay mòng mòng mất kiểm soát.
* **Tinh Chỉnh Phím Tắt Bàn Phím Rời**:
  - Giảm bước xoay của phím mũi tên Left/Right trên bàn phím iPad từ `0.02 rad` xuống `0.008 rad` (~0.45°/lần bấm) để hỗ trợ căn góc chi tiết.

---

## 5. HƯỚNG DẪN BIÊN DỊCH & KIỂM THỬ

### 5.1. Môi trường Yêu cầu
- Máy tính macOS (khuyến nghị macOS Sonoma hoặc Sequoia).
- Xcode 15.0 trở lên.
- Thiết bị iOS 16.0+ (iPhone hoặc iPad) hoặc iOS Simulator.

### 5.2. Lệnh Biên Dịch Qua Terminal (xcodebuild)
```bash
xcodebuild -project "Carom Billiards.xcodeproj" \
           -scheme "Carom Billiards" \
           -destination "generic/platform=iOS Simulator" \
           build
```

### 5.3. Kiểm thử Tính năng Bluetooth P2P
1. Chuẩn bị 2 thiết bị iPhone/iPad thật.
2. Bật Bluetooth trên cả 2 máy (không cần bật Wi-Fi hoặc 4G/5G).
3. Mở ứng dụng, vào mục **Bluetooth**.
4. Máy 1 chọn **Create Table** (Tạo bàn), Máy 2 sẽ thấy bàn xuất hiện trong danh sách và bấm **Join** (Tham gia).
5. Sau khi kết nối, Chủ bàn bấm **Start Match** -> Cả 2 máy tự động vào bàn thi đấu.

---

## 6. KẾ HOẠCH & ĐỀ XUẤT PHÁT TRIỂN TIẾP THEO
- [ ] **Tùy biến Gậy Cơ & Màu Nỉ Bàn**: Thêm cửa hàng vật phẩm với nhiều mẫu cơ cẩn xà cừ, thay đổi màu nỉ (Xanh dương Royal Blue, Xanh lá Tournament Green, Xám Than).
- [ ] **Chế độ Thách đấu Giải Đấu (Tournament Mode)**: Vòng đấu bảng và loại trực tiếp với các cơ thủ AI tên tuổi.
- [ ] **Chỉ dẫn Hệ thống Nút Số (Diamond System Visualizer)**: Hiển thị vạch tính toán số điểm tia tới, tia phản xạ phục vụ người mới tập chơi 3 băng.
- [ ] **Hỗ trợ Tay cầm Chơi game (Game Controller)**: Tích hợp tay cầm PlayStation / Xbox qua GameController framework.
