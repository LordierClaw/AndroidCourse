
# 1. Component trong Android

## 1.1. Khái niệm

Các Component là các khối kiến trúc nền tảng tạo nên một ứng dụng Android. Những Component này được ghép bởi `AndroidManifest.xml`, miêu tả mỗi thành phần và cách chúng tương tác với nhau.

## 1.2. Component chính

Các Component là các khối kiến trúc nền tảng tạo nên một ứng dụng Android. Những Component này được ghép bởi `AndroidManifest.xml`, miêu tả mỗi thành phần và cách chúng tương tác với nhau.

|Component|Mô tả|
|--|--|
|Activities|Chúng điều khiển UI và xử lý tương tác người dùng trên màn hình smartphone|
|Services|Chúng xử lý các tiến trình background được gắn kết với một ứng dụng|
|Broadcast Receivers|Chúng xử lý giao tiếp giữa Android OS và các ứng dụng|
|Content Providers|Chúng xử lý dữ liệu và quản lý cơ sở dữ liệu|

## 1.3. Các Component khác

Ngoài ra, còn có một số Component khác sẽ được sử dụng trong xây dựng các thực thể, tính logic của chúng và kết nối chúng với nhau. Chúng bao gồm:

|Component|Mô tả|
|--|--|
|Fragments|Biểu diễn một phần của giao diện UI trong một Activity|
|Views|Các phần tử giao diện UI mà được vẽ trên màn hình bao gồm button, list, form, …|
|Layouts|Cấu trúc thứ bậc của view mà điều khiển định dạng màn hình và bề mặt của các view|
|Intents|Các thành phần kết nối các thông báo với nhau|
|Resources|Các phần tử ngoại vi, như các chuỗi, hằng, và hình ảnh có thể vẽ|
|Manifest|Chính là Configuration file cho ứng dụng|

# 2. Resource trong Android

## 2.1. Khái niệm

Ngoài việc viết code cho ứng dụng, ta cũng cần quan tâm đến các **Resource** khác, chẳng hạn nội dụng tĩnh mà code của ta sử dụng như bitmap, color, layout, UI,…

Những resource này là luôn luôn được duy trì riêng rẽ trong các thư mục con dưới thư mục `res/` của project.

## 2.2. Tổ chức Resource
|Thư mục|Kiểu Resource|
|--|--|
|`anim/`|Các XML file định nghĩa thuộc tính hiệu ứng. Chúng được lưu trong thư mục `res/anim` và được truy cập từ lớp `R.anim`|
|`drawable/`|Các Image file như *.png*, *.jpg*, *.gif* hoặc XML file mà được biên dịch vào trong *bitmap*, *state list*, *shape*, *animation drawable*. Chúng được lưu trong `res/drawable/` và được truy cập từ lớp `R.drawable`|
|`layout/`|Các XML file định nghĩa layout cho UI. Chúng được lưu trong `res/layout/` và được truy cập từ lớp `R.layout`|
|`menu/`|XML file định nghĩa một UI layout. Chúng được lưu trong `res/layout/` và được truy cập từ lớp `R.menu`|
|`raw/`|Các file riêng để lưu giữ trong dạng **raw from**. Bạn cần gọi `Resources.openRawResource()` với ***resource ID***, là `R.raw.filename` để mở các **raw file** này|
|`values/`|Chứa các **giá trị đơn giản** *(chuỗi, số nguyên và màu)*<br>Ví dụ, dưới đây là một số tên file qui ước cho các Resource bạn có thể tạo trong thư mục này: <br> `arrays.xml` cho các mảng và được truy cập từ lớp `R.array` <br>`integers.xml` cho số nguyên và được truy cập từ lớp `R.integer` <br> `bools.xml` cho boolean, và được truy cập từ lớp `R.bool`. <br> `colors.xml` cho các giá trị màu, và được truy cập từ lớp `R.color` <br> `dimens.xml` cho các giá trị chiều, và được truy cập từ lớp `R.dimen` <br> `strings.xml` cho các giá trị chuỗi, và được truy cập từ lớp `R.string` <br> `styles.xml` cho các style, và được truy cập từ lớp `R.style`|
|`xml/`|Các XML file riêng có thể được đọc tại **runtime** bởi gọi phương thức `Resources.getXML()`. Bạn có thể lưu giữ các file cấu hình đa dạng tại đây, các file này sẽ được sử dụng tại **runtime**|

## 2.3. Resource thay thế

### 2.3.1. Vì sao sử dụng Resource thay thế?

Ứng dụng của bạn nên cung cấp các Resource thay thế để hỗ trợ cho các cấu hình thiết bị cụ thể. Ví dụ, bạn nên cung cấp các Drawable Resource thay thế (ví dụ image) cho các màn hình có độ phân giải khác nhau và String Resource thay thế cho các ngôn ngữ khác nhau. Tại runtime, Android dò cấu hình thiết bị hiện tại và tải Resource thích hợp cho ứng dụng của bạn.

### 2.3.2. Xác định Resource thay thế

Để xác định cấu hình thay thế cho một tập các Resource, bạn theo các bước: −

Tạo một thư mục mới trong `res/` với tên dạng `<resources_name>-<config_qualifier>`

Trong đó:
- `resources_name` sẽ là bất kỳ Resource đã đề cập trong bảng trên, như *layout*, *drawable*,…
- `config_qualifier` sẽ xác định cấu hình riêng cho các Resource được sử dụng. 

Bạn có thể kiểm tra ***Offical Documentation*** để có danh sách đầy đủ các **qualifier** cho các kiểu Resource khác nhau.

**Ví dụ:** xác định layout cho một ngôn ngữ mặc định và layout thay thế cho ngôn ngữ *Arabic*.
```
MyProject/
   src/
	main/
	java/
	   MyActivity.java  
      res/
         drawable/  
            icon.png
            background.png
        layout/  
            activity_main.xml
            info.xml
        layout-ar/
            main.xml
        values/  
            strings.xml 
```