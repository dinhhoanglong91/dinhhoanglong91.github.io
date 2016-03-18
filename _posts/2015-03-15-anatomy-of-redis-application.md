---
title:  "Anatomy Of Redis Application"
show_top: 0
---

Ở các bài viết trước, chúng ta đã tìm hiểu [Tổng quan về Redis]({% post_url 2015-03-13-tong-quan-ve-redis %}) cũng như [Redis data types and commands]({% post_url 2015-03-14-redis-data-types-and-commands %}) và qua đó có được cái nhìn tổng quan nhất về Redis. Bài viết tiếp theo này sẽ là bước khởi động “nhẹ nhàng” trước khi chúng ta bắt tay vào viết 1 ứng dụng Redis. Chúng ta sẽ cùng nhau phân tích 1 case study cho việc ứng dụng Redis vào web application. Ví dụ được lấy ra từ section 1.3 của cuốn Redis in action, nhưng tách biệt với ngôn ngữ lập trình (Redis in action đưa ra thao tác cụ thể với python), và có 1 vài sửa đổi cho dễ tiếp cận hơn trong phạm vi bài viết.

## Voting articles

Với dân IT thì stackoverflow không có gì là xa lạ, nơi mà chúng ta vào đó hàng ngày để thu thập thông tin, tìm kiếm cách giải quyết vấn đề. Chúng ta có thể dễ dàng nhận ra stackoverflow cho phép người dùng có thể vote 1 câu hỏi hoặc 1 câu trả lời, từ đó stackoverflow sẽ quyết định thứ tự hiển thị của các câu trả lời trong mỗi thread. 1 trang web tương tự là [http://www.reddit.com/](http://www.reddit.com/) cũng cho phép người dùng vote, gồm vote up và vote down. Sau đây chúng ta sẽ cùng tìm hiểu về cách xây dựng 1 ứng dụng cho phép vote article, sử dụng Redis. Và ở mức độ newbie, sẽ chỉ tạm thời xem xét vote up mà không tính đến vote down.

Mô tả hệ thống: Chúng ta có 1 hệ thống cho phép người dùng đăng tải các bài viết với đủ chủ đề. Mỗi ngày, người dùng có thể đăng lên hàng nghìn article, trang chủ sẽ có danh sách 100 bài viết có rank cao nhất, trong đó có khoảng 50 bài viết là bài viết của ngày hôm đó. Thuật toán tính rank sẽ phụ thuộc vào thời gian post bài (bài cuối sẽ có điểm cao hơn) và số lượng vote up cho bài (càng nhiều vote rank càng cao). Do vậy để xếp thứ hạng các bài viết, chúng ta sẽ tạo ra 1 biến score, được tính toán dựa theo thời gian tạo (time) và số lượng vote (votes): ```score = f(time, votes)```.

### Sau đây chúng ta bắt tay vào việc thiết kế dataset

* Để lưu article, chúng ta sẽ sử dụng HASH. Trong đó key sẽ có dạng “article:$id”, trong đó $id sẽ là id của article (ví dụ: “article:12345″). Các phần tử trong hash sẽ có dạng key-value, trong đó có key có thể là title, link, poster, time, votes. Ví dụ:
{% highlight bash %}
  HASH
  ╒=article:12345=====================╕
  | title   |    Redis commands       |
  | link    |    http://goo.gl/adUbfp |
  | poster  |    user:33221           |
  | time    |    1409047277           |
  | votes   |    100                  |
  ╘================================== ╛
{% endhighlight %}

* Để lưu trữ score cho việc sắp xếp, chúng ta sử dụng zset. Vì sắp xếp theo score nên key của zset sẽ là “score:”. Còn value sẽ gồm 2 thành phần: member sẽ là key của danh sách article ở trên, còn score sẽ là score của article theo như tính toán ở function đã được định sẵn. Ví dụ:

{% highlight bash %}
  ZSET
  ╒=score:============================╕
  | article:12345   |    100          |
  | article:239     |    110          |
  | article:340     |    500          |
  ╘================================== ╛
{% endhighlight %}

* Lưu ý rằng mỗi user chỉ được vote cho article 1 lần duy nhất, do đó ta cần 1 tập hợp để lưu trữ những user đã vote cho 1 article nhất định, để đảm bảo không ai vote 1 article quá 1 lần. Chúng ta sẽ dùng SET trong trường hợp này, key của set sẽ là ```voted:$id``` trong đó id là id của article như định nghĩa ở phần 1, còn phần tử của set sẽ được tạo từ id của user, có dạng ```user:$id```. Ví dụ:

{% highlight bash %}
  SET
  ╒=voted:12345=====╕
  | user:1          |
  | user:34         |
  | user:400        |
  ╘================ ╛
{% endhighlight %}

### Tiếp theo chúng ta bắt tay vào thực hiện các thao tác cơ bản với ứng dụng voting articles

* Tạo 1 article mới

  Muốn add 1 article mới, chúng ta cần tạo 1 phần tử mới vào danh sách article (lưu dạng hash), id sẽ được sinh tăng dần với câu lệnh INCR, còn các thông tin sẽ được add vào hash qua câu lệnh HMSET.

  {% highlight bash %}
    redis> INCR article_id
    (integer) 2
    redis> HMSET article:2 title 'Redis commands' link 'http://goo.gl/ab12EF' user:10 time:1409047277 votes:0
    OK
  {% endhighlight %}


  Tiếp theo chúng ta cần khởi tạo score cho article. Score được tính theo thời gian tạo và curent vote (0).

  {% highlight bash %}
    redis> ZADD score: 100 article:2
    (integer) 1
  {% endhighlight %}

* Lấy ra danh sách top article

  Trong ví dụ này, chúng ta sẽ lấy ra 10 article có score lớn nhất. Do score được sắp xếp theo thứ tự tăng dần, nên để lấy được score cao nhất, chúng ta sẽ không dùng ZRANGE mà dùng ZREVRANGE. Từ đó sẽ lấy được articles’ id. Sau đó chúng ta sẽ dùng hàm HGETALL để lấy ra nội dung của các article.

  {% highlight bash %}
    redis> ZREVRANGE score: 0 9
    1) "article:3"
    2) "article:2"
    redis> HGETALL article:3
    1) "title"
    2) "Skip list"
    3) "link"
    4) "http://goo.gl/eF3rdt"
    5) "poster"
    6) "user:20"
    7) "time:1409047300"
    8) "votes:0"
    redis> HGETALL article:2
    1) "title"
    2) "Redis commands"
    3) "link"
    4) "http://goo.gl/ab12EF"
    5) "poster"
    6) "user:10"
    7) "time:1409047277"
    8) "votes:0"
  {% endhighlight %}

