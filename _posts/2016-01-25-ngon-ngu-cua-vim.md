---
title: "Ngôn ngữ của Vim"
---

## Máy bạn có sẵn Vim, chắc chắn, vậy nó từ đâu ra ?

* `Vim` là một text editor khá phổ biến, ra đời năm 1991 bởi [Bram Moolenaar](http://www.moolenaar.net/vim.html) như một phiên bản mở rộng của `Vi` viết bởi `Bill Joy`. Vim viết tắt của `Vi IMproved`, cái tên đủ cho thấy Vim có những cái tiến đáng kể so với cha đẻ của mình là `Vi`. Một trong những cái tiến đáng nói là việc hỗ trợ nhiều hệ điều hành hơn, highlight code tốt hơn, hỗ trợ split màn hình và các plugin,... Bạn nào muốn tham khảo thêm có thể đọc ở [đây](http://askubuntu.com/questions/418396/what-is-the-difference-between-vi-and-vim).

* Có thể bạn sẽ tự hỏi, trên máy bạn đang cài `vi` hay `vim`. Tôi xin đính chính rằng gần như chắc chắn đó là `vim`, mặc dù bạn thường hay gõ `vi filename`. Hầu hết các bản phân phối của Linux đều coi `vim` là mặc định, một sự thay thế cho `vi`-cái tên khá ngắn gọn. Bạn có thể kiểm tra như sau. Tôi làm trên máy chạy HĐH Ubuntu 14.04.

{% highlight bash %}
$ which vi
/usr/bin/vi
$ ls -l /usr/bin/vi
... /usr/bin/vi -> /etc/alternatives/vi
$ ls -l /etc/alternatives/vi
... /etc/alternatives/vi -> /usr/bin/vim.basic
{% endhighlight %}

  Vậy là chẳng qua `vi` thực chất là `vim.basic` (yaoming)

## Tại sao lại là Vim?

Vim ra đời từ rất lâu và đến nay vẫn còn tồn tại và phát triển với cộng đồng người sử dụng rất lớn. Có thể với nhiều người mới học, Vim là một editor rất khó hiểu. Tuy nhiên trước khi từ chối Vim, nên tìm hiểu tại sao lại có rất nhiều người đã, đang và sẽ sử dụng công cụ này cho editor chính của họ.

Bản thân tôi những ngày đầu dùng Vim, tôi vẫn mang tư tưởng của các "công cụ soạn thảo văn bản" khác. Đó là việc sử dụng các phím mũi tên trên bàn phím cho việc di chuyển, phím `home`, `end` để đến đầu và cuối dòng. Bôi đen và copy paste bằng `Ctrl+C`, `Ctrl+V`. Đôi khi dùng chuột cho các thao tác khác và dùng chuột để bật menu bar.

Việc sử dụng tư duy này trong Vim sẽ hoàn toàn thất bại, bởi Vim khác hoàn toàn các công cụ khác. Chỉ riêng việc dùng `j-k-l-;` cho việc điều khiển con trỏ có thể khiến bạn phát điên lên trong lần đầu gặp gỡ. Tuy nhiên, tin tôi đi, Vim được thiết kế tuyệt vời cho ... bàn tay bạn. Bạn không phải với tay xa hàng bước dài chỉ để tìm nút sang phải `->` hay với xa hơn nữa để đến nút `End`... Và còn nữa, nếu bạn quen với Vim, thì sẽ có ngày bạn muốn mọi thứ là `Vim`, và bạn sẽ như tôi, gõ `j-k-l-;` để lên xuống trong ...Microsoft Word (facepalm)

Vậy, đơn giản là, muốn làm việc với Vim, bạn cần thay đổi suy nghĩ của mình về "công cụ soạn thảo văn bản". Hãy học làm quen với nó, và dần dần thấy được sự tuyệt vời của nó.

Bài viết này ngoài mục tiêu giới thiệu Vim còn mong muốn các bạn có thể làm quen và sử dụng công cụ này một cách thành thạo.

## Ngôn ngữ của Vim

Như trên đã nói, bạn cần thay đổi cách nghĩ của mình khi làm việc với Vim. Một cách áp đặt hơn, khi làm việc với Vim, hãy "nghĩ" trước khi "làm". Tôi đã từng thấy những người dùng phím mũi tên hay chuột để di chuyển từ giữa đến cuối một dòng, cốt chỉ để xóa phần chữ được đánh dấu đi. Tôi đã thấy những người dùng chuột bôi đen cả 1 đoạn văn bản, cốt chỉ để xóa phần code giữa 2 dấu `{ }` đi và thay bởi đoạn code khác. Bạn có biết rằng với Vim, bạn chỉ cần gõ không quá 3 nút là xong công việc đó không (?)

![vim-language.gif](/assets/img/post/vim-language.gif)

Ở hình trên tôi làm 2 việc. Một là xóa phần ký tự thừa của dòng 11, 2 là thay đổi dòng 18-21 bằng đoạn text `// Changed`. Với công việc thứ nhất, tôi gõ `Shift + D`. Công việc thứ 2, tôi gõ `ci{`, không có một phím mũi tên nào. Chắc bạn nghĩ luôn, "Ui dzời, chắc nhớ mấy cái hot key vớ vẩn đi lòe đời =))". Cơ mà, uh đúng là nhớ đấy, làm sao =)) Tôi sẽ dạy bạn cách học, rồi nhớ, rồi làm y như tôi. Điều duy nhất bạn cần biết là 1 chút ... tiếng Anh (honho)

