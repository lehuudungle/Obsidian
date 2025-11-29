@Res() res: Response: nếu thêm mà ko dùng thì nest sẽ hiểu bạn đang dùng   **library-specific mode**

whitelist = true, Tự động xóa fields không có trong DTO: https://docs.nestjs.com/techniques/validation => vẫn lưu vào database các field hợp lệ

forbidNonWhitelisted: như kiểu cấm request gửi lên và trả về lỗi, nếu có field chưa validate thì sẽ ko lưu vào database

cấu trúc: ```typescript
constructor(private catsService: CatsService) {}
``` vừa bơm injection vừa giúp khởi tạo CatsService