---
title: "Sử dụng CDN để giảm tải cho Server"
---

## Câu chuyện bắt đầu

Gần đây, tôi có tham dự một lớp học Rails của các sinh viên năm thứ 4 ngành công nghệ thông tin. Trong buổi học đó, sinh viên tìm hiểu về thư viện `Bootstrap` của Twitter. Có 2 cách để import thư viện vào trong trang web, 1 là sử dụng 1 đường link ở "tận đẩu tận đâu" như thế này.
{% highlight html %}
<link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css"/>
{% endhighlight %}
2 là lên trang chủ của `Bootstrap`, download về server của mình rồi import file từ chính server của mình
{% highlight html %}
<link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css"/>
{% endhighlight %}
Khi tôi hỏi so sánh hai cách làm, cách nào hay hơn, thì hầu hết các sinh viên đều cho rằng cách thứ 2 hay hơn, lý do là vì nó: "Nhanh hơn?", "Cùng server mình thì chỉ gửi request 1 lần là xong" @@

Lý do thứ 2 chắc chắn sai, vì request đầu tiên sẽ nhận về file HTML, trình duyệt sẽ xử lý file đó và tìm các link để tiếp tục gửi request, trong trường hợp này thì tối thiểu là 2 request, trong đó request thứ hai để lấy về file CSS.
Vậy còn lý dó thứ nhất "Nhanh hơn?". Liệu có đúng? Có phải vì cùng server trả về file HTML nên mình cho rằng nó sẽ "nhanh"?

Câu trả lời trong hầu hết các trường hợp là "không"! (Chỉ hầu hết thôi nhé... :D )

Lý do tại sao, chúng ta sẽ cùng tìm hiểu về cái link "tận đẩu tận đâu" kia!
[https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css](https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css)

Đó là `CDN`!

## `CDN` là gì?

`CDN` viết tắt của `Content Delivery Network`, tạm dịch là "Mạng phân phối thông tin". CDN là một hệ thống các server phân tán trên khắp thế giới, đưa nội dung đến với người dùng một cách nhanh nhất có thể dựa theo vị trí địa lý của người dùng và các servers lưu trữ thông tin.

`CDN` đóng vai trò quan trọng trong việc tăng tốc độ truyền tải dữ liệu đến với người dùng, đặc biệt với các website có ý định phát triển vươn ra toàn cầu. Ví dụ đơn giản là khi một người dùng tại Hà Nội muốn nhận một tập tin, nếu tập tin đó được chuyển đến từ Mỹ thì sẽ phải mất nửa vòng trái đất để đến đích. Rõ ràng con đường đó quá dài và sẽ tốn thời gian hơn rất nhiều nếu nó được gửi từ Singapore hay Nhật Bản.

![network-map.png](/assets/img/post/network-map.png)

*___Ảnh: [cloudflare.com](www.cloudflare.com)*

Hình trên là mô hình mạng của trang web [cloudflare.com](www.cloudflare.com), một website cung cấp dịch vụ CDN. Có thể thấy có rất nhiều server phân tán trên khắp thế giới, và sẽ thật may mắn nếu bạn sống ở Hà Nội và nhận được tập tin từ một server tại Singapore, thay vì từ Chicago @@


## CDN hoạt động như thế nào?

### Nguyên tắc hoạt động

Nguyên tắc cơ bản của CDN là rải rác dữ liệu ở rất nhiều server trên thế giới (các file tĩnh như hình ảnh, video, Javascript, CSS,...). Server có khoảng cách địa lý đến người dùng gần nhất sẽ được dùng để gửi dữ liệu đến cho người dùng.

