---
title: "Nhóm phẫu thuật"
type: "book"
book: "the_mythical_man_month"
book_title: "The Mythical Man-Month"
show_top: 0
cat: "it"
---

Tiếp nối chương 2 của cuốn [The Mythical Man-Month]({% post_url 2016-09-25-the-mythical-man-month %}), chúng ta đến với chương số ba của cuốn sách này, nói về cách xây dựng một nhóm làm việc hiệu quả.

# The Surgical Team - Nhóm Phẫu Thuật

Trong một cuộc hội thảo về công nghệ thông tin, chúng tôi được nghe đi nghe lại ý kiến của những quản lý trẻ, rằng họ thích các nhóm nhỏ, với những người xuất sắc, hơn là một dự án gồm hàng trăm lập trình viên, nhưng ở trình độ tầm thường. Và thực ra là không chỉ các quản lý trẻ đó, mà chúng ta cũng nghĩ như vậy.

Tuy nhiên, tuyên bố đơn giản này đã cố tình lảng tránh khỏi một vấn đề rất khó khăn - làm cách nào để có thể xây dựng một hệ thống lớn theo một bản kế hoạch chấp nhận được? Giờ chúng ta cùng tìm hiểu vấn đề chi tiết.

## Vấn đề

Người quản lý dự án luôn có nhận thức rất rõ ràng, về sự khác biệt về hiệu năng rất lớn giữa lập trình viên tốt và lập trình viên tồi. Tuy nhiên, kết quả đo lường thực tế thực sự gây bất ngờ cho tất cả chúng ta. Trong một nghiên cứu, Sackman, Erikson và Grantt đã đo lường hiệu năng của một nhóm các lập trình viên có kinh nghiệm. Chỉ riêng trong nhóm đó thôi, tỷ lệ hiệu năng giữa người tốt nhất và tồi nhất đã là 10:1 và 5:1 trong tốc độ làm việc. Một cách ngắn gọn, 1 lập trình viên đáng giá 20.000$/1 năm sẽ có giá trị bằng 10 lập trình viên có giá 10.000$/1 năm. Kết quả này hoàn toàn không cho thấy sự liên quan gì giữa số năm kinh nghiệm và hiệu năng (có nghĩa là ít năm kinh nghiệm vẫn có thể có hiệu năng tốt, hay nhiều năm kinh nghiệm vẫn có thể có hiệu năng tồi).

Tôi luôn cho rằng việc có quá nhiều cái đầu cùng làm việc trong một dự án khiến chi phí tăng lên đáng kể, chủ yếu là cho việc giao tiếp và sửa chữa những sai lầm do việc hiểu sai ý của nhau (miscommunication). Chính vì thế, nó đưa chúng ta đến kết luận, rằng dự án nên được xây dựng dựa trên một nhóm người ít nhất có thể. Thực tế, các dự án lớn đều cho thấy việc sử dụng quá nhiều người đều tốn kém và không hiệu quả, và các hệ thống được tạo ra không tương thích được tốt với nhau. OS/360, Exec 8, Scope 6600, Multics, TSS, SAGE,... và các ứng dụng khác là ví dụ.

Kết luận thật đơn giản: Nếu chúng ta có dự án cần 200 người, mà có 25 project manager, vậy thì có thể sa thải 175 người còn lại, và đưa các managers trở lại với công việc lập trình.

Bây giờ, hãy cùng kiểm tra giải pháp này. Nghĩ theo cách khác, cách tiếp cận này không còn thoả mãn điều kiện của một team nhỏ, bởi thông thường chúng ta cho rằng team nhỏ không nên vượt quá 10 người. Nó cũng quá lớn khiến chúng ta phải thực hiện 2 mức quản lý, hoặc cần khoảng 5 managers. Thêm nữa cần hỗ trợ về tài chính, con người, không gian, thư ký cũng như là máy móc.

Ở một khía cạnh khác, nhóm 200 người chưa đủ lớn để xây dựng một hệ thống khổng lồ. Ví dụ OS/360. Vào lúc đỉnh điểm, có hơn 1000 người cùng làm việc - lập trình viên, quản lý máy móc, thư ký, quản lý, nhóm hỗ trợ,... Từ năm 1963 đến năm 1966, khoảng 5000 người/1 năm đã tham dự và quá trình xây dựng và viết tài liệu. Với 200 người như lý tưởng ban đầu, chúng ta cần 25 năm để hoàn thành dự án này, tất nhiên là với giả thiết man và months có thể thay thế cho nhau!

