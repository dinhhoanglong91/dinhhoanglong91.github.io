---
title: "Redis Data Types And Commands"
cat: "it"
---

Trong bài viết [Tổng quan về Redis]({% post_url 2015-03-13-tong-quan-ve-redis %}), chúng ta đã cùng nhau tìm hiểu cơ bản về Redis, bao gồm các đặc trưng thường thấy của 1 DBMS, kiểu dữ liệu trong Redis và cơ chế lưu data trong ổ cứng của Redis. Trong bài viết này, chúng ta sẽ cùng tìm hiểu về các câu lệnh của Redis, cụ thể là các câu lệnh đặc trưng ứng với từng kiểu dữ liệu.

Nếu như làm việc với DBMS truyền thống như MySQL, chúng ta cần học các câu lệnh để có thể quản lý bảng, dòng, và cột. Với Redis, do đặc thù lưu dữ liệu dạng key-alue với 5 kiểu dữ liệu chính (STRING, LIST, SET, HASH, ZSET), chúng ta cần học cách quản lý 5 kiểu dữ liệu này. Tất nhiên là vẫn có nhưng câu lệnh khác không làm việc cụ thể với kiểu dữ liệu nào, nhưng trong phạm vi bài viết này, chúng ta sẽ bám chặt vào các kiểu dữ liệu của Redis. Bài viết dựa theo 2 tài liệu chính là [An introduction to Redis data types](http://redis.io/topics/data-types-intro) and abstractions và section 1.2 của cuốn Redis in action.

### STRING

String là kiểu dữ liệu cơ bản nhất của Redis và đây cũng là kiểu dữ liệu duy nhất của Memcache. Có 3 câu lệnh cơ bản với String, đó là GET, SET và DEL.

| Câu lệnh | Chức năng                 |
|----------|---------------------------|
| GET      | Lấy giá trị value của key |
| SET      | Thiết lập giá trị cho key |
| DEL      | Xóa key và giá trị tương ứng (Làm việc với tất cả kiểu dữ liệu, không chỉ string) |


Ví dụ:

{% highlight bash %}
127.0.0.1:6379> SET name "Dinh Hoang Long"
OK
127.0.0.1:6379> GET name
"Dinh Hoang Long"
127.0.0.1:6379> DEL name
(integer) 1
127.0.0.1:6379> GET name
(nil)
{% endhighlight %}

### LIST

List trong Redis là linked list, lưu trữ 1 danh sách có thứ tự (trước sau) của các string. Cách lưu trữ này giúp cho thời gian add thêm 1 phần tử vào đầu hoặc cuối list là hằng số, bất kể size của list là bao nhiêu. Lợi thế này cũng có 1 mặt trái là việc truy xuất đến phần tử theo index của linked list là lâu hơn rất nhiều so với array.

Dưới đây là các câu lệnh cơ bản làm việc với List

| Câu lệnh | Chức năng                 |
|----------|---------------------------|
| RPUSH      | Cho thêm 1 giá trị vào phía bên phải của list |
| LRANGE      | Lấy 1 dải giá trị của list |
| LINDEX      | Lấy ra phần tử của list dựa theo index |
| LPOP      | Lấy ra giá trị ở phái ngoài cùng bên trái của list và xóa giá trị đó đi |

Ví dụ:

{% highlight bash %}
127.0.0.1:6379> RPUSH students 'Dinh Hoang Long'
(integer) 1
127.0.0.1:6379> RPUSH students 'Nguyen Hoai Nam'
(integer) 2
127.0.0.1:6379> RPUSH students 'Le Van Hiep'
(integer) 3
127.0.0.1:6379> LRANGE students 0 1
1) "Dinh Hoang Long"
2) "Nguyen Hoai Nam"
127.0.0.1:6379> LRANGE students 0 -1
1) "Dinh Hoang Long"
2) "Nguyen Hoai Nam"
3) "Le Van Hiep"
127.0.0.1:6379> LINDEX students 1
"Nguyen Hoai Nam"
127.0.0.1:6379> LPOP students
"Dinh Hoang Long"
127.0.0.1:6379> LRANGE students 0 -1
1) "Nguyen Hoai Nam"
2) "Le Van Hiep"
{% endhighlight %}

