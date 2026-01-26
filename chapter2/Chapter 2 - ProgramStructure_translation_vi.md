## Cấu trúc chương trình

Trong Go, cũng như trong bất kỳ ngôn ngữ lập trình nào khác, người ta xây dựng các chương trình lớn từ một tập hợp nhỏ các cấu trúc cơ bản. Biến (variables) lưu trữ các giá trị. Các biểu thức (expressions) đơn giản được kết hợp thành các biểu thức lớn hơn với các toán tử (operations) như cộng và trừ. Các kiểu (types) cơ bản được tập hợp thành các kiểu tổng hợp (aggregates) như mảng (arrays) và struct. Biểu thức được sử dụng trong các câu lệnh (statements) mà thứ tự thực thi của chúng được quyết định bởi các câu lệnh điều khiển luồng (control-flow statements) như `if` và `for`. Các câu lệnh được nhóm thành các hàm (functions) để cô lập và tái sử dụng. Các hàm được tập hợp vào các tệp nguồn (source files) và các gói (packages).

Chúng ta đã thấy các ví dụ về hầu hết những điều này trong chương trước. Trong chương này, chúng ta sẽ đi sâu hơn về các yếu tố cấu trúc cơ bản của một chương trình Go. Các chương trình ví dụ được chủ ý làm đơn giản, để chúng ta có thể tập trung vào ngôn ngữ mà không bị phân tâm bởi các thuật toán (algorithms) hoặc cấu trúc dữ liệu (data structures) phức tạp.

### 2.1. Tên (Names) 

Tên của các hàm, biến, hằng số, kiểu, nhãn câu lệnh và gói trong Go tuân theo một quy tắc đơn giản: một cái tên bắt đầu bằng một chữ cái (tức là, bất cứ thứ gì mà Unicode coi là một chữ cái) hoặc một dấu gạch dưới và có thể có bất kỳ số lượng chữ cái, chữ số và dấu gạch dưới bổ sung nào. Có phân biệt chữ hoa chữ thường: `heapSort` và `Heapsort` là hai tên khác nhau.

Go có 25 từ khóa như `if` và `switch` chỉ có thể được sử dụng ở những nơi cú pháp cho phép; chúng không thể được sử dụng làm tên.

```
break      default        func         interface    select
case       defer          go           map          struct
chan       else           goto         package      switch
const      fallthrough    if           range        type
continue   for            import       return       var
```

Ngoài ra, có khoảng ba chục tên được khai báo trước như `int` và `true` cho các hằng số, kiểu và hàm dựng sẵn:

**Hằng số (Constants):** `true` `false` `iota` `nil`
**Kiểu (Types):** `int` `int8` `int16` `int32` `int64`
`uint` `uint8` `uint16` `uint32` `uint64` `uintptr`
`float32` `float64` `complex128` `complex64`
`bool` `byte` `rune` `string` `error`
**Hàm (Functions):** `make` `len` `cap` `new` `append` `copy` `close` `delete`
`complex` `real` `imag`
`panic` `recover`

Những cái tên này không phải là từ khóa riêng (reserved), vì vậy bạn có thể sử dụng chúng trong các khai báo. Chúng ta sẽ thấy một vài trường hợp mà việc khai báo lại một trong số chúng là hợp lý, nhưng hãy cẩn thận về khả năng gây nhầm lẫn.

Nếu một thực thể (entity) được khai báo bên trong một hàm, nó là cục bộ (local) đối với hàm đó. Tuy nhiên, nếu được khai báo bên ngoài một hàm, nó sẽ hiển thị trong tất cả các tệp của gói mà nó thuộc về. Việc viết hoa chữ cái đầu tiên của một cái tên quyết định khả năng hiển thị của nó qua các ranh giới gói. Nếu tên bắt đầu bằng một chữ cái viết hoa, nó sẽ được xuất (exported), có nghĩa là nó hiển thị và có thể truy cập được từ bên ngoài gói của chính nó và có thể được các phần khác của chương trình tham chiếu đến, như với `Printf` trong gói `fmt`. Tên các gói luôn được viết bằng chữ thường.

Không có giới hạn về độ dài của tên, nhưng quy ước và phong cách trong các chương trình Go có xu hướng sử dụng các tên ngắn, đặc biệt đối với các biến cục bộ có phạm vi (scope) nhỏ; bạn sẽ có nhiều khả năng thấy các biến có tên là `i` hơn là `theLoopIndex`. Nói chung, phạm vi của một cái tên càng lớn thì nó càng nên dài và có ý nghĩa hơn.

Về mặt phong cách, các lập trình viên Go sử dụng "camel case" khi tạo tên bằng cách kết hợp các từ; tức là, các chữ cái viết hoa ở giữa được ưa thích hơn là các dấu gạch dưới ở giữa. Do đó, các thư viện chuẩn có các hàm với các tên như `QuoteRuneToASCII` và `parseRequestLine` nhưng không bao giờ có `quote_rune_to_ASCII` hoặc `parse_request_line`. Các chữ cái của các từ viết tắt và các từ viết tắt chữ đầu như ASCII và HTML luôn được viết hoa giống nhau, vì vậy một hàm có thể được gọi là `htmlEscape`, `HTMLEscape`, hoặc `escapeHTML`, nhưng không phải là `escapeHtml`.

### 2.2. Khai báo (Declarations)
Một khai báo (declaration) sẽ đặt tên cho một thực thể (entity) của chương trình và chỉ định một vài hoặc tất cả các thuộc tính của nó. Có bốn loại khai báo chính: `var`, `const`, `type`, và `func`. Chúng ta sẽ nói về biến và kiểu trong chương này, hằng số trong Chương 3, và hàm trong Chương 5.

Một chương trình Go được lưu trữ trong một hoặc nhiều tệp có tên kết thúc bằng `.go`. Mỗi tệp bắt đầu bằng một khai báo `package` để cho biết tệp đó thuộc về gói nào. Sau khai báo gói là các khai báo `import`, và sau đó là một chuỗi các khai báo ở cấp độ gói (package-level) của các kiểu, biến, hằng số và hàm, theo bất kỳ thứ tự nào. Ví dụ, chương trình này khai báo một hằng số, một hàm và một vài biến:

```go
gopl.io/ch2/boiling
// Boiling prints the boiling point of water.
package main
import "fmt"

const boilingF = 212.0

func main() {
    var f = boilingF
    var c = (f - 32) * 5 / 9
    fmt.Printf("boiling point = %g°F or %g°C\n", f, c)
    // Output:
    // boiling point = 212°F or 100°C
}
```

Hằng số `boilingF` là một khai báo ở cấp độ gói (cũng như `main`), trong khi các biến `f` và `c` là cục bộ (local) đối với hàm `main`. Tên của mỗi thực thể ở cấp độ gói không chỉ hiển thị trong toàn bộ tệp nguồn chứa khai báo của nó, mà còn trong tất cả các tệp của gói đó. Ngược lại, các khai báo cục bộ chỉ hiển thị bên trong hàm mà chúng được khai báo và có lẽ chỉ trong một phần nhỏ của hàm đó.

Một khai báo hàm có một tên, một danh sách các tham số (parameters) (các biến có giá trị được cung cấp bởi các trình gọi của hàm), một danh sách kết quả (results) tùy chọn, và thân hàm (body), chứa các câu lệnh định nghĩa những gì hàm đó làm. Danh sách kết quả được bỏ qua nếu hàm không trả về bất cứ thứ gì. Việc thực thi hàm bắt đầu bằng câu lệnh đầu tiên và tiếp tục cho đến khi gặp câu lệnh `return` hoặc đến cuối một hàm không có kết quả. Quyền điều khiển và bất kỳ kết quả nào sau đó được trả về cho trình gọi (caller).

Chúng ta đã thấy khá nhiều hàm và sẽ còn nhiều hàm nữa, bao gồm một cuộc thảo luận sâu rộng trong Chương 5, vì vậy đây chỉ là một bản phác thảo. Hàm `fToC` dưới đây đóng gói (encapsulates) logic chuyển đổi nhiệt độ để nó chỉ được định nghĩa một lần nhưng có thể được sử dụng từ nhiều nơi. Ở đây, `main` gọi nó hai lần, sử dụng giá trị của hai hằng số cục bộ khác nhau:

```go
gopl.io/ch2/ftoc
// Ftoc prints two Fahrenheit-to-Celsius conversions.
package main
import "fmt"

func main() {
    const freezingF, boilingF = 32.0, 212.0
    fmt.Printf("%g°F = %g°C\n", freezingF, fToC(freezingF)) // "32°F = 0°C"
    fmt.Printf("%g°F = %g°C\n", boilingF, fToC(boilingF)) // "212°F = 100°C"
    func fToC(f float64) float64 {
        return (f - 32) * 5 / 9
    }
}
```

