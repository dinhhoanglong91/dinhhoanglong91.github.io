---
title: "Chương 7: Thế Nào Là Một Thiết Kế Linh Hoạt?"
type: "book"
book: "agile_software_development"
book_title: "Agile Software Development"
show_top: 0
---

## Phần 2 - Thiết kế linh hoạt (Agile Design)

Nếu *sự mau lẹ* nói đến việc xây dựng phần mềm từ những sự cải tiến rất nhỏ, làm thế nào để chúng ta có thể *thiết kế* phần mềm? Làm cách nào chúng ta có thể dùng thời gian một cách hợp lý để có thể đảm bảo rằng phần mềm của chúng ta có kiến trúc mềm dẻo, dễ bảo trì và dễ tái sử dung? Nếu bạn đang cái tiển từng phần rất nhỏ, liệu bạn có thực sự tạo ra một trạng thái không tốt hay phải làm đi làm lại công đoạn tái kiến trúc (refactoring)? Liệu bạn có bỏ quên mục tiêu lớn của mình (big picture)?

Trong một nhóm phát triển linh hoạt, mục tiêu lớn của chúng ta phát triển cùng với phần mềm. Ở mỗi bước, nhóm phát triển cải thiện thiết kế của hệ thống để nó luôn có thể ở trạng thái tốt nhất như chính nó *bây giờ*. Nhóm phát triển không dành quá nhiều thời gian suy nghĩ về các yêu cầu trong tương lai, cũng như không xây dựng kiến trúc cho ngày hôm nay để đảm bảo cho tương lai mà họ nghĩ là cần có. Thay vào đó, họ tập trung vào kiến trúc hiện tại của hệ thống, làm cho nó tốt nhất có thể.

### Dấu hiệu của thiết kế tồi (poor design)

Làm cách nào để chúng ta biết được thiết kế phần mềm của mình là tốt? Chương đầu tiên của phần này sẽ liệt kê ra dấu hiệu của thiết kế tồi. Trong chương này, chúng tôi sẽ mô tả những dấu hiệu đó trong một dự án phần mềm và cách phòng tránh nó.

Các dấu hiệu đó bao gồm:

1. Tính cứng nhắc (`Rigidity`) - Thiết kế khó thay đổi.
2. Tính bất ổn, dễ vỡ (`Fragility`) - Thiết kế dễ bị phá vỡ.
3. Tính kém lưu động (`Immobility`) - Thiết kế khó tái sử dụng.
4. Tính nhớt (`Viscosity`) - Khó để làm những điều đúng.
5. Sự phức tạp một cách không cần thiết (`Needless Complexity`) - Thiết kế quá phức tạp.
6. Sự lặp đi lặp lại một cách không cần thiết (`Needless Repetetion`) - Lạm dụng chuột quá nhiều (Ý là dùng chuột copy paste quá nhiều @@).
7. Tính mờ đục (`Opacity`) - Sử lý ngoại lệ không được tổ chức rõ ràng.

Các dấu hiệu này giống với khái niệm về  `code có mùi` (code smells), nhưng ở một mức độ cao hơn. "Mùi" ở đây là về kiến trúc của cả hệ thống, không chỉ dừng lại ở những phần code nhỏ.

### Nguyên tắc (Principles)

Các chương còn lại của phần này sẽ mô tả nguyên tắc thiết kế trong lập trình hướng đối tượng, giúp các lập trình viên phòng trách các thiết kế tồi và đưa ra thiết kế tốt nhất cho các đặc trưng của hệ thống

Các nguyên tắc đó bao gồm:

1. SRP - Nguyên tắc về một trách nhiệm `The Single Responsibility Principle`
2. OCP - Nguyên tắc đóng mở `The Open-Closed Principle`
3. LSP - Nguyên tắc thay thế Liskov `The Liskov Substitution Principle`
4. DIP - Nguyên tắc đảo ngược sự phụ thuộc `The Dependency Inversion Principle`
5. ISP - Nguyên tắc tách biệt các lớp `The Interface Segregation Principle`

Những nguyên tắc này là thành quả khó khăn của hàng thập kỷ kinh nghiệm trong phát triển phần mềm. Đây không phải sản phẩm của một cá nhân, mà nó thể hiện sự hợp tác của rất nhiều kỹ sư phát triển phần mềm cũng như nhà nghiên cứu. Mặc dù họ mô tả chúng như là nguyên tắc của lập trình hướng đối tượng, chúng thực sự là một trường hợp đặc biệt của nguyên tắc trong ngành kỹ thuật phần mềm.

### Code có mùi và các nguyên tắc

Thiết kế có mùi là một dấu hiệu, nó có thể được đo đạt, chủ quan hoặc khách quan. Thông thường, mùi đó được gây ra do việc vi phạm một hay nhiều nguyên tắc. Ví dụ, mùi của sự cứng nhắc (Rigidity) thường là kết quả của cách tiếp cận thiếu triệt để với Nguyên tắc đóng mở (OCP).