Do vậy, chúng ta thấy được vấn đề của team nhỏ: nó quá chậm cho một hệ thống thực sự lớn. Hãy suy nghĩ xem nếu OS/360 được tạo nên bởi 1 team nhỏ. Lý tưởng, là 10 người. Thêm nữa, tưởng tượng chúng ta có 1 team đủ mạnh, mỗi người làm việc hiệu quả gấp 7 lần người tầm thường, về tất cả các lĩnh vực lập trình cũng như kỹ năng viết tài liệu. Giả sử OS/360 đã được xây dựng bởi những người tầm thường (điều này hoàn toàn không đúng sự thực). Tiếp tục, giả sử rằng chúng ta có thêm một lợi ích khác của team toàn những người giỏi, tức là giảm được chi phí cho giao tiếp do chỉ làm bởi 1 team nhỏ. Vậy là giảm thêm được 7 lần nữa. Tổng cộng, chúng ta cần 5000/(10 X 7 X 7) = 10, tức là có thể hoàn thành công việc 5000 man-year trong vòng 10 năm. Câu hỏi là, liệu sản phẩm của chúng ta có còn thú vị sau 10 năm kể từ thiết kế ban đầu của nó? Hay nó sẽ trở nên lạc hậu khi mà có cơ số sản phẩm kỹ thuật được ra đời hàng ngày?

Chúng ta đang đi vào thế tiến thoái lưỡng nan. Để đem về hiệu quả tốt nhất, một vài người cho rằng nên để những người giỏi nhất làm công việc thiết kế và xây dựng. Tuy nhiên, với một hệ thống lớn, một vài người lại cho rằng nên dành hầu hết sức người cho công việc xây dựng, và chúng ta sẽ sớm có một phiên bản có thể thấy được. Vậy làm cách nào để cân bằng những mong muốn trên?

## Đề xuất của Mills

Harlan Mills đưa ra một đề xuất khá sáng tạo. Mills cho rằng mỗi phần của một công việc lớn cần được đảm nhiệm bởi một team, nhưng team đó cần được cấu trúc như một nhóm phẫu thuật, hơn là một nhóm mổ lợn. Có nghĩa là, thay vì các thành viên trong nhóm làm cùng một công việc, chỉ một người duy nhất làm công việc chính, các thành viên còn lại hỗ trợ anh ta giúp nâng cao hiệu suất công việc của anh ta.

Suy nghĩ một chút chúng ta sẽ thấy rằng cách giải quyết này sẽ giúp đạt được mong muốn, nếu có thể làm được nó. Một vài cái đầu được dùng cho thiết kế vào xây dựng, và rất nhiều công sức dùng cho việc hỗ trợ các công việc liên quan. Liệu cách này có thực hiện được? Ai sẽ là bác sỹ gây mê và ý tá trong một team lập trình, và làm sao phân chia công việc? Hãy để tôi sử dụng phép ẩn dụ để giải thích cách mà một nhóm có thể hoạt động nếu nhận được đầy đủ sự hỗ trợ.

**Bác sỹ phẫu thuật - Surgeon** Mills gọi anh ta là lập trình viên trính *chief programmer*. Anh ta chỉ định các hàm, yêu cầu về hiệu năng, thiết kế, lập trình, kiểm thử và viết tài liệu. Anh ta viết theo một ngôn ngữ lập trình cấu trúc, kiểu như PL/I và có thể truy cập đến hệ thống máy tính, không chỉ kiểm thử chương trình của anh ta mà còn lưu trữ các phiên bản khác nhau của chương trình, giúp cho các tập tin có thể cập nhật dễ dàng và có thể chỉnh sửa các tài liệu. Vai trò này cần tài năng xuất chúng, 10 năm kinh nghiệm, hiểu biết sâu sắc về hệ thống và ứng dụng, cũng như có thể sử dụng được các kiến thức toán học trong việc quản lý dữ liệu kinh doanh.

**Phụ tá - Copilot** Anh ta đóng vai trò như bản ngã của bác sỹ phẫu thuật, người có khả năng làm bất cứ phần công việc nào, nhưng ít kinh nghiệm hơn. Vài trò chính của anh ta là chia sẻ thiết kế như một nhà tư tưởng và đánh giá công việc. Bác sỹ phẫu thuật chia sẻ tư tưởng cũng như nhận lời khuyên từ anh ta, nhưng không dừng lại ở đó. Phi công phụ chia sẽ với các thành viên trong team về các chức năng cũng như đứng ra làm cầu nối với các team khác. Anh ta hiểu rõ tất cả các dòng code. Anh ta cũng nghiên cứu các kỹ thuật thiết kế có thể thay thế được. Anh ta cũng đóng vai trò đảm bảo cho các vấn đề có liên quan đến bác sỹ phẫu thuật. Anh ta cũng có thể viết code, nhưng nhìn chung thì không chịu trách nhiệm cụ thể cho bất cứ phần code nào.