Trang redis.io đưa ra 2 trường hợp phổ biến cho việc dùng list. 1 là lưu lại các post mới nhất của users trên mạng xã hội, điển hình là mạng xã hội nổi tiếng Twitter dùng Redis cho việc này (lưu các tweets mới nhất của users). 2 là xây dựng mô hình tương tác giữa consumer và producer, trong đó producer đưa item vào list và consumer dùng các item này theo thứ tự quy định trong list.

### SET

Set trong Redis khá giống với list, nhưng khác 1 điều là các phần tử trong set không được sắp xếp theo thứ tự nào cả. Tuy nhiên, Redis đã tăng performance khi làm việc với set bằng cách sử dụng 1 bảng băm (hash table) để lưu trữ các phần tử của set. Hiểu đơn giản thì mỗi item được add vào set sẽ là 1 key trong bảng băm, còn value thì không có. Việc làm này giúp theo tác truy xuất dữ liệu trên SET nhanh hơn nhiều (do tận dụng ưu thế về tốc độ tìm kiếm trên bảng băm), nhất là khi muốn đảm bảo không bị trùng lặp phần tử trong set.

4 command cơ bản khi làm việc với set là SADD, SMEMBERS, SISMEMBER, SREM. Chức năng có thể tham khảo bảng sau.


| Câu lệnh | Chức năng                 |
|----------|---------------------------|
| SADD      | Add thêm 1 phần tử vào set |
| SMEMBERS      | Lấy ra tất cả các phần tử của set |
| SISMEMBER      | Check xem 1 phần tử có tồn tại trong set hay không |
| SREM      | Xóa đi 1 phần tử của set (nếu nó tồn tại) |

Ví dụ:

{% highlight bash %}
127.0.0.1:6379> SADD students 'Dinh Hoang Long'
(integer) 1
127.0.0.1:6379> SADD students 'Nguyen Hoai Nam'
(integer) 1
127.0.0.1:6379> SADD students 'Le Van Hiep'
(integer) 1
127.0.0.1:6379> SMEMBERS students
1) "Nguyen Hoai Nam"
2) "Dinh Hoang Long"
3) "Le Van Hiep"
127.0.0.1:6379> SISMEMBER students 'Dinh Hoang Long'
(integer) 1
127.0.0.1:6379> SISMEMBER students 'Nam'
(integer) 0
127.0.0.1:6379> SREM students 'Dinh Hoang Long'
(integer) 1
127.0.0.1:6379> SMEMBERS students
1) "Nguyen Hoai Nam"
2) "Le Van Hiep"
{% endhighlight %}

Cũng cần nói thêm rằng độ phức tạp tính toán của SISMEMBER là O(1), đây là ưu thế rất lớn của set so với dùng list khi sử dụng bảng băm. Chi tiết hơn về độ phức tạp tính toán trong Redis sẽ được nhắc đến ở phần sau của bài viết này.

## HASH

Không giống như LIST và SET lưu trữ 1 tập dữ liệu là các string, HASH lưu trữ tập các map của key và value. Key vẫn là string, còn value có thể là string hoặc số. Nếu là số thì chúng ta có thể làm các thao tác tăng, giảm giá trị 1 cách đơn giản. HASH được coi là 1 mô hình thu nhỏ của Redis, khi dữ liệu được tổ chức dạng key-value. Bảng sau liệt kê các câu lệnh cơ bản khi làm việc với HASH

| Câu lệnh | Chức năng                 |
|----------|---------------------------|
| HSET      | Thêm 1 cặp key-value vào hash hoặc thay đổi giá trị của key đã có. |
| HGET      | Lấy giá trị của key trong hash có sẵn |
| HGETALL      | Lấy ra tất cả các phần tử của hash |
| HDEL      | Xóa cặp key-value ra khỏi hash (nếu key tồn tại) |

Ví dụ:

{% highlight bash %}
127.0.0.1:6379> HSET ages 'long' 24
(integer) 1
127.0.0.1:6379> HSET ages 'nam' 23
(integer) 1
127.0.0.1:6379> HSET ages 'hiep' 30
(integer) 1
127.0.0.1:6379> HGET ages 'nam'
"23"
127.0.0.1:6379> HGETALL ages
1) "long"
2) "24"
3) "nam"
4) "23"
5) "hiep"
6) "30"
127.0.0.1:6379> HDEL ages 'hiep'
(integer) 1
127.0.0.1:6379> HGETALL ages
1) "long"
2) "24"
3) "nam"
4) "23"
{% endhighlight %}

