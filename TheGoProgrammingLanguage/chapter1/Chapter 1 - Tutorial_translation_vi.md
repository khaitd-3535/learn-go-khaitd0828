# Chương 1: Hướng dẫn

Chương này là một chuyến tham quan các thành phần cơ bản của Go. Chúng tôi hy vọng sẽ cung cấp đủ thông tin và ví dụ để bạn có thể bắt đầu và làm những điều hữu ích một cách nhanh chóng nhất có thể. Các ví dụ ở đây, và thực sự trong toàn bộ cuốn sách, đều nhắm đến các tác vụ mà bạn có thể phải làm trong thế giới thực. Trong chương này, chúng tôi sẽ cố gắng cho bạn thấy được sự đa dạng của các chương trình mà người ta có thể viết bằng Go, từ xử lý tệp đơn giản và một chút đồ họa đến các máy khách và máy chủ Internet đồng thời. Chúng tôi chắc chắn sẽ không giải thích mọi thứ trong chương đầu tiên, nhưng việc nghiên cứu các chương trình như vậy bằng một ngôn ngữ mới có thể là một cách hiệu quả để bắt đầu.

Khi bạn học một ngôn ngữ mới, có một xu hướng tự nhiên là viết mã theo cách bạn đã viết nó bằng một ngôn ngữ bạn đã biết. Hãy nhận thức về khuynh hướng này khi bạn học Go và cố gắng tránh nó. Chúng tôi đã cố gắng minh họa và giải thích cách viết mã Go tốt, vì vậy hãy sử dụng mã ở đây như một hướng dẫn khi bạn viết mã của riêng mình.

### 1.1. Hello, World

Chúng ta sẽ bắt đầu với ví dụ "hello, world" đã trở thành truyền thống, xuất hiện ở đầu cuốn sách "Ngôn ngữ lập trình C", xuất bản năm 1978. C là một trong những ảnh hưởng trực tiếp nhất đến Go, và "hello, world" minh họa một số ý tưởng trung tâm.

