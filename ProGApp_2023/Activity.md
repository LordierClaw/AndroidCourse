# 1. Activity là gì?

**Activity** đại diện cho một chức năng của app, là một giao diện màn hình, nơi user tương tác trực tiếp với app. Một ứng dụng có thể có một hoặc nhiều Activity.

Một Activity từ khi được gọi đến khi kết thúc sẽ có những ***trạng thái (state)*** khác nhau.

# 2. Vòng đời Activity

## 2.1. Giới thiệu

Trong quá trình sử dụng app, user có thể di chuyển sự tương tác giữa các Activity hoặc *thoát app*. Trong Android, Activity class đã cung cấp các hàm **callback** để chúng ta có thể nắm bắt được ***trạng thái*** của Activity mỗi khi có thay đổi.

## 2.2. Sơ đồ trạng thái

Sơ đồ về **Activity Lifecycle**:

![](https://i.imgur.com/GPQOAUX.png)

Một Activity sẽ có **4 trạng thái chính** như sau:

- **Active/Running**: Khi activity ở foreground và đang tương tác trực tiếp với user.
- **Visible/Pause**: Activity không thể tương tác nhưng vẫn được nhìn thấy (bị che khuất không toàn toàn bởi một Activity khác).
- **Hidden/Stop**: Activity bị che khuất hoàn toàn bởi một Activity khác, vẫn có thể lưu trữ thông tin những sẽ bị ưu tiên xóa bỏ nếu hệ thống thiếu bộ nhớ,
- **Destroy**: Khi Activity gọi tới hàm `finish()` hoặc bị hệ thống xóa bỏ. Khi acitivty đó hiển thị lại với user, nó được khởi tạo lại và khôi phục lại trạng thái trước đó.

Trong Activity Lifecycle có **3 vòng lặp** mà chúng ta cần phải nắm rõ:

- **Entire lifetime**: Xảy ra giữa lần đầu tiên hệ thống gọi tới `onCreate()` và `onDestroy()`. Activity sẽ setup tất cả mọi thứ **mang tính cục bộ (global)** ở `onCreate()` và **giải phóng toàn bộ** ở `onDestroy()`. Ví dụ một *thread* download file, *thread* này có thể tạo ở `onCreate()` và kết thúc ở `onDestroy()`.
- **Visible lifetime**: Xảy ra giữa `onStart()` và `onStop()`. Lúc này, Activity hiện hữu trên màn hình và user có thể nhìn thấy nó kể cả khi Activity không nằm trên cùng (foreground) và tương tác với user. Giữa 2 hàm callback này, ta có thể **lưu trữ các tài nguyên cần thiết** cho việc hiển thị activity đến user. Ví dụ như khi bạn có thể tạo *register* ***BroadcastReceiver*** ở `onStart()` để theo đõi những thay đổi liên quan đến giao diện người dùng (UI) và *unregister* nó ở `onStop()` khi user không còn nhìn thấy những gì được hiển thị
- **Foreground lifetime**: Xảy ra giữa `onResume()` và `onPause()`. Trong thời gian này, Activiy visible, active và có thể tương tác với user. Một Activity có thể thường xuyên qua lại giữa 2 trạng thái này(khi thiết bị rơi vào ***trạng thái sleep*** hoặc bị activity khác ***che khuất không hoàn toàn***).

## 2.1. Chi tiết về các trạng thái

Các trạng thái của Activity:

|Trạng thái|Giải thích|
|--|--|
|***`onCreate()`***|Đây là hàm bắt buộc phải được **implement**, được gọi đầu tiên một lần duy nhất khi hệ thống khởi tạo Activity, `callback` này chỉ được gọi lại khi hệ thống xóa Activity đi hoặc khi chuyển trạng thái giữa 2 chế độ màn hình (Landscape và Portrait). <br> Vì được gọi đầu tiên và một lần duy nhất nên phương thức `setContentView()` sẽ được gọi ở hàm này, tùy vào logic của Activity, một số logic cũng sẽ được cho vào hàm này.|
|***`onStart()`***|Hàm này được gọi khi giao diện được hiển thị nhưng chưa tương tác được với user.|
|***`onResume()`***|Khi hệ thống gọi đến callback này, có nghĩa là các View, Component,… của Activity đã **khởi tạo xong**, đây là trạng thái mà app có thể tương tác với user.<br>Activity sẽ ở trạng thái này cho đến khi có một Activity hoặc app khác được gọi chẳng hạn như có cuộc gọi đến, hoặc tắt màn hình điện thoại.|
|***`onPause()`***|Callback này sẽ được gọi tới đầu tiên và ngay lập tức khi user thoát khỏi Activity *(Activity chưa bị destroy)*. Tức là Activity không còn nằm ở foreground nữa nhưng vẫn có thể nhìn thấy đc Activity trên màn hình.<br>`onPause()` thường được sử dụng khi Activity đang **không được focus** bởi user, một số tính năng trong đó sẽ được tạm dừng hoặc duy trì ở một mức độ nào đó để chờ cho user quay lại.<br>Một số lý do mà bạn nên sử dụng `onPause()`:<br>-  Một số sự kiện làm cho Activity **bị gián đoạn**. Đây là trường hợp xảy ra phổ biến nhất.<br>- Từ Android 7.0 (API 24) trở lên, Android có chức năng **đa nhiệm (Multi Window)**. Khi chạy trong chế độ này ở bất kì thời điểm nào cũng chỉ có 1 app được focus bởi user, hệ thống sẽ tạm dừng tất cả các app khác.<br>- Khi bị một Activity khác **đè lên** (ví dụ như một dialog). Activity hiện tại sẽ không tương tác với user, tuy nhiên vẫn được nhìn thấy trên màn hình.|
|***`onStop()`***|Khi Activity không còn được nhìn thấy trên màn hình nữa, Activity sẽ rơi vào trạng thái `onStop()`, lúc này bất kỳ chức năng nào trong Activity cũng có thể bị dừng lại, qua đó app sẽ **giải phóng tài nguyên** không cần thiết khi mà nó không còn hữu dụng với user. Tuy nhiên, trong một số trường hợp `onStop()` cũng được dùng để **lưu trữ dữ liệu**.|
|***`onRestart()`***|Phương thức callback này gọi khi activity đã ***stopped***, gọi trước khi bắt đầu ***start*** lại Activity.|
|***`onDestroy()`***|Callback này được gọi khi user **thoát hoàn toàn** khỏi Activity(nhấn nút **back** hoặc gọi tới hàm `finish()` của Activity).|