Nhóm phát triển sự dụng các nguyên trắc để loại bỏ mùi đó. Họ không áp dụng nguyên tắc nếu không có mùi. Sẽ là sai lầm nếu cố gắng thỏa mãn các nguyên tắc một cách vô điều kiện, chỉ bởi vì nó là nguyên tắc. Nguyên tắc không phải là một loại nước hoa có thể tỏa hương trong khắp hệ thống của bạn. Việc áp dụng nguyên tắc một cách quá đáng sẽ dẫn đến mùi của `Sự phức tạp một cách không cần thiết`.

## Chương 7: Thế nào là một thiết kế linh hoạt?

> "Sau khi xem xét quy trình phát triển của nhiều phần mềm, tôi kết luận rằng, có duy nhất một tài liệu thực sự thỏa mãn yêu của của thiết kế kỹ thuật, đó là danh sách mà nguồn hệ thống " - Jack Reeves

Năm 1992, Jack Reeves viết một bài tương tự trên tạp trí về C++ (C++ Journal) với tiêu đề "Thế nào là thiết kế phần mềm?". Trong bài viết đó, Reeves cho rằng thiết kế của phần mềm ngày từ đầu đã chính là mà nguồn của nó. Các biểu đồ mô tả mã nguồn phụ thuộc vào thiết kế, và bản thân không phải là thiết kế. Điều này trở thành điềm báo trước của phát triển linh hoạt.

Trong các trang sách tiếp theo, chúng tôi sẽ nhắc đến "Thiết kế (The Design)". Bạn không nên suy nghĩ về nó như một tập các mô hình UML tách biệt khỏi code. Một tập các mô hình UML có thể mô tả một phần của thiết kế, nhưng nó không phải là thiết kế. Thiết kế của dự án phần mềm là một khái niệm trừu tượng. Nó cần hoạt động với kiến trúc chung của chương trình cũng như chi tiết của từng mô-đun, lớp, phương thức. Nó có thể được biểu diện bằng nhiều cách khác nhau, nhưng cuối cùng, nó hiện thân trong chính mã nguồn của chương trình. Cuối cùng, mã nguồn của chương trình chính là thiết kế.

### Điều gì đang diễn ra sai lầm với các phần mềm?

Nếu may mắn, bạn có thể khởi đầu dự án với một bức tranh rõ ràng về hệ thống tương lai của bạn. Thiết kế hệ thống sẽ là một bức tranh sống động trong đầu bạn. Nếu bạn vẫn còn gặp may, sự rõ ràng đó đưa đến bản phát hành đầu tiên.

Sau đó, một vài điều sai lầm diễn ra. Thiết kế của bạn bốc mùi như một miếng thịt hỏng. Theo thời gian, mùi cứ lan tỏa rộng ra và nhiều hơn. Hàng tá thứ xấu xí tiềm ẩn trong code của bạn, biết nó càng ngày càng khó bảo trì. Cuối cùng, bạn sẽ phải tốn rất nhiều sức lực chỉ để thay đổi một phần rất nhỏ trong hệ thống, điều đó khiến lập trình viên "khóc" trong việc tái thiết kế.

Những công việc tái thiết kế kiểu như vậy ít khi thành công. Mặc dù ban đầu người thiết kế bắt đầu với một suy nghĩ và khuynh hướng tốt, họ nhận ra hặn đang bắn vào chính mục tiêu của mình. Hệ thống cũ vẫn tiếp tục phát triển và thay đổi, thiết kế mới cần đuổi kịp. Mụn và loét tiềm tàng trong thiết kế mới trước khi nó kịp cho ra bản phát hành đầu tiên.

### Thiết kế có mùi - Mùi của một phần mềm mục nát

Bạn biết rằng phần mềm trở nên mục nát khi nó bắt đầu chưa đựng những mùi sau:

1. Tính cứng nhắc (`Rigidity`) - Hệ thống khó thay đổi, bởi mọi thay đổi buộc các phần khác phải thay đổi.
2. Tính bất ổn, dễ vỡ (`Fragility`) - Thay đổi đến hệ thống phá vỡ một phần khác, mà không có liên quan trực tiếp đến phần được thay đổi.
3. Tính kém lưu động (`Immobility`) - Khó để tách biệt hệ thống thành các thành phần khác nhau để có thể tái sử dụng trong hệ thống khác.
4. Tính nhớp (`Viscosity`) - Làm việc đúng còn khó hơn làm việc sai.
5. Sự phức tạp một cách không cần thiết (`Needless Complexity`) - Thiết kế bao gồm các thành phần không tạo ra lợi ích rõ ràng.
6. Sự lặp đi lặp lại một cách không cần thiết (`Needless Repetition`) - Thiết kế bao gồm kiến trúc lặp đi lặp lại mà có thể được thống nhất trong một lớp trừu tượng duy nhất.
7. Tính mờ (`Opacity`) - Khó đọc và hiểu. Không mô tả mục đích một cách rõ ràng.

