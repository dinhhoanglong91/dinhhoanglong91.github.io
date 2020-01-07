---
title: "MySQL Performance Tuning: A Real World Example"
cat: "it"
---

2 tuần trước mình có được liên hệ hỗ trợ 1 dự án cải thiện performance của 1 câu query MySQL khá là nặng, phía dự án chia sẻ cho mình thông tin sau.

{% highlight php %}
Explain SELECT `students`.`id`,
       `students`.`name`,
       `students`.`user_id`,
       `students`.`mark_id`,
       `students`.`blocked`,
       `students`.`icon_url`,
       `students`.`blocking`,
       `students`.`line_id`,
       `students`.`line_name`,
       `students`.`note`,
       (select count(id)
        FROM messages
        WHERE messages.student_id = students.id
          AND messages.status = 1
          AND messages.is_from_me = 1) AS unread,
       (select created_at FROM messages WHERE messages.student_id = students.id ORDER by created_at DESC limit 1000) AS lastMessageCreatedAt
FROM `students`
WHERE `students`.`user_id` IN (1, 2, 3)
  AND `students`.`user_id` = 1
  AND `students`.`line_id` <> 2
  AND `students`.`line_id` IS not NULL
  and `students`.`deleted_at` IS null
GROUP by `students`.`id`
ORDER by FIELD(students.mark_id, 1), `lastMessageCreatedAt` DESC
limit 1000 offset 1
{% endhighlight %}

Và đây là kết quả của câu `EXPLAIN`.

{% highlight php %}
*************************** 1. row ***************************
           id: 1
  select_type: PRIMARY
        table: students
   partitions: NULL
         type: ref
possible_keys: PRIMARY,students_applicant_no_unique,hash_id_unique,students_school_id_foreign,students_faculty_id_foreign,students_department_id_foreign,students_user_id_foreign,idx__students__deleted_at,idx__students__user_id_deleted_at,idx__students__mark_id,students_user_id_line_id_index
          key: idx__students__user_id_deleted_at
      key_len: 11
          ref: const,const
         rows: 25
     filtered: 81.00
        Extra: Using index condition; Using where; Using temporary; Using filesort
*************************** 2. row ***************************
           id: 3
  select_type: DEPENDENT SUBQUERY
        table: messages
   partitions: NULL
         type: ALL
possible_keys: messages_student_id_foreign,idx__messages__count,idx__messages__1
          key: messages_student_id_foreign
      key_len: 4
          ref: func
         rows: 6
     filtered: 100.00
        Extra: Using filesort
*************************** 3. row ***************************
           id: 2
  select_type: DEPENDENT SUBQUERY
        table: messages
   partitions: NULL
         type: ref
possible_keys: messages_student_id_foreign,idx__messages__count,idx__messages__1
          key: idx__messages__count
      key_len: 6
          ref: func,const,const
         rows: 3
     filtered: 100.00
        Extra: Using index
{% endhighlight %}

Sau khi nhận các thông tin trên, mình có hỏi phía dự án thêm 1 câu query nữa và nhận được kết quả như sau.

{% highlight php %}
mysql> select mark_id from students order by mark_id desc limit 10;
1137
1134
1133
1130
1124
1124
1121
1121
1116
1115
{% endhighlight %}

Hết rồi !!!

Đây là 1 trong những dự án lớn nhất trong năm của công ty, và vấn đề bảo mật thông tin trong dự án được đẩy lên rất cao, các bạn trong team ai cũng bận rộn với tỷ vấn đề khác nên mình cũng không dám lèo nhèo xin nhiều thông tin, do vậy mình bắt tay vào phân tích vấn đề luôn (go)

Dừng 1 phút :D Các bạn nghĩ giờ chúng ta phải làm gì với nhũng thông tin trên ?...

....

# Ý nghĩa của câu truy vấn

Trước khi phân tích xem câu truy vấn nhanh hay chậm, chúng ta cần biết về mục đích của câu truy vấn. Thú thực mình support cải thiện performance của  MySQL nhiều lần, hầu như lần nào nghe các bạn bên dev team giải thích xong mình cũng... không hiểu, vì đôi khi phải nắm rõ specs bên dự án các bạn. Mà làm cái thân đi support thì không có nhiều thời gian để đọc hiểu specs, nên tốt nhất là tự mình ngẫm ra là rõ nhất, đọc query rồi tự hiểu :D

Khá may là câu query trên chỉ làm việc với 2 bảng, là bảng students với messages. Đoạn đầu của `SELECT` chỉ tập trung vào bảng students nên có thể hiểu sơ sơ được, tiếp theo là đến 1 subquery