### 2.3. Biến (Variables)
Một khai báo `var` tạo ra một biến của một kiểu cụ thể, gắn một tên cho nó, và đặt giá trị ban đầu của nó. Mỗi khai báo có dạng chung
```go
var name type = expression
```
Phần `type` hoặc phần `= expression` có thể được bỏ qua, nhưng không phải cả hai. Nếu `type` được bỏ qua, nó sẽ được xác định bởi biểu thức khởi tạo (initializer expression). Nếu `expression` được bỏ qua, giá trị ban đầu là *giá trị zero* (zero value) cho kiểu đó, là `0` cho các kiểu số, `false` cho kiểu boolean, `""` cho chuỗi, và `nil` cho các interface và kiểu tham chiếu (slice, con trỏ, map, channel, function). Giá trị zero của một kiểu tập hợp như mảng hoặc struct sẽ có giá trị zero của tất cả các phần tử hoặc trường của nó.

Cơ chế giá trị zero đảm bảo rằng một biến luôn giữ một giá trị được định nghĩa rõ ràng của kiểu của nó; trong Go không có khái niệm biến chưa được khởi tạo (uninitialized variable). Điều này đơn giản hóa mã nguồn và thường đảm bảo hành vi hợp lý của các điều kiện biên mà không cần thêm công sức. Ví dụ:
```go
var s string
fmt.Println(s) // ""
```
in ra một chuỗi rỗng, thay vì gây ra một loại lỗi hoặc hành vi không thể đoán trước. Lập trình viên Go thường nỗ lực để làm cho giá trị zero của một kiểu phức tạp hơn trở nên có ý nghĩa, để các biến bắt đầu "cuộc sống" của chúng ở một trạng thái hữu ích.

Có thể khai báo và tùy chọn khởi tạo một tập hợp các biến trong một khai báo duy nhất, với một danh sách các biểu thức tương ứng. Việc bỏ qua kiểu cho phép khai báo nhiều biến có kiểu khác nhau:
```go
var i, j, k int                 // int, int, int
var b, f, s = true, 2.3, "four" // bool, float64, string
```
Các giá trị khởi tạo có thể là các giá trị chữ (literal values) hoặc các biểu thức tùy ý. Các biến ở cấp độ gói được khởi tạo trước khi `main` bắt đầu (§2.6.2), và các biến cục bộ được khởi tạo khi các khai báo của chúng được gặp trong quá trình thực thi hàm.

Một tập hợp các biến cũng có thể được khởi tạo bằng cách gọi một hàm trả về nhiều giá trị:
```go
var f, err = os.Open(name) // os.Open trả về một file và một error
```

#### 2.3.1. Khai báo biến ngắn (Short Variable Declarations)
Bên trong một hàm, một dạng thay thế được gọi là *khai báo biến ngắn* có thể được sử dụng để khai báo và khởi tạo các biến cục bộ. Nó có dạng `name := expression`, và kiểu của `name` được xác định bởi kiểu của `expression`. Dưới đây là ba trong số nhiều khai báo biến ngắn trong hàm `lissajous` (§1.4):
```go
anim := gif.GIF{LoopCount: nframes}
freq := rand.Float64() * 3.0
t := 0.0
```
Do tính ngắn gọn và linh hoạt, các khai báo biến ngắn được sử dụng để khai báo và khởi tạo phần lớn các biến cục bộ. Một khai báo `var` có xu hướng được dành riêng cho các biến cục bộ cần một kiểu tường minh khác với kiểu của biểu thức khởi tạo, hoặc khi biến sẽ được gán một giá trị sau này và giá trị ban đầu của nó không quan trọng.
```go
i := 100                 // một int
var boiling float64 = 100 // một float64
var names []string
var err error
var p Point
```
Cũng như với khai báo `var`, nhiều biến có thể được khai báo và khởi tạo trong cùng một khai báo biến ngắn,
```go
i, j := 0, 1
```
nhưng các khai báo với nhiều biểu thức khởi tạo chỉ nên được sử dụng khi chúng giúp dễ đọc, chẳng hạn như cho các nhóm ngắn và tự nhiên như phần khởi tạo của một vòng lặp `for`.

Hãy nhớ rằng `:=` là một *khai báo*, trong đó `=` là một *phép gán*. Một khai báo nhiều biến không nên bị nhầm lẫn với một *phép gán bộ* (tuple assignment) (§2.4.1), trong đó mỗi biến ở phía bên trái được gán giá trị tương ứng từ phía bên phải:
```go
i, j = j, i // hoán đổi giá trị của i và j
```
Giống như các khai báo `var` thông thường, các khai báo biến ngắn có thể được sử dụng cho các lệnh gọi hàm như `os.Open` trả về hai hoặc nhiều giá trị:
```go
f, err := os.Open(name)
if err != nil {
    return err
}
// ...sử dụng f...
f.Close()
```
Một điểm tinh tế nhưng quan trọng: một khai báo biến ngắn không nhất thiết phải khai báo tất cả các biến ở phía bên trái của nó. Nếu một số trong số chúng đã được khai báo trong cùng một *khối từ vựng* (lexical block) (§2.7), thì khai báo biến ngắn hoạt động giống như một phép gán cho các biến đó.

Trong đoạn mã dưới đây, câu lệnh đầu tiên khai báo cả `in` và `err`. Câu lệnh thứ hai khai báo `out` nhưng chỉ gán một giá trị cho biến `err` đã tồn tại.
```go
in, err := os.Open(infile)
// ...
out, err := os.Create(outfile)
```
Tuy nhiên, một khai báo biến ngắn phải khai báo ít nhất một biến mới, vì vậy đoạn mã này sẽ không biên dịch được:
```go
f, err := os.Open(infile)
// ...
f, err := os.Create(outfile) // lỗi biên dịch: không có biến mới
```
Cách khắc phục là sử dụng một phép gán thông thường cho câu lệnh thứ hai.

Một khai báo biến ngắn chỉ hoạt động như một phép gán đối với các biến đã được khai báo trong cùng một khối từ vựng; các khai báo trong một khối bên ngoài sẽ bị bỏ qua. Chúng ta sẽ thấy các ví dụ về điều này ở cuối chương.

#### 2.3.2. Con trỏ (Pointers)
Một *biến* là một mẩu bộ nhớ chứa một giá trị. Các biến được tạo ra bởi các khai báo được xác định bằng một tên, chẳng hạn như `x`, nhưng nhiều biến chỉ được xác định bằng các biểu thức như `x[i]` hoặc `x.f`. Tất cả các biểu thức này đều đọc giá trị của một biến, ngoại trừ khi chúng xuất hiện ở phía bên trái của một phép gán, trong trường hợp đó một giá trị mới được gán cho biến.

Một *giá trị con trỏ* là *địa chỉ* của một biến. Do đó, một con trỏ là vị trí nơi một giá trị được lưu trữ. Không phải mọi giá trị đều có địa chỉ, nhưng mọi biến thì có. Với một con trỏ, chúng ta có thể đọc hoặc cập nhật giá trị của một biến một cách gián tiếp, mà không cần sử dụng hoặc thậm chí không biết tên của biến đó, nếu thực sự nó có tên.

Nếu một biến được khai báo là `var x int`, biểu thức `&x` ("địa chỉ của x") sẽ tạo ra một con trỏ đến một biến số nguyên, tức là một giá trị có kiểu `*int`, được phát âm là "con trỏ đến int". Nếu giá trị này được gọi là `p`, chúng ta nói "`p` trỏ đến `x`," hoặc tương đương là "`p` chứa địa chỉ của `x`." Biến mà `p` trỏ đến được viết là `*p`. Biểu thức `*p` trả về giá trị của biến đó, một số nguyên, nhưng vì `*p` biểu thị một biến, nó cũng có thể xuất hiện ở phía bên trái của một phép gán, trong trường hợp đó phép gán sẽ cập nhật biến.
```go
x := 1
p := &x         // p, có kiểu *int, trỏ đến x
fmt.Println(*p) // "1"
*p = 2          // tương đương với x = 2
fmt.Println(x)  // "2"
```
Mỗi thành phần của một biến có kiểu tập hợp—một trường của một struct hoặc một phần tử của một mảng—cũng là một biến và do đó cũng có một địa chỉ.

Các biến đôi khi được mô tả là các *giá trị có thể lấy địa chỉ* (addressable values). Các biểu thức biểu thị các biến là những biểu thức duy nhất mà toán tử lấy địa chỉ `&` có thể được áp dụng.

