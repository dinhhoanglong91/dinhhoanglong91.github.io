---
title: "Chương 1: Agile Practices"
type: "book"
book: "agile_software_development"
---

Tôi là một lập trình viên PHP. Sau 4 năm đại học với một năm vừa học vừa làm, tôi nghĩ mình đã trang bị đầy đủ kiến thức để có thể đi làm. Một năm đầu ở công ty của tôi khá đơn giản, tôi không gặp khó khăn để hoàn thành công việc của mình. Tôi có thời gian để tìm hiểu thêm nhiều công nghệ mới. Tuy nhiên, gần đây tôi được làm việc với một kỹ sư hàng đầu, người đang làm việc cho 1 dự án mã nguồn mở. Tôi nhận ra mình còn thiếu sót rất nhiều để có thể trở thành một kỹ sư chuyên nghiệp. Có vẻ nếu tiếp tục dành một hai tuần để học một framework, một thư viện hay một công cụ mới, làm một dự án con con chạy được, sẽ chỉ biến tôi thành một "thợ code" hơn là một "kỹ sư".

Và tôi nghĩ đến việc đọc sách. Có một số cuốn sách khá hay về lập trình mà tôi có nhưng đã bỏ quên không đụng đến, chủ yếu là vì "lười". Do vậy tôi chọn cách dịch sách để có thể "đọc sách" một cách tử tế (không ngủ gật), và qua đó chia sẻ với mọi người những gì mình đọc được. Đầu tiên sẽ là một cuốn sách về "agile". Trong đó có những nguyên tắc cơ bản về phát triển phần mềm linh hoạt (agile software development), và tập quy tắc, mẫu thiết kế (design pattern) cho lập trình hướng đối tượng. Những thứ tôi cho là yêu cầu tối thiểu để trở thành một "kỹ sư phần mềm".

Cuối mỗi bài viết sẽ là quan điểm cá nhân tôi về những gì mình đọc được. Có thể là rất chủ quan, nhưng rất mong được bạn đọc ủng hộ (bow).

# *Lời nói đầu*

