****Side Effect**: những hành động xảy ra ngoài quy trình của Observable.
Một số toán tử thường dùng: do(onNext:), .subscribe(onNext:)

<span style="color:#ff0000">Trait</span>
bao gồm các loại: Single, Completable, Maybe
###### tài liệu: https://viblo.asia/p/rxswift-trait-trong-rxswift-single-completable-maybe-L4x5xk2mlBM



Observable khác với Subject ở chỗ:
- Observable lúc nào cũng phải nạp dữ liệu lúc đầu
- không thể thay đổi giá trị và thêm mới sự kiện từ bên ngoài

```swift
let observable: Observable<Int> = Observable.create { observer in observer.onNext(1) 
observer.onNext(2) 
observer.onCompleted() 
return Disposables.create() 
}
```


![[Screenshot 2024-01-08 at 14.49.44.png]]
https://fxstudio.dev/rxswift-combining-operators/#31_combineLatest

<span style="color:#ff0000">combineLatest</span>: sự kết hợp của 2 observable và lấy giá trị mới nhất của 2 observable. Kết quả trả về sẽ trả về 2 giá trị.
```Swift
Driver.combineLatest(input.duration, playTime)

            .map { $0.0 - $0.1 }
```

<span style="color:#ff0000">Merged</span>: gộp nhiều observable thành 1 một observable mới.
<span style="color:#ff0000">Zip</span>: khác với combineLatest chỉ là lấy 2 giá trị mới nhất của 2 observable thì nó sẽ lấy 2 giá trị mới nhất theo thứ tự.

<span style="color:#ff0000">withLatestFrom</span>: kiểu như nút button phát ra sự kiện tap sẽ lấy giá trị mới nhật từ thằng obsevable thứ 2.

****
# **![[Screenshot 2024-01-08 at 14.50.37.png]]**
https://fxstudio.dev/rxswift-transforming-operators/
**<span style="font-weight:bold; color:#ff0000">flatMap</span>**: chuyển đổi từng phần tử phát ra từ một `Observable` thành một `Observable` khác, sau đó "làm phẳng" (flatten) các `Observable` con này thành một `Observable` duy nhất.

```
//MAP
let observable = Observable.of(1, 2, 3) 
let mappedObservable = observable.map { value in return value * 2 } 
let subscription = mappedObservable.subscribe(onNext: { value in print(value) })

//FLATMAP
let observable = Observable.of(1, 2, 3) 
let flatMappedObservable = observable.flatMap { value in return Observable.of(value * 2, value * 3) } 
let subscription = flatMappedObservable.subscribe(onNext: { value in print(value) })

```

<span style="font-weight:bold; color:#92d050">flatMapLastest</span>**: cũng hợp nhất nhiều Obsevable thành Obsevable nhưng chỉ lắng nghe giá trị phát đi của Obsevable cuối cùng.Thực tế áp dụng kiểu Obsevable phát **đi**** sự kiện, trong hàm của Obsevable tầng 1, mình tạo ra 1 cái Obsevable mới (tầng 2). thì phải dùng flatMapLastest để chỉ lắng nghe Obsevable tầng 2.
Ví dụ:****
```swift
input.editProduct
            .withLatestFrom(productList) {
                return ($0, $1)
            }
            .filter { indexPath, items in indexPath.row < items.count }
            .map { indexPath, items in
                return items[indexPath.row]
            }
            .map {
                $0.product
            }

        // phải dùng flatMapLatest ở vì mình đang tạo 1 driver mới ở tầng thứ 2, chứ ko lắng nghe

        // observalbel ở tầng 1

            .flatMapLatest { product -> Driver<EditProductDelegate> in
                                self.navigator.toEditProduct(product)
                            }
            .drive { delegate in
```



![[Screenshot 2024-01-08 at 14.51.27.png]]
https://fxstudio.dev/rxswift-filtering-operators/#31_take

![[Screenshot 2024-01-30 at 10.05.18.png]]
https://fxstudio.dev/combine-time-manipulation-operators-trong-10-phut/
**<span style="color:#ff0000">Debounce</span>**: nghĩa là cứ 1s đầu tiên, nếu obserable ko có biến động gì thì sẽ phát ra giá trị .
Ví dụ như search bar, ta gõ text liên tục, đến khi nào ta dừng gõ text thì 1s sau đó mới call api search
Ví dụ:
```Swift
 let loadNextPage = tableView.rx
            .contentOffset
            .flatMap({ contentOffset in
                return self.isNearTheBottomEdge(contentOffset: contentOffset, self.tableView) ?
                Observable.just(()) : Observable.empty()
            }).debounce(.milliseconds(100), scheduler: MainScheduler.instance)
```
**<span style="color:#ff0000">throttle</span>**: ko quan tâm source dừng lại lúc nào, cứ sau 1 khoảng thời gian thì sẽ phát ra giá trị 