Giá trị zero cho một con trỏ của bất kỳ kiểu nào là `nil`. Phép kiểm tra `p != nil` là đúng nếu `p` trỏ đến một biến. Các con trỏ có thể so sánh được; hai con trỏ bằng nhau khi và chỉ khi chúng trỏ đến cùng một biến hoặc cả hai đều là `nil`.
```go
var x, y int
fmt.Println(&x == &x, &x == &y, &x == nil) // "true false false"
```
Việc một hàm trả về địa chỉ của một biến cục bộ là hoàn toàn an toàn. Ví dụ, trong đoạn mã dưới đây, biến cục bộ `v` được tạo bởi lệnh gọi cụ thể này đến `f` sẽ tiếp tục tồn tại ngay cả sau khi lệnh gọi đã trả về, và con trỏ `p` vẫn sẽ tham chiếu đến nó:

```go
var p = f()
func f() *int {
    v := 1
    return &v
}
```
Mỗi lệnh gọi `f` trả về một giá trị riêng biệt:
```go
fmt.Println(f() == f()) // "false"
```
Bởi vì một con trỏ chứa địa chỉ của một biến, việc truyền một đối số con trỏ cho một hàm làm cho hàm đó có thể cập nhật biến được truyền một cách gián tiếp. Ví dụ, hàm này tăng biến mà đối số của nó trỏ đến và trả về giá trị mới của biến để nó có thể được sử dụng trong một biểu thức:
```go
func incr(p *int) int {
    *p++ // tăng giá trị mà p trỏ đến; không thay đổi p
    return *p
}
v := 1
incr(&v)              // tác dụng phụ: v bây giờ là 2
fmt.Println(incr(&v)) // "3" (và v là 3)
```
Mỗi khi chúng ta lấy địa chỉ của một biến hoặc sao chép một con trỏ, chúng ta tạo ra các *bí danh* (alias) mới hoặc các cách để xác định cùng một biến. Ví dụ, `*p` là một bí danh cho `v`. Việc tạo bí danh bằng con trỏ rất hữu ích vì nó cho phép chúng ta truy cập một biến mà không cần sử dụng tên của nó, nhưng đây là một con dao hai lưỡi: để tìm tất cả các câu lệnh truy cập một biến, chúng ta phải biết tất cả các bí danh của nó. Không chỉ con trỏ mới tạo ra bí danh; bí danh cũng xảy ra khi chúng ta sao chép các giá trị của các kiểu tham chiếu khác như slice, map, và channel, và ngay cả struct, mảng, và interface chứa các kiểu này.

Con trỏ là chìa khóa cho gói `flag`, gói này sử dụng các đối số dòng lệnh của chương trình để đặt giá trị của một số biến nhất định được phân bổ khắp chương trình. Để minh họa, biến thể này của lệnh `echo` trước đó có hai cờ (flag) tùy chọn: `-n` làm cho `echo` bỏ qua dòng mới ở cuối mà thông thường sẽ được in ra, và `-s sep` làm cho nó phân tách các đối số đầu ra bằng nội dung của chuỗi `sep` thay vì khoảng trắng đơn mặc định. Vì đây là phiên bản thứ tư của chúng ta, gói được gọi là `gopl.io/ch2/echo4`.

gopl.io/ch2/echo4
```go
// Echo4 in các đối số dòng lệnh của nó.
package main

import (
    "flag"
    "fmt"
    "strings"
)

var n = flag.Bool("n", false, "bỏ qua dòng mới ở cuối")
var sep = flag.String("s", " ", "dấu phân tách")

func main() {
    flag.Parse()
    fmt.Print(strings.Join(flag.Args(), *sep))
    if !*n {
        fmt.Println()
    }
}
```
Hàm `flag.Bool` tạo một biến cờ mới có kiểu `bool`. Nó nhận ba đối số: tên của cờ ("n"), giá trị mặc định của biến (false), và một thông báo sẽ được in nếu người dùng cung cấp một đối số không hợp lệ, một cờ không hợp lệ, hoặc `-h` hoặc `-help`. Tương tự, `flag.String` nhận một tên, một giá trị mặc định, và một thông báo, và tạo ra một biến `string`. Các biến `sep` và `n` là các con trỏ đến các biến cờ, phải được truy cập một cách gián tiếp là `*sep` và `*n`.

Khi chương trình chạy, nó phải gọi `flag.Parse` trước khi các cờ được sử dụng, để cập nhật các biến cờ từ giá trị mặc định của chúng. Các đối số không phải là cờ có sẵn từ `flag.Args()` dưới dạng một slice của chuỗi. Nếu `flag.Parse` gặp lỗi, nó sẽ in một thông báo sử dụng và gọi `os.Exit(2)` để kết thúc chương trình.

Hãy chạy một số trường hợp thử nghiệm trên `echo`:
```sh
$ go build gopl.io/ch2/echo4
$ ./echo4 a bc def
a bc def
$ ./echo4 -s / a bc def
a/bc/def
$ ./echo4 -n a bc def
a bc def$
$ ./echo4 -help
Usage of ./echo4:
  -n    omit trailing newline
  -s string
        separator (default " ")
```

### 2.3.3. Hàm `new` (The `new` Function)
Một cách khác để tạo một biến là sử dụng hàm dựng sẵn `new`. Biểu thức `new(T)` tạo ra một biến không tên có kiểu `T`, khởi tạo nó về giá trị zero của `T`, và trả về địa chỉ của nó, là một giá trị có kiểu `*T`.
```go
p := new(int)   // p, có kiểu *int, trỏ đến một biến int không tên
fmt.Println(*p) // "0"
*p = 2          // đặt giá trị của biến int không tên thành 2
fmt.Println(*p) // "2"
```
Một biến được tạo bằng `new` không khác gì một biến cục bộ thông thường mà địa chỉ của nó được lấy, ngoại trừ việc không cần phải tự nghĩ ra (và khai báo) một tên giả, và chúng ta có thể sử dụng `new(T)` trong một biểu thức. Do đó, `new` chỉ là một tiện ích cú pháp, chứ không phải là một khái niệm cơ bản:

Hai hàm `newInt` dưới đây có hành vi giống hệt nhau.
```go
func newInt() *int {
    return new(int)
}
```
```go
func newInt() *int {
    var dummy int
    return &dummy
}
```
Mỗi lệnh gọi `new` trả về một biến riêng biệt với một địa chỉ duy nhất:
```go
p := new(int)
q := new(int)
fmt.Println(p == q) // "false"
```
Có một ngoại lệ cho quy tắc này: hai biến có kiểu không mang thông tin và do đó có kích thước bằng không, chẳng hạn như `struct{}` hoặc `[0]int`, có thể, tùy thuộc vào cách triển khai, có cùng địa chỉ.

Hàm `new` tương đối ít được sử dụng vì các biến không tên phổ biến nhất là các kiểu struct, mà cú pháp literal struct (§4.4.1) linh hoạt hơn.

Vì `new` là một hàm được khai báo trước, không phải từ khóa, nên có thể định nghĩa lại tên này cho một thứ khác trong một hàm, ví dụ:
```go
func delta(old, new int) int { return new - old }
```
Tất nhiên, bên trong `delta`, hàm `new` dựng sẵn sẽ không khả dụng.

### 2.3.4. Vòng đời của biến (Lifetime of Variables)
Vòng đời của một biến là khoảng thời gian mà nó tồn tại khi chương trình thực thi.

Vòng đời của một biến cấp gói là toàn bộ thời gian thực thi của chương trình. Ngược lại, các biến cục bộ có vòng đời động: một phiên bản mới được tạo mỗi khi câu lệnh khai báo được thực thi, và biến đó tồn tại cho đến khi nó trở nên không thể truy cập được, tại thời điểm đó bộ nhớ của nó có thể được tái chế. Các tham số và kết quả của hàm cũng là các biến cục bộ; chúng được tạo mỗi khi hàm chứa chúng được gọi.

Ví dụ, trong đoạn trích này từ chương trình Lissajous của Phần 1.4:
```go
for t := 0.0; t < cycles*2*math.Pi; t += res {
    x := math.Sin(t)
    y := math.Sin(t*freq + phase)
    img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5),
    blackIndex)
}
```
biến `t` được tạo mỗi khi vòng lặp `for` bắt đầu, và các biến `x` và `y` mới được tạo trên mỗi lần lặp của vòng lặp.

Làm thế nào bộ thu gom rác (garbage collector) biết rằng bộ nhớ của một biến có thể được thu hồi? Câu chuyện đầy đủ chi tiết hơn nhiều so với những gì chúng ta cần ở đây, nhưng ý tưởng cơ bản là mọi biến cấp gói, và mọi biến cục bộ của mỗi hàm đang hoạt động hiện tại, đều có khả năng là điểm bắt đầu hoặc...
gốc của một đường dẫn đến biến đang xét, theo sau các con trỏ và các loại tham chiếu khác mà cuối cùng dẫn đến biến đó. Nếu không có đường dẫn nào như vậy tồn tại, biến đó đã trở nên không thể truy cập được, do đó nó không còn có thể ảnh hưởng đến phần còn lại của tính toán.