**Tính cứng nhắc (`Rigidity`)**. Tính cứng nhắc là xu hướng của phần mềm khó thay đổi, cho dù là với cách đơn giản nhất. Một thiết kế là cứng nhắc, nếu một thay đổi nhỏ đòi hỏi thay đổi ở nhiều thành phần liên quan. Càng có nhiều mô-đun cần thay đổi, hệ thống càng cứng nhắc.

Hầu hết các lập trình viên gặp phải vấn đề này theo một hoặc nhiều cách khác nhau. Họ được yêu cầu tạo nên thay đổi đơn giản. Họ nhìn qua và đưa ra một ước tính thời gian hợp lý cho thay đổi đó. Nhưng sau đó, họ nhận ra còn rất nhiều phần cần thay đổi kéo theo mà họ không hề biết trước. Họ nhận ra mình cần thay đổi rất nhiều phần hơn là họ dự đoán ban đầu. Cuối cùng, thời gian cho việc thay đổi đó tốn quá nhiều so với ước tính của họ. Họ được hỏi tại sao ước tính lại sai lầm đến vậy, và sau đó, họ lặp đi lặp lại câu than thở truyền thống của lập trình viên "Nó phức tạp hơn nhiều so với tôi nghĩ!".

**Tính bất ổn, dễ vỡ (`Fragility`)**. Tính bất ổn, dễ vở là xu hướng của phần mềm dễ dàng bị phá vỡ ở nhiều thành phần khi một thay đổi nhỏ được tạo ra. Thông thường, vấn đề nảy sinh tại một khu vực mà không có liên hệ rõ ràng đến thành phần được thay đổi. Sửa các lỗi này dẫn đến nhiều lỗi hơn, và nhóm phát triển trở nên giống với con chó cố đuổi theo đuôi của mình.

Khi tính dễ vở của mô-đun tăng lên, các thay đổi sẽ dẫn đến những vấn đề không ngờ tới. Có vẻ vô lý, nhưng những mô-đun kiểu như vậy không hiếm. Đây là những mô-đun cần được sửa chữa, thứ mà chẳng bao giờ thoát khỏi danh sách lỗi, thứ mà lập trình viên cần tái cấu trúc (nhưng chẳng ai muốn đối mặt với ác mộng tái thiết kế chúng), thứ mà các tồi tệ nếu bạn càng cố sửa.

**Tính kém lưu động (`Immobility`)**. Một thiết kế kém lưu động là khó nó chưa đựng các thành phần cần thiết cho hệ thống, nhưng công sức và rũi ro sẽ xuất hiện nếu chúng ta muốn tách biệt các thành phần đó từ một hệ thống khổng lồ. Đây thực sự là một điều không may, nhưng rất phổ biến.

**Tính nhớp (`Viscosity`)**. Tính nhớp có 2 kiểu, nhớp của phần mềm và nhớp của môi trường.

Khi đối mặt với thay đổi, lập trình viên thường tìm ra nhiều cách để thực hiện điều đó. Một vài cách bảo tồn thiết kế của họ, một vài thì không (ví du, họ "hack"). Khi phương pháp bảo tồn thiết kế khó hơn là cách hack, tính nhớp của thiết kế ở mức cao. Nó quá dễ để làm những điều sai, nhưng khó để làm điều đúng. Chúng ta muốn thiết kế phần mềm mà những thay đổi phù hợp với thiết kế  được thực hiện một cách dễ dàng.

Tính nhớp của môi trường xảy ra khi môi trường phát triển chậm và thiếu hiệu quả. Ví dụ, khi thời gian biên dịch chậm, lập trình viên có xu hướng tạo ra những thay đổi mà không tốn quá nhiều công sức tái biên dịch (recompile), mặc dù những thay đổi đó không bảo tồn thiết kế. Nếu chương trình quản lý mã nguồn tốn hàng giờ chỉ để kiếm tra một vài tập tin, lập trình viên có xu hướng tạo ra các thay đổi tốn ít thời gian kiểm tra nhất có thể, không quan tâm có duy trì thiết kế hay không.

Trong cả hai trường hợp, một dự án nhớp là một dự án mà thiết kế khó có thể duy trì. Chúng ta cần tạo ra một hệ thống và môi trường phát triển mà thiết kế có thể dễ dàng được đảm bảo.

**Sự phức tạp một cách không cần thiết (`Needless Complexity`)**. Một thiết kế bao gồm những sự phức tạp không cần thiết khi nó chứa đựng các thành phần mà không được sử dụng ở thời điểm hiện tại. Điều này thường xảy ra khi lập trình viên biết trước thay đổi của hệ thống, bổ sung các thành phần lên phần mềm để đáp ứng những sự thay đổi tiềm tàng này. Ban đầu, suy nghĩ này có vẻ tốt. Sau cùng, việc chuẩn bị cho những thay đổi trong tương lai có thể giúp cho dự án được mềm dẻo và tránh được các sai lầm cho các thay đổi trong tương lai.