* Vote 1 article
  Giả sử user có id là 100 vote article số 2 ở trên, khi đó chúng ta cần tăng trường votes trong HASH với key là article:2 lên 1 đơn vị, tăng score của article 2 lên, đồng thời add user 1 vào danh sách những người đã vote article 2 để đảm bảo user này không thể vote thêm 1 lần nữa. Do vậy ta cần 3 câu lệnh, 1 là HINCRBY để tăng score, 2 là ZADD để cập nhật score cho article, 3 là SADD để add user vào tập những người đã vote article này.

  {% highlight bash %}
    redis> HINCRBY article:2 votes 1
    (integer) 1
    redis> ZADD score: 300 article:2
    (integer) 0
    redis> SADD voted:2 user:100
    (integer) 1
    redis> HGET article:2 votes
    "1"
    redis> ZSCORE score: article:2
    "300"
    redis> SMEMBERS voted:2
    1) "user:100"
  {% endhighlight %}

  3 câu lệnh sau dùng để kiếm tra tính đúng đắn của 3 câu lệnh trước. Chú ý chúng ta đã dùng liên tiếp 3 câu lệnh thay đổi dataset hiện tại là HINCRBY, ZADD và SADD, do đó trong thực tế cần sử dụng Redis transaction trong trường hợp này.

## Kết luận

Như vậy là đến đây chúng ta đã hiểu phần nào cách xây dựng 1 ứng dụng web cơ bản với Redis. Bạn đọc cũng có thể tham khảo chương 2 của cuốn Redis in action để biết thêm 1 vài case studies nữa, cho việc dùng Redis để lưu thông tin đăng nhập của user, lưu trữ shoping carts, và cả phân tích lưu lượng truy cập website.


