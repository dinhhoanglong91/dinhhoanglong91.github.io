---
title: "Laravel and PHP Magic Methods"
---

Theo kết quả survey của trang [sitepoint.com](http://www.sitepoint.com/best-php-framework-2015-sitepoint-survey-results/), Laravel vượt trội hoàn toàn so với các Framework khác của PHP về sự phổ biến. Nên nhớ rằng Laravel là một framework mới, rất mới so với Zend hay Symfony. Vậy điều gì đã làm cho Laravel trở nên nổi bật như vậy?

Đã có rất nhiều bài viết của Viblo phân tích về sự tuyệt vời của Laravel, trong đó phải kể đến loạt bài viết [Laravel Beauty](https://viblo.asia/search/laravel%20beauty) của tác giả @thangtd90 . Trong đó, tác giả đã giới thiệu các công cụ của Laravel, best practices khi làm việc với Laravel và đặc biệt là kiến trúc tổng thể của Laravel thông qua Service Container. Trong bài viết này, tôi sẽ không chú trọng quá nhiều vào kiến trúc Laravel, mà tôi muốn giới thiệu cho các bạn đặc trưng của PHP đóng góp vai trò quan trọng trong việc xây dựng 1 framework tuyệt vời như Laravel.

Chúng ta cùng nhau đến với `magic method`.

## Tổng quan về magic methods trong PHP

PHP có tất cả 15 [magic methods](http://php.net/manual/en/language.oop5.magic.php). Có 2 methods mà tôi nghĩ ai cũng biết và đã từng làm với PHP đó là `__construct()` và `__toString()`. Theo như tên gọi của mình, `magic methods` sẽ không được lập trình viên gọi trực tiếp mà được PHP gọi 1 cách tự động theo một cơ chế "ma thuật" nào đó được quy định sẵn. `__construct()` được gọi tự động khi bạn khởi tạo 1 object mới, còn `__toString()` được gọi tự động khi bạn muốn object của mình hoạt động như một string (ví dụ `echo $object`). Có 2 ưu điểm nổi bật nhất mà tôi thấy ở magic method, 1 là làm code của bạn sáng sủa và ngắn gọn hơn. Đơn giản là thay vì phải gọi `echo $object->toString()` thì bạn có thể gọi ngắn gọn `echo $object` và PHP sẽ làm phần việc chuyển đổi string cho bạn. 2 là cung cấp khả năng viết code trong code. Đây là tư tưởng cơ bản mà trong giới Ruby họ gọi là `Meta Programming`. Với khả năng "viết code trong code", bạn sẽ tạo ra được hàng tá function hoạt động mượt mà trong khi không phải viết hết function trong class, giúp class của bạn gọn gàng hơn và giảm được việc viết đi viết lại các hàm na ná nhau.

Do có đến 15 magic methods nên tôi không thể liệt kê hết ra trong bài viết này, tôi sẽ chỉ nêu ra 4 hàm overloading của PHP. Đó là `__get()`, `__set()`, `__call()` và `__callStatic()`. 4 hàm này được đặt chung là các hàm overloading bởi chúng hoạt động trên 1 cơ chế chung là "overload - chất thêm". Khi người dùng truy cập đến một thuộc tính hay một phương thức không tồn tại trong class, hay không access được trong phạm vi hiện tại (ví dụ access hàm private), thì các hàm này sẽ được gọi như một sự bổ sung cho class hiện tại của PHP.

### Property Overloading

Đầu tiên là 2 hàm overload với thuộc tính `__get` và `__set`. Thử xem chúng hoạt động thế nào qua ví dụ sau.

{% highlight php %}
<?php
class User {
    private $name;
    public function __construct($name) {
        $this->name = $name;
    }
    public function __get($attribute) {
        if (property_exists($this, $attribute)) {
            return $this->$attribute;
        }
        return null;
    }
    public function __set($attribute, $value) {
        if (property_exists($this, $attribute)) {
            $this->$attribute = $value;
            return true;
        }
        return false;
    }
}

$user = new User('Peter');
echo $user->name . PHP_EOL;  // Output: Peter
$user->name = 'David';
echo $user->name . PHP_EOL;  // Output: David
{% endhighlight %}

Khá đơn giản phải không các bạn. Việc gọi `$user->name` từ ngoài class `User` đồng nghĩa với việc bạn truy cập đến 1 thuộc tính không nhìn thấy được. Do vậy, hàm `__get()` được gọi và giá trị biến `$name` được trả về. Cách làm này đảm bảo tính đóng gói của lập trình hướng đối tượng mà vẫn mềm mại cho việc get giá trị. Cách giải thích thương tự cũng được áp dụng với `__set()` với câu lệnh `$user->name = 'David';`

### Method Overloading

Khác với `_get()` và `_set()` làm việc với thuộc tính, `__call()` và `__callStatic()` làm việc với method. Như tên gọi của nó, `__call()` sẽ được gọi khi truy cập đến 1 hàm không thể thấy được khi gọi thông qua object. `__callStatic()` sẽ được gọi khi truy cập đến 1 hàm không thể thấy được khi gọi thông qua hàm `static`. Xem xét ví dụ sau để biết cách hoạt động của các hàm này.

{% highlight php %}
<?php
class Person {
    private $name;
    public function __construct($name) {
        $this->name = $name;
    }
    public function __call($name, $arguments) {
        if (strpos($name, 'set') !== false) {
            $attribute = strtolower(substr($name, 3));
            if (property_exists($this, $attribute)) {
                $this->$attribute = $arguments[0];
            }
        }
    }
    public static function __callStatic($name, $arguments) {
        if ($name == 'sayHello') {
            return 'Hello, My name is ' . $arguments[0];
        }
    }
    public function getName() {
        return $this->name;
    }
}

$person = new Person('David');
echo $person->getName() . PHP_EOL;      // Output: David
$person->setName('Peter');
echo $person->getName() . PHP_EOL;      // Output: Peter
echo Person::sayHello('Tom') . PHP_EOL; // Output: Hello, My name is Tom
{% endhighlight %}

Rất dễ dàng để hiểu những dòng code trên, đây là một cách tiếp cận khác cho `getter`, `setter` :D

Vậy chúng ta đã có cái nhìn cơ bản về magic methods trong PHP, tiếp theo, hãy cùng nhau tìm hiểu xem Laravel đã sử dụng chúng như thế nào.

## How Laravel uses PHP magic methods

Như bao web framework khác, Laravel sử dụng kiến trúc MVC. Trong kiến trúc này, M-Model luôn được coi là hạt nhân quan trọng nhất. Với một người đã làm qua khá nhiều framework như tôi, tôi luôn xem các hoạt động của Model đầu tiên khi tìm hiểu về framework. Bởi xét cho cùng, đây là phần quan trọng nhất và phức tạp nhất.

Laravel có lẽ cũng đã thấy điều này, họ làm cho Model thực sự tuyệt vời để ai cũng có thể "chơi" với nó. Thêm nữa, với sự có mặt công cụ `tinker`, bạn có thể test Laravel model thoải mái không phải F5 lại trình duyệt. Vậy, quay lại chủ đề chính của bài viết, chúng ta hãy xem xem Laravel sử dụng magic methods như thế nào.

### JavaBeans Conventions and ... Laravel Eloquent Model

Đang yên đang lành lôi Java ra đây làm gì?

Ừ thì cái gì cũng có lý của nó! Khi nói đến lập trình hướng đối tượng, chúng ta không thể quên ngôn ngữ Java, ngôn ngữ mà ở đó mọi khái niệm về hướng đối tượng đều được làm rõ, các Design Patterns được sử dụng linh hoạt để tạo nên 1 ngôn ngữ hướng đối tượng tuyệt vời. Tuy nhiên, có 1 vấn đề khà nổi tiếng, đó là vấn đề liên quan tới JavaBeans.

JavaBeans, theo [wikipedia](https://en.wikipedia.org/wiki/JavaBeans), là các class đóng gói nhiều object lại vào trong 1 object (Bean - hạt đậu). Có 3 đặc tính cơ bản của JavaBeans. Thứ nhất là nó có thể chuyển đổi từ object sang chuỗi (serialize) mà sau đó có thể tạo lại object từ chuỗi đó (deserialize). Thứ 2 là nó có thể được tạo ra từ 1 constructor không có tham số. Thứ 3 là có thể truy cập đến các thuộc tính của đối tượng thông qua phương thức `getter` và `setter`.

Điều kiện 1 và 2 thì khỏi phải bàn, tuy nhiên điều kiện thứ 3 lại thực sự là một vấn đề nan giải. Về cơ bản, theo tư tưởng của Java, mỗi thuộc tính của class sẽ cần 2 hàm, 1 hàm get để lấy dữ liệu và 1 hàm set để thiết lập giá trị. Nếu class nhỏ thì không sao, nhưng nếu đó là một class cực lớn thì sẽ rất vất vả cho việc định nghĩa các hàm get và set. Vất vả ở đây không phải là viết khó, mà chúng quá nhiều và nội dung na ná nhau, cảm giác như bạn viết đi viết lại một hàm. Đây là vấn đề của JavaBeans Conventions, nó gây ra cái gọi  là `boilerplate code - code theo bản mẫu`.

Khái niệm JavaBeans đương nhiên là không tồn tại trong PHP, tuy nhiên trong PHP có những khái niệm tương đương. Hầu hết các Model trong kiến trúc MVC của các Framework PHP đều được viết theo kiểu của JavaBeans. Điển hình nhất phải nói đến Framework thuộc hàng nổi bật nhất của PHP là Symfony.

Model trong Symfony được lấy tên là `Doctrine Entity` (thực ra khái niệm Entity có phạm vi nhỏ hơn Model). Trong `Entity`, bạn không phải viết `getter` `setter` trực tiếp mà nó được sinh ra tự động trong code sau khi bạn khai báo property và chạy lệnh sinh `getter` `setter`. Mặc dù Symfony cung cấp công cụ cho công việc khá nhàm chán này, như xét cho cùng thì chúng vần nằm trong code của bạn. Và như nói ở trên, khi mà số lường trường lớn lên, thì `Entity` của bạn cũng lớn lên một cách không mong muốn.

Vậy Laravel đã giải quyết vấn đề này như thế nào? Câu trả lời chính là sử dụng magic method `__get()` và `__set()`.  Hãy xem source code của Laravel với hàm `__get()`

{% highlight php %}
<?php
// Illuminate/Database/Eloquent/Model.php
public function __get($key) {
    return $this->getAttribute($key);
}

public function getAttribute($key) {
    if (array_key_exists($key, $this->attributes) || $this->hasGetMutator($key)) {
        return $this->getAttributeValue($key);
    }
    return $this->getRelationValue($key);
}
{% endhighlight %}

Như các bạn thấy, khi người dùng truy cập đến 1 property của Model, Laravel sẽ gọi đến hàm `getAttribute`. Mỗi model có một property là `$attributes` trong đó lưu giá trị của tất cả các trường. Ngay cả khi bạn muốn truy cập đến một property không có trong mảng `$attributes`, bạn vẫn còn có cơ hội tìm thấy giá trị đó thông qua `Accessor` hoặc `relationValue`. Với `Accessor`, bạn có thể định nghĩa hàm để lấy bất cứ giá trị nào thông qua một quy tắc chung. Với `relationValue`, bạn có thể lấy giá trị từ các bảng liên quan thông qua việc định nghĩa quan hệ.

Điều tương tự cũng làm được với hàm `__set()`. Đáng nói là Laravel cung cấp `Accessor` và `Mutator` cho phép bạn tùy biến phương thức `get`, `set`, qua đó giúp cho model được sử dụng linh hoạt hơn và ngắn gọn hơn. Là một người làm việc với Laravel, tôi rất mong các bạn có thể sử dụng thuần thục `Accessor` và `Mutator` qua đó giúp code trở nên "đẹp" hơn :D

### Eloquent Model and Query Builder

Trong hầu hết các framework PHP, mọi thứ đều được đóng gói bên trong Model. Các thao tác tìm kiếm, sửa, xóa,... đều được viết dựa trên Model. Sử dụng Model là đầy đủ cho những chức năng cơ bản (không nói đến các query phức tạp).

Laravel thì sao, mới nhìn, bạn thấy nó cũng giống các Framework khác khi "mọi thứ" đều làm được với Model.

{% highlight bash %}
\App\Model\User::find(1)
=> App\Model\User {#720
    id: "1",
    name: "****",
    email: "****",
    created_at: "2015-12-20 05:04:51",
    updated_at: "2015-12-20 15:29:58",
    username: "****",
    user_role: "****",
}
{% endhighlight %}

Thao tác tưởng như đơn giản nhưng điều kỳ lạ là khi đọc source code của Eloquent Model, bạn không thể thấy hàm find (omg)
Không tin bạn có tìm ở link sau. Yên tâm là không có (hoho)
https://github.com/laravel/framework/blob/5.1/src/Illuminate/Database/Eloquent/Model.php

Trước khi tìm hiểu xem cái hàm `find` ở đâu chui ra và tại sao chúng ta có thể gọi hàm đó, tôi giới thiệu thêm 1 chút về Laravel. Trong tài liệu của Laravel,  người ta nói rằng có thể sử dụng Query Builder để lấy dữ liệu từ DB. Khác biệt giữa Query Builder và Eloquent Model là hàm gọi từ Query Builder trả về mảng, trong khi Eloquent Model trả về Collections (hoặc thực thể của Model). Thêm nữa, Query Builder đặc biệt hữu dụng với các câu truy vấn phức tạp.

Thử quay lại với hàm `find`. Hãy xem `Query Builder` xử lý như thế nào.

{% highlight bash %}
DB::table('users')->find(1)
=> {#703
    +"id": "1",
    +"name": "****",
    +"email": "****",
    +"password": "****",
    +"remember_token": "****",
    +"created_at": "2015-12-20 05:04:51",
    +"updated_at": "2015-12-20 15:29:58",
    +"username": "****",
    +"user_role": "****",
}
{% endhighlight %}

Cách dùng có thể khác nhưng rõ ràng điều đáng chú ý là Query Builder trả về mảng. Điều này có nghĩa bạn sẽ không thể tìm gì thêm từ đây ngoại trừ các giá trị trong mảng đó.

OK, vậy là bạn đã biết sơ sơ về Eloquent Model và Query Builder. Tuy nhiên câu chuyện của chúng ta là tìm xem hàm `find` của Eloquent Model ở đâu chui ra. Tin vui là trong Query Builder bạn tìm thấy hàm `find`, vậy phải chăng có liên hệ gì giữa hàm `find` được dùng ở Eloquent Model với hàm `find` được viết trong `Query Builder`.

Do hàm `find` không có trong Eloquent Model và vì nó được gọi `static`, nên chúng ta sẽ tìm đến `__callStatic()`

{% highlight php %}
<?php
// Illuminate/Database/Eloquent/Model.php
public static function __callStatic($method, $parameters) {
    $instance = new static;
    return call_user_func_array([$instance, $method], $parameters);
}
{% endhighlight %}

Khoan hãy nói về sự khác biệt giữa `new static` và `new self` bởi nó có ý nghĩa về mặt đa hình, nhưng tư tưởng chung là đều khởi tạo 1 thực thể cho class. Vậy tại thời điểm này, do hàm `find` không được định nghĩa trong class `Model`, hàm `__callStatic()` được gọi. Tại đây, 1 thực thể được tạo ra và hàm này sẽ được chuyển tới `__call()`.

Vậy có gì ở `__call()`.

{% highlight php %}
<?php
// Illuminate/Database/Eloquent/Model.php
public function __call($method, $parameters) {
    if (in_array($method, ['increment', 'decrement'])) {
        return call_user_func_array([$this, $method], $parameters);
    }
    $query = $this->newQuery();
    return call_user_func_array([$query, $method], $parameters);
}
{% endhighlight %}

Vậy là có thể thấy, các hàm không có ở Eloquent Model sẽ được tự động chuyển đến gọi thông qua kết quả của `newQuery()`. Xin được nói thêm rằng `newQuery()` trả lại `EloquentBuilder`. Vậy `EloquentBuilder` có liên quan gì đến `QueryBuilder` mà chúng ta nói ở trên không.

Câu trả lời là bản thân `EloquentBuilder` khi khởi tạo đã được inject vào một thực thể của `QueryBuilder` thông qua cơ chế của DI. Qua đó, `EloquentBuilder` có thể dễ dàng thực hiện các hàm của QueryBuilder. Nó gọi đến QueryBuilder, nhận kết quả là một mảng rồi chuyển đổi mảng đó về dạng Collections. Đó là lý do chúng ta thấy sự khác biệt giữa việc gọi hàm qua `EloquentModel` và qua `QueryBuilder`.

Ok, vậy là bạn đã hiểu được cách Eloquent Model sử dụng `__call` và `_callStatic` cũng như con đường thực hiện một hàm khá phổ biến của nó. Sự phức tạp là điều có thể thấy rõ ở đây. Câu hỏi đặt ra là tại sao lại phải phức tạp như vậy, tại sao không chỉ dùng 1 class, hay tại sao phải dùng đến hàm static mà chung quy lại quay lại hàm non-static.

Thứ nhất, việc chia class ra khiến cho kiến trúc của Laravel trở nên rõ ràng. Eloquent Model đảm nhiệm các thao tác cơ bản với model là CRUD (Creat-Read-Update-Delete) và các thao tác liên quan đến quan hệ giữa các bảng (relationship). Query Builder đóng vai trò làm việc trực tiếp với database thông qua các truy vấn `SELECT`, `UPDATE`,... Có thể nói Query Builder là tầng dưới của `Eloquent Model`. 2 tầng này liên kết với nhau thông qua `EloquentBuilder`. 1 kiến trúc mới nhìn có vẻ phức tạp nhưng lại khá rõ ràng và mạch lạc. Qua đó giúp cho source code của Laravel trở nên mềm dẻo, dễ dàng thích ứng với các thay đổi. Bạn cũng không phải định nghĩa các hàm của `QueryBuilder` ở `EloquentModel` bởi đã có các magic methods thực hiện công việc kết nối cho bạn.

Thứ hai, việc bổ sung gọi hàm static khiến cho model của Laravel hoạt động hết sức tiện lợi. Các thao tác được gọi ngắn nhất có thể thông qua method chaining. Bạn không phải tạo ra một instance của class để gọi method, mà có thể gọi trực tiếp thông qua cách gọi static. Tôi thực sự rất thích cách tổ chức code này.

## Lời kết

Trên đây tôi đã giới thiệu cho các bạn về magic methods trong PHP và cách Laravel sử dụng nó trong kiến trúc Model của mình. Hy vọng bạn sẽ hiểu hơn về PHP cũng như hiểu hơn về Laravel, một framework mà việc đọc code khá phức tạp, qua đó sẽ cải thiện được công việc của mình khi phải đối mặt với các vấn đề phức tạp liên quan đến bản thân Framework.

Điều cuối cùng, đó là dù magic methods tuyệt vời như vậy, nhưng không phải ở đâu người ta cũng dùng nó. Bạn có thể thấy 1 framework lâu đời như Symfony không dùng magic methods mà lại viết tất cả getter setter ra. Tôi nghĩ không phải họ không thể viết được magic methods, mà vì họ không muốn viết. Thay vì tập trung đến tính dễ sử dụng khi có magic methods, họ tập trung vào hiệu suất của chương trình. Magic methods luôn chậm hơn method thông thường khi làm cùng một công việc, việc caching magic methods cũng khó khăn hơn so với các method thông thường khác, đó là lý do nó làm giảm tốc độ hệ thống. Thêm nữa, code trở nên không minh bạch và khó thu hút được các lập trình viên khó tính.

Laravel tập trung nâng cao trải nghiệm của người dùng, tính dễ sử dụng và khả năng tạo prototype nhanh, chính vì vậy nó sử dụng magic methods để thực hiện những điều mình muốn. Đổi lại, tốc độ Laravel giảm đáng kể và thuộc hàng khá chậm trong các Framework PHP (tham khảo tại [đây](http://blog.a-way-out.net/blog/2015/03/27/php-framework-benchmark/)). Tuy nhiên, với sự tiện lợi của mình và sự hỗ trợ của PHP7, HHVM trong việc cải thiện tốc độ của PHP, tôi tin rằng Laravel sẽ còn tiến xa hơn nữa và số lượng người sử dụng vẫn sẽ tiếp tục tăng.

## Tài liệu tham khảo

* [PHP Magic Methods](http://php.net/manual/en/language.oop5.magic.php)
* [What and Why - Query Builders and Eloquent](http://ryantablada.com/post/what-and-why-query-builders-and-eloquent)