Tuy nhiên, không may là ảnh hưởng của cách làm này trở nên tiêu cực. Bằng việc chuẩn bị cho quá nhiều thành phần, thiết kế bị rải rác bởi những đoạn code không bao giờ được dùng đến. Một vài sự chuẩn bị sẽ được thanh toán hết, nhưng hầu hết là không. Trong khi đó thiết kế chưa đựng hàng tá thành phần vô dụng. Nó biến phần mềm trở nên phức tạp và khó hiểu.

**Sự lặp đi lặp lại một cách không cần thiết (`Needless Repetition`)**. Cắt/dán (Cut/paste) có thể thể một thao tác soạn thảo văn bản hữu dụng, nhưng nó có thể là một thao tác code thảm họa. Một cách rất phổ biến, hệ thống phần mềm được xây dựng trên hàng tá, thậm chí hàng trăm dòng code được lặp đi lặp lại. Nó xuất hiện như sau:

* Ralph cần viết code cho chức năng của mình. Anh ta nhìn xung quanh xem đâu đó có chức năng tương tự (hoặc giống hệt). Anh ta copy và dán phần code đó vào mô-đun của mình. Kết quả, anh ta tạo ra một thay đổi chấp nhận được.

* Ralph không biết rằng, code mà anh ta vừa đi cóp nhặt về được viết bởi Todd, người đã lấy nó từ mô-đun của Lilly. Lilly là người đầu tiên làm việc đó. Khi Lilly bắt đầu làm việc này, cô nhận ra là đã có người làm một việc tương tự với đối tượng khác, vì vậy cô chỉ cóp nhặt và tạo ra một chút thay đổi cho đối tượng của hành động đó.

Khi nhưng dòng code giống nhau lặp đi lặp lại, chỉ khác biệt chút ít, đó là khi lập trình viên quên mấp lớp trừu tượng. Tìm kiếm sự lặp đi lặp lại và loại trừ nó bằng một lớp trừu tượng có vẻ không ở mức độ ưu tiên cao trong công việc của họ, nhưng nó sẽ tạo ra một bước tiến xa cho việc phát triển một hệ thống đơn giản và dễ hiểu, dễ bảo trì.

Khi có những dòng code dư thừa trong hệ thống, việc thay đổi hệ thông có thể rất gian truân. Lỗi được phát hiện ở những thành phần lặp đi lặp lại đó dẫn đến công đoạn sửa lỗi lặp đi lặp lại. Tuy nhiên, do các thành phần có đôi chút khác biệt, nên việc sửa đổi không phải lúc nào cũng giống hệt nhau.

**Tính mờ đục (`Opacity`)** Tính mà là xu hướng của các mô-đun gây ra sự khó hiểu. Code được việc rõ ràng và dễ hiểu, hoặc có thể được viết mờ mịt, quấn lại với nhau. Code tốn nhiều thời gian càng có xu hướng trở nên mờ mịt theo thời gian. Một nỗ lực kiên định để đảm bảo code rõ ràng luôn cần thiết để đảm bảo tính mờ ở mức thấp nhất có thể.

Khi lập trình viên lần đầu viết một mô-đun, code có vẻ rõ ràng với họ. Đó là vì họ viết ra nó và họ hiểu nó ở mức ban đầu. Sau đó, họ dần quên nó đi, họ quay lại mô-đun đó và tự hỏi ai đã viết ra những dòng code thảm họa này. Để phòng tránh điều đó, lập trình viên lập trình viên cần đạt mình vào vị trí của người đọc, và đầu tư sức lực để có thể sửa những dòng code sao cho người đọc có thể hiểu được. Code của họ cũng cần được xem lại bởi những lập trình viên khác.

**Điều gì kích thích một phần mềm trở nên mục nát**

Trong môi trường không linh hoạt, thiết kế trở nên tồi tệ vì hệ thống thay đổi mà thiết kế không thể dự đoán trước được. Thông thường, thay đổi đó cần được thực hiện nhanh chóng, và thường được thực hiện bởi các lập trình viên không quen với triết lý thiết kế ban đầu. Từng lúc một, thay đổi cứ tiếp tục diễn ra, và sự vi phạ cứ tích trữ dần lên, thiết kế trở nên có mùi.

Tuy nhiên, chúng ta không thể đổi lỗi cho sự thay đổi yêu cầu dẫn đến sự xuống cấp của thiết kế. Chúng ta, những lập trình viên phần mềm, biết rõ rằng yêu cầu sẽ thay đổi. Thật sự, hầu hết chúng ta nhận ra rằng yêu cầu là thứ không ổn định nhất trong dự án. Nếu thiết kế của chúng ta thất bại bởi sự thay đổi không ngừng của yêu cầu, đó là lỗi của thiết kế và cách làm của chúng ta. Chúng ta cần tìm cách để thiết kế của mình luôn kiên định với thay đổi, áp dụng những kỹ thuật tốt để bảo vệ thiết kế của mình

**Nhóm phát triển phần mềm linh hoạt không cho phép phần mềm trở nên mục nát**

