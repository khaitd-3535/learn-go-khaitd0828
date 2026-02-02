## Microservices: Cái Nhìn Tổng Quan

Microservices đã trở thành một lựa chọn kiến trúc (architecture) ngày càng phổ biến trong hơn nửa thập kỷ qua, kể từ khi tôi viết ấn bản đầu tiên của cuốn sách này. Tôi không dám nhận công lao cho sự bùng nổ phổ biến sau đó, nhưng việc đổ xô sử dụng kiến trúc microservices có nghĩa là: trong khi nhiều ý tưởng tôi đúc kết trước đây đã được kiểm chứng, thì những ý tưởng mới cũng đồng thời xuất hiện, và một số thực hành cũ đã không còn được ưa chuộng. Vì vậy, đây là lúc để chắt lọc lại tinh hoa của kiến trúc microservices, đồng thời làm nổi bật các khái niệm cốt lõi giúp microservices hoạt động hiệu quả.

Cuốn sách này được thiết kế để mang lại cái nhìn tổng quan rộng lớn về tác động của microservices lên các khía cạnh khác nhau của việc chuyển giao phần mềm (software delivery). Để bắt đầu, chương này sẽ xem xét các ý tưởng cốt lõi đằng sau microservices, những nền tảng trước đó đã dẫn dắt chúng ta đến đây, và lý do tại sao các kiến trúc này lại được sử dụng rộng rãi đến vậy.

### Microservices Sơ Lược (Microservices at a Glance)

**Microservices** là các dịch vụ có thể phát hành độc lập (**independently releasable**), được mô hình hóa xoay quanh một nghiệp vụ (**business domain**). Một dịch vụ sẽ đóng gói các chức năng và làm cho chúng có thể truy cập được bởi các dịch vụ khác thông qua mạng lưới — bạn xây dựng một hệ thống phức tạp hơn từ những khối xây dựng (building blocks) này.

> *Giải thích thuật ngữ:*
> *   **Microservices:** Một phong cách kiến trúc phần mềm mà ở đó ứng dụng được cấu trúc thành một tập hợp các dịch vụ nhỏ, độc lập.
> *   **Independently releasable (Có thể phát hành độc lập):** Khả năng cập nhật, sửa lỗi, hoặc triển khai lại một dịch vụ duy nhất mà không cần phải triển khai lại toàn bộ hệ thống lớn.
> *   **Business Domain (Miền nghiệp vụ):** Lĩnh vực hoạt động cụ thể của doanh nghiệp (ví dụ: Bán hàng, Kho vận, Kế toán). Microservices thường được chia theo các chức năng nghiệp vụ này thay vì chia theo kỹ thuật (như UI, Database).

Một microservice có thể đại diện cho kho hàng (inventory), một cái khác quản lý đơn hàng (order management), và cái khác nữa là vận chuyển (shipping), nhưng cùng nhau chúng có thể tạo thành một hệ thống thương mại điện tử hoàn chỉnh.

Microservices là một lựa chọn kiến trúc tập trung vào việc cung cấp cho bạn nhiều tùy chọn để giải quyết các vấn đề mà bạn có thể gặp phải. Chúng là một loại kiến trúc hướng dịch vụ (**Service-Oriented Architecture - SOA**), mặc dù là một loại có quan điểm riêng (opinionated) về cách phân chia ranh giới dịch vụ, và là nơi mà khả năng triển khai độc lập là chìa khóa. Chúng không phụ thuộc vào công nghệ (**technology agnostic**), đây là một trong những lợi thế mà chúng mang lại.

> *Giải thích thuật ngữ:*
> *   **Service-Oriented Architecture (SOA - Kiến trúc hướng dịch vụ):** Một kiểu thiết kế phần mềm nơi các thành phần ứng dụng cung cấp dịch vụ cho các thành phần khác thông qua giao thức truyền thông qua mạng. Microservices thường được xem là một dạng tiến hóa của SOA.
> *   **Technology agnostic (Không phụ thuộc công nghệ):** Khả năng tự do lựa chọn ngôn ngữ lập trình, cơ sở dữ liệu, hoặc công cụ khác nhau cho mỗi microservice, miễn là chúng giao tiếp được với nhau.

