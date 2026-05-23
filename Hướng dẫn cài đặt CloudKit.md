# Hướng dẫn cấu hình CloudKit để đồng bộ giữa 2 iPhone

## Bước 1: Mở Xcode và project

1. Mở Xcode
2. File → Open → Chọn thư mục `BabyHealthTracker`
3. Mở file `BabyHealthTracker.xcodeproj`

## Bước 2: Thêm iCloud Capability

1. Trong Xcode, click vào `BabyHealthTracker` ở thanh bên trái
2. Click vào **Signing & Capabilities** ở thanh công cụ chính
3. Click nút **+ Capability**
4. Tìm và chọn **iCloud**
5. Đợi Xcode tự động thêm các quyền cần thiết

## Bước 3: Cấu hình CloudKit Container

1. Sau khi thêm iCloud, bạn sẽ thấy mục **iCloud**
2. Check vào ô **CloudKit**
3. Click nút **+** để tạo container mới
4. Đặt tên container: `iCloud.com.yourname.babyhealthtracker`
   - Thay `yourname` bằng tên của bạn (không có dấu cách)
5. Click **Create**

## Bước 4: Cấu hình CloudKit Dashboard

1. Mở trang https://cloudkit.apple.com/
2. Đăng nhập bằng Apple ID của bạn
3. Chọn container vừa tạo (`iCloud.com.yourname.babyhealthtracker`)
4. Click **Record Types** ở menu bên trái
5. Tạo các Record Types sau (click dấu + để thêm):

### 5.1. Baby
- Fields:
  - `id` (String) - Queryable, Sortable
  - `name` (String) - Queryable, Sortable
  - `dateOfBirth` (Date) - Queryable, Sortable
  - `gender` (String) - Queryable
  - `bloodType` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable
  - `updatedAt` (Date) - Queryable, Sortable

### 5.2. SleepEntry
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `startTime` (Date) - Queryable, Sortable
  - `endTime` (Date) - Queryable
  - `sleepType` (String) - Queryable
  - `notes` (String)
  - `loggedBy` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

### 5.3. DiaperEntry
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `time` (Date) - Queryable, Sortable
  - `type` (String) - Queryable
  - `condition` (String) - Queryable
  - `notes` (String)
  - `loggedBy` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

### 5.4. GrowthEntry
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `date` (Date) - Queryable, Sortable
  - `weight` (Double)
  - `height` (Double)
  - `headCircumference` (Double)
  - `notes` (String)
  - `doctorVisitId` (String) - Queryable
  - `loggedBy` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

### 5.5. VaccinationEntry
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `vaccineName` (String) - Queryable
  - `doseNumber` (Int64) - Queryable, Sortable
  - `scheduledDate` (Date) - Queryable, Sortable
  - `actualDate` (Date) - Queryable
  - `clinicName` (String)
  - `lotNumber` (String)
  - `notes` (String)
  - `status` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

### 5.6. MedicationEntry
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `medicationName` (String) - Queryable
  - `dosageAmount` (Double)
  - `dosageUnit` (String) - Queryable
  - `route` (String) - Queryable
  - `timeGiven` (Date) - Queryable, Sortable
  - `notes` (String)
  - `loggedBy` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

### 5.7. SymptomEntry
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `date` (Date) - Queryable, Sortable
  - `symptomTypes` (String List) - Queryable
  - `severity` (String) - Queryable
  - `temperature` (Double)
  - `temperatureUnit` (String)
  - `deviceType` (String)
  - `notes` (String)
  - `loggedBy` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

### 5.8. DoctorVisit
- Fields:
  - `id` (String) - Queryable, Sortable
  - `babyId` (String) - Queryable, Sortable
  - `date` (Date) - Queryable, Sortable
  - `doctorName` (String)
  - `clinicName` (String)
  - `reason` (String)
  - `diagnosis` (String)
  - `prescriptions` (String)
  - `followUpDate` (Date) - Queryable
  - `notes` (String)
  - `photoURLs` (String List)
  - `linkedGrowthEntryId` (String) - Queryable
  - `linkedVaccinationIds` (String List)
  - `loggedBy` (String) - Queryable
  - `createdAt` (Date) - Queryable, Sortable

## Bước 5: Cấu hình User Entitlements

1. Quay lại Xcode
2. Mở file `BabyHealthTracker.entitlements`
3. Thay thế `$(PRODUCT_BUNDLE_IDENTIFIER)` bằng Bundle ID thực tế của bạn
   - Ví dụ: `com.yourname.babyhealthtracker`

## Bước 6: Kiểm tra

1. Build project trong Xcode (Cmd + B)
2. Nếu có lỗi, kiểm tra lại các bước trên
3. Chạy app trên 2 thiết bị iPhone khác nhau với cùng Apple ID

## Cách sử dụng tính năng đồng bộ

1. Khi đăng nhập vào app trên iPhone thứ 2 với cùng Apple ID
2. Dữ liệu sẽ tự động đồng bộ qua CloudKit
3. Có thể xem trạng thái CloudKit trong Settings app

## Lưu ý quan trọng

- CloudKit miễn phí với giới hạn:
  - 5GB Cloud Storage
  - 100MB Database Transfer/tháng
  - 2000 API Requests/tháng
- Đủ dùng cho app theo dõi sức khỏe bé
- Nếu cần nhiều hơn, có thể nâng cấp lên CloudKit Plus ($0.99/tháng)