**Người quản lý - Administrator** Bác sỹ phẫu thuật chính là ông chủ, và đương nhiên anh ta có quyền đưa ra quyết định cuối cùng về các nhân viên cũng như môi trường làm việc,... nhưng anh ta hầu như không dành thời gian vào việc này. Anh ta cần một người quản lý chuyên nghiệp, giúp quản lý tiền, con người, không gian, máy móc và trao đổi với các nhà quản lý của các bộ phận khác trong tổ chức. Baker cho rằng người quản lý chỉ nên tập trung 100% sức lực cho một nhóm, nếu bản thân dự án có các ràng buộc pháp lý phức tạp, báo cáo cũng như yêu cầu về tài chính một cách rõ ràng. Còn không, một người quản lý nên làm việc với 2 nhóm.

**Người chỉnh sửa - Editor** Bác sỹ phẫu thuật có trách nhiệm viết ra tài liệu - rõ ràng nhất mà anh ta có thể viết. Điều này đúng cả với các chi tiết bên trong và bên ngoài. Tuy nhiên, người chỉnh sửa cần dựa theo bản nháp của bác sỹ phẫu thuật, xem xét, biên soạn lại và giải giải thích rõ ràng cho các thành phần trong tài liệu cũng như liên hệ đến các tài liệu khác.

**Hai thư ký - Secretary** Người quản lý và người chỉnh sửa, mỗi người cần một thư ký. Thư ký của người quản lý cần quản lý các tài liệu không liên quan đến sản phẩm.

**Đốc công - Program Clerk** Anh ta có trách nhiệm đảm bảo tất cả các báo cáo kỹ thuật đều được lưu trữ trong thư viện tài liệu của dự án. Anh ta được đào tạo như một thư ký và có trách nhiệm quản lý tài liệu bao gồm cả tài liệu cho máy đọc và cho người dọc.

Tất cả đầu vào của máy tính đều phải qua tay đốc công, người có vai trò lưu trữ lại và đánh chỉ mục nếu cần thiết. Kết quả đưa đến anh ta sẽ là các tập tin đã được đánh chỉ mục. Các phiên bản của sản phẩm đều được tóm tắt lại cho các cuốn sổ và các phiên bản cũ sẽ được cất giữ cẫn thận.

Quan điểm của Mills được hiện thực hoá nhờ sự biến đổi trong suy nghĩ về lập trình "từ nghệ thuật của một cá nhân đến trải nghiệm của tập thể", bằng cách khiến cho tất cả các chương trình máy tính đều được chạy và có thể nhìn thấy được bởi tất cả các thành viên và xem như toàn bộ các chương trình, tài liệu là thuộc về cả team, chứ không thuộc về bất cứ cá nhân riêng biệt nào cả.

**Thợ rèn - Toolsmith** Công việc chỉnh sửa file, chỉnh sửa text, công cụ debug đều có người sẵn sàng đảm nhiệm, và các thành viên trong team hiếm khi phải sử dụng máy hay sửa chữ máy móc của riêng họ. Tuy nhiên, các dịch vụ này cần hoạt động một cách hoàn hảo và độ tin cậy tuyệt đối, bác sỹ phẫu thuật cũng cần chịu trách nhiệm cho điều đó. Do vậy, anh ta cần một người thợ rèn, một người chịu trách nhiệm cho trách nhiệm cho các dịch vụ và công cụ cơ bản, chủ yếu tương tác với máy tính mà các thành viên trong team cần đến. Mỗi team đều cần một người thợ rèn, người có hiểu biết rõ ràng về các công cụ mà bác sỹ phẫu thuật cần. Một cỗ máy tạo ra thiết bị có vai trò quan trọng trong việc chế tạo các thiết bị chuyên dụng, hay các thư viện có khả năng sử dụng ở nhiều nơi.

**Người kiểm thử - Tester** Bác sỹ phẫu thuật cần hàng tá test cases để kiểm tra từ thành phần công việc của anh ta, cũng như kiểm tra toàn bộ dự án. Tester vừa là kẻ địch, người nghĩ ra các test case để kiểm tra các chức năng, vừa là một trợ thủ, người đưa ra các dữ liệu test hàng ngày phục vụ cho công việc debug. Anh ta cũng là người lên kế hoạch kiểm thử và chuẩn bị kịch bản kiểm thử cho các thành phần tích hợp trong dự án.