Từ bên ngoài, một microservice đơn lẻ được coi là một hộp đen (**black box**). Nó lưu trữ (host) các chức năng nghiệp vụ trên một hoặc nhiều điểm đầu cuối mạng (**network endpoints**) (ví dụ: một hàng đợi - **queue** hoặc một **REST API**, như trong Hình 1-1), qua bất kỳ giao thức nào phù hợp nhất.

> *Giải thích thuật ngữ:*
> *   **Black box (Hộp đen):** Thành phần mà người dùng bên ngoài chỉ quan tâm đến đầu vào (input) và đầu ra (output) mà không cần biết cấu tạo hay xử lý bên trong.
> *   **Network endpoints (Điểm đầu cuối mạng):** Địa chỉ cụ thể (như URL) nơi dịch vụ nhận yêu cầu.
> *   **Queue (Hàng đợi):** Một cơ chế giao tiếp không đồng bộ, nơi tin nhắn được xếp hàng chờ xử lý (ví dụ: RabbitMQ, Kafka).
> *   **REST API:** Một chuẩn thiết kế API phổ biến dùng HTTP để giao tiếp giữa các hệ thống.

Người tiêu dùng (consumers) — dù là các microservices khác hay các loại chương trình khác — truy cập chức năng này thông qua các network endpoints đó. Chi tiết triển khai nội bộ (chẳng hạn như công nghệ mà dịch vụ được viết hoặc cách dữ liệu được lưu trữ) hoàn toàn bị ẩn khỏi thế giới bên ngoài. Điều này có nghĩa là kiến trúc microservices tránh sử dụng cơ sở dữ liệu dùng chung (**shared databases**) trong hầu hết các trường hợp; thay vào đó, mỗi microservice đóng gói cơ sở dữ liệu riêng của nó khi cần thiết.

Microservices đề cao khái niệm che giấu thông tin (**information hiding**). Che giấu thông tin có nghĩa là giấu càng nhiều thông tin càng tốt bên trong một thành phần và để lộ ra càng ít càng tốt qua các giao diện bên ngoài. Điều này cho phép sự phân tách rõ ràng giữa những gì có thể thay đổi dễ dàng và những gì khó thay đổi hơn.

Việc triển khai (implementation) được ẩn khỏi các bên bên ngoài có thể thay đổi tự do miễn là các giao diện mạng mà microservice cung cấp không thay đổi theo cách không tương thích ngược (backward-incompatible). Những thay đổi bên trong ranh giới microservice (như Hình 1-1) không nên ảnh hưởng đến người tiêu dùng ở thượng nguồn (upstream consumer), cho phép khả năng phát hành chức năng một cách độc lập. Điều này rất cần thiết để cho phép microservices của chúng ta được phát triển biệt lập và phát hành theo yêu cầu.

Việc có các ranh giới dịch vụ rõ ràng, ổn định và không thay đổi khi việc triển khai nội bộ thay đổi sẽ dẫn đến các hệ thống có sự liên kết lỏng lẻo (**loose coupling**) và độ kết dính mạnh (**strong cohesion**).

> *Giải thích thuật ngữ:*
> *   **Shared Database (Cơ sở dữ liệu dùng chung):** Mô hình cũ nơi nhiều dịch vụ cùng đọc/ghi vào một DB lớn. Microservices hạn chế điều này để tránh phụ thuộc lẫn nhau.
> *   **Information Hiding (Che giấu thông tin):** Nguyên lý thiết kế phần mềm nhằm giảm sự phụ thuộc bằng cách ẩn chi tiết cài đặt.
> *   **Loose Coupling (Liên kết lỏng lẻo):** Các thành phần hệ thống ít phụ thuộc vào nhau. Nếu A thay đổi, B ít bị ảnh hưởng.
> *   **Strong Cohesion (Kết dính mạnh/cao):** Các chức năng liên quan chặt chẽ với nhau được gom vào cùng một chỗ (một service). "Cái gì thay đổi cùng nhau thì nên ở cùng nhau".