Nhóm phát triển phần mềm linh hoạt phát triển tốt trên những thay đổi. Họ đầu tư nhiều hơn, do đó họ không bị bó buộc bởi thiết kế ban đầu. Thay vào đó, họ duy trì thiết kế hệ thống đơn giản và rõ ràng nhất có thể, bảo vệ nó bởi những bài kiểm thử đơn vị và kiểm thử chấp nhận. Họ duy trì thiết kế đoan giản và dễ thay đổi. Họ sử dụng sự mềm dẻo đó để liên tục cải thiện thiết kế, do vậy họ luôn đưa ra được sản phẩm thõa mãn thiết kế cũng như yêu cầu khách hàng.

### Chương trình `Copy`

Xem xét một thiết kế mục nát có thể mô phỏng những quan điểm nêu ở trên. Hãy nghĩ rằng ông chủ của bạn đến với bạn vào sáng sớm thứ Hai, và yêu cầu bạn viết một chương trình copy ký tự từ bàn phím ra máy in. Suy nghĩ một lát, bạn đưa ra kết luận rằng sẽ không tốn đến 10 dòng code. Thiết kế và code sẽ không mất đến 1 giờ. Kết hợp với các buổi họp chức năng trong nhóm, họp về chất lượng đào tạo, họp hàng ngày của nhóm, và vấn đề của ba khủng hoảng đang diễn ra, chương trình này có thể tiêu tốn của bạn một tuần-nếu bạn làm việc thêm giờ. Tuy nhiên, bạn nhân nó lên ba.

"Ba tuần", bạn nói với sếp của mình như vậy. Anh ta đồng ý, quay về chỗ và để bạn lại với công việc của mình.

**Thiết kế đầu tiên**. Bạn có thời gian trước khi buổi họp xem lại quá trình bắt đầu, bạn quyết định thiết kế cho chương trình của mình. Sử dụng mô hình kến trúc, bạn đưa ra kết quả như hình 7-1.

![Agile Design](/assets/img/post/agile/fig_7_1.png)


Sẽ có 3 mô-đun trong ứng dụng của bạn. Mô-đun `Copy` gọi hai mô-đun còn lại. Chương trình `Copy` duyệt các ký tự nhân được từ mô-đun `Read Keyboard` và đưa chúng ra cho mô-đun `Write Printer`.

Bạn nhìn lại và thấy thiết kế của mình tốt. Vui vẻ, bạn rời văn phòng để đi một buổi họp khác. Bạn có thể ngủ lúc này.

Ngày thứ Ba, bạn đến sớm một chút và kết thúc chương trình `Copy`. Không may là một trong những khủng hoảng đã xảy ra vào tối qua, bạn cần đến phòng nghiên cứu để giải quyết nó. Giờ nghỉ trưa kết thúc lúc 3 giờ chiều, và bạn cố gắng viết code cho chương trình `Copy`. Kết quả như sau.

{% highlight c %}
// Chương trình Copy
void Copy()
{
  int c;
  while ((c=RdKbd()) != EOF)
    WrtPrt(c);
}
{% endhighlight %}

Bạn cố gắng lưu lại chương trình, khi bạn nhận ra là mình đã bị muộn cho cuộc họp chất lượng. Bạn biết đó là một cuộc họp quan trọng, khi mà người ta sẽ nói về mức độ của các khuyết điểm. Bạn dừng code lại và tham gia buổi họp.

Thứ Tư, bạn lại đến sớm và có vẻ không có vấn đề gì. Bạn chạy thử chương trình `Copy` của mình, và không có lỗi nào! Điều đó thật tốt, bởi sếp của bạn gọi bạn vào một cuộc họp không được lên lịch trước về việc duy trì máy in laze.

Thứ Năm, sau khi dành bốn giờ trao đổi điện thoại với nhân viên kỹ thuật ở Rocky Mount, Nam California, về việc sửa lỗi hệ thống, bạn quay lại kiểm tra chương trình `Copy`. Nó hoạt động tốt! Thật tót, bởi một sinh viên đã xóa code trong thư mục trên server và bạn phải tìm bản dự phòng cuối cùng cho nó. Bản dự phòng cuối cùng được tạo ra ba tháng trước, và bạn có đến 94 phần bổ sung cho nó.

Thứ Sáu, một ngày không có lịch gì hết cả. Thật tốt, bởi bạn mất cả ngày để đưa chương trình `Copy` của mình vào hệ quản trị mã nguồn.

Đương nhiên là chương trình của bạn thành công, và được triển khai trong công ty bạn. Danh tiếng của một lập trình viên có kinh nghiệm của bạn lại được đảm bảo thêm, và bạn thoải mái với thành quả của mình. Với sự may mắn của mình, bạn mới chỉ viết ra 30 dòng code cho năm nay.