Giớ chúng ta đến với ngôn ngữ của Vim.

> Lưu ý khi thực hiện theo các bảng hướng dẫn dưới đây

> * Vùng được `highlight` là vị trí con trỏ.

> * Để tránh bị ảnh hưởng bởi các plugin khác bạn cài, tôi khuyến cáo bạn bật vim mà không cần file `.vimrc`. Cú pháp như sau `vi -u NONE filename`

### Động từ - Verb

Ừ thì ngôn ngữ phải có danh từ, động từ, tính từ, trạng từ đúng không, có mấy cái này là thành ngôn ngữ được rồi (lủng củng 1 tí nhưng nói là hiểu hết :v )

Bắt đầu từ động từ, sau đây là bảng động từ của Vim

| Động từ | Tiếng Anh | Ý nghĩa |
|---------|-----------|---------|
| `y` | Yank | Nghĩa tương tự copy |
| `c` | Change/Cut | Thay đổi nội dung và cho phép chèn nội dung mới vào |
| `d` | Delete | Xóa |
| `p` | Paste | Dán |
| `f` | Find | Tìm kiếm và di chuyển con trỏ ký tự được tìm thấy |
| `t` | To | Tìm kiếm và di chuyển con trỏ đến trước ký tự được tìm thấy |
| `i` | Insert | Chèn vào trước vị trí con trỏ hiện tại. Di chuyển đến insert mode |
| `a` | Append | Chèn vào sau vị trí con trỏ hiện tại. Di chuyển đến insert mode |

Điều đó có nghĩa nếu bạn gõ các ký tự kia trong chế độ normal mode thì bạn sẽ nhận được kết quả theo đúng action mình muốn. Ví dụ sau.

| Thao tác | Ý nghĩa | Trạng thái văn bản |
|---------|---------|--------------------|
| -       | -       | This is a `text`. Another here. |
| `y`       | `Yank`. Copy vùng được chọn `text` | This is a tex`t`. Another here. |
| `fr`       | `Find r`. Di chuyển đến chữ `r` | This is a text. Anothe`r` here. |
| `p`       | `Paste`. Dán vùng được chọn vào | This is a text. Anothertex`t` here. |

Các động từ trên, ngoài dạng chữ thường (`lower case`) thì khi viết chữ hoa (`upper case`), nó lại có ý nghĩa - bổ sung cho chế độ chữ thường. Ví dụ `D` thay vì `d` sẽ là xóa cả dòng tính từ vị trí hiện tại. `I` thay vì `i` sẽ là insert vào vị trí đầu tiên của dòng. `A` thay vì `a` sẽ là append vào vị trí cuối cùng của dòng,...

### Danh từ - Noun

| Danh từ | Tiếng Anh |
|---------|-----------|
| `w` | Word |
| `s` | Sentence |
| `p` | Paragraph |
| `b` | Blog/parentheses |
| `t` | Tag |

Trên đây là một số danh từ của Vim. Hay xem cách hoạt động qua ví dụ sau.

| Thao tác | Ý nghĩa | Trạng thái văn bản |
|---------|---------|--------------------|
| -       | -       | This is a `t`ext. It is too short. |
| `yw`       | `Yank Word`. Copy từ bắt đầu từ vị trì con trỏ. | This is a `t`ext. It is too short. |
| `4w`       | Di chuyển 4 word | This is a text. It is `t`oo short. |
| `Shift + p`       | `Paste` before. Dán vào trước ký tự hiện tại | This is a text. It is texttoo short. |
| `l`       | Di chuyển sang phải | This is a text. It is text`t`oo short. |
| `d2w`       | Xóa 2 từ từ vị trí con trỏ hiện tại | This is a text. It is text`.` |