![how-cdn-works.png](/assets/img/post/how-cdn-works.png)
*Ảnh: [cdnreviews.com](http://www.cdnreviews.com/)*

Hình trên mô tả một mô hình đơn giản của CDN. Có một server chính (Original server) đóng vai trò trung gian nhận request, dữ liệu và phân phối request, dữ liệu đến các server cạnh (edge server). Người dùng ở gần server cạnh nào nhất sẽ nhận được dữ liệu trả về từ server đó. Từ "server" ở hình trên chỉ mang tính mô phỏng, bởi các điểm được chỉ định sẽ có một data center với hàng tá máy chủ hoạt động ngày đêm, với vai trò đảm bảo cho CDN luôn luôn hoạt động.

CDN ra đời cách đây khoảng 15 năm nhưng chỉ gần đây mới thực sự phát triển khi mà lượng người dùng và tài nguyên internet tăng không ngừng cũng như thiết kế đã có những bước đột phá nhất định. Một vài nhà cung cấp dịch vụ CDN quen thuộc với dân lập trình là `BootstrapCDN` của Twitter, `Amazon CloudFront` của Amazon, `Windows Azure CDN` của Microsoft,...

Câu hỏi được đặt ra là làm cách nào mà CDN có thể tìm ra được máy chủ gần người dùng nhất để cho phép tập tin gửi từ đó mà không phải từ máy chủ khác. Rõ ràng chúng ta chỉ có một đường link duy nhất, gửi một request từ browser, trong khi có vẻ có rất nhiều server ?

Chúng ta cùng tìm hiểu cách định tuyến (routing) trong CDN.

### Request Routing

Theo phân tích của website [WindowsITPro](http://windowsitpro.com/networking/speedy-web-content-delivery-cdns), CDN có ba cách định tuyến, đó là:  `DNS Request Routing` (Định tuyến sử dụng DNS), `Transport Layer Request Routing` (Định tuyến ở tầng vận tải), và cuối cùng là `Application-layer Request Routing` (Định tuyến ở tầng ứng dụng).　Cả 3 cách thức đều khá phức tạp và đòi hỏi kiến thức cao về network protocol. Thêm vào nữa đây chỉ là phân tích của các nhà nghiên cứu, không phải của bản thân nhà cung cấp dịch vụ CDN (theo tôi biết thì ông nào cũng quảng cáo thuật toán của mình bá nhất @@), do vậy tôi sẽ chỉ "thử" giải thích một kỹ thuật mà tôi thấy dễ hiểu nhất, đó là `Transport Layer Request Routing`.

![cdn-routing.gif](/assets/img/post/cdn-routing.gif)
*Ảnh: [WindowsITPro](http://windowsitpro.com/networking/speedy-web-content-delivery-cdns)*

Hình trên là mô tả chung cho `Transport Layer Request Routing`. Để khớp với mô hình CDN ở trên, chúng ta tạm coi POP1 là Original Server, và POP2 là một Edge Server. Tại mỗi server đều có sẵn hệ thống định tuyến ở tầng vận tải (Transport Layer Routing System - TLRS). Con đường request của người dùng sẽ như sau.

1. Sau khi nhận kết quả từ DNS Server, người dùng gửi request đến địa chỉ IP của POP1 (Original Server). Đây là một địa chỉ IP ảo (Virtual IP - VIP)

2. TLRS ở tầng POP1 sẽ kiểm tra thông tin của người dùng, kiểm tra trong số các Edge Server của nó xem đâu là server thích hợp nhất để trả gói tin. Sau khi xác định được Edge Server thích hợp, POP1 sẽ chuyển tiếp (forward) request  của người dùng đến Edge Server đó (POP2).

3. Sau khi kiểm tra thông tin được gửi từ POP1, POP2 sẽ đổi địa chỉ IP nguồn của nó theo IP ảo của POP1, rồi gửi tập tin lại cho người dùng. Khi nhận được tập tin, người dùng (chính xác hơn là trình duyệt của người dùng) vẫn tin là nó nhận được từ POP1 mà tiếp tục gửi các request tiếp theo (nếu có).

Trên đây là một giải thích đơn giản cho cơ chế định tuyến của CDN. Bạn đọc có hứng thú có thể tự tìm hiểu thêm :D


## Vai trò của CDN trong việc giảm tải cho server

Trên đây bạn đọc đã có được khái niệm cơ bản về CDN. Chắc các bạn cũng đã hình dung được tương đối lợi ích mà nó mang lại, sau đây tôi xin được tổng hợp các lợi ích của CDN trong việc giảm tải cho server cũng như cải thiện trải nghiệm người dùng, đem đến một website có tốc độ load nhanh hơn.

* **Tăng tốc độ, tiết kiệm băng thông**

  Như đề cập ở đầu bài viết, với mỗi một request đến các link có trong tập tin HTML đầu tiên, bao gồm các link CSS, Javascript, video, hình ảnh,... trình duyệt sẽ gửi một request lên địa chỉ đó. Trong trường hợp chúng ta sử dụng CDN, thay vì request lên server của chúng ta, trình duyệt sẽ gửi request lên một server khác. Qua đó giảm được lưu lượn truy cập đến server hiện tại. Và như có đề cập ở trên, server này sẽ được tối ưu để "gần" nhất với máy tính của chúng ta, qua đó tốc độ được đảm bảo luôn ở mức tốt nhất.

* **Tập tin có thể được được lưu tại cache của trình duyệt**

  Ngày này, rất nhiều trang web sử dụng CDN. Ví dụ, jQuery hay Bootstrap. Nếu người dùng truy cập đến trang web có sử dụng thư viện jQuery của bạn, mà máy tính họ đã truy cập vào một trang web cũng sử dụng thư viện đó với nhà cung cấp CDN như bạn, trình duyệt của họ sẽ không cần gửi request đi đâu cả mà sẽ load lại từ cache ra. Qua đó, tốc độ được cải thiện một cách không ngờ :D

* **Tăng số lượng tập tin được load tại một thời điểm**

  Có một điều thú vị về các trình duyệt, đó là các trình duyệt sẽ giới hạn số lượng request đến một domain tại một thời điểm nhất định, con số này thường là 4. Vậy nếu bạn có request số 5 đến cùng một domain, trình duyệt sẽ đợi cho ít nhất 1 trong 4 request đang chạy được hoàn thành rồi mới đến request số 5. Do vậy, bằng việc phân tán request sang một server khác (CDN), bạn không những giảm số lượng request lên server mình, mà còn cho trình duyệt có cơ hội thực hiện request khác cùng thời điểm.

* **Các phiên bản đều được quản lý**

  Quản lý phiên bản (version control) là chức năng có sẵn của các dịch vụ CDN. Với vai trò nhà phát triển web, bạn dễ dàng xác định tập tin nào với phiên bản nào cần gửi cho người dùng mà không lo mất mát hay bị lỗi.

* **Cơ sở hạ tầng cực kỳ tốt**

  Tin hay không tùy bạn, dù bạn có thuê được một dịch vụ lưu trữ tốt đến mấy, tôi vẫn không chắc nó tốt hơn những gì mà người khổng lồ Google, Amazon, Microsoft mang lại. Do vậy, bạn có thể hoàn toàn tin tưởng rằng tập tin sẽ luôn được truyền đến đích khi sử dụng dịch vụ do các CDN hàng đầu cung cấp. Tham khảo các CDN phổ biến ở đây: [http://www.cdnreviews.com/popular-cdns/](http://www.cdnreviews.com/popular-cdns/)

* **Phân tích người dùng (User Analytics)**
  Một điều tuyệt vời nữa là các nhà cung cấp CDN còn bổ sung thêm chức năng phân tích lưu lượng người dùng. Thuê một dịch vụ CDN, đồng nghĩa với việc bạn có một dịch vụ phân tích người dùng, để biết ai, từ đâu, thời gian nào truy cập vào dịch vụ của bạn. Qua đó có những biện pháp thích hợp để cải thiện chức năng hệ thống

## Một vài lưu ý về sử dụng CDN

Quay trở lại với câu hỏi câu chuyện ở đầu bài viết, khi tôi kết luận rằng, nếu đặt file CSS trên server của bạn thì THƯỜNG sẽ load chậm hơn là dùng file từ CDN. Tại sao lại là "thường" khi mà ở trên chúng ta đã liệt kê hàng loạt ích lợi của CDN? OK, tôi xin phép đưa ra một số lưu ý theo kinh nghiệm và hiểu biết cá nhân.

* Không phải lúc nào CDN cũng nhanh hơn, nếu các tập tin đó chỉ được truy cập thông qua trang web của bạn (khả năng được cache sẽ thấp), hay khi bạn có một server đủ nhanh để tốc độ có thể nhanh hơn server của CDN. (Ví dụ nếu người dùng truy cập từ Việt Nam và server bạn đặt ở Việt Nam thì có thể sẽ nhanh hơn CDN, bởi theo tôi thấy thì CDN đặt server gần nhất là tại Singapore, không phải Việt Nam)

* Không phải lúc nào CDN cũng là lựa chọn tốt, nếu bạn chỉ muốn hướng đối tượng người dùng đến một quốc gia nhất định. Hay nói cách khác là khi bạn không có ý định toàn cầu hóa trang web của mình, thì việc sử dụng CDN là thừa thãi và không cần thiết. Thử tưởng tượng bạn có một trang web cho người Việt Nam, mà lại đặt một video quảng cáo ở server tận Los Angeles thì...chả biết để làm gì @@ (nếu dịch vụ của bạn cũng hướng đến Việt kiều thì lại là hợp lý :D )

* Không phải công ty nào cũng đủ khả năng tự xây dựng một hệ thống CDN cho riêng mình, bởi nó quá phức tạp, đòi hỏi sự tham dự của nhiều bên liên quan ở nhiều quốc gia. Do vậy, việc sử dụng các dịch vụ cung cấp CDN có sẵn là lựa chọn thích hợp. Tuy nhiên, bạn nên có lựa chọn đúng đắn, ngoài giá cả thì các vấn đề về bảo mật cũng rất quan trọng. Nguy hiểm có thể rình rập khi tập tin Javascript của bạn bị chỉnh sửa, cài mã độc khi về đến phía người dùng, gây nguy hiểm cho cả người dùng và website của bạn. Do vậy, tôi khuyến cáo đã không dùng thì thôi, đã dùng thì nên chọn một nhà cung cấp đáng tin cậy.

* Điều cuối cùng, khi bạn deploy server với các tập tin tĩnh (Javascript, CSS, Video, Ảnh,...) cần đẩy lên server của CDN, điều đó có nghĩa là công việc deploy của bạn thêm một mức phức tạp và một mức rủi ro, do vậy, cần đảm bảo cơ chế deploy của mình là tốt nhất có thể để tránh gây ra các lỗi không đáng có.

## Tài liệu tham khảo

* CDN Review [http://www.cdnreviews.com/](http://www.cdnreviews.com/)

* CDN - Content Delivery Network [http://www.webopedia.com/TERM/C/CDN.html](http://www.webopedia.com/TERM/C/CDN.html)

* 7 Reasons to use a Content Delivery Network[http://www.sitepoint.com/7-reasons-to-use-a-cdn/](http://www.sitepoint.com/7-reasons-to-use-a-cdn/)

* Speedy Web Content Delivery with CDNs [http://windowsitpro.com/networking/speedy-web-content-delivery-cdns](http://windowsitpro.com/networking/speedy-web-content-delivery-cdns)


