---
title: "Chương 8: Nguyên tắc một trách nhiệm"
type: "book"
book: "agile_software_development"
book_title: "Agile Software Development"
show_top: 0
cat: "it"
---

Tiếp nối loạt bài viết dịch cuốn sách "Agile Software Development, Principles Patterns and Practices", ngày hôm nay, chúng ta đến với nguyên tắc đầu tiên trong thiết kế linh hoạt, đó là `SRP`.

## Nguyên tắc một trách nhiệm - Single Responsibility Principle

Nguyên tắc được đưa ra bởi Tom DeMarco và Meilir Page-Jones. Họ gọi nó là sự liên kết (cohesion). Họ định nghĩa sự liên kết là sự liên quan về mặt chức năng giữa các thành phần trong mô-đun. Trong chương này, chúng ta sẽ thay đổi ý nghĩa này một chút, và coi sự liên kết là các lý do bắt buộc mô-đun, class phải thay đổi.

### SRP: Nguyên tắc một trách nhiệm - (Single Responsibility Principle)

> Một class nên chỉ có một lý do để thay đổi

Chúng ta cùng xem xét trò chơi được nêu ra ở chương 6. Trong hầu hết các bước phát triển, class `Game` có 2 trách nhiệm tách biệt. Nó cần lưu lại trạng thái hiện tại của trò chơi, cũng như tính toán điểm số. Cuối cùng, RCM và RSK tách nó thành 2 class với 2 trách nhiệm khác nhau. Class `Game` đóng vai trò lưu trữ trạng thái của trò chơi, và class `Score` chịu trách nhiệm tính toán điểm số.

Vậy, đâu là lý do chúng ta phải tách hai trách nhiệm đó ra 2 class khác nhau? Bởi mỗi trách nhiệm có một trục quản lý thay đổi khác nhau. Khi yêu cầu thay đổi, hiển nhiên là có thay đổi đến trách nhiệm của một trong số các class. Nếu một class có nhiều hơn một trách nhiệm, sẽ có nhiều hơn một lý do để nó phải thay đổi.

Nếu class có nhiều hơn một trách nhiệm, khi đó các trách nhiệm sẽ bị ghép lại với nhau. Thay đổi đến một trách nhiệm có thể làm suy yếu hoặc làm hỏng khả năng của class để đáp ứng trách nhiệm còn lại. Kiểu ràng buộc như thế này dẫn đến những thiết kế dễ dàng bị phá vỡ theo một cách thức không ngờ tới.

Ví dụ, xem xét thiết kế ở hình 8-1. Class `Rectangle` (hình chữ nhật) có hai phương thức. Một dùng để vẽ hình chữ nhật ra màn hình, và hai là tính diện tích hình chữ nhật.

![fig_8_1.png](/assets/img/post/agile/fig_8_1.png)

2 ứng dụng khác nhau sử dụng class `Rectangle`. Một ứng dụng thực hiện các tính toán hình học, nó sử dụng class `Rectangle` với các tính toán số học và ình học. Nó không bao giờ vẽ hình chữ nhật ra ngoài màn hình. Ứng dụng còn lại là một ứng dụng đồ hoạ đơn thuần. Nó có thể thực hiện các tính toán hình học, nhưng chắc chắn là luôn vẽ hình chữ nhật lên màn hình.

Thiết kế này vi phạm nguyên tắc một trách nhiệm (SRP). Class `Rectangle` có hai trách nhiệm. Một là cung cấp mô hình tính toán cho hình chữ nhật. Hai là đưa hình chữ nhật ra cho giao diện người dùng.

Sự vi phạm này gây ra rất nhiều vấn đề liên quan. Đầu tiên, chúng ta cần thêm thư viện `GUI` trong ứng dụng tính toán hình học. Nếu đây là một ứng dụng C++, `GUI` cần được liên kết vào, tính toán thời gian liên kết, dịch chương trình cũng như tiêu tốn bộ nhớ trong quá trình tính toán. Nếu là ứng dụng Java, tập tin `.class` cho GUI cần được đẩy lên nền tảng chạy ứng dụng.

Thứ hai, nếu một thay đổi trong ứng dụng `GraphicalApplication` khiến class `Rectangle` thay đổi vì lý do nào đó, thay đổi đó có thể buộc chúng ta tái cấu trúc, dịch, kiểm tra lại ứng dụng `ComputationalGeometryApplication`. Nếu chúng ta quên làm việc đó, ứng dụng có thể bị hỏng theo một cách không dự đoán trước được.

Một thiết kế tốt hơn có được khi chúng ta tách hai trách nhiệm ra hai class khác nhau như hình 8-2. Thiết kế này đẩy phần tính toán ra class `GeometricRectangle`. Lúc này, thay đổi về việc hiển thị hình chữ nhật không hề ảnh hưởng đến ứng dụng `ComputationalGeometryApplication`.

![fig_8-2.jpg](/assets/img/post/agile/fig_8_2.jpg)

### Trách nhiệm là gì?

Khi nói đến SRP, chúng ta định nghĩa trách nhiệm là `lý do để thay đổi`. Nếu bạn có thể nghĩ đến nhiều hơn một động cơ để thay đổi class, nghĩa là class đó có nhiều hơn một trách nhiệm. Điều này đôi khi rất khó để nhận ra. Chúng ta thường nghĩ về trách nhiệm trong một nhóm. Ví dụ, xem xét lớp `Modem` dưới đây. Hầu hết chúng ta đều đồng ý rằng lớp này nhìn có vẻ rất ổn. Cả bốn hàm đều được định nghĩa rõ ràng với các chứng năng liên quan đến một mô-đem.