Bởi vì vòng đời của một biến chỉ được xác định bởi việc nó có thể truy cập được hay không, một biến cục bộ có thể tồn tại lâu hơn một vòng lặp của vòng lặp bao quanh nó. Nó có thể tiếp tục tồn tại ngay cả sau khi hàm bao quanh nó đã trả về.

Một trình biên dịch có thể chọn phân bổ các biến cục bộ trên heap hoặc trên stack nhưng, có lẽ đáng ngạc nhiên, lựa chọn này không được xác định bởi việc `var` hay `new` đã được sử dụng để khai báo biến.

var global *int
func f() { var x int
    x = 1
    func g() {
        y := new(int)
        *y = 1
        global = &x
    }
}

Ở đây, `x` phải được phân bổ trên heap vì nó vẫn có thể truy cập được từ biến `global` sau khi `f` đã trả về, mặc dù được khai báo là một biến cục bộ; chúng ta nói `x` "thoát" khỏi `f`. Ngược lại, khi `g` trả về, biến `*y` trở nên không thể truy cập được và có thể được tái chế. Vì `*y` không "thoát" khỏi `g`, trình biên dịch có thể an toàn phân bổ `*y` trên stack, mặc dù nó được phân bổ bằng `new`. Trong mọi trường hợp, khái niệm "thoát" không phải là điều bạn cần lo lắng để viết mã đúng, mặc dù nên ghi nhớ trong quá trình tối ưu hóa hiệu suất, vì mỗi biến "thoát" ra đều yêu cầu một lần cấp phát bộ nhớ bổ sung.

Thu gom rác là một sự trợ giúp to lớn trong việc viết các chương trình đúng đắn, nhưng nó không giải thoát bạn khỏi gánh nặng suy nghĩ về bộ nhớ. Bạn không cần phải cấp phát và giải phóng bộ nhớ một cách tường minh, nhưng để viết các chương trình hiệu quả, bạn vẫn cần phải nhận thức được về vòng đời của các biến. Ví dụ, việc giữ các con trỏ không cần thiết đến các đối tượng có vòng đời ngắn bên trong các đối tượng có vòng đời dài, đặc biệt là các biến toàn cục, sẽ ngăn trình thu gom rác thu hồi các đối tượng có vòng đời ngắn đó.

## 2.4 Phép gán (Assignments)

Giá trị được nắm giữ bởi một **variable** (biến) được cập nhật bằng một câu lệnh gán (**assignment statement**). Ở dạng đơn giản nhất, nó có một biến ở bên trái dấu `=` và một biểu thức (**expression**) ở bên phải.

```go
x = 1                       // biến có tên (named variable)
*p = true                   // biến gián tiếp (indirect variable)
person.name = "bob"         // trường của struct (struct field)
count[x] = count[x] * scale // phần tử của mảng, slice hoặc map
```

Mỗi toán tử nhị phân (**binary operator**) số học và thao tác bit đều có một toán tử gán tương ứng. Ví dụ, câu lệnh cuối cùng ở trên có thể được viết lại thành:

```go
count[x] *= scale
```

Cách này giúp chúng ta tránh việc phải lặp lại (và tính toán lại - **re-evaluate**) biểu thức của biến đó.

Các biến kiểu số (**numeric variables**) cũng có thể được tăng (**increment**) hoặc giảm (**decrement**) giá trị bằng các câu lệnh `++` và `--`:

```go
v := 1
v++    // tương đương v = v + 1; v trở thành 2
v--    // tương đương v = v - 1; v trở thành 1 lại
```

## 2.4.1 Phép gán Tuple (Tuple Assignment)

Một dạng gán khác, được gọi là **tuple assignment**, cho phép gán nhiều biến cùng một lúc. Tất cả các biểu thức ở vế phải đều được tính toán (**evaluated**) trước khi bất kỳ biến nào được cập nhật. Điều này làm cho nó hữu ích nhất khi một số biến xuất hiện ở cả hai phía của phép gán, ví dụ như khi hoán đổi (**swapping**) giá trị của hai biến:

```go
x, y = y, x
a[i], a[j] = a[j], a[i]
```

Hoặc khi tính ước chung lớn nhất (GCD) của hai số nguyên:

```go
func gcd(x, y int) int {
    for y != 0 {
        x, y = y, x%y
    }
    return x
}
```

Hoặc khi tính số Fibonacci thứ n theo cách lặp (**iteratively**):

```go
func fib(n int) int {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        x, y = y, x+y
    }
    return x
}
```

Phép gán Tuple cũng có thể làm cho một chuỗi các phép gán đơn giản trở nên gọn gàng (**compact**) hơn:

```go
i, j, k = 2, 3, 5
```

Tuy nhiên, về mặt phong cách (**style**), hãy tránh dùng dạng tuple nếu các biểu thức phức tạp; một chuỗi các câu lệnh riêng biệt sẽ dễ đọc hơn.

Một số biểu thức nhất định, chẳng hạn như lời gọi tới một hàm có nhiều kết quả trả về, sẽ sinh ra nhiều giá trị. Khi một lời gọi như vậy được sử dụng trong một câu lệnh gán, vế trái phải có số lượng biến bằng với số lượng kết quả mà hàm trả về.

```go
f, err = os.Open("foo.txt") // lời gọi hàm trả về hai giá trị
```

Thông thường, các hàm sử dụng các kết quả bổ sung này để chỉ ra một loại lỗi nào đó, hoặc bằng cách trả về một `error` như trong lời gọi `os.Open`, hoặc một `bool`, thường được gọi là `ok`. Như chúng ta sẽ thấy ở các chương sau, có ba toán tử đôi khi cũng hành xử theo cách này. Nếu một phép tra cứu map (**map lookup**) (§4.3), khẳng định kiểu (**type assertion**) (§7.10), hoặc nhận từ channel (**channel receive**) (§8.4.2) xuất hiện trong một phép gán mà mong đợi hai kết quả, mỗi phép toán này sẽ sinh ra thêm một kết quả boolean:

```go
v, ok = m[key] // tra cứu map
v, ok = x.(T)  // khẳng định kiểu
v, ok = <-ch   // nhận từ channel
```

Cũng giống như khai báo biến, chúng ta có thể gán các giá trị không mong muốn cho **blank identifier** (định danh trống):

```go
_, err = io.Copy(dst, src) // bỏ qua số lượng byte (discard byte count)
_, ok = x.(T)              // kiểm tra kiểu nhưng bỏ qua kết quả (discard result)
```

## 2.4.2 Khả năng gán (Assignability)

Các câu lệnh gán là một hình thức gán tường minh (**explicit assignment**), nhưng có nhiều nơi trong chương trình mà việc gán xảy ra một cách ngầm định (**implicitly**): một lời gọi hàm ngầm định gán các giá trị đối số (**argument values**) cho các biến tham số (**parameter variables**) tương ứng; một câu lệnh `return` ngầm định gán các toán hạng trả về cho các biến kết quả tương ứng; và một biểu thức literal cho một kiểu dữ liệu hỗn hợp (**composite type**) chẳng hạn như slice này:

```go
medals := []string{"gold", "silver", "bronze"}
```

ngầm định gán từng phần tử, giống như thể nó được viết như sau:

```go
medals[0] = "gold"
medals[1] = "silver"
medals[2] = "bronze"
```

Các phần tử của map và channel, mặc dù không phải là các biến thông thường, cũng phải tuân theo các phép gán ngầm định tương tự.

Một phép gán, dù là tường minh hay ngầm định, luôn hợp lệ nếu vế trái (biến) và vế phải (giá trị) có cùng kiểu dữ liệu. Nói một cách tổng quát hơn, phép gán chỉ hợp lệ nếu giá trị đó có thể gán được (**assignable**) cho kiểu của biến.

Quy tắc về khả năng gán có các trường hợp cho nhiều kiểu dữ liệu khác nhau, vì vậy chúng tôi sẽ giải thích trường hợp liên quan khi giới thiệu từng kiểu mới. Đối với các kiểu chúng ta đã thảo luận cho đến nay, các quy tắc rất đơn giản: các kiểu phải khớp chính xác, và `nil` có thể được gán cho bất kỳ biến nào thuộc kiểu interface hoặc kiểu tham chiếu (**reference type**). Hằng số (**Constants**) có các quy tắc linh hoạt hơn về khả năng gán giúp tránh việc phải thực hiện hầu hết các chuyển đổi tường minh (**explicit conversions**).

Liệu hai giá trị có thể được so sánh bằng `==` và `!=` hay không có liên quan đến khả năng gán: trong bất kỳ phép so sánh nào, toán hạng đầu tiên phải có khả năng gán được cho kiểu của toán hạng thứ hai, hoặc ngược lại. Tương tự như khả năng gán, chúng tôi sẽ giải thích các trường hợp liên quan về khả năng so sánh (**comparability**) khi trình bày từng kiểu mới.