**Yêu cầu đang thay đổi** Vài tháng sau, ông chủ của bạn đến và nói với bạn rằng đôi khi họ muốn đọc thông tin từ các băng giấy. Bạn suy nghĩ một chút. Bạn tự hỏi tại sao con người liên tục thay đổi yêu cầu. Chương trình của bạn không được thiết kế cho máy đọc băng giấy! Bạn cảnh báo sếp của mình rằng thay đổi đó có thể phá huỷ thiết kế của bạn. Mặc dù vậy, sếp của bạn khá cương quyết. Anh ta nói rằng người dùng thực sự muốn đọc từ máy đọc băng giấy.

Bạn đồng ý và chuẩn bị cho thay đổi. Bạn cần thêm một biến boolean (biến chỉ nhận giá trị đúng hoặc sai) cho chương trình `Copy` của mình. Nếu nó đúng, bạn sẽ đọc từ máy đọc băng giấy, nếu sai, bạn sẽ đọc từ bàn phìm như trước. Thật không may, có quá nhiều chương trình đang sử dụng chương trình `Copy` của bạn, do vậy bạn không thể thay đổi giao diện của chương trình này. Thay đổi giao diện có thể dẫn đến hàng tuần để biên dịch lại và kiểm tra lại. Kỹ sư kiểm tra hệ thống có thể buộc tội bạn, không quan tam đến bảy anh chàng khác trong nhóm quản lý các thiết lập. Nhóm quản lý tiến trình có thể bắt buộc kiểm tra lại tất cả các đoạn code có sử dụng chương trình `Copy`!

Không, thay đổi giao diện cần phải bỏ qua. Vậy thì, làm cách nào để chương trình `Copy` có thể đọc từ máy đọc băng giấy? Bạn sẽ dùng biến toàn cục. Bạn cũng có thể dùng một trong những đặc trưng hữu ích nhất của `C`, phép toán `?:`! Kết quả như sau:

{% highlight c %}
// Phiên bản sửa đổi đầu tiên của chương trình Copy
bool ptFlag = false;
void Copy()
{
  int c;
  while ((c=(ptFlag ? RdPt() : RdKbd())) != EOF)
    WrtPrt(c);
}
{% endhighlight %}

Những người gọi đến chương trình `Copy` mà muốn đọc từ máy đọc băng giấy cần thiết lập biến `ptFlag` có giá trị đúng (`true`). Khi đó họ có thể gọi chương trình `Copy` và hạnh phúc khi nó có thể đọc từ băng giấy. Khi chương trình `Copy` kết thúc, họ cần khởi tạo lại biến `ptFlag`, nếu không, những ai gọi chương trình này sau sẽ đọc từ máy đọc băng giấy chứ không phải bàn phím. Để nhắc nhở lập trình viên làm việc này, bạn có thể thêm một vài chú thích hợp lý.

Một lần nữa, bạn phát hành phần mềm của mình đến với sự hoan hô nồng nhiệt từ mọi người. Nó thành công hơn nhiều so với phiên bản trước đó, và các lập trình viên khác mong chờ cơ hội được dùng nó. Cuộc sống thật đẹp!

**Cho họ thêm một chút...** Một vài tuần sau, ông chủ của bạn (người vẫn làm chủ của bạn mặc dù có tới ba vụ tái cơ cấu lớn trong có vài tháng), hỏi bạn rằng đôi khi khách hàng muốn chương trình `Copy` có đầu ra là máy đục lỗ băng giấy.

Khách hàng! Họ luốn phá hủy thiết kế. `Viết phấn mềm sẽ đơn giản hơn rất nhiều nếu không có khách hàng` ( =)) )

Bản nói với ông chủ của bạn rằng thay đổi đó có thể ảnh hưởng tiêu cực đến thiết kế của bạn. Bạn cảnh báo anh ta rằng nếu thay đổi cứ diễn ra một khách khó chịu như thế này, sản phẩm sẽ không thể bảo trì được trước khi kết thúc năm. Nhưng sếp của bạn vẫn như cũ, yêu cần bạn thay đổi bằng mọi cách.

Thiết kế thay đổi tương tự như lần trước. Điều chúng ta cần chỉ là dùng biến toàn cục và phép toán `?:`. Kết quả như sau:

{% highlight c %}
bool ptFlag = false;
bool punchFlag = false;
// Nhớ khởi tạo lại các biến này
voi Copy()
{
  int c;
  while ((c=(ptFlag ? RdPt() : RdKbd())) != EOF)
    punchFlag ? WrtPunch(c) : WrtPrt(c);
}
{% endhighlight %}

Bạn thực sự tự hào vì những gì bàn làm và việc không quên ghi chú vào code. Bạn vẫn lo lắng rằng kiến trúc của chương trình có thể bị lật đổ. Bất kỳ thay đổi nào lúc này cũng buộc bạn phải tái cấu trúc hoàn toàn vòn `while` của mình. Có lẽ, đó là lúc CV của bạn dính bụi...

