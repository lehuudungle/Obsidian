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
provide như kiểu bạn muuốn xe gì, useClass tôi cần xe honda
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

Flow dùng passport:
Request
  ↓
1️⃣ <span style="color:rgb(146, 208, 80)">canActivate</span>(context) -<span style="color:rgb(255, 255, 0)"> LUÔN được gọi nhưng có thể ko cần implement dùng mặc định </span>
  ↓
  ├─ Custom logic (check @Public(), roles, etc.)
  │  ├─ return true → STOP (bypass authentication)
  │  └─ return false → STOP (reject request)
  │
  └─ super.<span style="color:rgb(146, 208, 80)">canActivate</span>(context) → Gọi Passport AuthGuard
     ↓
     2️⃣ Extract credentials (token, username/password, etc.)
     ↓
     3️⃣ Verify credentials
     ↓
     4️⃣ Strategy.validate() - Transform payload to user
     ↓
     5️⃣ handleRequest(err, user, info) - ĐƯỢC GỌI Ở ĐÂY (<span style="color:rgb(255, 255, 0)">hàm này có thể gọi hoặc không</span>)
     ↓
     6️⃣ req.user = user
     ↓
Route Handler


<span style="color:rgb(255, 0, 0)">@Injectable()</span> : được hiểu là 1 decoderator khiến nest.js hiểu rằng class này sẽ được quản lý IOC container giúp cho ở 1 class ko cần khởi tạo class đươc khai bao với từ khoá này 
chỉ cần viết: 
<span style="color:rgb(0, 176, 240)">constructor(private catsService: CatsService) {}</span>

- [@Controller()](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) = đây là controller → tự động là provider → có thể inject service vào.
- `@Injectable()` = đây là service/provider → có thể inject vào controller hoặc service khác.

<span style="color:rgb(146, 208, 80)">UsersModule.imports </span>→ MongooseModule.forFeature([...]) 
    ↓
    khai báo model User này được sử dụng trong module UsersModule,
    ví dụ module khác mà muốn sử dụng model User thì <span style="color:rgb(255, 0, 0)">ta export UsersModule để module khác sử dụng chứ ko khai báo lại một lần nữa MongooseModule.forFeature([...])</span>
    ↓
<span style="color:rgb(146, 208, 80)">UsersService.constructor</span> → @InjectModel(User.name)
    ↓
    Lấy model User từ container, tiêm vào UsersService
    ↓
<span style="color:rgb(146, 208, 80)">UsersController.constructor</span> → inject UsersService
    ↓
    Controller dùng Service, Service dùng Model để query DB

<span style="color:rgb(255, 0, 0)"><h2>Pipe</h2></span>
Pipe: muốn validate cho các thuộc tính của class DTO ta sẽ dùng thằng pipe để validate các param từ request gửi lên trước khi mình handle logic gọi hàm service
Tác dụng thứ 2: convert kiểu dữ liệu 

Chú ý muốn sử dụng lib: class-validator thì phải config thêm ở main.ts đoạn code: 
app.useGlobalPipes(new ValidationPipe());
<span style="color:rgb(112, 48, 160)">Cần học kĩ chương này</span>



<span style="color:rgb(255, 0, 0)"><h2>Passport</h2></span>
Nếu một module sử dụng 1 module khác cần phải import Modules đấy vào :
```
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { UsersModule } from 'src/users/users.module';

@Module({
  providers: [AuthService],
  imports: [UsersModule], // import module này vào 
})
export class AuthModule {}

```
tiếp đến thằng userService được sử dụng ở auth service thì cần export thằng UserService ra để thằng khác sử dụng 

![[Screenshot 2025-12-14 at 15.56.24.png]]