## 2.5 Khai báo kiểu (Type Declarations)

Kiểu của một biến hoặc biểu thức xác định các đặc tính của giá trị mà nó có thể nắm giữ, chẳng hạn như kích thước (số lượng bit hoặc số lượng phần tử), cách chúng được biểu diễn nội bộ, các hoạt động cơ bản (**intrinsic operations**) có thể thực hiện trên chúng và các phương thức (**methods**) liên kết với chúng.

Trong bất kỳ chương trình nào, có những biến chia sẻ cùng một cách biểu diễn nhưng lại mang ý nghĩa cho các khái niệm rất khác nhau. Ví dụ, một `int` có thể được dùng để biểu diễn chỉ số vòng lặp, dấu thời gian (**timestamp**), bộ mô tả tệp (**file descriptor**) hoặc một tháng; một `float64` có thể biểu diễn vận tốc tính bằng mét trên giây hoặc nhiệt độ ở một trong nhiều thang đo; và một `string` có thể biểu diễn mật khẩu hoặc tên của một màu sắc.

Một **type declaration** (khai báo kiểu) định nghĩa một kiểu dữ liệu mới có tên, có cùng **underlying type** (kiểu cơ sở/kiểu thực thể) với một kiểu hiện có. Kiểu có tên cung cấp một cách để tách biệt các cách sử dụng khác nhau và có thể không tương thích của kiểu cơ sở để chúng không bị trộn lẫn một cách vô tình.

```go
type name underlying-type
```

Khai báo kiểu thường xuất hiện ở cấp độ package, nơi kiểu có tên có thể hiển thị trong toàn bộ package, và nếu tên được xuất khẩu (**exported** - bắt đầu bằng chữ cái viết hoa), nó có thể truy cập được từ các package khác.

Để minh họa, hãy biến các thang đo nhiệt độ khác nhau thành các kiểu khác nhau:

```go
// Package tempconv thực hiện các tính toán nhiệt độ Celsius và Fahrenheit.
package tempconv

import "fmt"

type Celsius float64
type Fahrenheit float64

const (
    AbsoluteZeroC Celsius = -273.15
    FreezingC     Celsius = 0
    BoilingC      Celsius = 100
)

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }
func FToC(f Fahrenheit) Celsius { return Celsius((f - 32) * 5 / 9) }
```