### SORTED SET

Sorted Set (ZSET) là 1 phiên bản đầy đủ của set, khi mà phần value của item được thiết lập, và bắt buộc là 1 số (float number) được gọi là score. Ở điểm này thì zset khá giống với hash khi lưu trữ 1 cặp key, value (trong zset gọi là member và score). Và vì là “sorted”, nên các cặp member-score được add vào sorted set sẽ được sắp xếp theo thứ tự của các score, nếu score trùng nhau thì tiếp tục sắp xếp theo member. Ngoài ra cũng cần chú ý là không cho phép 2 phần tử khác nhau của zset có member trùng nhau. Sau đây là 4 câu lệnh cơ bản với zset.

| Câu lệnh | Chức năng                 |
|----------|---------------------------|
| ZADD      | Thêm 1 phần tử vào tập hợp với score của nó. |
| ZRANGE      | Lấy ra các phần tử của tập hợp theo vị trí của chúng trong zset |
| ZRANGEBYSCORE      | Lấy ra các phần tử của tập hợp theo phạm vi của score |
| ZREM      | Xóa 1 phần tử khỏi tập hợp (nếu nó tồn tại) |

Ví dụ:

{% highlight bash %}
127.0.0.1:6379> ZADD scores 100 long
(integer) 1
127.0.0.1:6379> ZADD scores 80 nam
(integer) 1
127.0.0.1:6379> ZADD scores 90 hiep
(integer) 1
127.0.0.1:6379> ZRANGE scores 0 -1 WITHSCORES
1) "nam"
2) "80"
3) "hiep"
4) "90"
5) "long"
6) "100"
127.0.0.1:6379> ZRANGEBYSCORE scores 89 100 WITHSCORES
1) "hiep"
2) "90"
3) "long"
4) "100"
127.0.0.1:6379> ZREM scores hiep
(integer) 1
127.0.0.1:6379> ZRANGE scores 0 -1 WITHSCORES
1) "nam"
2) "80"
3) "long"
4) "100"
{% endhighlight %}

ZSET là 1 cấu trúc dữ liệu đặc biệt của riêng Redis, nó chuyên dùng cho các bài toán dạng tìm “top”. Top người dùng theo score, top webpage theo số lượng view, “top whatever”. Cách Redis lưu trữ dữ liệu zset cũng rất thú vị, chúng ta sẽ tìm hiểu ở phần tiếp theo của bài viết.

## Redis Command Performance

Ở phần trên của bài viết, chúng ta đã cùng nhau tìm hiểu kỹ hơn các kiểu dữ liệu của Redis cũng như các câu lệnh cơ bản với dữ liệu của Redis. Đến đây người dùng đã có thể hiểu tương đối rõ về Redis và có thể bắt tay vào xây dựng ứng dụng nhỏ theo tutorial có sẵn. Phần còn lại của bài viết dành cho những ai muốn tìm hiểu kỹ hơn về cách thức hoạt động cũng như performance của các câu lệnh Redis.