Trong khi nói về việc che giấu chi tiết triển khai nội bộ, sẽ là thiếu sót nếu tôi không nhắc đến mẫu Kiến trúc Lục giác (**Hexagonal Architecture**), lần đầu tiên được trình bày chi tiết bởi Alistair Cockburn. Mẫu này mô tả tầm quan trọng của việc giữ cho việc triển khai nội bộ tách biệt khỏi các giao diện bên ngoài của nó, với ý tưởng rằng bạn có thể muốn tương tác với cùng một chức năng thông qua các loại giao diện khác nhau. Tôi vẽ các microservices của mình dưới dạng hình lục giác một phần để phân biệt chúng với các dịch vụ "thông thường", nhưng cũng là để tôn vinh tác phẩm nghệ thuật (ý tưởng) đi trước này.

> *Giải thích thuật ngữ:*
> *   **Hexagonal Architecture (Kiến trúc Lục giác):** Còn gọi là *Ports and Adapters*. Một mẫu thiết kế cho phép ứng dụng cốt lõi (core logic) được điều khiển bởi người dùng, chương trình, kiểm thử tự động hoặc script một cách bình đẳng, và được phát triển/kiểm thử tách biệt khỏi các thiết bị thực thi (như Database, Web Server).

---
### Kiến trúc Hướng Dịch vụ (SOA) và Microservices có khác nhau không?

Kiến trúc hướng dịch vụ (SOA) là một phương pháp thiết kế trong đó nhiều dịch vụ hợp tác với nhau để cung cấp một tập hợp các khả năng cuối cùng. (Ở đây, "dịch vụ" thường có nghĩa là một tiến trình hệ điều hành hoàn toàn riêng biệt.) Giao tiếp giữa các dịch vụ này diễn ra thông qua các lệnh gọi qua mạng thay vì các lệnh gọi phương thức (method calls) bên trong ranh giới của một tiến trình.

SOA nổi lên như một phương pháp để chống lại những thách thức của các ứng dụng nguyên khối (monolithic) lớn. Phương pháp này nhằm mục đích thúc đẩy khả năng tái sử dụng của phần mềm; ví dụ, hai hoặc nhiều ứng dụng người dùng cuối có thể sử dụng cùng một dịch vụ. SOA đặt mục tiêu giúp việc bảo trì hoặc viết lại phần mềm trở nên dễ dàng hơn, vì về mặt lý thuyết, chúng ta có thể thay thế một dịch vụ này bằng một dịch vụ khác mà không ai biết, miễn là ngữ nghĩa (semantics) của dịch vụ không thay đổi quá nhiều.

Về bản chất, SOA là một ý tưởng hợp lý. Tuy nhiên, bất chấp nhiều nỗ lực, vẫn thiếu một sự đồng thuận tốt về cách thực hiện SOA cho đúng. Theo tôi, phần lớn ngành công nghiệp đã không nhìn nhận vấn đề một cách đủ toàn diện và không đưa ra được một giải pháp thay thế hấp dẫn cho câu chuyện do các nhà cung cấp (vendors) khác nhau trong lĩnh vực này đặt ra.

Nhiều vấn đề bị đổ lỗi cho SOA thực chất là vấn đề của những thứ như giao thức truyền thông (ví dụ: SOAP), phần mềm trung gian của nhà cung cấp (vendor middleware), sự thiếu hướng dẫn về độ chi tiết của dịch vụ (service granularity), hoặc hướng dẫn sai về việc chọn vị trí để chia tách hệ thống của bạn. Một người hoài nghi có thể cho rằng các nhà cung cấp đã đồng lựa chọn (và trong một số trường hợp, đã thúc đẩy) phong trào SOA như một cách để bán nhiều sản phẩm hơn, và chính những sản phẩm đó cuối cùng đã làm suy yếu mục tiêu của SOA.

Tôi đã thấy rất nhiều ví dụ về SOA trong đó các đội ngũ cố gắng làm cho các dịch vụ nhỏ hơn, nhưng họ vẫn kết nối mọi thứ vào một cơ sở dữ liệu chung và phải triển khai mọi thứ cùng nhau. Có hướng dịch vụ không? Có. Nhưng đó không phải là microservices.

Phương pháp microservice đã nổi lên từ việc sử dụng trong thực tế, vận dụng sự hiểu biết tốt hơn của chúng ta về hệ thống và kiến trúc để thực hiện SOA một cách đúng đắn. Bạn nên nghĩ về microservices như là một phương pháp cụ thể cho SOA, giống như Lập trình Cực đoan (Extreme Programming - XP) hay Scrum là một phương pháp cụ thể cho phát triển phần mềm Agile.

---