Package này định nghĩa hai kiểu, `Celsius` và `Fahrenheit`, cho hai đơn vị nhiệt độ. Mặc dù cả hai đều có cùng kiểu cơ sở là `float64`, chúng không phải là cùng một kiểu, vì vậy chúng không thể so sánh hoặc kết hợp trong các biểu thức số học. Việc phân biệt các kiểu giúp tránh được các lỗi như vô tình kết hợp nhiệt độ ở hai thang đo khác nhau; một sự chuyển đổi kiểu tường minh (**explicit type conversion**) như `Celsius(t)` hoặc `Fahrenheit(t)` là bắt buộc để chuyển đổi từ một kiểu `float64`. `Celsius(t)` và `Fahrenheit(t)" là các phép chuyển đổi, không phải là lời gọi hàm. Chúng không làm thay đổi giá trị hoặc cách biểu diễn dưới bất kỳ hình thức nào, nhưng chúng làm cho sự thay đổi về ý nghĩa trở nên rõ ràng.

Mặt khác, các hàm `CToF` và `FToC` thực hiện chuyển đổi giữa hai thang đo; chúng trả về các giá trị khác nhau.

Đối với mỗi kiểu `T`, có một toán tử chuyển đổi tương ứng `T(x)` giúp chuyển đổi giá trị `x` sang kiểu `T`. Một sự chuyển đổi từ kiểu này sang kiểu khác được cho phép nếu cả hai có cùng kiểu cơ sở, hoặc nếu cả hai đều là các kiểu con trỏ không tên trỏ đến các biến có cùng kiểu cơ sở; những chuyển đổi này thay đổi kiểu nhưng không thay đổi cách biểu diễn của giá trị. Nếu `x` có thể gán cho `T`, một phép chuyển đổi được cho phép nhưng thường là dư thừa (**redundant**).

Chuyển đổi cũng được cho phép giữa các kiểu số, và giữa `string` với một số kiểu `slice`. Những chuyển đổi này có thể làm thay đổi cách biểu diễn của giá trị. Ví dụ, chuyển đổi một số dấu phẩy động thành số nguyên sẽ loại bỏ phần phân số, và chuyển đổi một `string` thành `[]byte` slice sẽ cấp phát một bản sao của dữ liệu chuỗi. Trong mọi trường hợp, một phép chuyển đổi không bao giờ thất bại khi chạy (**run time**).

Kiểu cơ sở của một kiểu có tên xác định cấu trúc và cách biểu diễn của nó, cũng như tập hợp các toán tử cơ bản mà nó hỗ trợ, vốn giống như thể kiểu cơ sở đó được sử dụng trực tiếp. Điều đó có nghĩa là các toán tử số học hoạt động với `Celsius` và `Fahrenheit` giống hệt như với `float64`.

```go
fmt.Printf("%g
", BoilingC-FreezingC) // "100" °C
boilingF := CToF(BoilingC)
fmt.Printf("%g
", boilingF-CToF(FreezingC)) // "180" °F
fmt.Printf("%g
", boilingF-FreezingC) // compile error: type mismatch
```

Các toán tử so sánh như `==` và `<` cũng có thể được dùng để so sánh một giá trị của kiểu có tên với một giá trị khác cùng kiểu, hoặc với một giá trị của kiểu cơ sở. Nhưng hai giá trị của các kiểu có tên khác nhau không thể so sánh trực tiếp với nhau:

```go
var c Celsius
var f Fahrenheit
fmt.Println(c == 0) // "true"
fmt.Println(f >= 0) // "true"
fmt.Println(c == f) // compile error: type mismatch
fmt.Println(c == Celsius(f)) // "true"!
```

Hãy lưu ý kỹ trường hợp cuối cùng. Bất kể tên gọi của nó là gì, phép chuyển đổi kiểu `Celsius(f)` không làm thay đổi giá trị đối số của nó, chỉ thay đổi kiểu của nó. Phép thử là `true` vì cả `c` và `f` đều bằng không.

Một kiểu có tên có thể mang lại sự tiện lợi về mặt ký hiệu (**notational convenience**) nếu nó giúp tránh việc phải viết đi viết lại các kiểu phức tạp. Lợi thế này là nhỏ khi kiểu cơ sở đơn giản như `float64`, nhưng sẽ lớn đối với các kiểu phức tạp, như chúng ta sẽ thấy khi thảo luận về **structs**.

Các kiểu có tên cũng giúp có thể định nghĩa các hành vi mới cho các giá trị của kiểu đó. Các hành vi này được thể hiện dưới dạng một tập hợp các hàm liên kết với kiểu, được gọi là các phương thức (**methods**) của kiểu.

Khai báo bên dưới, trong đó tham số `Celsius c` xuất hiện trước tên hàm, liên kết với kiểu `Celsius` một phương thức tên là `String` trả về giá trị số của `c` theo sau là `°C`:

```go
func (c Celsius) String() string { return fmt.Sprintf("%g°C", c) }
```

Nhiều kiểu khai báo một phương thức `String` ở dạng này vì nó kiểm soát cách các giá trị của kiểu xuất hiện khi được in ra dưới dạng chuỗi bởi package `fmt`.

```go
c := FToC(212.0)
fmt.Println(c.String()) // "100°C"
fmt.Printf("%v
", c)   // "100°C"; không cần gọi String tường minh
fmt.Printf("%s
", c)   // "100°C"
fmt.Println(c)          // "100°C"
fmt.Printf("%g
", c)   // "100"; không gọi String
fmt.Println(float64(c)) // "100"; không gọi String
```

## 2.6 Package và File (Packages and Files)

Các package trong Go phục vụ các mục đích tương tự như thư viện hoặc module trong các ngôn ngữ khác, hỗ trợ tính **modularity** (mô đun hóa), **encapsulation** (đóng gói), **separate compilation** (biên dịch riêng biệt) và **reuse** (tái sử dụng). Mã nguồn cho một package nằm trong một hoặc nhiều file `.go`, thường là trong một thư mục có tên kết thúc bằng đường dẫn import; ví dụ, các file của package `gopl.io/ch1/helloworld` được lưu trữ trong thư mục `/src/gopl.io/ch1/helloworld`.

Mỗi package đóng vai trò là một **namespace** (không gian tên) riêng biệt cho các khai báo (**declarations**) của nó. Chẳng hạn, trong package `image`, định danh (**identifier**) `Decode` tham chiếu đến một hàm khác so với cùng định danh đó trong package `unicode/utf16`. Để tham chiếu đến một hàm từ bên ngoài package của nó, chúng ta phải **qualify** (định danh đầy đủ) định danh đó để làm rõ chúng ta muốn `image.Decode` hay `utf16.Decode`.

Các package cũng cho phép chúng ta ẩn thông tin bằng cách kiểm soát những tên nào hiển thị bên ngoài package, hay còn gọi là **exported** (được xuất). Trong Go, một quy tắc đơn giản điều chỉnh các định danh nào được xuất và định danh nào không: các định danh được xuất bắt đầu bằng chữ cái viết hoa.

Để minh họa các kiến thức cơ bản, giả sử phần mềm chuyển đổi nhiệt độ của chúng ta đã trở nên phổ biến và chúng ta muốn cung cấp nó cho cộng đồng Go dưới dạng một package mới. Chúng ta làm điều đó như thế nào?

Hãy tạo một package có tên `gopl.io/ch2/tempconv`, một biến thể của ví dụ trước. (Ở đây chúng ta đã bỏ qua quy tắc thông thường về đánh số ví dụ theo trình tự, để đường dẫn package có thể thực tế hơn.) Package này được lưu trữ trong hai file để cho thấy cách các khai báo trong các file riêng biệt của một package được truy cập; trong thực tế, một package nhỏ như thế này chỉ cần một file.

Chúng ta đã đặt các khai báo về kiểu, các hằng số của chúng và các phương thức của chúng trong `tempconv.go`:

```go
// Package tempconv thực hiện chuyển đổi nhiệt độ Celsius và Fahrenheit.
package tempconv
import "fmt"
type Celsius float64
type Fahrenheit float64
const (
AbsoluteZeroC Celsius = -273.15
FreezingC Celsius = 0
BoilingC Celsius = 100
)
func (c Celsius) String() string { return fmt.Sprintf("%g°C", c) }
func (f Fahrenheit) String() string { return fmt.Sprintf("%g°F", f) }
```

và các hàm chuyển đổi trong `conv.go`:

```go
package tempconv
// CToF chuyển đổi nhiệt độ Celsius sang Fahrenheit.
func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }
// FToC chuyển đổi nhiệt độ Fahrenheit sang Celsius.
func FToC(f Fahrenheit) Celsius { return Celsius((f - 32) * 5 / 9) }
```

Mỗi file bắt đầu bằng một khai báo package định nghĩa tên package. Khi package được import, các thành viên của nó được tham chiếu là `tempconv.CToF` và cứ thế. Các tên cấp package như các kiểu và hằng số được khai báo trong một file của package đều hiển thị cho tất cả các file khác của package, như thể mã nguồn đều nằm trong một file duy nhất. Lưu ý rằng `tempconv.go` import `fmt`, nhưng `conv.go` thì không, bởi vì nó không sử dụng bất cứ thứ gì từ `fmt`.

Bởi vì các tên hằng số cấp package bắt đầu bằng chữ cái viết hoa, chúng cũng có thể truy cập được bằng các tên định danh đầy đủ như `tempconv.AbsoluteZeroC`:

```go
fmt.Printf("Brrrr! %v
", tempconv.AbsoluteZeroC) // "Brrrr! -273.15°C"
```

Để chuyển đổi nhiệt độ Celsius sang Fahrenheit trong một package mà import `gopl.io/ch2/tempconv`, chúng ta có thể viết đoạn code sau:

```go
fmt.Println(tempconv.CToF(tempconv.BoilingC)) // "212°F"
```

**Doc comment** (Bình luận tài liệu) ngay trước khai báo package tài liệu hóa toàn bộ package. Theo **convention** (quy ước), nó nên bắt đầu bằng một câu tóm tắt theo phong cách đã minh họa. Chỉ một file trong mỗi package nên có bình luận tài liệu package. Các bình luận tài liệu mở rộng thường được đặt trong một file riêng, theo quy ước được gọi là `doc.go`.

**Bài tập 2.1:** Thêm các kiểu, hằng số và hàm vào `tempconv` để xử lý nhiệt độ theo thang Kelvin, trong đó 0 Kelvin là `-273.15°C` và độ chênh lệch 1K có cùng độ lớn với `1°C`.

## 2.6.1 Import (Imports)

Trong một chương trình Go, mỗi package được xác định bằng một chuỗi duy nhất gọi là **import path** (đường dẫn import). Đây là những chuỗi xuất hiện trong một khai báo import như `"gopl.io/ch2/tempconv"`. Đặc tả ngôn ngữ (**language specification**) không định nghĩa những chuỗi này đến từ đâu hay chúng có ý nghĩa gì; việc giải thích chúng là tùy thuộc vào các công cụ. Khi sử dụng công cụ `go` (Chương 10), một đường dẫn import biểu thị (**denotes**) một thư mục chứa một hoặc nhiều file mã nguồn Go cùng nhau tạo nên package.

Ngoài đường dẫn import, mỗi package còn có một **package name** (tên package), là tên ngắn (và không nhất thiết là duy nhất) xuất hiện trong khai báo package của nó. Theo **convention** (quy ước), tên của một package khớp với phần cuối (**segment**) của đường dẫn import của nó, giúp dễ dàng dự đoán rằng tên package của `gopl.io/ch2/tempconv` là `tempconv`.

Để sử dụng `gopl.io/ch2/tempconv`, chúng ta phải import nó:

```go
// gopl.io/ch2/cf
package main

// Cf converts its numeric argument to Celsius and Fahrenheit.
import (
	"fmt"
	"os"
	"strconv"

	"gopl.io/ch2/tempconv"
)

func main() {
	for _, arg := range os.Args[1:] {
		t, err := strconv.ParseFloat(arg, 64)
		if err != nil {
			fmt.Fprintf(os.Stderr, "cf: %v
", err)
			os.Exit(1)
		}
		f := tempconv.Fahrenheit(t)
		c := tempconv.Celsius(t)
		fmt.Printf("%s = %s, %s = %s
",
			f, tempconv.FToC(f), c, tempconv.CToF(c))
	}
}
```

Khai báo import **binds** (liên kết) một tên ngắn với package đã import, tên này có thể được sử dụng để tham chiếu đến nội dung của nó trong suốt file. Lệnh import trên cho phép chúng ta tham chiếu đến các tên trong `gopl.io/ch2/tempconv` bằng cách sử dụng một **qualified identifier** (định danh đầy đủ) như `tempconv.CToF`. Theo mặc định, tên ngắn là tên package—`tempconv` trong trường hợp này—nhưng một khai báo import có thể chỉ định một **alternative name** (tên thay thế) để tránh **conflict** (xung đột) (§10.3).

Chương trình `cf` chuyển đổi một đối số số từ dòng lệnh (**command-line argument**) thành giá trị tương ứng ở cả Celsius và Fahrenheit:

```bash
$ go build gopl.io/ch2/cf
$ ./cf 32
32°F = 0°C, 32°C = 89.6°F
$ ./cf 212
212°F = 100°C, 212°C = 413.6°F
$ ./cf -40
-40°F = -40°C, -40°C = -40°F
```

Việc import một package mà sau đó không tham chiếu đến nó là một lỗi. Việc kiểm tra này giúp loại bỏ các **dependencies** (phụ thuộc) trở nên **unnecessary** (không cần thiết) khi code phát triển, mặc dù nó có thể gây **nuisance** (khó chịu) trong quá trình **debugging** (gỡ lỗi), vì việc comment một dòng code như `log.Print("got here!")` có thể loại bỏ **sole reference** (tham chiếu duy nhất) đến tên package `log`, khiến trình biên dịch phát ra lỗi. Trong tình huống này, bạn cần comment hoặc xóa import không cần thiết.

Tốt hơn nữa, hãy sử dụng công cụ `golang.org/x/tools/cmd/goimports`, công cụ này tự động chèn và loại bỏ các package khỏi khai báo import khi cần thiết; hầu hết các editor có thể được cấu hình để chạy `goimports` mỗi khi bạn lưu file. Giống như công cụ `gofmt`, nó cũng **pretty-prints** (định dạng đẹp) các file mã nguồn Go theo **canonical format** (định dạng chuẩn).

**Bài tập 2.2:** Viết một chương trình chuyển đổi đơn vị **general-purpose** (đa năng/tổng quát) **analogous** (tương tự) như `cf` đọc các số từ các đối số dòng lệnh hoặc từ đầu vào chuẩn nếu không có đối số, và chuyển đổi mỗi số thành các đơn vị như nhiệt độ theo Celsius và Fahrenheit, chiều dài theo feet và mét, trọng lượng theo pound và kilogam, và những thứ tương tự.

## 2.6.2 Khởi tạo Package (Package Initialization)

Khởi tạo package bắt đầu bằng cách khởi tạo các biến cấp package theo thứ tự chúng được khai báo, ngoại trừ các **dependencies** (phụ thuộc) được **resolved** (giải quyết) trước tiên:

```go
var a = b + c // a được khởi tạo thứ ba, bằng 3
var b = f() // b được khởi tạo thứ hai, bằng 2, bằng cách gọi f
var c = 1 // c được khởi tạo đầu tiên, bằng 1
func f() int { return c + 1 }
```

Nếu package có nhiều file `.go`, chúng sẽ được khởi tạo theo thứ tự các file được cung cấp cho trình biên dịch; công cụ `go` sẽ sắp xếp các file `.go` theo tên trước khi gọi trình biên dịch.

Mỗi biến được khai báo ở cấp package bắt đầu với giá trị của biểu thức khởi tạo (**initializer expression**) của nó (nếu có), nhưng đối với một số biến, như bảng dữ liệu, một biểu thức khởi tạo có thể không phải là cách đơn giản nhất để đặt giá trị ban đầu của nó. Trong trường hợp đó, cơ chế hàm `init` có thể đơn giản hơn. Bất kỳ file nào cũng có thể chứa bất kỳ số lượng hàm nào có khai báo chỉ là:

```go
func init() { /* ... */ }
```

Các hàm `init` như vậy không thể được gọi hoặc tham chiếu, nhưng mặt khác chúng là các hàm thông thường. Trong mỗi file, các hàm `init` được thực thi tự động khi chương trình bắt đầu, theo thứ tự chúng được khai báo.

Một package được khởi tạo tại một thời điểm, theo thứ tự import trong chương trình, các phụ thuộc trước, do đó một package `p` import `q` có thể chắc chắn rằng `q` đã được khởi tạo hoàn toàn trước khi quá trình khởi tạo của `p` bắt đầu. Quá trình khởi tạo diễn ra từ dưới lên; package `main` là package cuối cùng được khởi tạo. Bằng cách này, tất cả các package được khởi tạo hoàn toàn trước khi hàm `main` của ứng dụng bắt đầu.

Package dưới đây định nghĩa một hàm `PopCount` trả về số lượng bit được đặt (**set bits**), tức là các bit có giá trị là 1, trong một giá trị `uint64`, được gọi là **population count** (số lượng dân số/số bit 1). Nó sử dụng một hàm `init` để **pre-compute** (tính toán trước) một **lookup table** (bảng tra cứu) `pc` cho mỗi giá trị 8-bit có thể có, để hàm `PopCount` không cần thực hiện 64 bước mà chỉ cần trả về tổng của tám lần tra cứu bảng. (Đây chắc chắn không phải là **algorithm** (thuật toán) nhanh nhất để đếm bit, nhưng nó tiện lợi để minh họa các hàm `init`, và để cho thấy cách tính toán trước một bảng các giá trị, đây thường là một kỹ thuật lập trình hữu ích.)

```go
// gopl.io/ch2/popcount
package popcount

// pc[i] là số bit 1 của i.
var pc [256]byte

func init() {
	for i := range pc {
		pc[i] = pc[i/2] + byte(i&1)
	}
}

// PopCount trả về số lượng bit 1 của x.
func PopCount(x uint64) int {
	return int(
		pc[byte(x>>(0*8))] +
		pc[byte(x>>(1*8))] +
		pc[byte(x>>(2*8))] +
		pc[byte(x>>(3*8))] +
		pc[byte(x>>(4*8))] +
		pc[byte(x>>(5*8))] +
		pc[byte(x>>(6*8))] +
		pc[byte(x>>(7*8))])
}
```

Lưu ý rằng vòng lặp **range loop** trong `init` chỉ sử dụng **index** (chỉ mục); **value** (giá trị) là không cần thiết và do đó không cần phải bao gồm. Vòng lặp cũng có thể được viết là:

```go
for i, _ := range pc {
```

Chúng ta sẽ thấy các cách sử dụng khác của hàm `init` trong phần tiếp theo và trong Phần 10.5.

**Bài tập 2.3:** Viết lại `PopCount` để sử dụng một vòng lặp thay vì một biểu thức duy nhất. So sánh **performance** (hiệu suất) của hai phiên bản. (Phần 11.4 cho thấy cách so sánh hiệu suất của các triển khai khác nhau một cách có hệ thống.)

**Bài tập 2.4:** Viết một phiên bản của `PopCount` đếm bit bằng cách **shifting** (dịch chuyển) đối số của nó qua 64 **bit position** (vị trí bit), kiểm tra **rightmost bit** (bit ngoài cùng bên phải) mỗi lần. So sánh hiệu suất của nó với phiên bản tra cứu bảng.

**Bài tập 2.5:** Biểu thức `x&(x-1)` xóa **rightmost non-zero bit** (bit không phải số 0 ngoài cùng bên phải) của `x`. Viết một phiên bản của `PopCount` đếm bit bằng cách sử dụng thực tế này, và **assess its performance** (đánh giá hiệu suất của nó).



## 2.7. Scope (Phạm vi)

Một **khai báo (declaration)** liên kết một tên với một **thực thể chương trình (program entity)**, chẳng hạn như một hàm hoặc một biến. **Phạm vi (scope)** của một khai báo là phần mã nguồn nơi mà việc sử dụng tên đã khai báo tham chiếu đến khai báo đó. Đừng nhầm lẫn phạm vi với **vòng đời (lifetime)**. Phạm vi của một khai báo là một vùng văn bản của chương trình; nó là một thuộc tính **thời gian biên dịch (compile-time)**. Vòng đời của một biến là khoảng thời gian trong quá trình thực thi khi biến đó có thể được tham chiếu bởi các phần khác của chương trình; nó là một thuộc tính **thời gian chạy (run-time)**.

Một **khối cú pháp (syntactic block)** là một chuỗi các câu lệnh được bao quanh bởi các dấu ngoặc nhọn, giống như các dấu bao quanh thân hàm hoặc vòng lặp. Một tên được khai báo bên trong một khối cú pháp sẽ không được nhìn thấy bên ngoài khối đó. Khối này bao bọc các khai báo của nó và xác định phạm vi của chúng. Chúng ta có thể tổng quát hóa khái niệm khối này để bao gồm các nhóm khai báo khác không được bao quanh rõ ràng bởi dấu ngoặc nhọn trong mã nguồn; chúng ta sẽ gọi tất cả chúng là **khối từ vựng (lexical blocks)**. Có một khối từ vựng cho toàn bộ mã nguồn, được gọi là **khối vũ trụ (universe block)**; cho mỗi gói (package); cho mỗi tệp; cho mỗi câu lệnh `for`, `if`, và `switch`; cho mỗi `case` trong câu lệnh `switch` hoặc `select`; và tất nhiên, cho mỗi khối cú pháp rõ ràng.

Khối từ vựng của một khai báo xác định phạm vi của nó, có thể lớn hoặc nhỏ. Các khai báo của **các kiểu dựng sẵn (built-in types)**, hàm và hằng số như `int`, `len` và `true` nằm trong khối vũ trụ và có thể được tham chiếu trong toàn bộ chương trình. Các khai báo bên ngoài bất kỳ hàm nào, tức là ở **cấp gói (package level)**, có thể được tham chiếu từ bất kỳ tệp nào trong cùng một gói. Các gói được nhập (imported), chẳng hạn như `fmt` trong ví dụ `tempconv`, được khai báo ở cấp tệp, vì vậy chúng có thể được tham chiếu từ cùng một tệp, nhưng không phải từ tệp khác trong cùng một gói nếu không có lệnh import khác. Nhiều khai báo, giống như khai báo của biến `c` trong hàm `tempconv.CToF`, là **cục bộ (local)**, vì vậy chúng chỉ có thể được tham chiếu từ bên trong cùng một hàm hoặc có thể chỉ là một phần của nó.

Phạm vi của một **nhãn luồng điều khiển (control-flow label)**, được sử dụng bởi các câu lệnh `break`, `continue`, và `goto`, là toàn bộ hàm bao quanh.

Một chương trình có thể chứa nhiều khai báo của cùng một tên miễn là mỗi khai báo nằm trong một khối từ vựng khác nhau. Ví dụ, bạn có thể khai báo một biến cục bộ có cùng tên với một biến cấp gói. Hoặc, như đã trình bày trong Phần 2.3.3, bạn có thể khai báo một tham số hàm gọi là `new`, mặc dù hàm có tên này đã được khai báo trước trong khối vũ trụ. Tuy nhiên, đừng lạm dụng nó; phạm vi của việc khai báo lại càng lớn, bạn càng dễ làm người đọc ngạc nhiên.

Khi trình biên dịch gặp một tham chiếu đến một tên, nó sẽ tìm kiếm một khai báo, bắt đầu với khối từ vựng bao quanh bên trong cùng và làm việc ngược lên khối vũ trụ. Nếu trình biên dịch không tìm thấy khai báo nào, nó sẽ báo lỗi "tên chưa được khai báo" ("undeclared name"). Nếu một tên được khai báo trong cả khối bên ngoài và khối bên trong, khai báo bên trong sẽ được tìm thấy trước. Trong trường hợp đó, khai báo bên trong được cho là **che khuất (shadow)** hoặc **ẩn (hide)** khai báo bên ngoài, làm cho nó không thể truy cập được:

```go
func f() {}

var g = "g"

func main() {
    f := "f"
    fmt.Println(f) // "f"; biến cục bộ f che khuất hàm f cấp gói
    fmt.Println(g) // "g"; biến cấp gói
    fmt.Println(h) // lỗi biên dịch: chưa định nghĩa: h
}
```



Trong một hàm, các khối từ vựng có thể được **lồng nhau (nested)** đến độ sâu tùy ý, vì vậy một khai báo cục bộ có thể **che khuất (shadow)** một khai báo khác. Hầu hết các khối được tạo ra bởi các cấu trúc luồng điều khiển như câu lệnh `if` và vòng lặp `for`. Chương trình dưới đây có ba biến khác nhau được gọi là `x` vì mỗi khai báo xuất hiện trong một khối từ vựng khác nhau. (Ví dụ này minh họa các quy tắc về phạm vi, không phải là phong cách lập trình tốt!)

```go
func main() {
    x := "hello!"
    for i := 0; i < len(x); i++ {
        x := x[i]
        if x != '!' {
            x := x + 'A' - 'a'
            fmt.Printf("%c", x) // "HELLO" (mỗi lần lặp một chữ cái)
        }
    }
}
```

Các biểu thức `x[i]` và `x + 'A' - 'a'` đều tham chiếu đến khai báo của `x` từ một khối bên ngoài; chúng tôi sẽ giải thích điều đó ngay sau đây. (Lưu ý rằng biểu thức sau không tương đương với `unicode.ToUpper`.)

Như đã đề cập ở trên, không phải tất cả các khối từ vựng đều tương ứng với các chuỗi câu lệnh được giới hạn rõ ràng bằng dấu ngoặc nhọn; một số chỉ là **ngụ ý (implied)** hay còn gọi là **khối ẩn**. Vòng lặp `for` ở trên tạo ra hai khối từ vựng: khối **tường minh (explicit)** cho thân vòng lặp, và một khối ẩn bao quanh thêm các biến được khai báo bởi **mệnh đề khởi tạo (initialization clause)**, chẳng hạn như `i`. Phạm vi của một biến được khai báo trong khối ẩn là điều kiện, câu lệnh sau vòng lặp (post-statement, ví dụ `i++`), và thân của câu lệnh `for`.

Ví dụ dưới đây cũng có ba biến tên là `x`, mỗi biến được khai báo trong một khối khác nhau—một trong thân hàm, một trong khối của câu lệnh `for`, và một trong thân vòng lặp—nhưng chỉ có hai trong số các khối là tường minh:

```go
func main() {
    x := "hello"
    for _, x := range x {
        x := x + 'A' - 'a'
        fmt.Printf("%c", x) // "HELLO" (mỗi lần lặp một chữ cái)
    }
}
```

Giống như vòng lặp `for`, các câu lệnh `if` và `switch` cũng tạo ra các khối ẩn bên cạnh các khối thân của chúng. Đoạn mã trong chuỗi `if-else` sau đây cho thấy phạm vi của `x` và `y`:

```go
if x := f(); x == 0 {
    fmt.Println(x)
} else if y := g(x); x == y {
    fmt.Println(x, y)
} else {
    fmt.Println(x, y)
}
fmt.Println(x, y) // lỗi biên dịch: x và y không hiển thị ở đây
```

Câu lệnh `if` thứ hai được lồng bên trong câu lệnh thứ nhất, vì vậy các biến được khai báo trong bộ khởi tạo của câu lệnh thứ nhất sẽ hiển thị bên trong câu lệnh thứ hai. Các quy tắc tương tự cũng áp dụng cho mỗi `case` của câu lệnh `switch`: có một khối cho điều kiện và một khối cho mỗi thân `case`.

Ở cấp độ gói (package level), thứ tự xuất hiện của các khai báo không ảnh hưởng đến phạm vi của chúng, vì vậy một khai báo có thể tham chiếu đến chính nó hoặc đến một khai báo khác theo sau nó, cho phép chúng ta khai báo các kiểu và hàm đệ quy hoặc đệ quy tương hỗ. Tuy nhiên, trình biên dịch sẽ báo lỗi nếu một khai báo hằng số hoặc biến tham chiếu đến chính nó.

Trong chương trình này:

```go
if f, err := os.Open(fname); err != nil { // lỗi biên dịch: chưa sử dụng: f
    return err
}
f.ReadByte() // lỗi biên dịch: f chưa được định nghĩa
f.Close() // lỗi biên dịch: f chưa được định nghĩa
```

Phạm vi của `f` chỉ là câu lệnh `if`, vì vậy `f` không thể truy cập được đối với các câu lệnh theo sau, dẫn đến lỗi biên dịch. Tùy thuộc vào trình biên dịch, bạn có thể nhận thêm lỗi báo rằng biến cục bộ `f` không bao giờ được sử dụng.

Do đó, thường cần phải khai báo `f` trước điều kiện để nó có thể truy cập được sau đó:

```go
f, err := os.Open(fname)
if err != nil {
    return err
}
f.ReadByte()
f.Close()
```

Bạn có thể bị cám dỗ tránh việc khai báo `f` và `err` ở khối bên ngoài bằng cách di chuyển các lệnh gọi `ReadByte` và `Close` vào bên trong khối `else`:

```go
if f, err := os.Open(fname); err != nil {
    return err
} else {
    // f và err cũng hiển thị ở đây
    f.ReadByte()
    f.Close()
}
```

Nhưng **thông lệ bình thường (normal practice)** trong Go là xử lý lỗi trong khối `if` và sau đó `return`, để luồng thực thi thành công không bị thụt đầu dòng (indented).

Các khai báo biến ngắn gọn (`:=`) đòi hỏi sự nhận thức về phạm vi. Hãy xem xét chương trình dưới đây, bắt đầu bằng việc lấy thư mục làm việc hiện tại và lưu nó vào một biến cấp gói. Điều này có thể được thực hiện bằng cách gọi `os.Getwd` trong hàm `main`, nhưng có thể tốt hơn nếu tách mối quan tâm này khỏi logic chính, đặc biệt nếu việc không lấy được thư mục là một lỗi nghiêm trọng. Hàm `log.Fatalf` in ra một thông báo và gọi `os.Exit(1)`.

```go
var cwd string
func init() {
    cwd, err := os.Getwd() // lỗi biên dịch: chưa sử dụng: cwd
    if err != nil {
        log.Fatalf("os.Getwd failed: %v", err)
    }
}
```

Vì cả `cwd` lẫn `err` đều chưa được khai báo trong khối của hàm `init`, câu lệnh `:=` khai báo **cả hai** là biến cục bộ. Khai báo bên trong của `cwd` làm cho khai báo bên ngoài không thể truy cập được, vì vậy câu lệnh không cập nhật biến `cwd` cấp gói như dự định.

Các trình biên dịch Go hiện tại phát hiện rằng biến `cwd` cục bộ không bao giờ được sử dụng và báo cáo đây là một lỗi, nhưng chúng không bắt buộc phải thực hiện kiểm tra này một cách nghiêm ngặt. Hơn nữa, một thay đổi nhỏ, chẳng hạn như thêm một câu lệnh ghi log tham chiếu đến `cwd` cục bộ sẽ đánh bại việc kiểm tra này.

```go
var cwd string
func init() {
    cwd, err := os.Getwd() // CHÚ Ý: sai!
    if err != nil {
        log.Fatalf("os.Getwd failed: %v", err)
    }
    log.Printf("Working directory = %s", cwd)
}
```

Biến `cwd` toàn cục vẫn chưa được khởi tạo, và kết quả log có vẻ bình thường sẽ **làm lu mờ (obfuscate)** lỗi này.

Có một số cách để giải quyết vấn đề tiềm ẩn này. Cách trực tiếp nhất là tránh `:=` bằng cách khai báo `err` trong một khai báo `var` riêng biệt:

```go
var cwd string
func init() {
    var err error
    cwd, err = os.Getwd()
    if err != nil {
        log.Fatalf("os.Getwd failed: %v", err)
    }
}
```

Chúng ta đã thấy cách các gói, tệp, khai báo và câu lệnh thể hiện cấu trúc của chương trình. Trong hai chương tiếp theo, chúng ta sẽ xem xét cấu trúc của dữ liệu.