**Mong chờ thay đổi** Tôi sẽ để phần còn lại cho bạn để xác định sự châm biếm đến mức nào. Mục đích của câu chuyện là cho bạn thấy làm cách nào mà thiết kế của bạn liên tục suy thoái khi có sự thay đổi. Thiết kế ban đầu của chương trình đơn giản và tao nhã. Vậy mà chỉ sau có hai thay đổi, nó bắt đầu cho thấy dấu hiệu của `Tính cứng nhắc`, `Tính mỏng manh, dễ vỡ`, `Tính kém lưu động`, `Sự phức tạp`, `Sự lặp đi lặp lại`, `Tính mờ đục`. Xu hướng này sẽ còn tiếp diễn, và chương trình trẻ thành một mớ hỗn độn.

Bạn có thể ngồi lại và đổ lỗi cho sự thay đổi. Bạn có thể than phiền rằng thiết kế đã tốt cho yêu cầu ban đầu, nhưng các yêu cầu sau đó gây ra sự suy thoái của thiết kế. Tuy nhiên, bạn đã quên mất điều quan trọng nhất trong phát triển phần mềm: *Yêu cầu luôn luôn thay đổi*!

Nhớ rằng, điều không ổn định nhất trong hầu hết các dự án phần mềm là yêu cầu. Yêu cầu luôn ở trong trạng thái thay đổi. Đó là điều mà chúng ta, những lập trình viên, phải chấp nhận! *Chúng ta sống trong một thế giới của sự thay đổi yêu cầu, và công việc của chúng ta là đảm bảo rằng phần mềm vẫn sống sót với những thay đổi đó*. Nếu thiết kế của chúng ta suy yếu vì thay đổi của các yêu cầu, chúng ta đã không linh hoạt.

**Thiết kế linh hoạt của chương trình `Copy`**

Cách phát triển linh hoạt có thể bắt đầu giống hệt với phiên bản đầu tiên của chương trình `Copy` trên. Nhưng khi ông chủ yêu cầu nhóm phát triển đọc từ máy đọc băng giấy, họ cần đáp ứng bằng cách thay đổi thiết kế cho phù hợp với loại thay đổi này. Kết quả có thể như sau:

{% highlight c++ %}
// Giải pháp linh hoạt cho phiên bản số 2 của `Copy`
class Reader
{
  public:
  virtual int read() = 0;
};

class KeyboardReader : public Reader
{
  public:
    virtual int read() {return Rdkbd();}
};

KeyboardReader GdefaultReader;

void Copy(Reader& reader = GdefaultReader)
{
  int c;
  while ((c=reader.read()) == EOF)
    WrtPrt(c);
}
{% endhighlight %}

Thay ví vá cho thiết kế của mình để đáp ứng yêu cầu mới, nhóm phát triển coi đó là cơ hội để cải thiện thiết kế để đáp ứng với thay đổi trong tương lai. Từ nay, mỗi khi ông chủ của bạn yêu cầu thêm một thiết vị đầu vào, nhóm có thể đáp ứng ngay lập tức mà không gây ra sự hư hại nào đến thiết kế của mình.

Nhóm đã tuân thủ `Nguyên tắc đóng mở(OCP)`, sẽ được đề cập đến ở chương 9. Nguyên tắc định hướng chúng ta thiết kế các mô-đun mà có thể mở rộng kể khi không có sự thay đổi. Đó là những gì chúng ta đã làm. Khi bất cứ thiết bị nào mà ông chủ của bạn yêu cầu thêm, bạn không cần thay đổi chương trình `Copy`.

Tuy nhiên, mặc dù vậy, nhóm của bản không cố dự đoán cách mà chương trình có thể thay đổi trong thiết kế đầu tiên của mình. Thay vào đó, họ viết chương trình bằng cách đơn giản nhất có thể. Chỉ khi yêu cầu thay đổi, họ mới thay đổi thiết kế để đáp ứng loại thay đổi đó.

Bạn có thể phản đối rằng họ mới chỉ làm một nửa công việc. Khi họ đang bảo vệ mình trước những đầu vào khác nhau, họ cũng có thể bảo vệ mình trước những đầu ra khác nhau. Tuy nhiên, họ không có bất kỳ ý niệm nào về việc đầu ra có thể thay đổi. Bổ sung thêm hàng rào bảo vệ lúc này có thể dẫn đến những mục đích không thiết thực. Thật rõ ràng là nếu sự bảo vệ này là cần thiết, nó có thể bổ sung đơn giản sau này. Do vậy, không có lý do gì để thêm nó lúc này.

**Làm cách nào một lập trình viên linh hoạt biết điều mình cần làm**

Lập trình viên linh hoạt nêu ra ở trên đã tạo ra một lớp trừu tượng nhằm bảo vệ họ trước những thay đổi của thiết bị đầu vào. Làm cách nào họ biết làm điều đó? Có một vài điều cần làm với triết lý của thiết kế hướng đối tượng.

