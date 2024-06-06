Cách config flavors: https://docs.flutter.dev/deployment/flavors
https://docs.flutter.dev/testing/build-modes
Sau đó tạo trên android studio:
![[Screenshot 2024-06-06 at 10.06.45 1.png]]
Chú ý để chạy được project ios trong flutter cần xoá file podlock. podfile,Runner.xcworkspace để chạy lại để ko bị lỗi

- Giờ mới hiểu vấn đề mấy cái schema chỉ để chỉ định chạy ở chế độ config là dev hay statging, release thôi. Build thì dùng debug, Archive thì dùng release
![[Screenshot 2024-06-06 at 14.17.43.png]]


Tạo bao nhiêu config ở đây thì chỗ Signing & Capabilities sẽ có bấy nhiêu môi trường để mình config provision debug hoặc provision distribution
![[Screenshot 2024-06-06 at 14.28.39.png]]
