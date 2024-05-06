# Command Design Pattern
![Hình giới thiệu](https://images.viblo.asia/29765288-ec0b-4616-a6bb-c059e7264313.png)
## **Giới thiệu**

*“Encapsulate a resquest as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operation.”*

- Command (hay còn gọi là Action, Transaction) là một mẫu thiết kế trong nhóm Behavior.
- Là một pattern cho phép bạn chuyển đổi một request thành một object độc lập chứa tất cả thông tin về request. Việc chuyển đổi này cho phép bạn tham số hóa (parameterize) các methods với các yêu cầu khác nhau như log, queue (undo/redo), transaction.
- Khái niệm Command Object giống như **một class trung gian** được tạo ra để lưu trữ các lệnh và trạng thái của object tại một thời điểm nào đó.
- Command có nghĩa là ra lệnh. Commander là người chỉ huy, người này **không làm mà chỉ ra lệnh** cho người khác làm. Như vậy, phải có người nhận lệnh và thi hành lệnh. Người ra lệnh **cần cung cấp một class đóng gói những mệnh lệnh**. Người nhận mệnh lệnh cần **phân biệt** những interface nào để thực hiện đúng mệnh lệnh.
- Tần suất sử dụng pattern này: Khá cao.

## Mục đích ra đời

![Untitled](https://images.viblo.asia/fb74f510-f6fb-456b-9973-686b5bd202d3.png)

![Untitled](https://images.viblo.asia/810ae80b-70cf-4f9a-bc29-ac7055db2adf.png)

- Trong thiết kế hướng đối tượng - OOP, đôi khi chúng ta cần gửi các requests cho các objects mà không cần biết bất cứ điều gì về hoạt động được yêu cầu hoặc người nhận yêu cầu. Chẳng hạn chúng có một ứng dụng văn bản, khi click lên button undo/redo, save, … yêu cầu sẽ được chuyển đến hệ thống xử lý, chúng ta sẽ không thể biết được object nào sẽ nhận xử lý, các nó thực hiện như thế nào. Command Pattern là một pattern được thiết kế cho những ứng dụng như vậy, giúp chúng ta:
    - Tránh các hard-wired (kết nối cứng). Việc triển khai hard-wired vào 1 lớp là không linh hoạt.
    - Không nên để lớp đối tượng **phụ thuộc** cụ thể vào một yêu cầu nào đó
    - Cần đưa ra yêu cầu cho đối tượng mà không cần biết bất cứ gì về hoạt động được yêu cầu cũng như cụ thể nơi nhận yêu cầu.
    
    ![Untitled](https://images.viblo.asia/24df6d69-fb26-45a4-b15d-6aea0eb5e5ac.png)
    
    ![Untitled](https://images.viblo.asia/704b0d85-e456-41b6-8d68-91648f49efc4.png)
    

## **Ưu và nhược điểm**

### **Ưu điểm**

- Đảm bảo nguyên tắc Single Responsibility
- Đảm bảo nguyên tắc Open/Closed
- Có thể thực hiện hoàn tác
- Giảm kết nối phụ thuộc giữa Invoker và Receiver
- Cho phép đóng gói yêu cầu thành đối tượng, dễ dàng chuyển dữ liệu giữa các thành phần hệ thống

### **Nhược điểm**

- Khiến code trở nên phức tạp hơn, sinh ra các lớp mới gây phức tạp cho mã nguồn.

## Khi nào thì sử dụng

- Khi cần tham số hóa các đối tượng theo một hành động thực hiện (biến action thành parameter)
- Khi cần tạo và thực thi các yêu cầu vào các thời điểm khác nhau (delay action)
- Khi cần hỗ trợ tính năng undo, log, callback hoặc transaction
- Phối hợp nhiều Command với nhau theo thứ tự

## Cài đặt Command Pattern như thế nào?

![Untitled](https://images.viblo.asia/20aac27a-6d1f-44bd-aa9e-cd9d34ac93ed.png)

- Các thành phần trong pattern:
    - **Command**: là một interface hoặc abstract class, chứa một phương thức trừu tượng thực thi (execute) một hành động (operation). Resquest sẽ được đóng gói dưới dạng Command.
    - ConcreteCommand: là các implementation của Command. Định nghĩa một sự gắn kết giữa một đối tượng Reciever và một hành động. Thực thi execute() bằng việc gọi operation đang hoãn trên Recieve.
    - Client: tiếp nhận request từ phía người dùng, đóng gói request thành ConcreteCommand thích hợp và thiết lập reciever của nó.
    - Invoker: tiếp nhận ConcreteCommand từ Client và gọi execute() của ConcreteCommand để thực thi request.
    - Reciever: đây là thành phần thực sự xử lý business logic cho case resquest. Trong phương thức execute() của ConcreteCommand chúng ta sẽ gọi method thích hợp trong Reciever.
    
    → Như vậy, **Client** và **Invoker** sẽ thực hiện việc **tiếp nhận** request. Còn việc **thực thi** request sẽ do **Command**, **ConcreteCommand** và **Receiver** đảm nhận.
    
    ## Ví dụ: **Command Pattern trong ứng dụng mở tài khoản ngân hàng**
    
    Một hệ thống ngân hàng cung cấp ứng dụng cho khách hàng (client) có thể mở (open) hoặc đóng (close) tài khoản trực tuyến. Hệ thống này được thiết kế theo dạng module, mỗi module sẽ thực hiện một nhiệm vụ riêng của nó, chẳng hạn mở tài khoản (OpenAccount), đóng tài khoản (CloseAccount). Do hệ thống không biết mỗi module sẽ làm gì, nên khi có yêu cầu client (chẳng hạn clickOpenAccount, clickCloseAccount), nó sẽ đóng gói yêu cầu này và gọi module xử lý.
    
    ![Untitled](https://gpcoder.com/wp-content/uploads/2018/12/design-patterns-command-example1.png)
    
    Ứng dụng của chúng ta bao gồm các lớp xử lý sau:
    
    - **Account** : là một request class.
    - **Command** : là một interface của Command Pattern, cung cấp phương thức execute().
    - **OpenAccount**, **CloseAccount** : là các **ConcreteCommand**, cài đặt các phương thức của Command, sẽ thực hiện các xử lý thực tế.
    - **BankApp** : là một class, hoạt động như **Invoker**, gọi **execute()** của **ConcreteCommand** để thực thi request.
    - **Client** : tiếp nhận request từ phía người dùng, đóng gói request thành ConcreteCommand thích hợp và gọi thực thi các Command.