### Trạng từ - Modifier

| Trạng từ | Tiếng Anh |
|---------|-----------|
| `a` | Around |
| `i` | Inside |

Để ý thì ở trên kia chúng ta cũng có `i` và `a`, nhưng với tư cách là động từ. Còn ở đây `i`, `a` lại có nghĩa khác là trạng từ. Mà trạng từ thì cần có động từ, do vậy chúng ta sẽ ghép động từ với trạng từ và danh từ để thành hành động có nghĩa.

| Thao tác | Ý nghĩa | Trạng thái văn bản |
|---------|---------|--------------------|
| -       | -       | T`h`is is a 'text'. It is too short. |
| `ciw`       | `Change Inside Word`. Thay đổi từ bao gồm ký tự ở vị trí con trỏ. Di chuyển đến insert mode | ` ` is a 'text'. It is too short. |
| `That`       | Nhập text | That` `is a 'text'. It is too short. |
| `Esc`       | Di chuyển đến normal mode | Tha`t` is a 'text'. It is too short. |
| `ft`       | `Find t`. Di chuyển đến ký tự `t` | That is a '`t`ext'. It is too short.  |
| `da'`       | `Delete around '`. Xóa phần bên trong `'` và chính nó. | That is a. It is too short. |

Vậy là đến đây bạn đã thấy trong Vim có đầy đủ động từ, trạng từ, danh từ. Cũng như việc ghép từ thành câu trong ngôn ngữ tự nhiên, việc ghép các động từ, trạng từ, danh từ cùng với các con số trên bàn phím, cho chúng ta sẽ có rất, rất nhiều hành động giúp cải thiện tốc độ làm việc đáng kể. Điều quan trọng là bạn phải học cách "nghĩ" khi làm việc với Vim. Thay vì chỉ dùng con trỏ, di chuyển lên xuống, đôi khi là trong vô thức...

## Vim Plugin - Vim Surround

Phần tiếp theo sẽ là giới thiệu plugin. Có vẻ bạn thấy "Trò này cổ rồi, giống các bài viết khác về Vim vãi" =))

Thực sự tôi dùng khá nhiều plugin, nhưng ở đây tôi sẽ chỉ giới thiệu ở đây plugin tôi thấy hay nhất và tuân thủ đúng nhất ngôn ngữ của Vim. Bạn có thể cài đặt Vim Surround theo hướng dẫn ở đây [https://github.com/tpope/vim-surround](https://github.com/tpope/vim-surround).

Với plugin này, bạn cần nhớ thêm 1 trạng từ nữa, đó là `s` - surround. Tiếp theo, bạn chỉ cần tuân thủ ngôn ngữ của Vim.

Hay tưởng tượng một bài toàn đơn giản, là bạn muốn đổi ký tự xung quanh string của bạn, thay vì `"` thì là `'`. Bạn làm gì với nó. Tôi đã thấy rất nhiều người phải xóa ký tự `"` rồi di chuột đến vị trí dấu `"` còn lại và đổi thành `'`. Vậy với Vim Surround bạn làm gì.

```
"This is a text" -> ??? -> 'This is a text'
```

Câu trả lời khá đơn giản
`cs'"` - Change Surround ' ". Thay đổi tứ tự bao quanh, từ `'` thành `"`.

Sau đây là một demo sử dụng Vim Surround cho edit tập tin HTML. Khá hay :D Các bạn có thể tham khảo và đoán xem đoạn ký tự nào được dùng.

![vim-surround.gif](/assets/img/post/vim-surround.gif)

## Kết luận

Theo như nhận định của trang [whileimautomaton.net](http://whileimautomaton.net/2008/11/vimm3/operator), có 7 level đối với 1 người dùng Vim. Tác giả đang tự thấy mình ở ngấp nghé giữa level 3 và 4 khi chưa hoàn toàn sử dụng hết điểm mạnh của Text Object và còn phụ thuộc vào Visual Mode. Hay nói đơn giản là bản thân tôi vẫn chưa thành thạo "Ngôn ngữ của Vim". Bài viết nhằm mục đích chia sẻ với bạn đọc một cái nhìn khác về Vim, những gì tôi biết và rất mong có thể giúp đỡ bạn trong việc học editor tuyệt vời này :D

## Tài liệu tham khảo

* [Learn to speak vim – verbs, nouns, and modifiers!](http://yanpritzker.com/2011/12/16/learn-to-speak-vim-verbs-nouns-and-modifiers/)

* [A vim Tutorial and Primer](https://danielmiessler.com/study/vim/?fb_ref=118ef0e03ab54c0d8197214328648a68-Hackernews)