Các command của Redis được liệt kê đầy đủ trên trang [http://redis.io/commands](http://redis.io/commands), và khi xem chi tiếc từng câu lệnh chúng ta sẽ thấy được độ phức tạp tính toán của câu lệnh đó. Phần phân tích dưới đây tham khảo 1 thread khá hấp dẫn trên stackoverflow về cơ chế bên trong của Redis. Chính cha đẻ của Redis là antirez (có nói đến ở bài viết trước) đã tham gia trả lời câu hỏi ở mục này.

### STRING

String trong Redis không đơn thuần là char* bình thường, nó là 1 kiểu dữ liệu cấu trúc, trong cấu trúc có sẵn 1 phần tử lưu độ dài string, do đó thao tác lấy chiều dài string có độ phức tạp tính toán là O(1). Ngoài ra 2 phép toán cơ bản nhất của string (cũng là cơ bản nhất của Redis) là GET và SET cũng đều có độ phức tạp tính toán là O(1). Bởi Redis lưu dữ liệu key-value dưới dạng bảng băm (hash table) nên những ưu thế này là hiển nhiên có được và vượt trội hơn hẳn BTree trong MySQl khi các thao tác INSERT và SEARCH đều có độ phức tạp tính toán là O(log(n)).

### LIST

List trong Redis là Double Linked List, điều đó giải thích tại sao chúng ta có thể push và pop từ cả 2 phía của list với các câu lệnh: LPUSH, RPUSH, LPOP, RPOP. Có 1 điều khác biệt duy nhất so với kiểu dữ liệu truyền thống, là list trong Redis được định nghĩa với thêm 1 thuộc tính lưu chiều dài của list. Điều đó giải thích tại sao câu lệnh LLEN (lấy ra chiều dài của list) có độ phức tạp tính toán là O(1).

### SET

Như đã giải thích ở phần trên, SET trong Redis được lưu trữ dưới dạng bảng băm, trong đó key là phần tử của set, còn value thì không có. Điều đó giúp các câu lệnh SADD, SREM, SISMEMBER khi làm việc với 1 phần tử đều có độ phức tạp tính toán là O(1).

### HASH

HASH trong Redis đơn thuần là bảng băm và không có gì đặc biệt khi các thao tác cơ bản với hash trong Redis đều có độ phức tạp tính toán là O(1).

### SORTED SET (ZSET)

Đây là kiểu dữ liệu của riêng Redis, và có thể nói là chưa thấy ở đâu cả. Nó sinh ra để giải quyết bài toán sắp xếp rất rất hay gặp khi làm việc với các hệ thống web. Cấu trúc của zset phức tạp hơn 4 kiểu dữ liệu còn lại và nhớ đó nó có thể giải quyết nhiều bài toán khác nhau. Trước khi tìm hiểu về zset, chúng ta cùng xem xét ví dụ sau.

Giả sử chúng ta có 1 tập dữ liệu về students như sau trong dataset.

{% highlight bash %}
╒=rank===member=======score=====╕
| 0    | long     |    10       |
| 1    | nam      |    15       |
| 2    | hiep     |    18       |
| 3    | quang    |    30       |
╘===============================╛
{% endhighlight %}

Tìm ra phần tử có rank từ 0 đến 1

{% highlight bash %}
127.0.0.1:6379> ZRANGE students 0 1 WITHSCORES
1) "long"
2) "10"
3) "nam"
4) "15"
{% endhighlight %}

Tìm ra các phần tử có score từ 11 đến 19

{% highlight bash %}
127.0.0.1:6379> ZRANGEBYSCORE students 11 19 WITHSCORES
1) "nam"
2) "15"
3) "hiep"
4) "18"
{% endhighlight %}

Lấy ra score của member “hiep”

{% highlight bash %}
127.0.0.1:6379> ZSCORE students hiep
"18"
{% endhighlight %}

3 câu lệnh tưởng chừng như rất đơn giản nhưng bên trong nó lại là 1 cấu trúc dữ liệu khá phức tạp. Để thấy được cái hay trong cách tổ chức dữ liệu của Redis, trước tiên chúng ta cùng thử xem MySQL InnoDB sẽ làm việc trên như thế nào.

Giả sử chúng ta có 1 table là students với các column như trên. Để thực hiện được câu lệnh thứ nhất là tìm theo phạm vi của rank, chúng ta cần đánh index cho trường rank, và vì rank là duy nhất nên đây sẽ là 1 loại UNIQUE KEY. Độ phức tạp tính toán sẽ là O(log(n)).
Đến câu lệnh thứ 2 là tìm theo phạm vi của score, do score được phép trùng lặp nên chúng ta cần 1 MULTIPLE KEY, độ phức tạp tính toàn trung bình vẫn là O(log(n)).
Trường hợp cuối cùng là tìm score theo member, vì member là duy nhất nên chúng ta cần 1 UNIQUE KEY, tuy nhiên do member có thể là string với độ dài tùy ý, nên chúng ta không thể dại dột đánh index theo full chiều dài của key (index theo int thì mỗi key chỉ là 4 bytes, nhưng string 128 thì tốn đến tận 128 bytes hoặc hơn, việc đánh index gần như không có nghĩa). Chúng ta sẽ dùng MULTIPLE KEY cho trường hợp này với index là khoảng 10 ký tự đầu cho member (key sẽ là member(10)). Và sau đây là những gì có được nếu làm bằng MySQL.

{% highlight bash %}
+--------+--------------+------+-----+---------+----------------+
| Field  | Type         | Null | Key | Default | Extra          |
+--------+--------------+------+-----+---------+----------------+
| id     | int(10)      | NO   | PRI | NULL    | auto_increment |
| member | varchar(128) | NO   | MUL | NULL    |                |
| score  | float        | NO   | MUL | NULL    |                |
| rank   | int(10)      | NO   | UNI | NULL    |                |
+--------+--------------+------+-----+---------+----------------+
{% endhighlight %}