{% highlight java %}
// Modem.java -- SRP Violation
interface Modem
{
  public void dial(String pno);
  public void hangup();
  public void send(char c);
  public void recv();
}
{% endhighlight %}

Tuy nhiên, có hai trách nhiệm được đưa ra ở đây. Một là quản lý kết nối. Hai là truyền đạt thông tin. Hàm `dial` và `hangup` quản lý sự kết nối của mô-đem, trong khi `send` và `recv` chịu trách nhiệm truyền đạt thông tin.

Liệu hai trách nhiệm này có nên được tách biệt? Điều này phụ thuộc vào cách mác chương trình có thể thay đổi. Nếu ứng dụng thay đổi ảnh hưởng đến hàm thực hiện kết nối, khi đó thiết kế này sẽ có mùi `Cứng nhắc` (Rigidity), bởi class gọi đến `send` và `recv` sẽ phải biên dịch lại nhiều lần hơn những gì chúng ta mong đợi. Khi đó, hai trách nhiệm cần được tách biệt như hình 8-3. Nó giúp cho ứng dụng của chúng ta tránh khỏi việc ràng buộc hai trách nhiệm vào nhau.

![fig_8-3.jpg](/assets/img/post/agile/fig_8_3.jpg)

Tuy nhiên, theo một cách khác, nếu ứng dụng thay đổi mà không khiến các trách nhiệm phải thay đổi thường xuyên, chúng ta sẽ không cần phải tách class ra. Khi đó, việc tách biệt chúng sẽ dẫn đến sự phức tạp không cần thiết (Needless Complexity).

Cần có một sự bình tĩnh ở đây. *Một trục của sự thay đổi chỉ thực sự là một trục của sự thay đổi nếu thay đổi thực sự diễn ra.* Sẽ không khôn khéo nếu áp dụng SRP hoặc bất cứ nguyên tắc nào nếu không có bất kỳ triệu chứng nào cần được sửa chữa.

### Tách biệt các chức năng bị liên kết

Chú ý rằng ở hình 8-3, chúng ta đã liên kết 2 chức năng trong một class `ModemImplementation`. Đó không phải là những gì chúng ta thực sự muốn, nhưng nó là cần thiết. Đôi khi có những lý do, như là việc tương thích với phần cứng hay hệ điều hành, buộc chúng ta liên kết các thành phần lại chứ không phải tách biệt chúng ra. Tuy nhiên bằng cách tách biệt các lớp ra, chúng ta đã tách biệt các khái niệm khác nhau trong ứng dụng của mình.

Chúng ta có coi `ModemImplementation` là một thiết kế không hoàn chỉnh, tuy nhiên, nên nhớ rằng tất cả các thành phần phụ thuộc đã được tách biệt khỏi nó. Không ai cần phụ thuộc vào class đó nữa. Không ai ngoại trừ chương trình chính (main) biết là nó tồn tại. Do vậy, chúng ta đã để sự xấu xí đằng sau hàng rào bảo vệ, sự xấu xí đó không lọt ra ngoài và không làm xấu đi các thành phần khác.

### Sự kiên định

Hình 8-4 cho thấy ví dụ phổ biến của việc vi phạm SRP. Class `Employee` bao gồm cả logic về nghiệp vụ và quản lý việc lưu trữ. Hai trách nhiệm này gần như không bao giờ nên được ghép vào một class. Logic về nghiệp vụ có xu hướng thay đổi liên tục, trong khi công việc lưu trữ thì ít khi thay đổi, và thường thay đổi ví một lý do hoàn toàn khác. Đẩy logic nghiệp vụ cùng class với công việc lưu trữ có thể gây ra những rắc rối không ngờ tới.

![fig_8-4.jpg](/assets/img/post/agile/fig_8_4.jpg)

Thật may mắn, như đã đề cập ở chương 4, bằng việc áp dụng phương pháp phát triển địn hướng bởi kiểm thử (test-driven development), chúng ta thường buộc hai trách nhiệm này tách biệt trong hai class khác nhau. Tuy nhiên, trong trường hợp bài kiểm thử không buộc việc tách biệt được thực hiện, chương trình sẽ có mùi cứng nhắc (Rigidity) và dễ vỡ (Fragility), chương trình cần được tái cấu trúc sử dụng mẫu thiết kế ` FACADE` và `PROXY`.

### Kết luận

SRP là một trong những nguyên tắc đơn giản nhất nhưng khó làm đúng nhất. Kết hợp các trách nhiệm lại là việc chúng ta làm một cách bình thường. Tìm kiếm và tách biệt những trách nhiệm khỏi những trách nhiệm khác là việc chúng ta làm trong thiết kế phần mềm, và đó thực sự là những gì nhà thiết kế phải làm. Những nguyên tắc được nhắc đến tiếp theo sẽ thảo luận lại vấn đề này theo một cách khác.

> Bạn đọc có thể tham khảo slide sau của tôi để có được cái nhìn tổng quát về 5 nguyên tắc thiết kế linh hoạt:

> [http://www.slideshare.net/dinhhoanglong91/object-oriented-design-principle](http://www.slideshare.net/dinhhoanglong91/object-oriented-design-principle)