{% highlight php %}
  (select count(id)
        FROM messages
        WHERE messages.student_id = students.id
          AND messages.status = 1
          AND messages.is_from_me = 1) AS unread
{% endhighlight %}

Nhìn đoạn `AS unread` là có thể đoán được 1 students có nhiều messages, và mục tiêu ở đây là tìm kiếm số lượng messages mà students này chưa đọc.

Tiếp đến câu subquery thứ 2.

{% highlight php %}
(select created_at FROM messages WHERE messages.student_id = students.id ORDER by created_at DESC limit 1000) AS lastMessageCreatedAt
{% endhighlight %}

Lại nhìn đoạn `AS lastMessageCreatedAt` thì có thể thấy là trong các messages gửi đến students thì tìm ra message được tạo cuối cùng.

Vậy là xong đoạn `SELECT`. Tiếp đến đoạn `WHERE` thì trông cũng không có gì phức tạp, có thể bỏ qua. Đoạn `GROUP BY` càng cho thấy rõ là đang muốn GROUP lại theo đơn vị `students.id` . Đến đoạn ORDER BY thì khá `kinh dị`.

{% highlight php %}
ORDER by FIELD(students.mark_id, 1), `lastMessageCreatedAt` DESC
{% endhighlight %}

Danh sách students được sắp xếp theo 2 tham số, 1 tham số là kết quả của hàm FIELD với đầu vào là `students.mark_id` và `1`, tham số còn lại là `lastMessageCreatedAt` là kết quả của subquery ở trên.

Đến đây chắc các bạn cũng hiểu sơ sơ câu query làm việc gì rồi, và lờ mờ đoán ra nó chậm vì cái gì rồi đúng không :D

Nói ngắn gọn thì câu query có mục đích là

>   Tìm kiếm 1000 students đầu tiên, lấy ra 1 số thông tin cá nhân của students, số lượng message họ chưa đọc  rồi sắp xếp theo `FIELD(students.mark_id, 1)` và thời gian cuối cùng họ được nhận message.

# Các vấn đề của câu truy vấn

Sau khi tương đối hiểu được mục đích của câu truy vấn, chúng ta sẽ đi đến phân tích chi tiết các thành phần có nguy cơ làm chậm hiệu năng.

## Using temporary, using filesort

"Using temporary" là gì"? "Using filesort" là gì? Bạn đã bao giờ được hỏi câu đó chưa? Nhất là trong các cuộc phỏng vấn ?