Dù không được nhắc đến nhưng index dạng primary key là cần thiết trong mỗi thiết kế DB của MySQL. Ngoài ra, mặc dù rank là unique, nhưng do thứ tự cho vào zset là ngẫu nhiên nên không thể đánh đồng rank với id được. Với thiết kế MySQL này, chúng ta cần 4 index tương đương với 4 BTree, và độ phức tạp tính toán khi thực hiện các câu lệnh tương đương với 3 câu lệnh nêu trên đều ở mức O(log(n)).

Tất cả nằm trong cách tổ chức dữ liệu của Redis. Để lưu trữ zset, Redis sử dụng song song 2 cấu trúc dữ liệu: hash table và skip list. Hash table dùng để lưu trữ cặp key-value tương ứng là member-score. Điều đó giải thích tại sao ZSCORE có độ phức tạp tính toán là O(1). Còn skip list dùng để lưu trữ cặp (score, member) theo thứ tự là thứ tự của score (rồi đến member). Skip list là 1 cấu trúc dữ liệu khá đặc biệt, khởi nguồn từ linked list nhưng lại có những tính năng như BTree. Vì giống với BTree nên không khó để thấy được là ZRANGEBYSCORE sẽ có độ phức tạp tính toán là O(log(n)), do sắp xếp cơ bản dựa theo score. Nhưng điều đặc biệt là mặc dù cơ bản chỉ lưu member, và score, skip list vẫn cung cấp phương thức cho phép hàm ZRANGE có độ phức tạp tính toán là O(log(n)), điều không thể khi làm việc với BTree. Rõ ràng nói đến đây người đọc sẽ rất tò mò muốn biến skip list là gì và tại sao nó lại hữu dụng đến vậy. Bản thân tác giả khi viết bài này cũng đã tìm hiểu về skip list và do phạm vi bài viết có hạn, nên xin phép dẫn link wikipedia để bạn đọc tham khảo. Lưu ý là ZRANGE hoạt động được nhờ Indexable skiplist.

*Chú ý: Đánh giá O(log(n)) ở trên đã bỏ đi số phần tử trả về, bởi thực sự độ phức tạp tính toán phải là O(log(n)+m) với m là số phần tử trả về. Tuy nhiên ở các bài toán thực tế, n thường rất lớn, trong khi m chỉ có tính chất phân trang và ở khoảng 10 đến 20, có thể coi như hằng số. Do đó có thể lược bỏ m trong trường hợp này.*

Đến đây chắc bạn đọc đã hiểu cách Redis lưu trữ dữ liệu như thế nào, và trùng lặp dữ liệu là điều không thể tránh khỏi. Bản thân antirez cũng thừa nhận zset tiêu tốn dung lượng bộ nhớ, và vì vậy trong quá trình thiết kế, cần xem xét kỹ để tối giảm dung lượng, đặc biệt là của member.

## Kết luận

Trang redis.io liệt kê khoảng 150 câu lệnh Redis và đó có thể coi là thư viện tiện lợi nhất khi muốn tìm hiểu các câu lệnh Redis. Tuy nhiên cũng như việc không phải ai làm việc với MySQL cũng có thể đổi kiểu dữ liệu column từ VARCHAR(64) sang TEXT mà không dùng Google, chúng ta không thể bắt buộc bản thân học đủ cả 150 câu lệnh Redis. Dù vậy việc tìm hiểu cơ chế hoạt động của các câu lệnh cơ bản lại là điều rất cần thiết cho các mục tiêu xa hơn khi thiết kế schema cũng như tối ưu performance.

Tài liệu tham khảo

* Redis Commands [http://redis.io/commands](http://redis.io/commands)

* An introduction to Redis data types and abstractions [http://redis.io/topics/data-types-intro](http://redis.io/topics/data-types-intro)

* What are the underlying data structures used for Redis?
[http://stackoverflow.com/questions/9625246/what-are-the-underlying-data-structures-used-for-redis](http://stackoverflow.com/questions/9625246/what-are-the-underlying-data-structures-used-for-redis)

* Skip list [http://en.wikipedia.org/wiki/Skip_list]([http://en.wikipedia.org/wiki/Skip_list)

