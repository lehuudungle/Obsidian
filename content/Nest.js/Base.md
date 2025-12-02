@Res() res: Response: nếu thêm mà ko dùng thì nest sẽ hiểu bạn đang dùng   **library-specific mode**

<span style="color:rgb(255, 0, 0)">whitelist</span> = true, Tự động xóa fields không có trong DTO: https://docs.nestjs.com/techniques/validation => ko lưu các field ko validate vào database.
Ví dụ: có 3 field: id, name, phone. Mà chỉ có 2 field id, name được validate decode thì trường phone sẽ ko update vào database.
còn <span style="color:rgb(255, 0, 0)">forbidNonWhitelisted</span> = true, khi response trả về sẽ thông báo cho người dùng những field đang chưa được validate.



cấu trúc typescript
<span style="color:rgb(0, 176, 240)">constructor(private catsService: CatsService) {}</span>
vừa bơm injection vừa giúp khởi tạo CatsService 

lệnh : <span style="color:rgb(0, 176, 240)">nest g resource auth --no-spec</span> giúp tạo module


<span style="color:rgb(255, 255, 0)">@Public()</span> : đánh dấu router api này ko cần token 

```typescrip 
privider như kiểu bạn muuốn xe gì, useClass tôi cần xe honda
Set global app(#### Enable authentication globally: https://docs.nestjs.com/recipes/passport#implementing-passport-local)

{
provide: APP_GUARD,
useClass: JwtAuthGuard,
},
```

cách trên là cách set global app, còn cách 2 là cách set theo từng router:
```
@Post('profile')
@UseGuards(JwtAuthGuard) // dùng cái này

getProfile(@Request() req) {
 return { profile: req.user };
}
```