Thiết kế đầu tiên của chương trình `Copy` là mềm dẻo, bởi luồng của các thành phần liên quan. Khi bạn nhìn vào phiên bản đầu tiên của chương trình `Copy`, bạn nhận ra rằng mô-đun `Copy` phục thuộc trực tiếp vào `KeyboardReader` và `PrinterWriter`. Mô-đun `Copy` ở mức cao trong ứng dụng này. Nó định nghĩa cách hoạt động của chương trình. Nó biết làm cách nào để copy ký tự. Không may là, nó được tạo ra để phục thuộc vào chi tiết của lớp ở mức thấp là bàn phìm và máy in. Do vậy, khi chi tiết của các lớp ở mức thấp thay đổi, nó sẽ ảnh hưởng đến cách hoạt động của lớp ở mức cao.

Một khi sự thiếu mềm dẻo bị lộ ra, lập trình viên linh hoạt cần biết rằng sự phục thuộc của mô-đun `Copy` đến thiết bị đầu vào cần được `đảo ngược(inverted)`, do đó lớp `Copy` không cần phải phục thuộc vào thiết bị đầu vào. Khi đó, họ sử dung kiểu mẫu chiến lược `Strategy Pattern` (từ sau sẽ không dịch từ này :( ) để tạo ra một sự đảo ngược hợp lý.

Do vậy, nói ngắn gọn, lập trình viên linh hoạt biết điều phải làm bởi vì

1. Họ phát hiện ra vấn đề bằng cách tuần theo những kỹ thuật phát triển linh hoạt;
2. Họ phân tích vấn đề bằng cách áp dụng nguyên tắc thiết kế của phát triển phần mềm linh hoạt; và
3. Họ giải quyết vấn đề bằng cách sử dụng các thiết kế mẫu.

Tác động qua lại lẫn nhau giữa ba phạm trù này trong phát triển phần mềm là nghệ thuật của thiết kế.

### Giữ vững thiết kế tốt nhất có thể

Lập trình viên linh hoạt có vai trò giữ thiết kế luôn hợp lý và rõ ràng nhất có thể. Đó không phải là một vai trò ngẫu nhiên. Lập trình viên linh hoạt không "xóa sạch" thiết kế trong một vài tuần. Thay vào đó, họ duy trì phần mềm rõ ràng, đơn giản, dễ hiểu nhất có thể, hàng ngày, hàng giờ thậm chí hàng phút. Họ không bao giờ nói, "Chúng tôi sẽ quay lại và sửa nó sau". Họ không bao giờ để cho sự mục nát bắt đầu.

Thái độ của lập trình viên linh hoạt đối với thiết kế của phần mềm giống như thái độ của bác sỹ phẫu thuật với công việc khử trùng. Công việc khử trung làm cho ca phẫu thuật hoạt động. Không có nó, nguy cơ viêm nhiễm trở nên rất cao và không thể chấp nhận. Lập trình viên cảm giác tương tự với thiết kế của mình. Nguy cơ của việc để xảy ra dù chỉ là một sự mục nát nhỏ cũng quá khó để chấp nhận.

Thiết kế cần được duy trì rõ ràng, bởi mà nguồn là thành phần quan trọng nhất mô tả thiết kế, nó cần được duy trì sạch! Các chuyên gia bắt buộc rằng, chúng ta, những lập trình viên phần mềm, không được chấp nhận sự mục nát trong code.

### Kết luận

Vậy, thế nào là thiết kế linh hoạt? Thiết kế linh hoạt là quá trình, không phải sự kiện. Nó là một sự ứng dụng liên tục của các nguyên tác, mẫu thiết kế, kinh nghiệm để cái thiện kiến trúc và tính dễ đọc của phần mềm. Đó là điều bắt buộc để duy trì thiết kế của hệ thống đơn giản, rõ ràng và dễ hiểu trong mọi thời điểm.

Trong các chương tiếp theo, chúng ta sẽ khám phá các nguyên tắc và mẫu thiết kế của thiết kế phần mềm. Khi đọc nó, nhớ rằng một lập trình viên linh hoạt không ứng dụng tất cả các nguyên tắc này để có một thiết kế khổng lồ. Thay vào đó, họ áp dụng cách tiếp cận này theo các chu trình, để giữ code và thiết kế của mình đơn giản, rõ ràng.

## Quan điểm cá nhân

Bản thân tôi đã từng đọc tới 7 dấu hiệu của thiết kế tồi trong quá khứ thông qua một vài blog các nhân. Nhưng đây là lần đầu tiên đọc sách nguyên gốc về khái niệm này. Rất ấn tượng và không hề cường điệu một chút nào khi bản thân tôi đã phải đối mặt với hầu hết 7 vấn đề ở đây.

Trong phần giải pháp sử dụng thiết kế linh hoạt, Nguyên tắc đóng mở (`Open-Closed Principle`) và `Strategy Pattern` được đề cập nhen nhóm đưa đến giải pháp đơn giản nhưng tính tế cho vấn đề của thiết kế tồi. Tôi sẽ nhanh chóng dịch đến hai phần này để có thể hiểu hơn về thiết kế linh hoạt `Agile Design`.  :sure:

[Back to chapter list](/books/agile-software-development)