**Luật sư về ngôn ngữ - Language Lawyer** Theo thời gian, con người nhận ra rằng các chương trình máy tính đều cần từ 1 đến 2 chuyên đóng vai trò hỗ trợ giải quyết các vấn đề khó khăn của ngôn ngữ lập trình. Những chuyên gia này thực sự rất có ích cho dự án và luôn có những lời khuyên rất giá trị. Tài năng của họ khác với bác sỹ phẫu thuật, những người mà đóng vai trò chính như một kỹ sư hệ thống. Luật sư ngôn ngữ đặc biệt hiệu quả trong việc sử dụng ngôn ngữ để làm các công việc khó khăn gây ra nhiều cản trở. Thông thường, họ cần một thời gian ngắn (2 đến 3 ngày) để chọn ra kỹ thuật và cách giải quyết phù hợp. Một luật sự ngôn ngữ có thể hỗ trợ cho 2 đến 3 bác sỹ phẫu thuật.

Đây chính là 10 người có thể đóng góp cho dự án theo những cách khác nhau với vai trò khác nhau để xây dựng một team theo mô hình phẫu thuật.

## Công việc sẽ được vận hành như thế nào

Team vừa được chúng ta thiết kế thoả mãn mong muốn ban đầu của chúng ta theo nhiều hướng khác nhau. 10 người, 7 trong số họ là những chuyên gia cùng nhau xây dựng một sản phẩm, nhưng hệ thống này lại là sản phẩm của 1 cái đầu - hoặc tối đa là 2.

Thử xem xét sự khác biệt giữa một nhóm gồm hai lập trình viên được cấu trúc theo kiểu truyền thống, với team gồm có bác sỹ phẫu thuật và trợ thủ. Trước tiên, ở team truyền thống cả 2 người cùng nhau phân chia công việc, mỗi người chịu trách nhiệm cho thiết kế, cài đặt từng thành phần công việc. Trong nhóm phẫu thuật, cả bác sỹ phẫu thuật và trợ thủ đều chịu trách nhiệm cho tất cả công việc thiết kế và tất cả code. Nó giúp cho công việc có thể phân tán tốt hơn cho các thành viên và đảm bảo chắc chắn tính đúng đắn của công việc.

Thứ hai, trong team truyền thống, các thành viên có vai trò tương đương, và các quyết định khác nhau sẽ được đưa ra thảo luận, đôi khi là thoả hiệp. Do công việc và tài nguyên bị chia làm nhiều phần, sự khác biệt giữa các quyết định bị ảnh hưởng bởi chiến lược chung, tuy nhiên lại được kết hợp lại từ những suy nghĩ khác nhau. Trong team phẫu thuật, không có sự khác biệt đó, và sự khác biệt trong việc đưa ra quyết định giờ chỉ còn là quyết định của bác sỹ phẫu thuật. Hai sự khác biết này - thiếu sự phân chia trong team và quan hệ giữa quản lý cấp cao và phụ tá - khiến team phẫu thuật có thể hoạt động trong một tư tưởng thống nhất.

Do vậy, việc phân chia chức năng giữa các thành viên trong team là chìa khoá cho sự thành công, tạo ra các mô hình giao tiếp đơn giản và không tốn nhiều chi phí trong team.

# Quan điểm cá nhân

40 năm từ khi The mythical man-month ra đời, chương sách này đưa ra một ví dụ từ những năm 60 của thế ký 20. Công cụ, cách lập trình ở thời đó khác rất nhiều so với hiện tại, và vì không hiểu được hết công việc con người thời đó, nên đôi khi tôi cũng không hiểu hết phép ẩn dụ của tác giả, và rất xin lỗi bạn đọc nếu có chỗ nào dịch khó hiểu.

Tuy nhiên, những gì tác giả nêu ra ở đây vẫn rất có ý nghĩa thực tiễn. Thứ nhất, nếu như chương hai nói về việc thêm người vào một dự án đang chậm chỉ làm nó chậm thêm, thì chương này lại yêu cầu việc làm việc với dự án lớn có nhiều người. Không phải lúc nào chúng ta cũng có thể làm việc với team nhỏ và toàn người giỏi, trong các dự án lớn, buộc chúng ta phải làm việc với nhiều người ở các trình độ khác nhau. Khi đó, việc phân chia công việc là hoàn toàn cần thiết.

Mô hình 10 người trong bài viết này tập trung hết vào bác sỹ phẫu thuật, và đặt tất cả trọng trách lên vai 1 con người. Xét về thực tế thì không nên đặt cả hệ thống lên vai 1 người, và team lớn thì nhất định phải chia nhỏ ra thành các sub-team với leder tương ứng. Tuy nhiên, dù gì đi nữa, ý tưởng của tác giả vẫn có lý đúng. Đó là chúng ta luôn cần 1 người đi đầu về kỹ thuật cho cả dự án, với vai trò tổng công trình sư để đưa ra thiết kế tổng quát, viết lên những dòng code ban đầu và đưa ra giải pháp kỹ thuật cho các vấn đề trong team.
avatar
