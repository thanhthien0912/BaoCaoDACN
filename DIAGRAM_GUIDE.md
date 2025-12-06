# HƯỚNG DẪN VẼ SƠ ĐỒ BÁO CÁO (Sử dụng Mermaid.js)

Tài liệu này cung cấp mã nguồn để vẽ lại các sơ đồ trong Báo cáo Đồ án bằng công cụ **Mermaid Live Editor**. Đây là cách nhanh nhất để tạo ra các sơ đồ chất lượng cao, đồng bộ và dễ dàng chỉnh sửa.

**Cách thực hiện:**
1. Truy cập: [https://mermaid.live/](https://mermaid.live/)
2. Sao chép (Copy) đoạn code tương ứng bên dưới.
3. Dán (Paste) vào khung "Code" bên trái của trang web.
4. Sơ đồ sẽ hiển thị bên phải. Bạn có thể tải xuống dưới dạng PNG hoặc SVG để chèn vào báo cáo.

---

## Hình 3.1: Sơ đồ Kiến trúc Hệ thống
*(Chưa có trong báo cáo, có thể thêm vào phần 3.2.2 Kiến trúc phần mềm và Công nghệ, mục 2 - Hình đề xuất: fig:sys_arch)*

```mermaid
graph TD
    %% Define Styles
    classDef client fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef server fill:#fff3e0,stroke:#ff6f00,stroke-width:2px;
    classDef db fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef ext fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,stroke-dasharray: 5 5;

    subgraph ClientLayer ["Client Layer"]
        MobileApp["📱 Flutter Mobile App<br>(Android/iOS)"]:::client
        WebApp["💻 React Web App<br>(Admin/Student)"]:::client
        NFCReader["📡 NFC Terminal<br>(Hardware)"]:::client
    end

    subgraph ServerLayer ["Backend Layer (Node.js/Express)"]
        APIGateway["API Gateway<br>(Auth, Rate Limit, CORS)"]:::server
        
        subgraph Services
            AuthSvc[Auth Service]:::server
            WalletSvc[Wallet Service]:::server
            PaymentSvc[Payment Service]:::server
            AISvc[AI Chat Service]:::server
        end
    end

    subgraph DataLayer ["Data Layer"]
        MongoDB[("🍃 MongoDB Atlas<br>(Database)")]:::db
        Redis[("🔴 Redis<br>(Cache/Session)")]:::db
    end

    subgraph External ["External Services"]
        MoMo["💳 MoMo Payment"]:::ext
        Gemini["🤖 Google Gemini AI"]:::ext
    end

    %% Connections
    MobileApp & WebApp & NFCReader -->|HTTPS/REST| APIGateway
    APIGateway --> AuthSvc & WalletSvc & PaymentSvc & AISvc
    
    AuthSvc & WalletSvc & PaymentSvc & AISvc --> MongoDB
    AuthSvc --> Redis

    PaymentSvc -->|API| MoMo
    AISvc -->|API| Gemini

    %% Styling
    style ClientLayer fill:#ffffff,stroke:#333,stroke-width:1px
    style ServerLayer fill:#ffffff,stroke:#333,stroke-width:1px
    style DataLayer fill:#ffffff,stroke:#333,stroke-width:1px
    style External fill:#ffffff,stroke:#333,stroke-width:1px
```

---

## Hình 5.1: Sơ đồ Use Case Tổng quát
*(Tương ứng Figure label: fig:usecase_tongquat trong mục 5.1.1)*

```mermaid
graph LR
    %% Styles
    classDef actorStyle fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef usecaseStyle fill:#fff3e0,stroke:#ff6f00,stroke-width:2px;
    classDef systemStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,stroke-dasharray: 5 5;

    %% Actors
    Student["👤 Sinh viên"]:::actorStyle
    Admin["👤 Quản trị viên"]:::actorStyle
    MoMo["🏢 Hệ thống MoMo"]:::systemStyle
    AI["🤖 AI Gemini"]:::systemStyle

    %% System Boundary
    subgraph System ["Hệ thống Ví Điện Tử Sinh Viên"]
        direction TB
        UC1(["Đăng nhập / Đăng ký"]):::usecaseStyle
        UC2(["Xem số dư & Lịch sử"]):::usecaseStyle
        UC3(["Thanh toán NFC"]):::usecaseStyle
        UC4(["Nạp tiền vào ví"]):::usecaseStyle
        UC5(["Chat tư vấn tài chính"]):::usecaseStyle
        UC6(["Quản lý sinh viên"]):::usecaseStyle
        UC7(["Quản lý giao dịch"]):::usecaseStyle
        UC8(["Xem báo cáo thống kê"]):::usecaseStyle
    end

    %% Connections
    Student --> UC1
    Student --> UC2
    Student --> UC3
    Student --> UC4
    Student --> UC5

    Admin --> UC1
    Admin --> UC6
    Admin --> UC7
    Admin --> UC8

    UC4 -.->|<< include >>| MoMo
    UC5 -.->|<< include >>| AI
```

---

## Hình 5.2: Sơ đồ hoạt động chức năng Thanh toán NFC
*(Tương ứng Figure label: fig:activity_nfc trong mục 5.2.1)*

```mermaid
graph TB
    %% Style
    classDef default fill:#fff,stroke:#333,stroke-width:1px;
    
    %% Dòng 1: Khởi tạo và Xác thực
    subgraph Row1 [Giai đoạn 1: Xác thực]
        direction LR
        Start([Bắt đầu]) --> DetectNFC[Phát hiện NFC]
        DetectNFC --> ReadCard[Đọc thẻ]
        ReadCard --> VerifySig{Xác thực?}
    end

    %% Dòng 2: Xử lý và Kết thúc
    subgraph Row2 [Giai đoạn 2: Xử lý]
        direction LR
        CheckBalance{Số dư?} --> Deduct[Trừ tiền]
        Deduct --> SaveTx[Lưu GD]
        SaveTx --> Notify[Thông báo]
        Notify --> End([Kết thúc])
    end

    %% Kết nối giữa 2 dòng
    VerifySig -->|Hợp lệ| CheckBalance
    
    %% Các luồng lỗi (Rẽ nhánh)
    VerifySig -- Sai --> End
    CheckBalance -- Thiếu --> End
```

---

## Hình 5.3: Sơ đồ hoạt động chức năng Nạp tiền MoMo
*(Tương ứng Figure label: fig:activity_momo trong mục 5.2.1)*

```mermaid
graph TB
    %% Style
    classDef default fill:#fff,stroke:#333,stroke-width:1px;

    %% Dòng 1: Yêu cầu phía App
    subgraph Row1 [Giai đoạn 1: Yêu cầu]
        direction LR
        Start([Bắt đầu]) --> Request[Yêu cầu nạp]
        Request --> API[Gọi API]
        API --> GetLink[Nhận Link]
        GetLink --> OpenApp[Mở MoMo]
    end

    %% Dòng 2: Xử lý phía Server
    subgraph Row2 [Giai đoạn 2: Xử lý & IPN]
        direction LR
        UserConfirm[User Xác nhận] --> Callback[IPN Server]
        Callback --> VerifyIPN{Hợp lệ?}
        VerifyIPN -->|Đúng| UpdateWallet[Cập nhật Ví]
        UpdateWallet --> NotifyUser[Thông báo]
        NotifyUser --> End([Kết thúc])
    end

    %% Kết nối giữa 2 dòng
    OpenApp --> UserConfirm
    
    %% Luồng lỗi
    VerifyIPN -- Sai --> End
```

---

## Hình 5.4: Sơ đồ tuần tự chức năng Chat AI
*(Tương ứng Figure label: fig:sequence_ai trong mục 5.2.2)*

```mermaid
sequenceDiagram
    autonumber
    actor User as Sinh viên
    participant App as Mobile App
    participant Server as Backend API
    participant DB as Database
    participant Gemini as Google Gemini API

    User->>App: Gửi câu hỏi ("Tháng này tôi tiêu gì?")
    App->>Server: POST /api/chat (message)
    
    activate Server
    Server->>DB: Lấy thông tin Ví & Giao dịch (30 ngày)
    DB-->>Server: Trả về dữ liệu JSON
    
    Note over Server: Ẩn danh dữ liệu &<br/>Tạo Prompt ngữ cảnh
    
    Server->>Gemini: Gửi Prompt + Câu hỏi User
    activate Gemini
    Gemini-->>Server: Trả về câu trả lời phân tích
    deactivate Gemini
    
    Server->>DB: Lưu lịch sử chat
    Server-->>App: Trả về câu trả lời
    deactivate Server
    
    App-->>User: Hiển thị câu trả lời của AI
```

---

## Hình 6.1: Sơ đồ quan hệ cơ sở dữ liệu (ER Schema)
*(Tương ứng Figure label: fig:db_schema trong mục 6.3)*

```mermaid
erDiagram
    USER ||--|| WALLET : has
    USER ||--o{ TRANSACTION : performs
    USER ||--o{ CHAT_SESSION : has
    USER ||--o{ CARD : owns
    USER ||--o{ TOKEN : has

    WALLET ||--o{ TRANSACTION : contains
    
    USER {
        ObjectId _id
        String studentId "Unique"
        String email
        String password
        String role "Student/Admin"
        String fullName
    }

    WALLET {
        ObjectId _id
        ObjectId userId
        Number balance
        Number dailyLimit
        Date lastTransaction
    }

    TRANSACTION {
        ObjectId _id
        ObjectId walletId
        String type "Topup/Payment"
        Number amount
        String status
        Date createdAt
    }

    CHAT_SESSION {
        ObjectId _id
        ObjectId userId
        Array messages
        Date updatedAt
    }
    
    CARD {
        ObjectId _id
        ObjectId userId
        String cardNumber
        String signature
        Boolean isActive
    }

    TOKEN {
        ObjectId _id
        ObjectId userId
        String token
        Date expiresAt
    }
```