Chắc hẳn trong chúng ta đã ít nhiều nghe đến những khái niệm như "Agile", "Scrum", "Extreme Programming", đó là những khái niệm rất phổ biến trong giới phát triển phần mềm, đã và đang được ứng dụng trong nhiều dự án của nhiều công ty. Loạt bài viết sau đây sẽ dịch từ cuốn [Agile Software Development, Principles, Patterns, and Practices](http://www.amazon.com/Software-Development-Principles-Patterns-Practices/dp/0135974445) của tác giả [Robert C. Martin](http://www.objectmentor.com/omTeam/martin_r.html) , với mong muốn mang đến cho bạn đọc cái nhìn rõ ràng về phát triển phần mềm linh hoạt (agile software development) cũng như những kỹ thuật tiên tiến nhất (best practices) cho việc phát triển phần mềm theo hướng linh hoạt.

# Chương 1. Agile Practices

Rất nhiều người trong chúng ta đã phải trải qua ác mộng khi làm việc với 1 dự án mà không dựa trên những kỹ thuật tốt nhất. Việc thiếu những kỹ thuật tốt để tuân theo dẫn đến những lỗi lặp đi lặp lại trong dự án, cũng như sự hao tốn năng lực con người. Khách hàng thất vọng bởi lịch phát hành liên tục thay đổi, vốn đầu tư ngày càng tăng nhưng chất lượng thì ngày cảng giảm. Lập trình viên cảm thấy chán nản bởi phải làm việc nhiều giờ hơn nhưng chỉ đem đến sản phẩm với chất lượng thấp hơn.

Một khi chúng ta phải trải qua sự thất bại đó, chúng ta bắt đầu sợ hãi việc phải lặp đi lặp lại những sai lầm mình đã làm. Nỗi sợ hãi đưa chúng ta đến việc hình thành 1 "quy trình", ở đó chúng ta sẽ giới hạn những gì mình làm và chỉ đòi hỏi những gì thực sự cần thiết. Chúng ta tạo ra tập những việc cần làm dựa theo kinh nghiệm từ các dự án đã qua, chọn ra những thứ đã hoạt động tốt ở dự án trước. Chúng ta hy vọng nó sẽ hoạt động tốt 1 lần nữa và nỗi sợ hãi sẽ biến mất.

Tuy nhiên, các dự án thì không bao giờ đơn giản, 1 vài luật/quy định không bao giờ đảm bảo sẽ không có lỗi. Và theo đó lỗi sẽ tiếp tục xảy ra, chúng ta lại phân tích lỗi, và tiếp tục bổ sung thêm luật để đảm bảo sẽ không có lỗi trong tương lai. Sau rất nhiều dự án, chúng ta nhận ra mình bị ngập lụt trong hàng tá luật lệ, hàng tá rào cản, những thứ cản trở chúng ta hoàn thành bất cứ việc gì.

Một quy trình vướng víu có thể gây ra vấn đề mà chính nó đã cố gắng ngăn chặn. Nó có thể làm chậm nhóm phát triển và tiêu tốn vốn đầu tư. Nó còn làm giảm khả năng của nhóm phát triển, khi mà họ sẽ chỉ luôn tạo ra sản phẩm không hoàn chỉnh. Không may là, quy trình đó làm cho nhóm phát triển tin rằng họ vẫn chưa làm đủ "quy trình". Và do đó, họ sẽ chỉ làm cho "quy trình" của mình trở nên phức tạp hơn. Chúng ta gọi đó là sự lạm phát về quy trình (runaway-process inflation).

Sự lam phát về quy trình là khái niệm hợp lý dùng để miêu tả nỗi lo ngại ở rất nhiều công ty phát triển phần mềm trong những năm 2000. Mặc dù có rất nhiều nhóm làm việc không theo 1 quy trình nào hết, nhưng việc áp dụng các quy trình nặng nề vẫn tiếp tục diễn ra nhanh chóng, đặc biệt ở các tập đoàn lớn.

## Agile Alliance

Đầu năm 2001, nhận thấy có nhiều nhóm phát triển trong các tập đoàn lớn gặp vấn đề với những quy trình nặng nề, 1 nhóm các chuyên gia đã họp lại và đưa ra các nguyên tắc để đảm bảo các nhóm phát triển sẽ làm việc nhanh chóng và thích ứng với các thay đổi từ phía người sử dụng. Họ gọi mình là "Agile alliance" (Liên minh phát triển phần mềm linh hoạt). 7 tháng sau, họ đưa ra tuyên ngôn cho phát triển phần mềm linh hoạt. Họ gọi đó là "The Manifesto of the Agile Alliance" (Tuyên ngôn của liên minh phát triển phần mềm linh hoạt)

### The Manifesto of the Agile Alliance

> Chúng tôi đưa ra những cách làm việc tốt hơn cho phát triển phần mềm bằng cách thực hiện nó và giúp những người khác làm việc. Qua quá trình đó, chúng tôi đưa đến thống nhất:
>
> * Đặt các cá nhân và sự tương tác lên trên tiến trình và công cụ
> * Đặt phần mềm đang hoạt động lên trên tài liệu đầy đủ
> * Đặt sự hợp tác với khách hàng lên trên hợp đồng đã được đàm phán
> * Đặt sự thích ứng với thay đổi 1 cách kịp thời lên trên việc tuân theo kế hoạch đã định ra từ đầu
>
> Trong đó, chúng ta đặt giá trị của những gì đưa ra bên trái cao hơn bên phải.


### Đặt các cá nhân và sự tương tác lên trên tiến trình và công cụ

Con người là thành phần quan trọng nhất tạo nên thành công. Một quy trình tốt không bao giờ đảm báo là dự án sẽ không thất bại, nếu trong đội ngũ phát triển không có ai đủ giỏi. Nhưng một quy trình tồi tệ có thể biến cá nhân giỏi giang nhất trở thành 1 người vô ích. Kể cả 1 nhóm của những người giỏi nhất cũng có thể thất bại nếu như họ không biết hợp tác với nhau.

Một nhân viên giỏi không cần thiết là một lập trình viên thực sự xuất sắc. Một nhân viên giỏi có thể là một lập trình viên trung bình, nhưng anh ta biết cách để hợp tác với những người khác. Hợp tác với người khác, giao tiếp và tương tác quan trọng hơn rất nhiều hơn một tài năng lập trình đơn thuần. Một nhóm của những lập trình viên trung bình biết cách hợp tác với nhau sẽ có cơ hội thành công lớn hơn nhiều mốt nhóm của những lập trình viên xuất chúng nhưng không biết hợp tác với nhau.

Một công cụ hữu ích có thể rất quan trọng đối với sự thành công. Trình biên dịch (compilers), công cụ phát triển phần mềm (IDEs), hệ thống quản lý mã nguồn (source-code control systems),... là những thành phần thiết yếu đối với lập trình viên. Tuy nhiên, các công cụ có thể bị đánh gía quá mức cần thiết. Một công cụ quá phức tạp nhưng chậm chạp cũng tồi tệ như sự thiếu công cụ vậy.

Lời khuyên của tối là hãy khởi đầu bằng một công cụ nhỏ thôi. Đừng giả sử bạn cần nhiều hơn phạm vi của công cụ đó cho đến khi bạn đã thử và nhận thấy không thể sử dụng nó. Thay vì mua một hệ thống quản lý mã nguồn đắt đỏ, hãy thử sử dụng một công cụ miễn phí cho đến khi bạn nhận ra nó không còn đáp ứng được yêu cầu của bạn. Thay vì mua một công cụ tốm kém cho việc quản lý mã nguồn của mình, bạn hãy thử một phần mềm miễn phí, dùng nó cho đến khi bạn nhận ra bạn cần một thứ lớn hơn. Trước khi mua giấy phép cho công cụ quản lý tốt nhất, hãy thử dùng bảng trắng và giấy cho đến khi bạn thực sự cần nhiều hơn thế. Trước khi quyết định dùng một hệ quản trị cơ sở dữ liệu khổng lồ, hãy dùng những file đơn giản. Đừng nghĩ rằng một công cụ lớn hơn và tốt hơn sẽ tự động giúp bạn làm việc tốt hơn. Thông thường, nó cản trở bạn nhiều hơn những gì nó có thể giúp đỡ.

Hãy nhớ rằng, xây dựng 1 nhóm làm việc quan trọng hơn nhiều là xây dựng môi trường làm việc. Rất nhiều nhóm cũng như nhà quản lý sai lầm khi xây dựng môi trường làm việc trước, và mong chờ nhóm sẽ tự động làm tốt. Ngược lại, trước tiên hãy tạo dựng 1 nhóm, và hãy để họ tự tùy chỉnh môi trường làm việc theo nhu cầu của họ.

### Đặt phần mềm đang hoạt động lên trên tài liệu đầy đủ

Phần mềm mà không có tài liệu thực sự là thảm họa. Những dòng code không bao giờ là phương tiện lý tưởng cho giao tiếp và chia sẻ kiến trúc của hệ thống. Thay vào đó, nhóm phát triển cần cung cấp tài liệu đầy đủ để ai cũng có thể đọc được, trong đó mô tả đầy đủ hệ thống cũng như lý do đưa đến quyết định thiết kế của họ.

Tuy nhiên, quá nhiều tài liệu còn tồi tệ hơn ít tài liệu. Một tài liệu khổng lồ  tốn rất nhiều thời gian để viết lên cũng như cần rất nhiều thời gian để nó luôn phản ánh đúng những dòng code. Nếu không được cập nhật với những dòng code, chúng biến thành những lời nói dối phức tạp, nguồn tài liệu sai lệch nghiêm trọng với hướng phát triển.

Ý tưởng viết và cập nhật tài liệu cho hệ thống là rất tốt, tuy nhiên tài liệu đó cần "ngắn gọn" và "nổi bật". "Ngắn gọn", nghĩa là nó chỉ tầm một đến hai tá trang giấy là đủ. "Nổi bật", nghĩa là nó phải nêu được thiết kế tổng quan của hệ thống, và chỉ thiết kế ở mức cao nhất.

Nếu chúng ta có một tài liệu ngắn gọn và có tổ chức, làm cách nào chúng ta hướng dẫn các thành viên mới làm việc với hệ thống của mình? Để trả lời câu hỏi đó, chúng ta cần làm việc thật gần gũi với họ. Chúng ta truyền đạt những gì mình biết cho họ bằng cách ngồi cạnh và giúp đỡ họ. Chúng ta biến họ thành thành viên của nhóm bằng quá trình đào tạo chặt chẽ thông qua sự tương tác lẫn nhau.

Hai tài liệu quan trọng nhất để truyền đạt thông tin đến các thành viên mới là những dòng code và nhóm phát triển. Code sẽ không bao giờ nói dối về những gì nó làm. Nắm bắt thông tin về hệ thống từ những dòng code không bao giờ đơn giản, nhưng code là nguồn thông tin duy nhất rõ ràng, không mơ hồ. Các thành viên trong nhóm phát triển sẽ nắm trong đầu cả quá trình phát triển với đầy dẫy thay đổi của hệ thống. Không có cách nào truyền đạt con đường với đầy thay đổi đó đến người khác nhanh và chính xác hơn là thông qua sự tương tác giữa người với người.

Rất nhiều nhóm đề cao tài liệu hơn là phần mềm của mình. Đây thực sự là một sai lầm chết người. Có một luật đơn giản được gọi là "Martin's first law of documentation" (luật đầu tiền về tài liệu của Martin) ngăn chặn suy nghĩ đó:

> Không cần viết tài liệu nếu nó không quá quan trọng và chúng ta không cần nó ngay lập tức

### Đặt sự hợp tác với khách hàng lên trên hợp đồng đã được đàm phán

Phần mềm không bao giờ được đặt hàng như các hàng hóa thông thường. Bạn không thể viết yêu cầu cho phần mềm bạn muốn, rồi mong chờ các người khác sẽ phát triển nó trong một khoảng thời gian nhất định với giá cả cố định. Theo thời gian, suy nghĩ đó sẽ trở nên sai lầm, và đôi khi sai lầm 1 cách ngoạn mục.

Suy nghĩ đó dẫn đến một hoàn cảnh phổ biến, rằng quản lý của một công ty nói với nhân viên những gì anh ta cần, và mong chờ những nhân viên sẽ ra ngoài một lúc rồi quay lại với hệ thống mà anh ta cần. Tuy nhiên, cách quản lý như vậy chỉ dẫn tới chất lượng thấp và thất bại.

Dự án thành công đòi hỏi ý kiến nhận xét của khách hàng một cách thường xuyên. Thay vì tuân theo hợp đồng hay những mệnh lệnh trong công việc, khách hàng của một phần mềm cần kết hợp chặt chẽ với nhóm phát triển, đưa đến cho họ những nhận xét thường xuyên, liên tục.

Một bản hợp đồng chỉ ra đầy đủ các yêu cầu, lịch trình, về cơ bản sẽ luôn có thiếu sót. Trong hầu hết các trường hợp, những gì họ đưa ra sẽ trở nên vô nghĩa sớm hơn nhiều trước khi dự án thực sự kết thúc. Hợp đồng tốt nhất là là hợp đồng quản lý cách mà khách hàng và nhóm phát triển làm việc cùng nhau.

Sau đây tôi sẽ đưa ra ví dụ về một bản hợp đồng thành công, đó là nhừng gì tôi đã làm năm 1994, cho một dự án rất lớn trong nhiều năm với nửa triệu dòng code. Đội ngũ phát triển chúng tôi chỉ được trả một phần khá nhỏ theo từng tháng. Một lượng lớn được thanh toán khi chúng tôi đưa ra phiên bản với nhiều chức năng. Những chức năng đó không được nêu chi tiết trong bản hợp đồng. Thay vào đó, hợp đồng chỉ ra rằng khoản thanh toán sẽ được đưa ra khi chức năng vượt qua bài kiểm tra của khách hàng. Chi tiết về bài kiểm tra đó không được nêu ra trong bản hợp đồng.

Trong suốt quá trình làm việc với dự án đó, chúng tôi và khách hàng đã kêts hợp chặt chẽ với nhau. Chung tôi bàn giao sản phẩm cho anh ta thứ 6 hàng tuần. Trước ngày thứ 2 hoặc thứ 3 của tuần tiếp theo, anh ta đưa ra danh sách các thay đổi cho phần mềm. Chúng tôi xem xét độ ưu tiên cho các công việc đó và lên lịch làm việc cho các tuần tiếp theo. Khách hàng kết hợp chặt chẽ với chúng tôi để đảm bảo quá trình kiểm thử không bao giờ có lỗi. Anh ta luôn biết chức năng nào đáp ứng được yêu cầu của anh ta bởi anh ta kiểm tra nó từ tuần này qua tuần khác.

Yêu câu đối với dự án này là một loạt các thanh đổi không ngừng diễn ra. Những thay đổi chính không phải là hiếm gặp. Có hàng tá chức năng bị xóa đi và thay vào đó là các chức năng khác. Nhưng cuối cùng, hợp đồng và dự án đã sống sót và thành công. Chía khóa dẫn đến sự thành công đó chính là sự hợp tác mạnh mẽ với khách hàng và một bản hợp đồng quản lý sự hợp tác đó, hơn là cố gắng chỉ rõ phạm vi phát triển hay một lịch làm việc với thù lao được định sẵn.

### Đặt sự thích ứng với thay đổi 1 cách kịp thời lên trên việc tuân theo kế hoạch đã định ra từ đầu

Khả năng thích ứng với thay đổi thường quyết định xem một dự án có thành công hay không. Khi chúng ta lập ra kế hoạch, chúng ta cần đảm bảo chắc chắn rằng kế hoạch đủ mềm dẻo để cập nhật thay đổi về kinh doanh cũng như kỹ thuật.

Một dự án không thể được lập kế hoạch ở một tương lai quá xa. Trước tiên, môi trường kinh doanh thường xuyên thay đổi, làm cho yêu cầu cũng bị thay đổi theo. Thêm vào đó, khách hàng có xu hướng thay đổi yêu cầu khi họ thấy hệ thống bắt đầu hoạt động. Cuối cùng, kể cả khi chúng ta biết hết yêu cầu và biết chắc chúng sẽ không thay đổi, chúng ta cũng khó có thể ước lượng chính xác khoảng thời gian cần thiết để phát triển chúng.

Một người mới bắt đầu quản lý sẽ có xu hướng tạo ra một biểu đồ PERT hay Gantt cho cả dự án và đặt chúng ở một nơi nào đó dễ nhìn. Họ nhận thấy biểu đồ sẽ giúp họ quản lý dự án dễ dàng hơn. Họ có thể kiểm tra công việc đang làm của từng cá nhân và đưa họ ra ngoài biểu đồ khi họ hoàn thành công việc. Họ cũng có thể so sách thời gian làm việc thực sự với những gì đưa ra trong kết hoạch, từ đó phản ứng với bất kỳ sự khác lạ nào.

Điều thực sự diễn ra là kiến trúc của biểu đồ ngày càng xấu đi. Khi mà các thành viên làm quen với hệ thống, khách hàng thực sự biết được những gì họ cần, mốt số tác vụ trong biểu đồ trở nên không cần thiết. Một vài tác vụ khác được nhận ra là cần thiết và được thêm vào biểu đồ. Nói ngắn gọn, kế hoạch sẽ bị thay đổi theo hình dạng, không phải theo thời gian.

Một chiến lược lập kế hoạch tốt hơn, là viết ra kế hoạch chi tiết cho mỗi hai tuần, kế hoạch không quá chi tiết cho ba tháng tiếp theo, và một kế hoạch tương đối mơ hồ cho khoảng thời gian sau đó. Chúng ta nên biết những gì chúng ta sẽ làm trong hai tuần tiếp theo. Chúng ta cần biết dù không chi tiết những yêu cầu mà chúng ta cần đáp ứng cho ba tháng tiếp theo. Và chúng ta chỉ cần có khái niệm mơ hồ về những gì hệ thống sẽ làm được sau một năm.

Giải pháp giảm dần của các kế hoạch có nghĩa chúng ta chỉ nên tập chung vào kế hoạch chi tiết cho những tác vụ cần ngay lập tức. Một khi kế hoạch đã được lên, nó rất khó bị thay đổi vì nhóm đang trên đà làm việc với những phần công việc đã được hoàn thành. Tuy nhiên, bởi kế hoạch chỉ quản lý cho một vài tuần nhất định, phần còn lại của kế hoạch vẫn đủ mềm dẻo.

## Principles

Dựa theo 4 tuyên ngôn của phát triển phần mềm linh hoạt, chúng ta có 12 nguyên tắc nêu bật lên sự khác biệt của phát triển phần mềm linh hoạt so với một quy trình nặng nề

**1. Ưu tiên cao nhất của chúng ta là làm vừa lòng khách hàng thông qua việc liên tục và nhanh chóng đưa ra các phiên bản phần mềm có giá trị sử dụng**

Tổ chức "MIT Sloan Management Review" công bố một bài phân tích về những kỹ thuật phát triển phần mềm giúp các công ty xây dựng phần mềm có chất lượng cao. Bài viết đưa ra hàng loạt kỹ thuật có ảnh hưởng rất lớn đến chất lượng của hệ thống cuối cùng. Một trong số đó là sự liên kết chặt chẽ giữa chất lượng sản phẩm với việc nhanh chóng đưa ra các thành phần chức năng của hệ thống. Bài viết còn chỉ ra rằng, phiên bản đầu tiên các ít chức năng bao nhiêu, phiên bản cuối cùng các có chức năng tốt bấy nhiêu.

Một phát kiến nữa trong bài viết đó là sự liên kết chặt chẽ giữa chật lượng cuối cùng với sự tần số các phiên bản đưa ra. Các phiên bản đưa ra càng nhanh, chức năng cuối cùng càng tốt.

Phát triển phần mềm linh hoạt yêu cầu đưa ra các sản phầm nhanh chóng và thường xuyên. Chúng ta cố gắng đưa ra phiên bản thô sơ trong một vài tuần ngay khi dự án bắt đầu. Sau đó chúng ta cố gắng tiếp tục đưa ra các phiên bản được bổ sung thêm các chức năng 2 tuần một lần.

**2. Luôn chào đón sự thay đổi, kể cả khi quá trình phát triển đã đi qua 1 chặng đường dài. Phát triển phần mềm linh hoạt luôn chấp nhận thay đổi vì lợi ích của khách hàng**

Câu nói trên có thể coi là tuyên ngôn cho thái độ làm việc của nhóm phát triển linh hoạt. Những người tham gia phát triển phần mềm linh hoạt sẽ không ngại thay đổi. Họ xem thay đổi là những điều tốt đẹp, bới những thay đổi đó có nghĩa là họ sẽ học nhiềm thêm những gì sẽ đáp ứng được thị trường không ngừng cạnh tranh.

Một nhóm phát triển phần mềm linh hoạt sẽ làm việc chăm chỉ để đảm bảo kiến trúc của phần mềm luôn mềm dẻo, để những thay đổi sẽ ảnh hưởng ít nhất đến hệ thống của họ. Trong phần tiếp theo của cuốn sách, chúng ta sẽ học những kỹ thuật, những mô hình phát triển lập trình hướng đối tượng giúp chúng ta giữ vững sự mềm dẻo đó.

**3. Đưa phần mềm đến với khách hàng thường xuyên, từ 2 tuần cho đến 2 tháng, trong đó ưu tiên khoảng thời gian nhỏ hơn**

Chúng ta đưa phần mềm "hoạt động" đến với khách hàng, đưa chúng 1 cách nhanh chóng (sau một vài tuần đầu tiên) và thường xuyên (một vài tuần sau đó). Chúng ta không vừa lòng với việc đưa ra hàng tá tài liệu và kế hoạch. Chúng ta không gọi đó là những lượt giao hàng có giá trị. Cái chúng ta cần tập trung với là đem đến sản phầm làm thỏa mãn yêu cần của khách hàng.

**4. Nhân viên kinh doanh và lập trình viên nên làm việc cùng nhau trong toàn bộ dự án**

Để có thể trở nên linh hoạt, chúng ta cần sự hợp tác chặt chẽ và thường xuyên giữa khách hàng, lập trình viên và các bên liên quan. Một dự án phầm mềm không phải là một loại vũ khí dùng xong là xong (fire-and-forget weapon). Một phần mềm cần có sự chỉ đạo liên tục.

**5. Xây dựng dự án cùng với việc thúc đẩy các thành viên. Đem đến cho họ môi trường làm việc tốt nhất và hỗ trợ họ, tin tưởng rằng họ sẽ hoàn thành công việc**

Ở dự án phần mềm phát triển linh hoạt, con người được xem là yêu tố quan trọng nhất đưa đến thành công. Tất cả những yếu tố khác - quy trình, môi trường, cách quản lý,... - được xem như yếu tố thứ hai và chúng cần thay đổi nếu chúng gây bất lợi đến con người.

**6. Cách thức tốt nhất để truyền đạt thông tin giữa các thành viên là giao tiếp mặt đối mặt**

Trong một dự án phát triển linh hoạt, con người nói chuyện với nhau. Cách thức truyền đạt thông tin chính sẽ là giao tiếp bằng lời nói Tài liệu có thể được tạo ra, nhưng chúng không thể có được toàn bộ thông tin của hệ thống. Một nhóm phát triển linh hoạt không yêu cầu phải viết tài liệu, kế hoạch hoặc thiêts kê.s Họ có thể tạo chúng nếu họ cảm thấy chúng thực sự cần thiết, nhưng chúng không mặc định phải có. Cái mặc định phải có là sự giao tiếp.

**7. Phần mềm hoạt động là những gì phản ánh chính xác nhất hiệu quả của quá trình làm việc**

Một dự án phát triển phần mềm linh hoạt sẽ kiểm chứng quá trình làm việc của mình bằng những chức năng họ tạo ra thỏa mãn yêu cầu của khách hàng. Họ không đo đạc quá trình làm việc của mình bằng những tài liệu họ đã tạo ra hay lượng code họ đã viết. Họ hoàn thành 30% khi 30% chức năng cần thiết chính thức hoạt động.

**8. Quy trình phát triển phần mềm linh hoạt khuyến khích sự phát triền bền vững. Nhà tài trợ, nhà phát triển và người sử dụng có thể duy trì nhịp độ ổn định một cách vô thời hạn**

Một dự án phát triển phần mềm linh hoạt không phải là một cuộc chạy ngắn, nó là một cuộc thi chạy đường dài. Nhóm phát triển không cần phải đạt tới tốc độ cao nhất và cố duy trì tốc đó. Thay vào đó, họ cần chạy đủ nhanh, nhưng bền vững và ổn định.

Chạy quá nhanh dẫn đến kiệt sức, tìm đường đi tắt và cuối cùng là sụp đổ hoàn toàn. Một nhóm phát triển linh hoạt cần điều chỉnh nhịp độ của chính họ. Họ không được làm cho mình kiệt sức. Họ không mượn năng lượng của ngày mai chỉ để hoàn thành một phần nhỏ ngày hôm nay. Họ làm việc ở một mức vừa phải, đủ để họ duy trì chất lượng tốt nhất cho cả quá trình phát triển của dự án.

**9. Liên tục nghĩ về những giải phát kỹ thuật và những thiết kế tốt nhất**

Chất lượng tốt là chìa khóa để có được tốc độ tốt. Cách để phát triển nhanh là luôn giữ phần mềm hoàn chỉnh, "sạch", và vững chắc hết mức có thể. Do đó, tất cả các thành viên luôn phải đảm bảo răng họ viết ra những dòng code với chất lượng tốt nhất họ có thể làm được. Họ không được tạo ra mốt mớ hỗn độn và nói rằng sẽ sửa nó sau này. Nếu họ tạo ra một mớ hỗn độn, họ phải sửa nó trước khi kết thúc ngày làm việc.

**10. Đơn giản, nghệ thuật của tốt ưu hóa khối lượng công việc, là yêu cầu thiết yếu**

Một nhóm phát triển phần mềm linh hoạt không cần cố gắng tạo ra một sản phẩm khổng lồ. Thay vào đó, họ thực hiện các bước đơn giản nhưng vững chắc để đạt được những gì mình cần. Họ không cần quá xem trọng những vấn đề có thể xảy ra vào ngày hôm sau, hoặc cố tránh những vấn đề mà họ tiên đoán ra từ ngày hôm nay. Thay vào đó, họ làm đơn giản nhất với chất lượng tốt nhất, và tự tin rằng chúng có thể dễ dàng thay đổi nếu ngày mai có vấn đề gì xảy ra.

**11. Kiến trúc, yêu cầu và thiết kế tốt nhất đến từ một nhóm có thể tự tổ chức**

Một nhóm phát triển phần mềm linh hoạt là một nhóm biết tự tổ chức (self-organizing team). Trách nhiệm sẽ không đè lên vai bất kỳ một cá nhân cụ thể nào cả. Từ phía bên ngoài, trách nhiệm sẽ được xem như đặt lên vai cả nhóm. Việc phân công sẽ được nhóm tự xử lý.

Nhóm phát triển phần mềm linh hoạt sẽ làm việc với nhau trong tất cả các phạm trù của dự án. Không có bất kỳ cá nhân cụ thể nào chịu trách nhiệm cho kiến trúc hay yêu cầu hay kiểm thử của dự án. Mọi người chia sẻ nhiệm vụ, và mỗi thành viên đều có ảnh hưởng đến nhiệm vụ đó.

**12. Sau một khoảng thời gian nhất định, nhóm phát triển suy nghĩ về cách để làm việc hiệu quả hơn, từ đó đưa ra những điều chỉnh phù hợp**

Một nhóm phát triển phần mềm linh hoạt không ngừng điều chỉnh tổ chức, luật, quy định và cả các mối quan hệ. Họ biết rằng môi trường luôn thay đổi, vậy nên họ cũng phải thay đổi để luôn "linh hoạt".

## Kết luận

Mục đích của các lập trình viên cũng như các nhóm phát triển là đem đến sản phẩm tốt nhất có thể cho người quản lý và khách hàng. Sẽ rất thất vọng nếu như sản phầm của họ thất bại và không đưa ra được bất cứ giá trị gì. Mặc dù được định hình rất tốt, nhưng một quy trình nặng nề sẽ là một phần lỗi cho sự thất bại đó. Do đó, những quy tắc và tuyên ngôn phát triển phần mềm linh hoạt được đưa ra, như một cách để giúp đỡ các nhóm thoát khỏi vòng luẩn quẩn của những quy trình nặng nề, tập trung vào những kỹ thuật đơn giản giúp họ đạt được mục tiêu.

Ở thời điểm cuốn sách này được viết, có nhất nhiều quy trình phát triển linh hoạt được đưa ra. Trong đó có SCRUM, Crystal, Feature Driven Development, Adaptive Software Development (ADP), và nổi bật nhất là Extreme Programming.

# Quan điểm cá nhân

Chương đầu cuốn sách đưa ra 4 tuyên ngôn cho phát triển phần mềm linh hoạt và 12 nguyên tắc để đạt được điều đó. Với tuổi nghề chỉ chưa đầy 2 năm, tôi không đủ tư cách để nhận xét/phê phán những quan điểm của những kỹ sư với hai, ba mươi năm kinh nghiệm. Tuy nhiên, từ góc độ bản thân, tôi cho rằng rất khó để đạt được tất cả những gì nêu ra ở trên, hay đúng hơn đó là hình mẫu lý tưởng, mà con người luôn cố gắng nhưng rất khó để vươn tới. Cho dù bạn có cố làm theo những gì ở trên, thì không chắc khách hàng của bạn sẽ biết hay đồng ý làm theo, và như vậy bạn mới chỉ đi được một nửa chặng đường.

Tuy nhiên lý tưởng là lý tưởng, nó luôn ở đó, và tôi nghĩ chúng ta cần cố gắng để đạt được hoàn thiện hết mức có thể. Ở quy tắc số 10 về sự đơn giản, thực sự nó không "đơn giản" chút nào. Bạn sẽ phải học cả tá "Design pattern" nếu muốn viết code "đơn giản" và dễ dàng thay đổi cho ngày hôm sau. Do vậy điều bạn cần làm là không ngừng học hỏi để tiến bộ và bắt kịp các kỹ thuật, công nghệ mới.

Phần cuối cùng tác giả nói về các mô hình phát triển phần mềm linh hoạt, dù SCRUM được đưa ra nhưng có lẽ ở thời đó, người ta đề cao "Extreme Programming" hơn. Do đó chương tiếp theo cuốn sách nói về "Extreme Programming". Tuy nhiên, những năm gần đây "SCRUM" đang trở nên vượt trội và tôi đang làm việc theo mô hình đó. Do vậy tôi sẽ không dịch chương tiếp theo, nếu có thời gian sẽ viết bài về "SCRUM".