`gopl.io/ch1/helloworld`

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, BF")
}
```

Go là một ngôn ngữ biên dịch. Bộ công cụ Go chuyển đổi một chương trình nguồn và những thứ nó phụ thuộc vào thành các chỉ thị bằng ngôn ngữ máy gốc của máy tính. Các công cụ này được truy cập thông qua một lệnh duy nhất gọi là `go` có một số lệnh con. Lệnh con đơn giản nhất trong số này là `run`, nó biên dịch mã nguồn từ một hoặc nhiều tệp nguồn có tên kết thúc bằng `.go`, liên kết nó với các thư viện, sau đó chạy tệp thực thi kết quả. (Chúng tôi sẽ sử dụng `$` làm dấu nhắc lệnh trong suốt cuốn sách.)

```sh
$ go run helloworld.go
```

Không ngạc nhiên, lệnh này in ra

```
Hello, BF
```

Go xử lý Unicode một cách tự nhiên, vì vậy nó có thể xử lý văn bản bằng tất cả các ngôn ngữ trên thế giới.

Nếu chương trình không chỉ là một thử nghiệm dùng một lần, có lẽ bạn sẽ muốn biên dịch nó một lần và lưu kết quả đã biên dịch để sử dụng sau. Điều đó được thực hiện bằng `go build`:

```sh
$ go build helloworld.go
```

Lệnh này tạo ra một tệp nhị phân thực thi có tên là `helloworld` có thể chạy bất cứ lúc nào mà không cần xử lý thêm:

```sh
$ ./helloworld
Hello, BF
```

Chúng tôi đã gắn nhãn cho mỗi ví dụ quan trọng như một lời nhắc nhở rằng bạn có thể lấy mã từ kho mã nguồn của cuốn sách tại gopl.io:

`gopl.io/ch1/helloworld`

Nếu bạn chạy `go get gopl.io/ch1/helloworld`, nó sẽ tìm nạp mã nguồn và đặt nó vào thư mục tương ứng. Có thêm thông tin về chủ đề này trong Mục 2.6 và Mục 10.7.

Bây giờ chúng ta hãy nói về chính chương trình. Mã Go được tổ chức thành các gói (packages), tương tự như thư viện (libraries) hoặc mô-đun (modules) trong các ngôn ngữ khác. Một gói bao gồm một hoặc nhiều tệp nguồn `.go` trong một thư mục duy nhất xác định những gì gói đó làm. Mỗi tệp nguồn bắt đầu bằng một khai báo `package`, ở đây là `package main`, cho biết tệp thuộc về gói nào, theo sau là danh sách các gói khác mà nó nhập (import), và sau đó là các khai báo của chương trình được lưu trữ trong tệp đó.

Thư viện chuẩn của Go có hơn 100 gói cho các tác vụ phổ biến như nhập và xuất (input and output), sắp xếp và xử lý văn bản. Ví dụ, gói `fmt` chứa các hàm để in đầu ra đã định dạng và quét đầu vào. `Println` là một trong những hàm đầu ra cơ bản trong `fmt`; nó in một hoặc nhiều giá trị, được phân tách bằng dấu cách, với một ký tự dòng mới ở cuối để các giá trị xuất hiện dưới dạng một dòng đầu ra duy nhất.

Gói `main` là đặc biệt. Nó định nghĩa một chương trình thực thi độc lập, không phải là một thư viện. Trong gói `main`, hàm `main` cũng đặc biệt—đó là nơi bắt đầu thực thi chương trình. Bất cứ điều gì `main` làm là những gì chương trình làm. Tất nhiên, `main` thường sẽ gọi các hàm trong các gói khác để thực hiện phần lớn công việc, chẳng hạn như hàm `fmt.Println`.

Chúng ta phải cho trình biên dịch biết những gói nào cần thiết cho tệp nguồn này; đó là vai trò của khai báo `import` theo sau khai báo `package`. Chương trình "hello, world" chỉ sử dụng một hàm từ một gói khác, nhưng hầu hết các chương trình sẽ nhập nhiều gói hơn.
Bạn phải nhập chính xác các gói bạn cần. Một chương trình sẽ không biên dịch nếu thiếu các `import` hoặc nếu có những `import` không cần thiết. Yêu cầu nghiêm ngặt này ngăn chặn việc tích tụ các tham chiếu đến các gói không sử dụng khi chương trình phát triển.
Các khai báo `import` phải theo sau khai báo `package`. Sau đó, một chương trình bao gồm các khai báo của các hàm, biến, hằng và kiểu (được giới thiệu bởi các từ khóa `func`, `var`, `const`, và `type`); phần lớn, thứ tự của các khai báo không quan trọng. Chương trình này ngắn gọn nhất có thể vì nó chỉ khai báo một hàm, và hàm này lại chỉ gọi một hàm khác. Để tiết kiệm không gian, đôi khi chúng tôi sẽ không hiển thị các khai báo `package` và `import` khi trình bày các ví dụ, nhưng chúng có trong tệp nguồn và phải có ở đó để biên dịch mã.

Một khai báo hàm bao gồm từ khóa `func`, tên của hàm, một danh sách tham số (rỗng đối với `main`), một danh sách kết quả (cũng rỗng ở đây), và thân hàm—các câu lệnh xác định những gì nó làm—được đặt trong dấu ngoặc nhọn. Chúng ta sẽ xem xét kỹ hơn về các hàm trong Chương 5.

Go không yêu cầu dấu chấm phẩy ở cuối các câu lệnh hoặc khai báo, ngoại trừ trường hợp có hai hoặc nhiều câu lệnh xuất hiện trên cùng một dòng. Thực tế, các dòng mới theo sau một số token nhất định được chuyển đổi thành dấu chấm phẩy, vì vậy vị trí của các dòng mới rất quan trọng để phân tích cú pháp mã Go một cách chính xác. Ví dụ, dấu ngoặc nhọn mở `{` của hàm phải nằm trên cùng một dòng với phần cuối của khai báo `func`, không phải trên một dòng riêng, và trong biểu thức `x + y`, một dòng mới được phép sau nhưng không phải trước toán tử `+`.

Go có một lập trường mạnh mẽ về định dạng mã. Công cụ `gofmt` viết lại mã thành định dạng chuẩn, và lệnh con `fmt` của công cụ `go` áp dụng `gofmt` cho tất cả các tệp trong gói được chỉ định, hoặc các tệp trong thư mục hiện tại theo mặc định. Tất cả các tệp nguồn Go trong cuốn sách đã được chạy qua `gofmt`, và bạn nên tập thói quen làm điều tương tự cho mã của riêng mình. Việc tuyên bố một định dạng chuẩn bằng mệnh lệnh sẽ loại bỏ rất nhiều cuộc tranh luận vô nghĩa về những điều nhỏ nhặt và, quan trọng hơn, cho phép một loạt các biến đổi mã nguồn tự động mà sẽ không khả thi nếu cho phép định dạng tùy ý.

Nhiều trình soạn thảo văn bản có thể được cấu hình để chạy `gofmt` mỗi khi bạn lưu tệp, để mã nguồn của bạn luôn được định dạng đúng cách. Một công cụ liên quan, `goimports`, ngoài ra còn quản lý việc chèn và xóa các khai báo `import` khi cần thiết. Nó không phải là một phần của bản phân phối chuẩn nhưng bạn có thể lấy nó bằng lệnh này:

`$ go get golang.org/x/tools/cmd/goimports`

Đối với hầu hết người dùng, cách thông thường để tải xuống và xây dựng các gói, chạy các bài kiểm tra của chúng, hiển thị tài liệu của chúng, v.v., là bằng công cụ `go`, mà chúng ta sẽ xem xét trong Mục 10.7.

### 1.2. Đối số dòng lệnh

Hầu hết các chương trình xử lý một số đầu vào để tạo ra một số đầu ra; đó là định nghĩa của tính toán. Nhưng làm thế nào một chương trình nhận được dữ liệu đầu vào để hoạt động? Một số chương trình tự tạo dữ liệu của riêng chúng, nhưng thường xuyên hơn, đầu vào đến từ một nguồn bên ngoài: một tệp, một kết nối mạng, đầu ra của một chương trình khác, người dùng tại bàn phím, các đối số dòng lệnh hoặc những thứ tương tự. Một vài ví dụ tiếp theo sẽ thảo luận về một số lựa chọn thay thế này, bắt đầu với các đối số dòng lệnh.

Gói `os` cung cấp các hàm và các giá trị khác để xử lý hệ điều hành theo cách độc lập với nền tảng. Các đối số dòng lệnh có sẵn cho một chương trình trong một biến có tên `Args` là một phần của gói `os`; do đó, tên của nó ở bất cứ đâu bên ngoài gói `os` là `os.Args`.

Biến `os.Args` là một slice các chuỗi. Slice là một khái niệm cơ bản trong Go, và chúng ta sẽ nói nhiều hơn về chúng sớm. Hiện tại, hãy nghĩ về một slice như một chuỗi các phần tử mảng có kích thước động, nơi các phần tử riêng lẻ có thể được truy cập dưới dạng `s[i]` và một chuỗi con liên tục dưới dạng `s[m:n]`. Số lượng phần tử được cho bởi `len(s)`. Như trong hầu hết các ngôn ngữ lập trình khác, tất cả các chỉ mục trong Go đều sử dụng các khoảng nửa mở bao gồm chỉ mục đầu tiên nhưng loại trừ chỉ mục cuối cùng, bởi vì nó đơn giản hóa logic. Ví dụ, slice `s[m:n]`, trong đó `0 ≤ m ≤ n ≤ len(s)`, chứa `n-m` phần tử.

Phần tử đầu tiên của `os.Args`, `os.Args[0]` là tên của chính lệnh đó; các phần tử khác là các đối số được cung cấp cho chương trình khi nó bắt đầu thực thi. Một biểu thức slice có dạng `s[m:n]` tạo ra một slice tham chiếu đến các phần tử từ `m` đến `n-1`, vì vậy các phần tử chúng ta cần cho ví dụ tiếp theo là những phần tử trong slice `os.Args[1:len(os.Args)]`. Nếu `m` hoặc `n` bị bỏ qua, nó mặc định là `0` hoặc `len(s)` tương ứng, vì vậy chúng ta có thể viết tắt slice mong muốn là `os.Args[1:]`.

Đây là một triển khai của lệnh `echo` của Unix, lệnh này in các đối số dòng lệnh của nó trên một dòng duy nhất. Nó nhập hai gói, được cung cấp dưới dạng danh sách trong ngoặc đơn thay vì các khai báo `import` riêng lẻ. Cả hai dạng đều hợp lệ, nhưng theo quy ước, dạng danh sách được sử dụng. Thứ tự của các `import` không quan trọng; công cụ `gofmt` sắp xếp tên gói theo thứ tự bảng chữ cái. (Khi có nhiều phiên bản của một ví dụ, chúng tôi thường sẽ đánh số chúng để bạn có thể chắc chắn chúng tôi đang nói về phiên bản nào.)

`gopl.io/ch1/echo1`

```go
// Echo1 prints its command-line arguments.
package main
import (
    "fmt"
    "os"
)

func main() {
    var s, sep string
    for i := 1; i < len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Println(s)
}
```

Các comment (bình luận) bắt đầu bằng `//`. Tất cả văn bản từ `//` đến cuối dòng là phần bình luận dành cho lập trình viên và bị trình biên dịch bỏ qua. Theo quy ước, chúng tôi mô tả mỗi gói (package) bằng một bình luận ngay trước khai báo gói đó; đối với gói `main`, bình luận này là một hoặc nhiều câu hoàn chỉnh mô tả chương trình tổng thể.

Khai báo `var` khai báo hai biến `s` và `sep`, thuộc kiểu `string`. Một biến có thể được khởi tạo như một phần khai báo của nó. Nếu nó không được khởi tạo rõ ràng, nó sẽ được khởi tạo ngầm định với *giá trị zero* (zero value) cho kiểu của nó, đó là `0` cho các kiểu số và chuỗi rỗng `""` cho các chuỗi. Do đó, trong ví dụ này, khai báo khởi tạo ngầm định `s` và `sep` thành chuỗi rỗng. Chúng ta sẽ nói thêm về biến và khai báo trong Chương 2.

Đối với các số, Go cung cấp các toán tử số học và logic thông thường. Tuy nhiên, khi áp dụng cho chuỗi, toán tử `+` nối các giá trị, vì vậy biểu thức
`sep + os.Args[i]`
đại diện cho sự nối của các chuỗi `sep` và `os.Args[i]`. Câu lệnh chúng ta đã sử dụng trong chương trình,
`s += sep + os.Args[i]`
là một *câu lệnh gán* (assignment statement) nối giá trị cũ của `s` với `sep` và `os.Args[i]` và gán lại nó cho `s`; nó tương đương với
`s = s + sep + os.Args[i]`
Toán tử `+=` là một toán tử gán. Mỗi toán tử số học và logic như `+` hoặc `*` đều có một toán tử gán tương ứng.

Chương trình `echo` có thể đã in đầu ra của nó trong một vòng lặp từng phần một, nhưng phiên bản này thay vào đó xây dựng một chuỗi bằng cách liên tục thêm văn bản mới vào cuối. Chuỗi `s` bắt đầu cuộc sống rỗng, nghĩa là, với giá trị `""`, và mỗi chuyến đi qua vòng lặp thêm một số văn bản vào nó; sau lần lặp đầu tiên, một dấu cách cũng được chèn vào để khi vòng lặp kết thúc, có một khoảng trắng giữa mỗi đối số. Đây là một quá trình bậc hai (quadratic process) có thể tốn kém nếu số lượng đối số lớn, nhưng đối với `echo`, điều đó khó có thể xảy ra. Chúng tôi sẽ hiển thị một số phiên bản cải tiến của `echo` trong chương này và chương tiếp theo để giải quyết mọi sự kém hiệu quả thực sự.

Biến chỉ số vòng lặp `i` được khai báo trong phần đầu tiên của vòng lặp `for`. Ký hiệu `:=` là một phần của *khai báo biến ngắn gọn* (short variable declaration), một câu lệnh khai báo một hoặc nhiều biến và cung cấp cho chúng các kiểu thích hợp dựa trên các giá trị khởi tạo; có thêm về điều này trong chương tiếp theo.

Câu lệnh tăng `i++` cộng 1 vào `i`; nó tương đương với `i += 1` mà lần lượt tương đương với `i = i + 1`. Có một câu lệnh giảm tương ứng `i--` trừ đi 1. Đây là các câu lệnh, không phải biểu thức như trong hầu hết các ngôn ngữ thuộc họ C, vì vậy `j = i++` là không hợp lệ, và chúng chỉ là hậu tố (postfix), vì vậy `--i` cũng không hợp lệ.

Vòng lặp `for` là câu lệnh vòng lặp duy nhất trong Go. Nó có một số dạng, một trong số đó được minh họa ở đây:

```go
for initialization; condition; post {
    // zero or more statements
}
```

Dấu ngoặc đơn không bao giờ được sử dụng xung quanh ba thành phần của vòng lặp `for`. Tuy nhiên, dấu ngoặc nhọn là bắt buộc, và dấu ngoặc nhọn mở phải nằm trên cùng một dòng với câu lệnh `post`. 

Câu lệnh `initialization` (khởi tạo) tùy chọn được thực thi trước khi vòng lặp bắt đầu. Nếu nó hiện diện, nó phải là một câu lệnh đơn giản, nghĩa là, một khai báo biến ngắn gọn, một câu lệnh tăng hoặc gán, hoặc một lời gọi hàm. `condition` (điều kiện) là một biểu thức boolean được đánh giá ở đầu mỗi lần lặp của vòng lặp; nếu nó đánh giá là `true`, các câu lệnh được điều khiển bởi vòng lặp sẽ được thực thi. Câu lệnh `post` được thực thi sau thân vòng lặp, sau đó điều kiện được đánh giá lại. Vòng lặp kết thúc khi điều kiện trở thành `false`.

Bất kỳ phần nào trong số này đều có thể bị bỏ qua. Nếu không có `initialization` và không có `post`, các dấu chấm phẩy cũng có thể bị bỏ qua:

```go
// a traditional "while" loop
for condition {
    // ...
}
```

Nếu điều kiện bị bỏ qua hoàn toàn trong bất kỳ dạng nào trong số này, ví dụ trong
```go
// a traditional infinite loop
for {
    // ...
}
```
vòng lặp là vô hạn, mặc dù các vòng lặp dạng này có thể bị chấm dứt theo một cách khác, như câu lệnh `break` hoặc `return`.

Một dạng khác của vòng lặp `for` lặp qua một *phạm vi* (range) các giá trị từ một kiểu dữ liệu như chuỗi hoặc slice. Để minh họa, đây là phiên bản thứ hai của `echo`:

`gopl.io/ch1/echo2`

```go
// Echo2 prints its command-line arguments.
package main

import (
    "fmt"
    "os"
)

func main() {
    s, sep := "", ""
    for _, arg := range os.Args[1:] {
        s += sep + arg
        sep = " "
    }
    fmt.Println(s)
}
```

Trong mỗi lần lặp của vòng lặp, `range` tạo ra một cặp giá trị: chỉ số (index) và giá trị của phần tử tại chỉ số đó. Trong ví dụ này, chúng ta không cần chỉ số, nhưng cú pháp của vòng lặp `range` yêu cầu nếu chúng ta xử lý phần tử, chúng ta cũng phải xử lý chỉ số. Một ý tưởng là gán chỉ số cho một biến tạm thời rõ ràng như `temp` và bỏ qua giá trị của nó, nhưng Go không cho phép các biến cục bộ không được sử dụng, vì vậy điều này sẽ dẫn đến lỗi biên dịch.

Giải pháp là sử dụng **định danh trống** (blank identifier), tên của nó là `_` (nghĩa là một dấu gạch dưới). Định danh trống có thể được sử dụng bất cứ khi nào cú pháp yêu cầu tên biến nhưng logic chương trình thì không, ví dụ để loại bỏ một chỉ số vòng lặp không mong muốn khi chúng ta chỉ yêu cầu giá trị phần tử. Hầu hết các lập trình viên Go có lẽ sẽ sử dụng `range` và `_` để viết chương trình `echo` như trên, vì việc lập chỉ mục trên `os.Args` là ngầm định, không phải rõ ràng, và do đó dễ thực hiện chính xác hơn.

Phiên bản này của chương trình sử dụng khai báo biến ngắn gọn để khai báo và khởi tạo `s` và `sep`, nhưng chúng ta cũng có thể khai báo các biến một cách riêng biệt. Có một vài cách để khai báo một biến chuỗi; tất cả chúng đều tương đương:

```go
s := ""
var s string
var s = ""
var s string = ""
```

Tại sao bạn nên thích dạng này hơn dạng khác? Dạng đầu tiên, khai báo biến ngắn gọn, là gọn gàng nhất, nhưng nó chỉ có thể được sử dụng bên trong một hàm, không dành cho các biến cấp gói (package-level). Dạng thứ hai dựa vào khởi tạo mặc định cho giá trị zero của chuỗi, là `""`. Dạng thứ ba hiếm khi được sử dụng trừ khi khai báo nhiều biến. Dạng thứ tư thể hiện rõ ràng về kiểu của biến, điều này là dư thừa khi nó giống với kiểu của giá trị ban đầu nhưng cần thiết trong các trường hợp khác khi chúng không cùng kiểu. Trong thực tế, bạn nên thường xuyên sử dụng một trong hai dạng đầu tiên, với khởi tạo rõ ràng để nói rằng giá trị ban đầu là quan trọng và khởi tạo ngầm định để nói rằng giá trị ban đầu không quan trọng.

Như đã lưu ý ở trên, mỗi lần vòng lặp chạy, chuỗi `s` nhận nội dung hoàn toàn mới. Câu lệnh `+=` tạo ra một chuỗi mới bằng cách nối chuỗi cũ, một ký tự khoảng trắng và đối số tiếp theo, sau đó gán chuỗi mới cho `s`. Nội dung cũ của `s` không còn được sử dụng nữa, vì vậy chúng sẽ được thu gom rác (garbage-collected) vào thời điểm thích hợp.

Nếu lượng dữ liệu liên quan lớn, điều này có thể tốn kém. Một giải pháp đơn giản và hiệu quả hơn là sử dụng hàm `Join` từ gói `strings`:

`gopl.io/ch1/echo3`

```go
func main() {
    fmt.Println(strings.Join(os.Args[1:], " "))
}
```

Cuối cùng, nếu chúng ta không quan tâm đến định dạng mà chỉ muốn xem các giá trị, có lẽ để gỡ lỗi, chúng ta có thể để `Println` định dạng kết quả cho chúng ta:

```go
fmt.Println(os.Args[1:])
```

Đầu ra của câu lệnh này giống như những gì chúng ta nhận được từ `strings.Join`, nhưng có dấu ngoặc vuông bao quanh. Bất kỳ slice nào cũng có thể được in theo cách này.

**Bài tập 1.1:** Sửa đổi chương trình `echo` để in cả `os.Args[0]`, tên của lệnh đã gọi nó.

**Bài tập 1.2:** Sửa đổi chương trình `echo` để in chỉ số và giá trị của mỗi đối số của nó, mỗi đối số trên một dòng.

**Bài tập 1.3:** Thử nghiệm để đo lường sự khác biệt trong thời gian chạy giữa các phiên bản có khả năng không hiệu quả của chúng ta và phiên bản sử dụng `strings.Join`. (Mục 1.6 minh họa một phần của gói `time`, và Mục 11.4 trình bày cách viết các bài kiểm tra benchmark để đánh giá hiệu suất một cách hệ thống.)

### 1.3. Tìm các dòng lặp lại

Các chương trình sao chép tệp, in ấn, tìm kiếm, sắp xếp, đếm và tương tự đều có cấu trúc tương tự: một vòng lặp qua đầu vào, thực hiện một số tính toán trên mỗi phần tử và tạo đầu ra ngay lập tức hoặc ở cuối. Chúng tôi sẽ giới thiệu ba biến thể của một chương trình có tên là `dup`; nó được truyền cảm hứng một phần bởi lệnh `uniq` của Unix, lệnh này tìm kiếm các dòng trùng lặp liền kề. Các cấu trúc và gói được sử dụng là các mô hình có thể dễ dàng thích ứng.

Phiên bản đầu tiên của `dup` in mỗi dòng xuất hiện nhiều hơn một lần trong đầu vào tiêu chuẩn, trước đó là số lần xuất hiện của nó. Chương trình này giới thiệu câu lệnh `if`, kiểu dữ liệu `map`, và gói `bufio`.

`gopl.io/ch1/dup1`

```go
// Dup1 prints the text of each line that appears more than
// once in the standard input, preceded by its count.
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    counts := make(map[string]int)
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        counts[input.Text()]++
    }
    // NOTE: ignoring potential errors from input.Err()
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```

Giống như với `for`, dấu ngoặc đơn không bao giờ được sử dụng xung quanh điều kiện trong câu lệnh `if`, nhưng dấu ngoặc nhọn là bắt buộc cho thân hàm. Có thể có một phần `else` tùy chọn được thực thi nếu điều kiện là `false`.

Một `map` giữ một tập hợp các cặp khóa/giá trị (key/value) và cung cấp các thao tác thời gian hằng số để lưu trữ, truy xuất hoặc kiểm tra một mục trong tập hợp. Khóa (key) có thể thuộc bất kỳ kiểu nào mà các giá trị của nó có thể so sánh được bằng toán tử `==`, với chuỗi (string) là ví dụ phổ biến nhất; giá trị (value) có thể thuộc bất kỳ kiểu nào. Trong ví dụ này, các khóa là chuỗi và các giá trị là số nguyên (`int`). Hàm tích hợp `make` tạo ra một map rỗng mới; nó cũng có các công dụng khác. Map được thảo luận chi tiết trong Mục 4.3.

Mỗi khi `dup` đọc một dòng đầu vào, dòng đó được sử dụng làm khóa trong map và giá trị tương ứng được tăng lên. Câu lệnh `counts[input.Text()]++` tương đương với hai câu lệnh sau:

```go
line := input.Text()
counts[line] = counts[line] + 1
```

Không thành vấn đề nếu map chưa chứa khóa đó. Lần đầu tiên nhìn thấy một dòng mới, biểu thức `counts[line]` ở phía bên phải đánh giá thành giá trị zero cho kiểu của nó, là `0` cho kiểu `int`.

Để in kết quả, chúng ta sử dụng một vòng lặp `for` dựa trên `range` khác, lần này là trên map `counts`. Như trước đây, mỗi lần lặp tạo ra hai kết quả, một khóa và giá trị của phần tử map cho khóa đó. Thứ tự lặp của map không được chỉ định, nhưng trong thực tế nó là ngẫu nhiên, thay đổi từ lần chạy này sang lần chạy khác. Thiết kế này là có ý đồ, vì nó ngăn cản các chương trình dựa dẫm vào bất kỳ thứ tự cụ thể nào mà không được đảm bảo.

Tiếp theo là gói `bufio`, giúp cho việc nhập và xuất hiệu quả và thuận tiện. Một trong những tính năng hữu ích nhất của nó là một kiểu gọi là `Scanner` để đọc đầu vào và chia nó thành các dòng hoặc từ; đây thường là cách dễ nhất để xử lý đầu vào đến một cách tự nhiên theo từng dòng.

Scanner đọc từ đầu vào tiêu chuẩn của chương trình. Mỗi lời gọi `input.Scan()` sẽ đọc dòng tiếp theo và loại bỏ ký tự xuống dòng ở cuối; kết quả có thể được truy xuất bằng cách gọi `input.Text()`. Hàm `Scan` trả về `true` nếu có một dòng và `false` khi không còn dữ liệu đầu vào.

Chương trình sử dụng khai báo biến ngắn gọn để tạo một biến mới `input` tham chiếu đến một `bufio.Scanner`:

```go
input := bufio.NewScanner(os.Stdin)
```

Hàm `fmt.Printf`, giống như `printf` trong C và các ngôn ngữ khác, tạo ra đầu ra được định dạng từ một danh sách các biểu thức. Đối số đầu tiên của nó là một chuỗi định dạng (format string) chỉ định cách các đối số tiếp theo được định dạng. Định dạng của mỗi đối số được xác định bởi một ký tự chuyển đổi (conversion character), một chữ cái theo sau dấu phần trăm. Ví dụ, `%d` định dạng một toán hạng số nguyên bằng cách sử dụng ký hiệu thập phân, và `%s` mở rộng thành giá trị của một toán hạng chuỗi.

`Printf` có hơn một tá các kiểu chuyển đổi như vậy, mà các lập trình viên Go gọi là các **động từ** (verbs). Bảng này còn lâu mới là một đặc tả đầy đủ nhưng nó minh họa nhiều tính năng có sẵn:

| Động từ | Mô tả |
| :--- | :--- |
| `%d` | số nguyên thập phân |
| `%x`, `%o`, `%b` | số nguyên ở hệ thập lục phân, bát phân, nhị phân |
| `%f`, `%g`, `%e` | số dấu phẩy động: 3.141593 3.141592653589793 3.141593e+00 |
| `%t` | kiểu boolean: true hoặc false |
| `%c` | rune (điểm mã Unicode) |
| `%s` | chuỗi |
| `%q` | chuỗi được trích dẫn "abc" hoặc rune 'c' |
| `%v` | bất kỳ giá trị nào ở định dạng tự nhiên |
| `%T` | kiểu của bất kỳ giá trị nào |
| `%%` | dấu phần trăm nguyên văn (không có toán hạng) |

Chuỗi định dạng trong `dup1` cũng chứa một tab `\t` và một dòng mới `\n`. Các chuỗi ký tự (string literals) có thể chứa các chuỗi thoát (escape sequences) như vậy để đại diện cho các ký tự vô hình khác. `Printf` không tự động xuống dòng theo mặc định. Theo quy ước, các hàm định dạng có tên kết thúc bằng `f`, chẳng hạn như `log.Printf` và `fmt.Errorf`, sử dụng các quy tắc định dạng của `fmt.Printf`, trong khi những hàm có tên kết thúc bằng `ln` thì tuân theo `Println`, định dạng các đối số của chúng như thể bằng `%v`, theo sau là một dòng mới.

Nhiều chương trình đọc từ đầu vào tiêu chuẩn của chúng, như trên, hoặc từ một chuỗi các tệp được đặt tên. Phiên bản tiếp theo của `dup` có thể đọc từ đầu vào tiêu chuẩn hoặc xử lý một danh sách các tên tệp, sử dụng `os.Open` để mở từng tệp một.

Phiên bản tiếp theo của `dup` có thể đọc từ đầu vào tiêu chuẩn hoặc từ một danh sách các tệp có tên:

`gopl.io/ch1/dup2`

```go
func main() {
    counts := make(map[string]int)
    files := os.Args[1:]
    if len(files) == 0 {
        countLines(os.Stdin, counts)
    } else {
        for _, arg := range files {
            f, err := os.Open(arg)
            if err != nil {
                fmt.Fprintf(os.Stderr, "dup2: %v\n", err)
                continue
            }
            countLines(f, counts)
            f.Close()
        }
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}

func countLines(f *os.File, counts map[string]int) {
    input := bufio.NewScanner(f)
    for input.Scan() {
        counts[input.Text()]++
    }
    // NOTE: ignoring potential errors from input.Err()
}
```

Hàm `os.Open` trả về hai giá trị. Giá trị đầu tiên là một tệp đã mở (`*os.File`) được sử dụng trong các lần đọc tiếp theo bởi `Scanner`.

Kết quả thứ hai của `os.Open` là một giá trị của kiểu tích hợp `error`. Nếu `err` bằng giá trị tích hợp đặc biệt `nil`, tệp đã được mở thành công. Tệp được đọc, và khi đạt đến cuối đầu vào, hàm `Close` đóng tệp và giải phóng mọi tài nguyên. Mặt khác, nếu `err` không phải là `nil`, đã có lỗi xảy ra. Trong trường hợp đó, giá trị lỗi mô tả vấn đề. Cách xử lý lỗi đơn giản của chúng tôi là in một thông báo lên luồng lỗi tiêu chuẩn (standard error stream) bằng cách sử dụng `Fprintf` và động từ `%v`, nó hiển thị một giá trị của bất kỳ kiểu nào theo định dạng mặc định, và sau đó `dup` tiếp tục với tệp tiếp theo; câu lệnh `continue` chuyển đến lần lặp tiếp theo của vòng lặp `for` bao quanh.

Vì mục đích giữ cho các ví dụ mã có kích thước hợp lý, các ví dụ ban đầu của chúng tôi đôi khi cố tình hơi "vô tư" (cavalier) về việc xử lý lỗi. Rõ ràng là chúng ta phải kiểm tra lỗi từ `os.Open`; tuy nhiên, chúng ta đang bỏ qua khả năng ít xảy ra hơn là lỗi có thể xuất hiện trong khi đọc tệp bằng `input.Scan`. Chúng tôi sẽ ghi chú những nơi chúng tôi đã bỏ qua việc kiểm tra lỗi và sẽ đi sâu vào chi tiết về xử lý lỗi trong Mục 5.4.

Lưu ý rằng lời gọi đến `countLines` nằm trước khai báo của nó. Các hàm và các thực thể cấp gói khác có thể được khai báo theo bất kỳ thứ tự nào.

Một map là một **tham chiếu** (reference) đến cấu trúc dữ liệu được tạo bởi `make`. Khi một map được truyền vào một hàm, hàm đó nhận được một bản sao của tham chiếu, vì vậy bất kỳ thay đổi nào mà hàm được gọi thực hiện đối với cấu trúc dữ liệu cơ sở cũng sẽ hiển thị thông qua tham chiếu map của trình gọi. Trong ví dụ của chúng ta, các giá trị được chèn vào map `counts` bởi `countLines` đều được nhìn thấy bởi `main`.

Các phiên bản của `dup` ở trên hoạt động ở chế độ "luồng" (streaming), trong đó đầu vào được đọc và chia thành các dòng khi cần thiết, vì vậy về nguyên tắc, các chương trình này có thể xử lý một lượng đầu vào tùy ý. Một cách tiếp cận thay thế là đọc toàn bộ đầu vào vào bộ nhớ trong một lần (in one big gulp), chia nó thành các dòng cùng một lúc, sau đó xử lý các dòng. Phiên bản tiếp theo, `dup3`, hoạt động theo cách đó. Nó giới thiệu hàm `ReadFile` (từ gói `io/ioutil`), hàm này đọc toàn bộ nội dung của một tệp được chỉ tên, và `strings.Split`, hàm này chia một chuỗi thành một slice gồm các chuỗi con. (`Split` là ngược lại với `strings.Join` mà chúng ta đã thấy trước đó.)

`gopl.io/ch1/dup3`

```go
package main

import (
    "fmt"
    "io/ioutil"
    "os"
    "strings"
)

func main() {
    counts := make(map[string]int)
    for _, filename := range os.Args[1:] {
        data, err := ioutil.ReadFile(filename)
        if err != nil {
            fmt.Fprintf(os.Stderr, "dup3: %v\n", err)
            continue
        }
        for _, line := range strings.Split(string(data), "\n") {
            counts[line]++
        }
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```

Chúng ta đã đơn giản hóa `dup3` đi một chút. Thứ nhất, nó chỉ đọc các file được đặt tên, chứ không phải đầu vào tiêu chuẩn, vì `ReadFile` yêu cầu một tham số là tên file. Thứ hai, chúng ta đã chuyển việc đếm các dòng trở lại hàm `main`, vì bây giờ nó chỉ cần thiết ở một nơi.

`ReadFile` trả về một byte slice (mảng byte) cần được chuyển đổi thành `string` để có thể được tách bởi `strings.Split`. Chúng ta sẽ thảo luận chi tiết về `string` và `byte slice` trong Mục 3.5.4.

Về bản chất, `bufio.Scanner`, `ioutil.ReadFile`, và `ioutil.WriteFile` sử dụng các phương thức `Read` và `Write` của `*os.File`, nhưng hiếm khi hầu hết các lập trình viên cần truy cập trực tiếp vào các routine cấp thấp đó. Các hàm cấp cao hơn như các hàm từ `bufio` và `io/ioutil` dễ sử dụng hơn.

**Bài tập 1.4:** Sửa đổi `dup2` để in ra tên của tất cả các file mà mỗi dòng lặp lại xuất hiện.
### 1.4. GIF Hoạt hình (Animated GIFs)

Chương trình tiếp theo minh họa cách sử dụng cơ bản các gói `image` chuẩn của Go, chúng tôi sẽ sử dụng chúng để tạo ra một chuỗi các hình ảnh bit-mapped và sau đó mã hóa chuỗi này thành một hoạt hình GIF. Các hình ảnh, được gọi là các hình Lissajous, là một hiệu ứng hình ảnh chủ đạo trong các bộ phim khoa học viễn tưởng của những năm 1960. Chúng là các đường cong tham số được tạo ra bởi dao động điều hòa trong hai chiều, chẳng hạn như hai sóng sin được đưa vào các đầu vào x và y của một máy hiện sóng (oscilloscope). Hình 1.1 hiển thị một số ví dụ.

Hình 1.1. Bốn hình Lissajous.

Có một vài cấu trúc mới trong mã này, bao gồm các khai báo `const`, các kiểu `struct`, và các composite literal (biểu thức tạo phức hợp). Không giống như hầu hết các ví dụ của chúng tôi, ví dụ này cũng liên quan đến các tính toán dấu phẩy động. Chúng tôi sẽ chỉ thảo luận ngắn gọn về các chủ đề này ở đây, đẩy hầu hết các chi tiết sang các chương sau, vì mục tiêu chính ngay bây giờ là cung cấp cho bạn một ý tưởng về việc Go trông như thế nào và các loại công việc có thể được thực hiện dễ dàng với ngôn ngữ và các thư viện của nó.

`gopl.io/ch1/lissajous`

```go
// Lissajous generates GIF animations of random Lissajous figures.
package main

import (
    "image"
    "image/color"
    "image/gif"
    "io"
    "math"
    "math/rand"
    "os"
)

var palette = []color.Color{color.White, color.Black}

const (
    whiteIndex = 0 // first color in palette
    blackIndex = 1 // next color in palette
)

func main() {
    lissajous(os.Stdout)
}

func lissajous(out io.Writer) {
    const (
        cycles  = 5     // number of complete x oscillator revolutions
        res     = 0.001 // angular resolution
        size    = 100   // image canvas covers [-size..+size]
        nframes = 64    // number of animation frames
        delay   = 8     // delay between frames in 10ms units
    )
    freq := rand.Float64() * 3.0 // relative frequency of y oscillator
    anim := gif.GIF{LoopCount: nframes}
    phase := 0.0 // phase difference
    for i := 0; i < nframes; i++ {
        rect := image.Rect(0, 0, 2*size+1, 2*size+1)
        img := image.NewPaletted(rect, palette)
        for t := 0.0; t < cycles*2*math.Pi; t += res {
            x := math.Sin(t)
            y := math.Sin(t*freq + phase)
            img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5),
                blackIndex)
        }
        phase += 0.1
        anim.Delay = append(anim.Delay, delay)
        anim.Image = append(anim.Image, img)
    }
    gif.EncodeAll(out, &anim) // NOTE: ignoring encoding errors
}
```

Sau khi nhập một gói có đường dẫn bao gồm nhiều thành phần, như `image/color`, chúng ta tham chiếu đến gói đó bằng tên đến từ thành phần cuối cùng. Vì vậy biến `color.White` thuộc về gói `image/color` và `gif.GIF` thuộc về `image/gif`.

Một khai báo `const` (§3.6) đặt tên cho các hằng số (constants), nghĩa là, các giá trị được cố định tại thời gian biên dịch, chẳng hạn như các tham số số học cho chu kỳ, khung hình và độ trễ. Giống như các khai báo `var`, các khai báo `const` có thể xuất hiện ở cấp gói (để các tên được nhìn thấy trong toàn bộ gói) hoặc bên trong một hàm (để các tên chỉ được nhìn thấy bên trong hàm đó). Giá trị của một hằng số phải là một số, chuỗi hoặc boolean.

Các biểu thức `[]color.Color{...}` và `gif.GIF{...}` là các composite literal (§4.2, §4.4.1), một ký hiệu nhỏ gọn để khởi tạo bất kỳ kiểu phức hợp nào của Go từ một chuỗi các giá trị phần tử. Ở đây, cái đầu tiên là một slice và cái thứ hai là một struct.

Kiểu `gif.GIF` là một kiểu cấu trúc (struct) (§4.4). Một struct là một nhóm các giá trị được gọi là các trường (fields), thường thuộc các kiểu dữ liệu khác nhau, được tập hợp lại trong một đối tượng duy nhất để có thể được xử lý như một đơn vị. Biến `anim` là một struct kiểu `gif.GIF`. Cấu trúc khởi tạo (struct literal) tạo ra một giá trị struct với trường `LoopCount` được gán bằng `nframes`; tất cả các trường khác đều có giá trị zero (giá trị mặc định) của kiểu dữ liệu tương ứng. Các trường riêng lẻ của một struct có thể được truy cập bằng cách sử dụng ký hiệu dấu chấm (dot notation), giống như trong hai lệnh gán cuối cùng giúp cập nhật rõ ràng các trường `Delay` và `Image` của `anim`.

Hàm `lissajous` có hai vòng lặp lồng nhau. Vòng lặp bên ngoài chạy 64 lần lặp, mỗi lần tạo ra một khung hình (frame) duy nhất cho ảnh động. Nó tạo ra một hình ảnh 201x201 mới với bảng màu gồm hai màu, trắng và đen. Tất cả các điểm ảnh (pixels) ban đầu được đặt thành giá trị 0 của bảng màu (màu thứ 0 trong bảng màu), mà chúng ta đã thiết lập là màu trắng. Mỗi lượt chạy qua vòng lặp bên trong sẽ tạo ra một hình ảnh mới bằng cách đặt một số điểm ảnh thành màu đen. Kết quả được nối thêm, bằng cách sử dụng hàm `append` có sẵn (§4.2.1), vào danh sách các khung hình trong `anim`, cùng với một khoảng thời gian trễ được chỉ định là 80ms. Cuối cùng, chuỗi các khung hình và độ trễ được mã hóa sang định dạng GIF và ghi vào luồng đầu ra `out`. Kiểu của `out` là `io.Writer`, cho phép chúng ta ghi vào một loạt các đích đến khả thi, như chúng ta sẽ sớm trình bày.

Vòng lặp bên trong chạy hai bộ dao động (oscillators). Bộ dao động x chỉ đơn giản là hàm sine. Bộ dao động y cũng là một đường hình sin, nhưng tần số của nó so với bộ dao động x là một số ngẫu nhiên từ 0 đến 3, và pha của nó so với bộ dao động x ban đầu là 0 nhưng tăng dần theo từng khung hình của ảnh động. Vòng lặp chạy cho đến khi bộ dao động x hoàn thành năm chu kỳ đầy đủ. Ở mỗi bước, nó gọi `SetColorIndex` để tô màu đen cho điểm ảnh tương ứng với (x, y), điểm ảnh này nằm ở vị trí số 1 trong bảng màu.

Hàm `main` gọi hàm `lissajous`, điều hướng nó ghi vào đầu ra tiêu chuẩn (standard output), vì vậy lệnh này tạo ra một ảnh GIF động với các khung hình giống như trong Hình 1.1:
```bash
$ go build gopl.io/ch1/lissajous
$ ./lissajous >out.gif
```

Bài tập 1.5: Thay đổi bảng màu của chương trình Lissajous thành màu xanh lá cây trên nền đen, để tăng thêm tính chân thực. Để tạo màu web #RRGGBB, hãy sử dụng `color.RGBA{0xRR, 0xGG, 0xBB, 0xff}`, trong đó mỗi cặp chữ số thập lục phân đại diện cho cường độ của thành phần đỏ, xanh lá cây hoặc xanh lam của điểm ảnh.

Bài tập 1.6: Sửa đổi chương trình Lissajous để tạo ra hình ảnh có nhiều màu bằng cách thêm nhiều giá trị hơn vào bảng màu và sau đó hiển thị chúng bằng cách thay đổi đối số thứ ba của `SetColorIndex` theo một cách thú vị nào đó.

### 1.5. Truy cập một URL (Fetching a URL)

Đối với nhiều ứng dụng, việc truy cập thông tin từ Internet cũng quan trọng như việc truy cập vào hệ thống tệp cục bộ. Go cung cấp một tập hợp các gói (packages), được nhóm trong nhóm `net`, giúp việc gửi và nhận thông tin qua Internet, thiết lập các kết nối mạng cấp thấp và xây dựng các máy chủ (servers) trở nên dễ dàng hơn. Trong đó, các tính năng xử lý đồng thời (concurrency) của Go (sẽ được giới thiệu trong Chương 8) đặc biệt hữu ích.

Để minh họa những gì tối thiểu cần thiết để lấy thông tin qua giao thức HTTP, dưới đây là một chương trình đơn giản tên là `fetch`. Nó sẽ lấy nội dung của từng URL được chỉ định và in ra dưới dạng văn bản thô (uninterpreted text); chương trình này được lấy cảm hứng từ tiện ích cực kỳ hữu dụng là `curl`. Thông thường chúng ta sẽ xử lý dữ liệu này nhiều hơn, nhưng ví dụ này sẽ trình bày ý tưởng cơ bản. Chúng ta sẽ sử dụng chương trình này thường xuyên trong suốt cuốn sách.

`gopl.io/ch1/fetch`

```go
// Fetch in ra nội dung tìm thấy tại một URL.
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"os"
)

func main() {
	for _, url := range os.Args[1:] {
		resp, err := http.Get(url)
		if err != nil {
			fmt.Fprintf(os.Stderr, "fetch: %v\n", err)
			os.Exit(1)
		}
		b, err := ioutil.ReadAll(resp.Body)
		resp.Body.Close() // không làm rò rỉ tài nguyên
		if err != nil {
			fmt.Fprintf(os.Stderr, "fetch: reading %s: %v\n", url, err)
			os.Exit(1)
		}
		fmt.Printf("%s", b)
	}
}
```

Chương trình này giới thiệu các hàm từ hai gói: `net/http` và `io/ioutil`. Hàm `http.Get` thực hiện một yêu cầu HTTP (HTTP request) và nếu không có lỗi, nó sẽ trả về kết quả trong một struct phản hồi là `resp`. Trường `Body` của `resp` chứa phản hồi từ máy chủ dưới dạng một luồng có thể đọc được (readable stream). Tiếp theo, `ioutil.ReadAll` đọc toàn bộ phản hồi; kết quả được lưu trữ trong biến `b`. Luồng `Body` được đóng lại để tránh rò rỉ tài nguyên (leaking resources), và `Printf` ghi phản hồi ra đầu ra tiêu chuẩn (standard output).

```bash
$ go build gopl.io/ch1/fetch
$ ./fetch http://gopl.io
<html>
<head>
<title>The Go Programming Language</title>
...
```

Nếu yêu cầu HTTP thất bại, `fetch` sẽ báo cáo lỗi thay thế:

```bash
$ ./fetch http://bad.gopl.io
fetch: Get http://bad.gopl.io: dial tcp: lookup bad.gopl.io: no such host
```

Trong cả hai trường hợp lỗi, `os.Exit(1)` khiến tiến trình thoát với mã trạng thái (status code) là 1.

**Bài tập 1.7:** Hàm gọi `io.Copy(dst, src)` đọc từ `src` và ghi vào `dst`. Hãy sử dụng nó thay cho `ioutil.ReadAll` để sao chép thân phản hồi (response body) trực tiếp vào `os.Stdout` mà không cần một bộ đệm (buffer) đủ lớn để chứa toàn bộ luồng. Đừng quên kiểm tra kết quả lỗi của `io.Copy`.

**Bài tập 1.8:** Sửa đổi `fetch` để thêm tiền tố `http://` vào mỗi URL đối số nếu nó bị thiếu. Bạn có thể sử dụng `strings.HasPrefix`.

**Bài tập 1.9:** Sửa đổi `fetch` để in thêm mã trạng thái HTTP (HTTP status code), có thể tìm thấy trong `resp.Status`.

### 1.6. Truy cập các URL đồng thời (Fetching URLs Concurrently)

Một trong những khía cạnh thú vị và mới lạ nhất của Go là sự hỗ trợ cho lập trình đồng thời (concurrent programming). Đây là một chủ đề lớn, sẽ được dành riêng trong Chương 8 và Chương 9, vì vậy hiện tại chúng tôi sẽ chỉ cung cấp cho bạn một chút trải nghiệm về các cơ chế đồng thời chính của Go: goroutines và channels.

Chương trình tiếp theo, `fetchall`, thực hiện cùng một công việc lấy nội dung URL như ví dụ trước, nhưng nó lấy nhiều URL cùng một lúc (đồng thời), nhờ đó quá trình này sẽ không mất nhiều thời gian hơn lần lấy lâu nhất thay vì bằng tổng thời gian của tất cả các lần lấy. Phiên bản `fetchall` này sẽ bỏ qua các phản hồi nhưng báo cáo kích thước và thời gian đã trôi qua cho mỗi lần lấy:

`gopl.io/ch1/fetchall`

```go
// Fetchall lấy các URL song song và báo cáo thời gian cũng như kích thước của chúng.
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"net/http"
	"os"
	"time"
)

func main() {
	start := time.Now()
	ch := make(chan string)
	for _, url := range os.Args[1:] {
		go fetch(url, ch) // bắt đầu một goroutine
	}
	for range os.Args[1:] {
		fmt.Println(<-ch) // nhận từ channel ch
	}
	fmt.Printf("%.2fs elapsed\n", time.Since(start).Seconds())
}

func fetch(url string, ch chan<- string) {
	start := time.Now()
	resp, err := http.Get(url)
	if err != nil {
		ch <- fmt.Sprint(err) // gửi tới channel ch
		return
	}
	nbytes, err := io.Copy(ioutil.Discard, resp.Body)
	resp.Body.Close() // không làm rò rỉ tài nguyên
	if err != nil {
		ch <- fmt.Sprintf("while reading %s: %v", url, err)
		return
	}
	secs := time.Since(start).Seconds()
	ch <- fmt.Sprintf("%.2fs %7d %s", secs, nbytes, url)
}
```

Dưới đây là một ví dụ:

```bash
$ go build gopl.io/ch1/fetchall
$ ./fetchall https://golang.org http://gopl.io https://godoc.org
0.14s    6852  https://godoc.org
0.16s    7261  https://golang.org
0.48s    2475  http://gopl.io
0.48s elapsed
```

Một **goroutine** là một quá trình thực thi hàm đồng thời. Một **channel** là một cơ chế giao tiếp cho phép một goroutine truyền các giá trị thuộc một kiểu dữ liệu cụ thể sang một goroutine khác. Hàm `main` chạy trong một goroutine và câu lệnh `go` sẽ tạo ra các goroutine bổ sung.

Hàm `main` tạo ra một channel chứa các chuỗi (strings) bằng cách sử dụng `make`. Đối với mỗi đối số dòng lệnh, câu lệnh `go` trong vòng lặp `range` đầu tiên sẽ bắt đầu một goroutine mới gọi hàm `fetch` một cách bất đồng bộ (asynchronously) để lấy URL bằng `http.Get`. Hàm `io.Copy` đọc thân của phản hồi và bỏ qua nó bằng cách ghi vào luồng đầu ra `ioutil.Discard`. `Copy` trả về số lượng byte, cùng với bất kỳ lỗi nào đã xảy ra. Khi mỗi kết quả đến, `fetch` sẽ gửi một dòng tóm tắt lên channel `ch`. Vòng lặp `range` thứ hai trong `main` sẽ nhận và in ra các dòng đó.

Khi một goroutine cố gắng gửi hoặc nhận trên một channel, nó sẽ bị chặn (blocks) cho đến khi một goroutine khác thực hiện thao tác nhận hoặc gửi tương ứng, tại thời điểm đó giá trị sẽ được truyền đi và cả hai goroutine đều tiếp tục. Trong ví dụ này, mỗi hàm `fetch` gửi một giá trị (`ch <- expression`) lên channel `ch`, và `main` nhận tất cả chúng (`<-ch`). Việc để `main` thực hiện tất cả các thao tác in ấn đảm bảo rằng đầu ra từ mỗi goroutine được xử lý như một đơn vị, không có nguy cơ bị xen kẽ (interleaving) nếu hai goroutine kết thúc cùng một lúc.

**Bài tập 1.10:** Tìm một trang web tạo ra một lượng lớn dữ liệu. Kiểm tra việc lưu bộ nhớ đệm (caching) bằng cách chạy `fetchall` hai lần liên tiếp để xem thời gian báo cáo có thay đổi nhiều hay không. Bạn có nhận được cùng một nội dung mỗi lần không? Sửa đổi `fetchall` để in đầu ra của nó ra một tệp để có thể kiểm tra.

**Bài tập 1.11:** Thử `fetchall` với danh sách đối số dài hơn, chẳng hạn như các mẫu từ một triệu trang web hàng đầu có sẵn tại alexa.com. Chương trình sẽ hoạt động như thế nào nếu một trang web đơn giản là không phản hồi? (Mục 8.9 mô tả các cơ chế để xử lý trong các trường hợp như vậy.)

### 1.7. Máy chủ Web (A Web Server)

Các thư viện của Go giúp việc viết một máy chủ web (web server) để phản hồi các yêu cầu từ máy khách (client requests) — giống như những yêu cầu được thực hiện bởi chương trình `fetch` — trở nên dễ dàng. Trong phần này, chúng tôi sẽ trình bày một máy chủ tối thiểu trả về thành phần đường dẫn (path component) của URL được sử dụng để truy cập máy chủ. Nghĩa là, nếu yêu cầu là `http://localhost:8000/hello`, phản hồi sẽ là `URL.Path = "/hello"`.

`gopl.io/ch1/server1`

```go
// Server1 là một máy chủ "echo" tối thiểu.
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", handler) // mỗi yêu cầu sẽ gọi hàm handler
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// handler sẽ "echo" lại thành phần Path của URL yêu cầu r.
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}
```

Chương trình này chỉ dài vài dòng vì các hàm thư viện đã thực hiện hầu hết các công việc. Hàm `main` kết nối một hàm xử lý (`handler`) với các URL đến bắt đầu bằng dấu `/` (tức là tất cả các URL) và khởi động một máy chủ lắng nghe các yêu cầu đến trên cổng 8000. Một yêu cầu được biểu diễn dưới dạng một struct kiểu `http.Request`, chứa một số trường liên quan, một trong số đó là URL của yêu cầu đến. Khi một yêu cầu đến, nó được đưa cho hàm `handler`, hàm này sẽ trích xuất thành phần đường dẫn (`/hello`) từ URL yêu cầu và gửi nó ngược trở lại dưới dạng phản hồi, sử dụng `fmt.Fprintf`. Máy chủ web sẽ được giải thích chi tiết trong Mục 7.7.

Hãy khởi động máy chủ ở chế độ nền (background). Trên Mac OS X hoặc Linux, hãy thêm dấu và (`&`) vào lệnh; trên Microsoft Windows, bạn sẽ cần chạy lệnh mà không có dấu `&` trong một cửa sổ lệnh riêng biệt.

```bash
$ go run code/chapter1/server1.go &
```

Sau đó, chúng ta có thể thực hiện các yêu cầu từ máy khách bằng dòng lệnh:

```bash
$ ./fetch http://localhost:8000
URL.Path = "/"
$ ./fetch http://localhost:8000/help
URL.Path = "/help"
```

Ngoài ra, chúng ta có thể truy cập máy chủ từ một trình duyệt web.

Việc thêm các tính năng vào máy chủ rất dễ dàng. Một sự bổ sung hữu ích là một URL cụ thể để trả về một loại trạng thái nào đó. Ví dụ, phiên bản này thực hiện việc "echo" tương tự nhưng cũng đếm số lượng yêu cầu; một yêu cầu đến URL `/count` sẽ trả về số lượng yêu cầu cho đến thời điểm hiện tại, không bao gồm chính các yêu cầu `/count`:

`gopl.io/ch1/server2`

```go
// Server2 là một máy chủ "echo" và bộ đếm tối thiểu.
package main

import (
	"fmt"
	"log"
	"net/http"
	"sync"
)

var mu sync.Mutex
var count int

func main() {
	http.HandleFunc("/", handler)
	http.HandleFunc("/count", counter)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// handler echo lại thành phần Path của URL được yêu cầu.
func handler(w http.ResponseWriter, r *http.Request) {
	mu.Lock()
	count++
	mu.Unlock()
	fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}

// counter echo lại số lượng các cuộc gọi cho đến nay.
func counter(w http.ResponseWriter, r *http.Request) {
	mu.Lock()
	fmt.Fprintf(w, "Count %d\n", count)
	mu.Unlock()
}
```

Máy chủ có hai hàm xử lý (`handlers`), và URL yêu cầu sẽ quyết định hàm nào được gọi: một yêu cầu cho `/count` sẽ gọi `counter` và tất cả các yêu cầu khác sẽ gọi `handler`. Một mẫu xử lý kết thúc bằng dấu gạch chéo sẽ khớp với bất kỳ URL nào có mẫu đó làm tiền tố. Đằng sau hậu trường, máy chủ chạy hàm xử lý cho mỗi yêu cầu đến trong một goroutine riêng biệt để nó có thể phục vụ nhiều yêu cầu đồng thời. Tuy nhiên, nếu hai yêu cầu đồng thời cố gắng cập nhật biến `count` cùng một lúc, nó có thể không được tăng lên một cách nhất quán; chương trình sẽ gặp một lỗi nghiêm trọng gọi là **race condition** (§9.1). Để tránh vấn đề này, chúng ta phải đảm bảo rằng tại một thời điểm chỉ có tối đa một goroutine truy cập vào biến đó, đó là mục đích của các lời gọi `mu.Lock()` và `mu.Unlock()` bao quanh mỗi lần truy cập vào `count`. Chúng ta sẽ xem xét kỹ hơn về tính đồng thời với các biến được chia sẻ trong Chương 9.
Để làm ví dụ phong phú hơn, hàm handler có thể báo cáo về các headers và form data mà nó nhận được, giúp server trở nên hữu ích cho việc kiểm tra (inspecting) và gỡ lỗi (debugging) các request:

**gopl.io/ch1/server3**
```go
// handler phản hồi lại HTTP request.
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "%s %s %s\n", r.Method, r.URL, r.Proto)
    for k, v := range r.Header {
        fmt.Fprintf(w, "Header[%q] = %q\n", k, v)
    }
    fmt.Fprintf(w, "Host = %q\n", r.Host)
    fmt.Fprintf(w, "RemoteAddr = %q\n", r.RemoteAddr)
    if err := r.ParseForm(); err != nil {
        log.Print(err)
    }
    for k, v := range r.Form {
        fmt.Fprintf(w, "Form[%q] = %q\n", k, v)
    }
}
```

Đoạn mã này sử dụng các trường (fields) của struct `http.Request` để tạo ra đầu ra như sau:
```text
GET /?q=query HTTP/1.1
Header["Accept-Encoding"] = ["gzip, deflate, sdch"]
Header["Accept-Language"] = ["en-US,en;q=0.8"]
Header["Connection"] = ["keep-alive"]
Header["Accept"] = ["text/html,application/xhtml+xml,application/xml;..."]
Header["User-Agent"] = ["Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_5)..."]
Host = "localhost:8000"
RemoteAddr = "127.0.0.1:59"
```

Lưu ý cách gọi hàm `ParseForm` được lồng bên trong một câu lệnh `if`. Go cho phép một câu lệnh đơn giản như khai báo biến cục bộ nằm ngay trước điều kiện `if`, điều này đặc biệt hữu ích cho việc xử lý lỗi như trong ví dụ này. Chúng ta có thể viết nó như sau:
```go
err := r.ParseForm()
if err != nil {
    log.Print(err)
}
```
nhưng việc kết hợp các câu lệnh sẽ ngắn gọn hơn và giảm phạm vi (scope) của biến `err`, đây là một thói quen lập trình tốt. Chúng ta sẽ định nghĩa rõ hơn về scope trong Mục 2.7.

Trong các chương trình này, chúng ta đã thấy ba loại thực thể khác nhau được sử dụng làm luồng đầu ra (output streams). Chương trình `fetch` đã sao chép dữ liệu phản hồi HTTP vào `os.Stdout` (một file), chương trình `lissajous` cũng làm tương tự. Chương trình `fetchall` thì loại bỏ phản hồi (trong khi vẫn đếm độ dài của nó) bằng cách sao chép nó vào một "bồn chứa" vô hại là `ioutil.Discard`. Và web server ở trên đã sử dụng `fmt.Fprintf` để ghi vào `http.ResponseWriter`, đại diện cho trình duyệt web.

Mặc dù ba loại này khác nhau về chi tiết cách thực hiện, nhưng chúng đều thỏa mãn một **interface** chung, cho phép bất kỳ ai trong số chúng có thể được sử dụng ở bất cứ nơi nào cần một luồng đầu ra. Interface đó được gọi là `io.Writer`, sẽ được thảo luận trong Mục 7.1.

Cơ chế interface của Go là chủ đề của Chương 7, nhưng để hình dung về khả năng của nó, hãy xem việc kết hợp web server với hàm `lissajous` dễ dàng như thế nào để các ảnh GIF động được ghi trực tiếp vào HTTP client thay vì đầu ra tiêu chuẩn. Chỉ cần thêm các dòng sau vào web server:
```go
handler := func(w http.ResponseWriter, r *http.Request) {
    lissajous(w)
}
http.HandleFunc("/", handler)
```
hoặc tương đương:
```go
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    lissajous(w)
})
```
Đối số thứ hai của hàm `HandleFunc` ở trên là một **function literal**, tức là một hàm ẩn danh (anonymous function) được định nghĩa ngay tại nơi sử dụng. Chúng ta sẽ giải thích thêm trong Mục 5.6.

Sau khi thực hiện thay đổi này, hãy truy cập `http://localhost:8000` trên trình duyệt. Mỗi lần bạn tải lại trang, bạn sẽ thấy một hình động mới.

**Bài tập 1.12:** Chỉnh sửa Lissajous server để đọc các giá trị tham số từ URL. Ví dụ, bạn có thể thiết lập sao cho một URL như `http://localhost:8000/?cycles=20` sẽ đặt số chu kỳ (cycles) thành 20 thay vì mặc định là 5. Sử dụng hàm `strconv.Atoi` để chuyển đổi tham số chuỗi thành số nguyên. Bạn có thể xem tài liệu của nó bằng lệnh `go doc strconv.Atoi`.

### 1.8. Các Vấn Đề Còn Lại

Còn rất nhiều điều về Go mà chúng ta chưa đề cập trong phần giới thiệu ngắn gọn này. Dưới đây là một số chủ đề mà chúng ta chỉ mới lướt qua hoặc bỏ qua hoàn toàn, với đủ phần thảo luận để chúng trở nên quen thuộc khi xuất hiện ngắn gọn trước khi được trình bày đầy đủ.

**Luồng điều khiển (Control flow):** Chúng ta đã đề cập đến hai câu lệnh luồng điều khiển cơ bản là `if` và `for`, nhưng chưa nói đến câu lệnh `switch`, một dạng rẽ nhánh đa chiều. Dưới đây là một ví dụ nhỏ:

```go
switch coinflip() {
case "heads":
    heads++
case "tails":
    tails++
default:
    fmt.Println("landed on edge!")
}
```

Kết quả của việc gọi hàm `coinflip` được so sánh với giá trị của mỗi `case`. Các `case` được đánh giá từ trên xuống dưới, nên `case` đầu tiên khớp sẽ được thực thi. `default` là một `case` tùy chọn, sẽ khớp nếu không có `case` nào khác khớp; nó có thể được đặt ở bất cứ đâu. Các `case` không "fall through" (thực thi tiếp `case` sau) như trong các ngôn ngữ giống C (mặc dù có một câu lệnh `fallthrough` ít được sử dụng để ghi đè hành vi này).

Một `switch` không cần toán hạng; nó có thể chỉ liệt kê các `case`, mỗi `case` là một biểu thức boolean:

```go
func Signum(x int) int {
    switch {
    case x > 0:
        return +1
    default:
        return 0
    case x < 0:
        return -1
    }
}
```

Dạng này được gọi là `tagless switch` (switch không có thẻ); nó tương đương với `switch true`.

Giống như các câu lệnh `for` và `if`, một `switch` có thể bao gồm một câu lệnh đơn giản tùy chọn—một khai báo biến ngắn, một câu lệnh tăng hoặc gán, hoặc một lệnh gọi hàm—có thể được sử dụng để đặt một giá trị trước khi nó được kiểm tra.

Các câu lệnh `break` và `continue` sửa đổi luồng điều khiển. Một `break` làm cho luồng điều khiển tiếp tục tại câu lệnh ngay sau câu lệnh `for`, `switch`, hoặc `select` (chúng ta sẽ thấy sau) trong cùng nhất, và như chúng ta đã thấy trong Mục 1.3, một `continue` làm cho vòng lặp `for` trong cùng nhất bắt đầu vòng lặp tiếp theo của nó. Các câu lệnh có thể được gán nhãn để `break` và `continue` có thể tham chiếu đến chúng, ví dụ để thoát khỏi nhiều vòng lặp lồng nhau cùng một lúc hoặc để bắt đầu vòng lặp tiếp theo của vòng lặp ngoài cùng. Thậm chí có cả câu lệnh `goto`, mặc dù nó được dành cho mã do máy tạo ra, không phải để các lập trình viên sử dụng thường xuyên.

**Kiểu dữ liệu có tên (Named types):** Một khai báo kiểu (`type declaration`) cho phép đặt tên cho một kiểu dữ liệu hiện có. Vì các kiểu `struct` thường dài, chúng gần như luôn được đặt tên. Một ví dụ quen thuộc là định nghĩa kiểu `Point` cho một hệ thống đồ họa 2D:

```go
type Point struct {
    X, Y int
}
var p Point
```

Khai báo kiểu và các kiểu có tên được đề cập trong Chương 2.

**Con trỏ (Pointers):** Go cung cấp con trỏ, tức là các giá trị chứa địa chỉ của một biến. Trong một số ngôn ngữ, đặc biệt là C, con trỏ tương đối không bị ràng buộc. Trong các ngôn ngữ khác, con trỏ được ngụy trang thành "tham chiếu" (references), và không có nhiều thứ có thể làm với chúng ngoại trừ việc truyền chúng đi. Go có một vị trí ở giữa. Con trỏ được hiển thị một cách tường minh. Toán tử `&` trả về địa chỉ của một biến, và toán tử `*` truy xuất biến mà con trỏ tham chiếu đến, nhưng không có phép toán con trỏ (`pointer arithmetic`). Chúng ta sẽ giải thích về con trỏ trong Mục 2.3.2.

**Phương thức và Interface (Methods and interfaces):** Một phương thức (`method`) là một hàm được liên kết với một kiểu có tên; Go đặc biệt ở chỗ các phương thức có thể được gắn vào hầu hết mọi kiểu có tên. Các phương thức được đề cập trong Chương 6. Interface là các kiểu trừu tượng cho phép chúng ta xử lý các kiểu cụ thể khác nhau theo cùng một cách dựa trên các phương thức chúng có, chứ không phải cách chúng được biểu diễn hay triển khai. Interface là chủ đề của Chương 7.

**Package:** Go đi kèm với một thư viện chuẩn phong phú gồm các package hữu ích, và cộng đồng Go đã tạo ra và chia sẻ nhiều hơn nữa. Lập trình thường là về việc sử dụng các package hiện có hơn là tự viết mã gốc. Trong suốt cuốn sách, chúng tôi sẽ chỉ ra vài chục package chuẩn quan trọng nhất, nhưng có nhiều package khác mà chúng tôi không có không gian để đề cập, và chúng tôi không thể cung cấp bất cứ thứ gì giống như một tài liệu tham khảo hoàn chỉnh cho bất kỳ package nào.

Trước khi bạn bắt tay vào bất kỳ chương trình mới nào, một ý tưởng hay là xem liệu đã có các package nào có thể giúp bạn hoàn thành công việc dễ dàng hơn chưa. Bạn có thể tìm thấy một chỉ mục của các package thư viện chuẩn tại https://golang.org/pkg và các package do cộng đồng đóng góp tại https://godoc.org. Công cụ `go doc` làm cho các tài liệu này dễ dàng truy cập từ dòng lệnh:

```sh
$ go doc http.ListenAndServe
package http // import "net/http"
func ListenAndServe(addr string, handler Handler) error
ListenAndServe listens on the TCP network address addr and then
cols Serve with handler to handle requests on incoming connections.
...
```

**Chú thích (Comments):** Chúng tôi đã đề cập đến các chú thích tài liệu ở đầu một chương trình hoặc package. Viết một chú thích trước khi khai báo mỗi hàm để chỉ định hành vi của nó cũng là một phong cách tốt. Các quy ước này rất quan trọng, vì chúng được sử dụng bởi các công cụ như `go doc` và `godoc` để định vị và hiển thị tài liệu (§10.7.4).

Đối với các chú thích kéo dài nhiều dòng hoặc xuất hiện trong một biểu thức hoặc câu lệnh, cũng có ký hiệu `/* ... */` quen thuộc từ các ngôn ngữ khác. Các chú thích như vậy đôi khi được sử dụng ở đầu một file cho một khối văn bản giải thích lớn để tránh phải có `//` trên mỗi dòng. Trong một chú thích, `//` và `/*` không có ý nghĩa đặc biệt, vì vậy các chú thích không lồng vào nhau.

