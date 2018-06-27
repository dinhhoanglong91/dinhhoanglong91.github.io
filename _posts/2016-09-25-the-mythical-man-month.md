---
title: "The Mythical Man-Month"
type: "book"
book: "the_mythical_man_month"
book_title: "The Mythical Man-Month"
cat: "it"
---

Bài viết sau dịch từ chương hai cuốn [The mythical Man-Month](https://www.amazon.com/Mythical-Man-Month-Software-Engineering-Anniversary/dp/0201835959) của tác giả [Frederick P. Brooks, JR.](https://www.cs.unc.edu/~brooks/) Tên chương hai cũng chính là tên sách, phân tích khái niệm man-month và khẳng định vai trò vô cùng quan trọng của giai đoạn lên kế hoạch đến toàn dự án. Mời bạn đọc cùng tìm hiểu.

# The mythical man-month (Man-month: Khái niệm hoang đường)

> Nấu ăn ngon tốn nhiều thời gian. Nếu bạn phải chờ đợi, đó là vì để phục vụ bạn, và để làm cho bạn hài lòng.
>
> ____MENU của nhà hàng Antoine, New Orleans

Không dành đủ thời gian cho giai đoạn lên kế hoạch là nguyên nhân lớn nhất gây ra thất bại của các dự án, hơn bất cứ nguyên nhân nào khác. Tại sao nguyên nhân này lại phổ biến vậy?

Thứ nhất, kỹ năng đánh giá (estimate) của chúng ta không được đào tạo bài bản, đúng hơn là rất nghèo nàn. Nghiêm trọng hơn, nó dựa theo những giả định mơ hồ của chúng ta, mà thường là không đúng.

Thứ hai, chúng ta thường nhầm lẫn giữa công sức (effort) với quá trình (progress), kèm theo giả định rằng con người (men) và thời gian (cụ thể là tháng - months) có thể thay thế cho nhau.

Thứ ba, bởi vì chúng ta đánh giá không chắc chắn, nó khiến cho người quản lý mất kiên nhẫn với các thành viên dự án.

Thứ tư, lịch của dự án được quản lý rất hời hợt. Trong khi các ngành kỹ thuật khác mọi người đều có thói quen tuân thủ kỷ luật, ngành phần mềm coi đó là điều xa lạ.

Thứ năm, khi dự án có dấu hiệu chậm, cách tự nhiên (và truyền thống) là bổ sung thêm nhân sự. Cách làm này giống như dập lửa bằng cách đổ thêm dầu. Càng nhiều dầu thì lửa càng to, vòng luẩn quẩn lặp đi lặp lại và kết thúc là một thảm hoạ.

Trong chương này chúng ta sẽ không phân tích cách quản lý tiến độ dự án, thay vào đó sẽ nghiên cứu một vấn đề khác cụ thể hơn.

## Chủ nghĩa lạc quan

Tất cả các lập trình viên đều là những người lạc quan. Có lẽ có một phép thuật nào đó đã thu hút những con người tin vào một cái kết có hậu. Có lẽ hàng trăm vấn đề khúc mắc cuối cùng cũng sẽ kết thúc. Có lẽ bởi vì máy tính còn trẻ, và lập trình viên cũng trẻ hơn cả máy tính, và vì người trẻ thì thường lạc quan. Tuy nhiên, quá trình chọn lọc tự nhiên vẫn không ngừng diễn ra, và kết quả là điều tất yếu. "Lần này chắc chắn chạy", hay là "Kiểu gì cũng không còn bug".

Vậy đó, giả định sai lầm đầu tiên của công việc lập lịch, đó là *Tất cả đều sẽ chạy tốt*, *Mỗi task sẽ tốn khoảng thời gian đúng như khoảng thời gian mà nó nên "tiêu tốn"*.

Sự lan toả của chủ nghĩa lạc quan trong giới lập trình viên thu hút rất nhiều nghiên cứu. Dorothy Sayers, trong cuốn sách tuyệt vời của mình, *The Mind of the Maker - Suy nghĩ của người sáng tạo*, chia hành động sáng tạo thành 3 bước: Suy nghĩ, thực hiện và tương tác. Một cuốn sách, một cái máy tính hay một chương trình ban đầu xuất hiện dưới dạng ý tưởng và được hoàn thành trong bộ não của tác giả. Nó được mô tả trong không gian và thời gian, bằng bút, mực và giấy, hay là những sợi dây,... Sự chế tạo kết thúc khi ai đó đọc được cuốn sách, sử dụng máy tính, hay chạy chương trình, đó cũng là khi họ tương tác với suy nghĩ của tác giả.

Ví dụ này của Miss Sayers không chỉ miêu tả quá trình sáng tạo của con người, mà cả giải thích cho học thuyết Trinity của đạo Cơ-đốc. Khi con người tạo ra cái gì đó, sự thiếu hoàn thiện và thiếu chắc chắn của ý tưởng sẽ dần dần được hoàn thiện trong quá trình thực hiện. Đó là lý do vì sao, viết, trải nghiệm, thực hành là yếu tố quan trọng bổ sung cho lý thuyết.

Trong nhiều hành động sáng tạo, phương tiện thực hiện đôi khi rất khó quản lý. Những thiếu xót vật lý của các công cụ làm hạn chế ý tưởng của chúng ta, và cũng có thể gây ra khó khăn không ngờ tới cho giai đoạn thực hiện.

Vì vậy,công việc thực hiện tốn nhiều thời gian hơn và có thể là gấp đôi dự kiến ban đầu, bởi các yếu tố vật lý và bởi vì sự không thoả đáng của chính ý tưởng. Chúng ta có xu hướng đổ lỗi cho các yếu tố vật lý gây ra các khó khăn trong quá trình thực hiện, cho những thứ không phải của "chúng ta" theo cách phán xét của chính chúng ta.

Tuy nhiên, lập trình máy tính, được tạo bởi vô số các yếu tố dễ kiểm soát. Bởi vì chúng dễ kiểm soát, chúng ta cho rằng sẽ có ít khó khăn trong công việc thực hiện, và do đó chúng ta trở nên lạc quan. Và bởi vì suy nghĩ của chúng ta không chính xác, chúng ta có lỗi, và rồi sự lạc quan của chúng ta không còn nguyên vẹn nữa.

Việc hoàn thành các task của một dự án và hoàn thành dự án có thể hiểu như một phân bố xác suất. Xác suất để hoàn thành cả dự án phụ thuộc vào xác suất hoàn thành của từng thành phần. Do vậy, ở dự án phức tạp khi số lượng thành phần các nhiều lên, thì xác suất thành công của cả dự án càng trở nên nhỏ đi.

## Khái niệm man-month

Suy nghĩ sai lầm thư hai được miêu tả bởi đơn vị dùng cho đánh giá và lập lịch dự án: khái niệm man-month. Quả thực chi phí dự án thay đổi tuỳ thuộc vào số lượng người và số lượng tháng. Tuy nhiên tiến trình không như vậy. *Do vậy, dùng man-month để định lượng phạm vi công việc thực rất nguy hiểm và là một điều hoang đường dối trá.* Việc sử dụng man-month để định lương dựa theo suy nghĩ rằng con người(men) và tháng(months) có thể thay thế cho nhau.

Men và months chỉ có thể thay thế cho nhau khi một task có thể phân chia cho nhiều người cùng làm với *không một sự liên lạc nào*. Điều này đúng với việc thu hoạch lúa mỳ hoặc hái quả bông. Nó hoàn toàn không đúng trong lập trình.

![2.1](/assets/img/post/mythical-mm/2.1.png)

Hình 2.1: Thời gian so với số lượng nhân công - task có thể phân chia hoàn toàn

Khi một task không thể phân chia, vì có sự phụ thuộc tuyến tính, việc bổ sung thêm người không có ảnh hưởng gì đến tiến độ cả (Ảnh 2.2). Việc sinh ra một đứa trẻ, cần 9 tháng, không quan tâm có bao nhiêu người phụ nữ cả. Rất nhiều task trong lập trình có đặc điểm này, bởi vì cần quá trình kiểm thử một cách tuyến tính.

![2.2](/assets/img/post/mythical-mm/2.2.png)

Hình 2.2 Thời gian so với số lượng nhân công - task không thể phân chia

Khi các task có thể phân chia, nhưng cần sự ràng buộc và liên hệ nhất định giữa các task nhỏ, công sức tạo ra sự liên kết này cần được bổ sung vào tổng số lượng công việc. Đôi khi, công sức để phân chia công việc còn lớn hơn công sức thực tế để làm ra sản phẩm (Hình 2.3).

![2.3](/assets/img/post/mythical-mm/2.3.png)

Hình 2.3 Thời gian so với số lượng nhân công - các task nhỏ cần sự liên kết

Phần bổ sung thêm cho sự liên kết, được tạo bởi hai phần, đào tạo và giao tiếp. Mỗi nhân công cần được đào tạo về kỹ thuật, mục đích công việc của họ, chiến lược tổng thể và kế hoạch công việc. Quá trình đào tạo này không thể bị chia nhỏ, và phần này phụ thuộc tuyến tính vào số lượng nhân công.

Giao tiếp thì tồi tệ hơn, nếu mỗi task đều liên hệ rời rạc với các task khác, công sức của chúng ta tăng thêm *n(n-1)/2*. Ba nhân công cần nhiều hơn ba lần giao tiếp so với hai, và bốn thì cần nhiều gấp 6 lần so với hai. Ngoài ra, chúng ta còn cần bàn bạc giữa ba, bốn,... (thay vì chỉ có hai) nhân công, để giải quyết công việc. Và do vậy, vấn đề càng tồi tệ hơn. Công sức dành cho giao tiếp đi ngược lại mong muốn ban đầu của chung ta chỉ là phân chia công việc, dẫn đến hoàn cảnh ở hình 2.4.

![2.4](/assets/img/post/mythical-mm/2.4.png)

Hình 2.4 Thời gian so với số lượng nhân công - các task cần sự ràng buộc phức tạp.

Lập trình vốn dĩ là công việc phức tạp, cần sự liên hệ phức tạp giữa các thành phần, và do vậy công sức dành cho kết nối giữa các thành phần sẽ càng nhiều hơn, vượt trội so với lợi ích của việc phân chia nhỏ công việc. Kết quả là thêm người không làm ngắn đi thời gian, mà chỉ làm tăng thêm con số đó.

## Kiểm thử hệ thống

Không có phần nào trong bản kế hoạch bị ảnh hưởng bởi sự ràng buộc tuyến tính hơn kiểm tra sự tương tác giữa các thành phần và kiểm thử hệ thống. Thêm nữa, khoảng thời gian này còn bị ảnh hưởng bởi số lượng và sự xuất hiện không tiên đoán được của các bugs. Về mặt lý thuyết, con số này đáng ra phải là 0. Và vì chúng ta là những người lạc quan, chúng ta thường nghĩ số lượng bugs sẽ nhỏ hơn số lượng thực tế mà chúng xuất hiện. Chính vì thế, kiểm thử thường là phần được lên kế hoạch sai lệch nhất trong toàn dự án.

Trong vài năm, tôi đã thành công trong việc sử dụng quy tắc sau cho công việc lên kết hoạch:

* 1/3 cho thiết kế và lập kế hoạch chi tiết
* 1/6 cho coding
* 1/4 cho kiểm thử thành phần và kiểm thử hệ thống ở mức cơ bản
* 1/4 cho kiểm thử hệ thống với toàn bộ các thành phần

Cách lập kế hoạch này khác biệt khá lớn và cách truyền thống ở các điểm sau

1. Khoảng thời gian cho thiết kế và lập kế hoạch chi tiết lớn hơn thông thường. Dù vậy chăng nữa, nó cũng chỉ là vừa đủ cho công việc tạo ra thiết kế chi tiết, yêu cầu cụ thể, không đủ cho việc nghiên cứu và tìm hiểu các công nghệ mới.

2. Một nửa khoảng thời gian dành cho kiểm thử các thành phần và hệ thống, lớn hơn rất nhiều so với thông thường.

3. Công việc dễ ướng lượng nhất, coding, chỉ chiếm 1/6 toàn bộ kế hoạch.

Khi theo dõi các dự án thông thường, tôi thấy rất ít người cho rằng một nửa dự án phải dành cho kiểm thử, nhưng đáng ra chúng phải là như thế. Rất nhiều thành phần hoạt động đúng kế hoạch ngoại trừ kiểm thử hệ thống, hoặc đến khi kiểm thử bắt đầu được thực hiện và các phần khác không còn đúng kế hoạch.

Sai lầm khi không có đủ thời gian cho kiểm thử hệ thống gây ra hậu quả rất tai hại. Vì sự chậm trễ thường chỉ thấy rõ ở giai đoạn cuối của dự án, không ai nhận ra vấn đề của kế hoạch, cho đến gần ngày bàn giao sản phẩm. Tin xấu, đến muộn, không một lời cảnh báo thực sự gây khó chịu cho khách hàng và người quản lý.

Hơn nữa, sự chẫm trễ trong thời gian này có ảnh hưởng rất xấu đến các vấn đề về kinh tế, về tâm lý cũng như cho các dự án trong tương lai. Nên nhớ là dự án đã được cung cấp đủ nhân lực, làm việc hết công suất hàng ngày. Càng nghiêm trọng hơn, bản thân phần mềm sinh ra để hỗ trợ cho một nghiệp vụ khác (bán máy tính, vận hành máy móc,...), chi phí tiếp theo gây ra bởi sự chậm trễ còn cao hơn nữa. Và thực sự, chi phí thứ hai này hơn rất nhiều so với các chi phí khác. Do vậy, chúng ta cần phải hết sức chú trọng đến thời gian cho kiểm thử hệ thống.

## Đánh giá thiếu chính xác

Hãy thử xem lập trình viên như những đầu bếp, chịu áp lực về thời gian từ ông chủ, những người quản lý sự hoàn thành của một công việc, nhưng không thể quản lý việc hoàn thành "thực sự". Món trứng ốp la, được hứa hẹn sẽ xong trong 2 phút, có thể xuất hiện một cách ngon nghẻ. Những nếu nó không phải là 2 phút, khách hàng sẽ có hai lựa chọn - đợi hoặc ăn trứng sống. Khách hàng của một phần mềm cũng có cùng những lựa chọn đó.

Đầu bếp còn có một lựa chọn khác, bật lửa to lên. Kết quả thông thường là món trứng không được nguyên vẹn, một phần bị cháy, một phần vẫn còn sống.

Tôi không cho rằng người quản lý một phần mềm lại có dũng khí không bằng ông chủ nhà bếp hay quản lý của bất cứ ngành kỹ thuật nào khác. Tuy nhiên sai lầm trong việc lập kết hoạch giống như thất bại của ông chủ ở quán ăn hơn bất cứ một ngành kỹ thuật nào khác. Rất khó để tạo ra một kế hoạch hợp lý, chính xác, phòng tránh các khó khăn tiềm tàng chỉ bởi một phương thức đánh giá không dựa trên các phương pháp định lượng, không được dựa trên các dữ liệu sẵn có, và chỉ dựa trên quan điểm cá nhân của người quản lý.

Rõ ràng, chúng ta cần 2 biện pháp. Chúng ta cần phát triển và công khai các mô hình về năng suất công việc, ảnh hưởng của bug, luật đánh giá (estimating rules),... Sự chuyên nghiệp chỉ có thể đạt được khi chia sẻ các dữ liệu đó.

Cho đến khi quy trình đánh giá được chuẩn bị bài bản, các nhà quản lý vẫn phải tự cũng cố thêm kỹ năng của mình và bảo vệ cho kết quả đánh giá của mình, với mong muốn rằng trực giác của họ sẽ tốt hơn đánh giá dựa theo niềm tin.

## Thảm hoạ của việc tái thiết lập kế hoạch

Chúng ta sẽ làm gì khi dự án chậm kế hoạch? Một cách tự nhiên, thêm người. Dựa theo hình 2.1 và 2.4, điều đó có thể có ích hoặc không.

Hãy xem xét ví dụ sau. Giả sử chúng ta có một task được đánh giá là 12 man-months và được giao cho 3 người làm trong 4 tháng. Có các mốc quan trọng theo từng tháng là A, B, C, D (Hình 2.5).

![2.5](/assets/img/post/mythical-mm/2.5.png)

Hình 2.5

Bây giờ, giả sử phần đầu tiên không thể hoàn thành đúng tiến độ, và phải hết tháng thứ hai mới xong. Khi đó người quản lý có các lựa chọn nào?

1. Giả sử tất cả phải hoàn thành đúng tiến độ. Cho rằng chỉ phần thứ nhất bị đánh giá không chính xác, khi đó hình 2.6 mô ta đúng hoàn cảnh của chúng ta. Khi đó chúng ta cần tổng số 9 man-months, làm trong 2 tháng, tức là cần 4.5 người 1 tháng. Do đó, chúng ta bổ sung thêm 2 người vào làm cùng 3 người hiện tại (2 + 3 = 5 > 4.5)

![2.6](/assets/img/post/mythical-mm/2.6.png)

Hình 2.6

2. Giả sử tất cả phải hoàn thánh đúng tiến độ. Cho rằng tất cả đều đã bị estimate sai theo cùng một kiểu, khi đó hình 2.7 mô tả đúng hoàn cảnh của chúng ta. Do đó, chúng ta cần 18 man-months lúc này, làm trong 2 tháng, tức là cần 9 người. Do vậy, chúng ta bổ sung thêm 6 người vào làm cùng 3 người hiện tại.

![2.7](/assets/img/post/mythical-mm/2.7.png)

Hình 2.7

3. Lập lại kế hoạch. Tôi thích lời khuyên của P.Fagg, một lập trình viên phần cứng nhiều năm kinh nghiệm, "Đừng mắc những sai lầm nhỏ". Điều đó có nghĩa, chúng ta cần dành nhiều thời gian cho việc lập lại kế hoạch, để chắc chắn công việc sẽ hoành thành tốt đúng hạn, và chắc chắn không phải lập lại kế hoạch lần nữa.

4. Rút ngắn công việc. Trong thực tế điều này có thể xảy ra, khi các thành viên nhận ra rằng kế hoạch không thực sự đúng. Nếu cái giá của việc chậm trễ gây ra cho các quá trình sau này là quá lớn, thì đây là một lựa chọn đúng đắn. Lựa chọn duy nhất của người quản lý là công khai rút ngắn công việc, tái thiết lập lại kế hoạch và theo dõi task đã bị rút ngắn dựa theo những thiết kế vội vàng và kiểm thử không đầy đủ.

Trong hai cách giải quyết đầu tiên, việc cố ép buộc task phải hoàn thành trong 4 tháng là một thảm hoạ. Hãy thử xem xét ảnh hưởng cho cách giải quyết đầu tiên ở hình 2.8. Hai người mới được bổ sung vào team, dù có khả năng đến mấy và được đưa vào nhanh đến mấy, thì cũng cần được đào tạo đôi chút bởi những người có kinh nghiệm. Nếu điều đó tốn một tháng, *3 man-months đã phải hy sinh cho công việc vốn dĩ không thuộc đánh giá ban đầu*. Hơn nữa, task, ban đầu được chia ra theo hướng dành cho 3 người, giờ cần chia theo một cách khác cho 5 người, tức là 5 phần; dẫn đến một vài phần công việc đã hoàn thành có thể có lỗi, gây khó khăn và kéo dài công đoạn kiểm thử hệ thống. Do vậy, đến cuối tháng thứ 3, về thực chất là còn tới 7 man-months công việc, 5 con người đã được đào tạo và một người nữa sẵn sàng cho công việc. Như gợi ý ở hình 2.8, task sẽ bị chậm nếu chúng ta không thêm ai đó vào (Hình 2.6).

![2.8](/assets/img/post/mythical-mm/2.8.png)

Hình 2.8

Với mong muốn kết thúc trong vòng 4 tháng, và xem như chúng ta chỉ tốn thời gian và không lo về việc phân chia lại công việc hay kiểm thử hệ thống, chúng ta cần 4 người vào cuối tháng thứ 2, chứ không phải 2. Để hỗ trợ cho việc phân chia lại công việc và không gây ảnh hưởng đến kiểm thử hệ thống, chúng ta cần thêm một người nữa. Tuy nhiên đến lúc này, chúng ta có tối thiểu 1 team có 7 người, không phải là 3 nữa, và đến lúc này việc quản lý team và phân chia công việc trở nên rất khó khăn.

Chú ý rằng đến cuối tháng 3, mọi việc trở nên rất đen tối. Mục tiêu của tháng thứ 3 không thể đạt được dù cho đã tốn rất nhiều nỗ lực. Các nỗ lực tiếp theo chỉ còn lại tiếp tục lặp đi lặp lại việc thêm người. Điều đó thật khủng khiếp.

Tính toán ở trên mới chỉ dựa vào việc phần đầu tiên của dự án được đánh giá không đúng. Nếu chúng ta vẫn bảo thủ và cho rằng toàn bộ kế hoạch đang đi đúng hướng, thì như hình 2.7, chúng ta cần thêm 6 người nữa. Việc tính toán cho quá trình đào tạo, phân chia công việc, ảnh hưởng đến kiểm thử hệ thống, xin dành cho bạn đọc. Không còn nghi ngờ gì nữa, thảm hoạ của việc lặp đi lặp lại những tính toán này sẽ chỉ tạo nên một sản phẩm tồi tệ, và cuối cùng, là lập lại kế hoạch với chính ba con người ban đầu, không thêm ai hết.

Nói tóm lại, một cách đơn giản và có phần phũ phàng, chúng tôi đưa ra luật của Brooks.

> Thêm người vào một dự án chậm chỉ làm nó chậm thêm.
>
> (Adding manpower to a late software project makes it later.

Đây chính là sự định nghĩa lại của man-month. Số lượng tháng cần cho một dự án phụ thuộc vào mối quan hệ tuyến tính giữa các công việc. Số lượng tối đa con người phụ thuộc vào số lượng công việc nhỏ có thể làm độc lập. Dựa theo hai con số này, chúng ta có thể điều chỉnh kế hoạch với ít người hơn và nhiều tháng hơn. (Rủi ro duy nhất là việc huỷ bỏ dự án). Tuy nhiên, chúng ta không thể tạo ra một kế hoạch hợp lý với nhiều người hơn và ít tháng hơn. Không dành đủ thời gian cho giai đoạn lên kế hoạch là nguyên nhân lớn nhất gây ra thất bại của các dự án, hơn bất cứ nguyên nhân nào khác.

# Tóm tắt

40 năm từ khi cuốn sách The mythical man-month ra đời (xuất bản lần đầu tiên năm 1975), những tư tưởng của tác giả vẫn còn nguyên giá trị. Dưới đây là tổng hợp 12 kết luận của chương số 2, được tác giả tóm tắt ở chương 18 của cuốn sách.

 * 2.1 Không dành đủ thời gian cho giai đoạn lên kế hoạch là nguyên nhân lớn nhất gây ra thất bại của các dự án, hơn bất cứ nguyên nhân nào khác.
 * 2.2 Nấu ăn ngon cần thời gian, công việc sẽ không thể hoàn thành một cách vội vàng nếu không làm hỏng chính kết quả.
 * 2.3 Tất cả các lập trình viên đều là người lạc quan. "Tất cả rồi sẽ chạy đúng".
 * 2.4 Bởi lập trình viên tạo ra sản phẩm từ những ý tưởng thuần tuỳ, họ cho rằng sẽ có ít khó khăn trong giai đoạn phát triển.
 * 2.5 Tuy nhiên ý tưởng của chúng ta không chính xác, do đó chúng ta có bugs.
 * 2.6 Kỹ năng đánh giá của chúng ta, xây dựng thông qua việc tính toán chi phí, có những hiểu lầm giữa công sức và quá trình. Man-month là một khái niệm ảo tưởng và nguy hiểm, nó cho rằng con người và thời gian có thể thay thế được cho nhau.
 * 2.7 Phân chia một task cho nhiều người khiến chúng ta tốn thêm công sức cho việc giao tiếp - đào tạo và trao đổi.
 * 2.8 Luật của tôi là dành 1/3 thời gian cho thiết kế, 1/6 thời gian cho coding, 1/4 thời gian cho kiểm thử thành phần và 1/4 thời gian cho kiểm thử hệ thống.
 * 2.9 Chúng ta thiếu các dữ liệu đánh giá.
 * 2.10 Bởi vì chúng ta thiếu các kỹ năng đánh giá dự án, chúng ta thường thiếu can đảm để bảo vệ mình khỏi áp lực từ nhà quản lý và khách hàng.
 * 2.11 Luật Brook: Thêm người vào một dự án chậm chỉ làm nó chậm thêm.
 * 2.12 Thêm người vào một dự án làm tăng thêm tổng số công sức theo 3 cách: công việc và cản trở đến việc phân chia công việc, đào tạo người mới, tăng thêm trao đổi.