Tin vui là trên blog của [Percona](https://www.percona.com/blog/2009/03/05/what-does-using-filesort-mean-in-mysql/) có giải thích rõ ràng về 2 keyword này.  Nếu bạn ứng tuyển vào Percona có thể họ sẽ không hỏi câu đó nữa đâu, nhưng các công ty khác thì có thể có đó, nên biết thì vẫn hơn :D

Tóm tắt thì

* `Using temporary`: Dữ liệu quá lớn, không thể để vào RAM được nên phải lưu trong ổ cứng, và thực hiện sort trên ổ cứng.
* `Using filesort`:  Bất kỳ loại sort nào được thực hiện với dữ liệu không được đánh index. Thuật toán ở đây là quicksort hoặc merge sort, và nó cũng chả liên quan gì đến `file` (nên dùng chữ `file` ở đây không hợp lý, nên hiểu là `Using sort`.

Trong MySQL chúng ta đã có các loại key rồi, các loại index rồi, nên cần cố gắng thao tác trên các trường đã được đánh index. Còn nếu bạn phải thực hiện sort với 1 loại dữ liệu không có index thì bạn sẽ bắt gặp kết quả với 2 keyword trên.

Về cơ bản, cả 2 đều phải tránh. Chúng ta cần tìm cách để hạn chế nó.

Cụ thể trong bài toán này. Nhìn vào kết quả của lệnh EXPLAIN, có 2 chỗ chúng ta dùng đến filesort.

1 là query chính base theo bảng students.

{% highlight php %}
SELECT ... FROM students WHERE ... ORDER BY FIELD(students.mark_id, 1), `lastMessageCreatedAt` DESC;
{% endhighlight %}

2 là subquery ở bảng messages lấy ra lastMessageCreatedAt

{% highlight php %}
(select created_at FROM messages WHERE messages.student_id = students.id ORDER by created_at DESC limit 1000) AS lastMessageCreatedAt
{% endhighlight %}

Chúng ta cùng phân tích từng field trong lệnh ORDER BY

* ORDER BY `FIELD(students.mark_id, 1)`

Mình khá bất ngờ khi đọc câu này vì thấy nó quá ảo. `FIELD` là hàm của MySQL trả về số thứ tự của  tham số đầu tiên (kể từ vị trí thứ 2) match với tham số đầu tiên của hàm.
Ví dụ

{% highlight php %}
mysql> SELECT FIELD(1, 2, 1, 3);
+-------------------+
| FIELD(1, 2, 1, 3) |
+-------------------+
|                 2 |
+-------------------+
{% endhighlight %}

Hàm FIELD trả về 2 vì từ tham số thứ 2 thì phải đến vị trí số 2 (tức là tham số thứ 3) mới match với tham số đầu tiên của hàm (là `1`).

Do nghi vấn đó, mình đã hỏi team phát triển xem thực sự `students.mark_id` lưu gì, và giống như mình đã chia sẻ ở đầu bài, kết quả trả về là 1 loạt các số kiểu: 1137, 1134, 1116,... Các số này khi cho vào hàm `FIELD(mark_id, 1)` đều trả về 0. Tất cả đều giống nhau thì việc ORDER không có ý nghĩa gì. Hàm sẽ chỉ trả về giá trị khác 0 nếu mark_id bằng 1.

{% highlight php %}
mysql> SELECT FIELD(1137, 1), FIELD(1134, 1), FIELD(1116, 1), FIELD(1, 1);
+----------------+----------------+----------------+-------------+
| FIELD(1137, 1) | FIELD(1134, 1) | FIELD(1116, 1) | FIELD(1, 1) |
+----------------+----------------+----------------+-------------+
|              0 |              0 |              0 |           1 |
+----------------+----------------+----------------+-------------+
{% endhighlight %}

Vậy nếu chỉ muốn tìm xem những ai có mark_id bằng 1 cho xuống dưới cùng thì hoàn toàn có thể `ORDER BY mark_id DESC` là đủ (với điều kiện là đã tối ưu cách lưu trữ để đảm bảo không còn giá trị nào nhỏ hơn 1 được lưu).

* ORDER BY `lastMessageCreatedAt` DESC

`lastMessageCreatedAt` là kết quả của 1 dependent subquery, nó bản thân không có ở trong bảng nào hết nên việc phải dùng đến filesort là điều không thể tránh khỏi.
Nếu specs của dự án bắt buộc phải như vậy thì không còn cách nào khác, cần chấp nhận rủi ro ở đây.

Tuy nhiên, phía application team có thể cải thiện vấn đề này bằng cách thêm trường `last_message_created_at` vào bảng students.
Mỗi khi có message mới thì update trường này cho bảng students, như thế câu lệnh ORDER BY sẽ chạy trực tiếp trên 1 field của bảng students. Lúc đó có thể đánh INDEX thoải mái để tăng tốc độ.

* ORDER by created_at DESC

Cả câu truy vấn

{% highlight php %}
(select created_at FROM messages WHERE messages.student_id = students.id ORDER by created_at DESC limit 1000) AS lastMessageCreatedAt
{% endhighlight %}

Câu query này mục tiêu tìm kiếm message cuối cùng được tạo cho student này. Nhưng không hiểu vì sao lại SELECT hết ra rồi ORDER BY rồi mới gán vào `AS lastMessageCreatedAt`.
Trong hầu hết các trường hợp, câu query này sai. Không chạy được và gặp lỗi này

```
ERROR 1242 (21000): Subquery returns more than 1 row
```

Bởi 1 student có nhiều message, câu lệnh trả về nhiều hơn 1 giá trị nên không thể gán vào 1 giá trị được. Chỉ may mắn chạy được nếu students đó có duy nhất 1 messages.
Câu query đúng phải viết như sau.

{% highlight php %}
(select MAX(created_at) FROM messages WHERE messages.student_id = students.id) AS lastMessageCreatedAt
{% endhighlight %}

Nếu may mắn performance vẫn ổn, thì chúng ta có thể tạm chạy theo cách này, không cần lưu  trường last_message_created_at vào bảng students.

## Dependent Subquery

Có 2 loại subquery, 1 là dependent subquery và 2 independent subquery. Dependent subquery phụ thuộc vào câu query bên ngoài (outer query) trong khi independent subquery không phụ thuộc vào query bên ngoài (outer query) và có thể chạy độc lập. 1 ví dụ về independent subquery với bảng students và messages.

{% highlight php %}
SELECT * FROM students WHERE id IN (SELECT DISTINCT(student_id) FROM messages WHERE created_at >= "2020-01-01");
{% endhighlight %}

Tìm tất cả các students được gửi messages từ đầu năm mới (2020) đến giờ.
Rõ ràng câu subquery

{% highlight php %}
SELECT DISTINCT(student_id) FROM messages WHERE created_at >= "2020-01-01"
{% endhighlight %}

Có thể chạy độc lập mà không cần biết `students` ở bên ngoài là gì.

Tuy nhiên trong bài toán của chúng ta, thì có đến 2 subquery và đều là depedent subquery. 1 số trường hợp subquery có thể giải quyết được bằng JOIN, nhưng trường hợp này thì không.

Cần biết rằng khi có depedent subquery, mỗi khi MySQL duyệt đến 1 dòng của bảng chính (qua outer query), nó sẽ phải tiếp tục chạy 1 câu query subquery nữa và cảm giác chúng ta đang chạy N+1 query chứ không phải 1 query. Vì vậy, cần hạn chế tối đa dependent subquery.

{% highlight php %}
(select count(id) FROM messages WHERE messages.student_id = students.id AND messages.status = 1 AND messages.is_from_me = 1) AS unread
(select created_at FROM messages WHERE messages.student_id = students.id ORDER by created_at DESC limit 1000) AS lastMessageCreatedAt
{% endhighlight %}

Cùng nhìn lại 2 subquery ở trên và nghĩ xem chúng ta có thể cải thiện được gì.

* Với trường hợp lấy ra trường `lastMessageCreatedAt`, như đã phân tích ở trên, thì cách tối ưu nhất là lưu thêm trường cho bảng students, đánh INDEX cho nó, và như vậy sẽ dùng nó sau này ở hàm `ORDER BY`.

* Còn với trường hợp `unread`, giải pháp có vẻ đơn giản hơn. Trường này không dùng ở đâu trong câu query hết, không dùng để sắp xếp gì hết, nên chúng ta hoàn toàn có thể tách nó ra và chạy 1 câu query riêng. Câu query này chỉ cần chạy với bảng messages và group by student_id để count xem 1 student có bao nhiêu message. Đối tượng student_id để WHERE là list `students.id` kết quả của câu query trước đó.

{% highlight php %}
SELECT student_id, count(id) FROM messages WHERE messages.student_id IN (LIST STUDENT ID) AND messages.status = 1 and messages.is_from_me = 1 GROUP BY messages.student_id;
{% endhighlight %}

Vậy là không cần depedent subquery, và hiệu năng câu query thứ 2 cũng hơn hẳn.

# Tl;dr

Trên đây là những phân tích của mình và mình đã chia sẻ với dev team, các bạn đang triển khai sửa và mình chờ ngày các bạn sẽ liên hệ với mình xem với data thực tế thì cải thiện được bao nhiêu. Cá nhân mình cũng muộn bạn đọc tự nghiền ngẫm xem liệu performance sẽ được cải thiện không và còn chỗ nào có thể cải thiện được không.

Mình muốn tóm tắt 1 vài điểm nên làm khi chúng ta nhận được 1 câu query nặng.

* Kiểm tra lại xem nó đúng specs hay không. Nếu sai specs thì cứ viết lại cho đúng đi rồi tính tiếp.
* Không nên cố gắng chỉ viết 1 câu query, đôi khi viết thêm 2,3 câu query còn nhanh hơn là cố đấm ăn xôi viết 1 câu query siêu nặng.
* Cố gắng hạn chế sửa code application, nhưng nếu phải sửa thì nên sửa. Đôi khi phía application chỉ cần bổ sung 1,2 xử lý thì phía SQL đã nhẹ hơn rất nhiều rồi. Không nên ép SQL engineer sấp mặt với 1 cấu trúc DB quá tù tội.

Finally, Happy new year :hugs::hugs::hugs::hugs:

Tài liệu tham khảo

* [Percona Blog](https://www.percona.com/blog/2009/03/05/what-does-using-filesort-mean-in-mysql/)
* [MySQL FIELD() function](https://www.w3resource.com/mysql/string-functions/mysql-field-function.php)
* [Sử dụng EXPLAIN để tối ưu câu lệnh MySQL](https://viblo.asia/p/su-dung-explain-de-toi-uu-cau-lenh-mysql-BYjv44gmvxpV)
* [MySQL Indexing](https://viblo.asia/p/mysql-indexing-PDOkqWxAGjx)
