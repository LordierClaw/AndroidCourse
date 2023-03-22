# 1. Fragment là gì?

Trong các ứng dụng android, tại một thời điểm, chỉ có một Activity được hiển thị duy nhất trên màn hình. Chúng ta muốn chia màn hình ra nhiều phần để dễ sử dụng thì **fragment** đáp ứng điều đó:

- **Fragment** là một thành phần android **độc lập**, được sử dụng bởi một activity, giống như một *sub-activity*.

- **Fragment** có vòng đời và UI riêng. Các Fragment thường có một file java đi kèm với file giao diện xml. Các fragment không có file giao diện xml thường được gọi là ***headless fragments***.

- **Fragment** sử dụng method `getActivity()` để lấy ra Activity cha

- **Fragment** được định nghĩa trong file xml của activity **(static definition)** hoặc có thể sửa đổi fragment khi đang chạy **(dynamic definition)**

Dưới đây là một ví dụ cụ thể về sử dụng Fragment|Thiết bị máy tính bảng với màn hình lớn thì **một Activity** có thể chứa **2 Fragment**, còn thiết bị cầm tay với màn hình nhỏ thì có thể có **2 Activities** với mỗi Activity là một Fragment.

![](https://vietjack.com/android/images/android_fragments.jpg)

# 2. Vòng đời của Fragment

Fragment trong Android có vòng đời riêng của nó, tương tự như một Activity trong Android.

![](https://images.viblo.asia/fde79322-a664-4504-8157-1d3d5cd19bd0.png)


|Trạng thái|Mô tả|
|--|--|
|***`onAttach()`***|Hàm này thực hiện tạo tham chiếu từ một fragment đến activity đã khởi tạo nó, và thực hiện một số bước trong quá trình khởi tạo|
|***`onCreate()`***|Thực hiện khởi tạo fragment|
|***`onCreateView()`***|Thực hiện tạo giao diện(view), trả về view là giao diện file xml tương ứng fragment. ko nên tương tác với activity trong hàm này bởi vì activity chưa được khởi tạo đầy đủ. Không cần thực hiện hàm này với các fragment không có header|
|***`onActivityCreated()`***|Thực hiện hoàn thành nốt việc khởi tạo activity và fragment. Trong bước này chúng ta có thể gọi `findViewById()`|
|***`onStart()`***|Thực hiện việc hiển thị fragment lên màn hình|
|***`onResume()`***|Fragment chính thức hoạt động hoàn toàn|
|***`onPause()`***|Fragment bị tạm dừng hoạt động, nó vẫn có thể được nhìn thấy|
|***`onStop()`***|Fragment bị ẩn|
|***`onDestroyView()`***|Giao diện(view) của fragment bị hủy. Nếu nó được gọi quay lại, nó sẽ quay trở lại thực hiện hàm `onCreateView()`|
|***`onDestroy()`***|Bị hủy|
|***`onDetach()`***|Bị hủy hoàn toàn|

# 3. So sánh giữa Activity và Fragment

|Tiêu chí|Activity|Fragment|
|--|--|--|
|Vị trí đối với ứng dụng|Activity là hoạt động,cửa số chính,tồn tại độc lập|Fragment là một phần của Activity, đóng góp UI và hoạt động của nó vào thành phần chính|
|Vị trí tương đối với nhau|Activity có thể chứa nhiều Fragment|Fragment là một phần của Activity|
|Tái hoạt động|Không thể tái hoạt động|Một fragment có thể được tái sử dụng trong một activity,do đó nó hoạt động như một thành phần tái sử dụng trong các hoạt động.|
|Tư tưởng hình thành|Các hoạt động là một trong những khối xây dựng cơ bản của ứng dụng trên nền tảng Android. Chúng đóng vai trò là điểm vào cho sự tương tác của người dùng với một ứng dụng và cũng là trung tâm cho cách người dùng điều hướng trong ứng dụng hoặc giữa các ứng dụng|Fragment đại diện cho một hành vi hoặc một phần của giao diện người dùng trong một hoạt động. Bạn có thể kết hợp nhiều fragment trong một hoạt động để xây dựng giao diện đa cửa sổ và sử dụng lại một đoạn trong nhiều hoạt động. Bạn có thể nghĩ ra một đoạn như một phần mô đun của một hoạt động, có vòng đời riêng của nó, nhận các sự kiện đầu vào của chính nó, và bạn có thể thêm hoặc xoá trong khi hoạt động đang chạy.|
|Tổ chức vòng đời|Các phương pháp vòng đời được tổ chức bởi hệ điều hành (OS).|Phương pháp vòng đời được tổ chức bởi tổ chức bởi hoạt động lưu trữ (hosting activity)|