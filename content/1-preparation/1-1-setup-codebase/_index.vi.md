+++
title = "Thiết lập mã nguồn"
date = 2025
weight = 1
chapter = false
+++

{{% notice note %}}
Trong bài này thì mình vẫn khuyên là bạn nên sử dụng WSL (Windows Subsystem for Linux) hoặc là dùng Linux Distro thật để thực hành. Chúng ta sẽ sử dụng bản Ubuntu 24.04.2 LTS để thực hiện phần setup này. Trong trường hợp mà bạn chưa cài WSL thì có thể xem bài hướng dẫn của Microsoft tại đường dẫn này: https://learn.microsoft.com/en-us/windows/wsl/install (Recommend WSL 2).
{{% /notice %}}

Ở trong phần này thì mình sẽ chia ra làm 2 mã nguồn cho 2 ngôn ngữ khác nhau, nên nếu như bạn có kinh nghiệm với ngôn ngữ nào thì hãy chọn ngôn ngữ đó, còn nếu muốn tìm hiểu thêm thì có thể chọn cả 2 để thực hành.

### Bài toán giả định

Giả sử mình có một bài toán như sau: khách hàng đang cần một ứng dụng để quản lý thông tin các khách hàng tiềm năng cho các sản phẩm, phục vụ cho đội Sales và Marketing trước tiên. Nhiệm vụ của mình bây giờ là sẽ tiến hành xây dựng ứng dụng như thế này, nhưng trước khi bắt tay vào xây dựng thì mình sẽ cần phải đi vào thiết kế trước tiên.

Mình sẽ suy ra được nghiệp vụ của từng team là như sau:

- Với **Marketing**: thì họ chỉ được xem thông tin của một hoặc nhiều khách hàng tiềm năng.
- Với **Sales**: thì họ có thể thêm / xoá / sửa thông tin của khách hàng tiềm năng, cùng với đó là xem được thông tin của các khách hàng đó.

Tuy nhiên, để phân biệt các user từ 2 teams này, thì chúng ta cần phải có thêm phần Authentication, và trao quyền đúng cho mỗi team với phần Authorization. Như vậy, ứng dụng của chúng ta sẽ tạm thời có 2 chức năng chính là: **Quản lý khách hàng tiềm năn**g và **Auth**.

Từ đây thì chúng ta sẽ có được sơ đồ Use Case đơn giản như sau:

**INSERT IMAGE**

Chúng ta đã xác định xong tác nhân trong bài toán cũng như là tính năng cần phải có, giờ thì tới việc xác định phần mô hình dữ liệu. Trước tiên chúng ta thấy rằng ứng dụng này sẽ phục vụ cho việc quản lý các khách hàng tiềm năng, vậy thì chúng ta cần phải biết được thông tin của khách hàng bao gồm những thuộc tính nào, từ đây chúng ta sẽ xây dựng được mô hình dữ liêu cho khách hàng tiềm năng, giả sử có các thuộc tính như sau:

- **id**: mã định danh cho khách hàng tiềm năng.
- **fullName**: tên đầy đủ của khách hàng.
- **phone**: số điện thoại của khách hàng.
- **age**: tuổi của khách hàng.
- **productCode**: mã sản phẩm mà khách hàng quan tâm.
- **type**: loại khách hàng, vì là khách hàng tiềm năng nên mình sẽ để giá trị mặc định là **potential_customer**. Mình sẽ giải thích cái này ở trong phần 3 - Thiết lập DynamoDB.
- **createAt**: thời điểm mà thông tin của khách hàng này được thêm vào.

Trên thực tế thì các thông tin này có thể sẽ nhiều hơn, dùng để phục vụ cho nhiều công việc khác nhau, nhưng trong bài này thì mình sẽ thiết kế đơn giản lại để tập trung nhiều hơn vào trong phần mục tiêu chính của bài.

**INSERT IMAGE**

Như vậy thì chúng ta đã phát biểu bài toán xong, tới đây thì chắc hẳn là bạn đã hiểu được là chúng ta sẽ cần phải làm gì rồi đúng không.

### Ý tưởng của cấu trúc mã nguồn

Trước khi code thì chúng ta hãy cùng phân tích về mã nguồn dự án đã, có bao giờ bạn đã băn khoanh khi bắt đầu một dự án là "Mình nên sắp xếp các thư mục trong mã nguồn như thế nào nhỉ?" Hay là "Điểm bắt đầu của mã nguồn của mình là ở đâu? Có những module nào?". Việc mà mình và bạn phân tách mã nguồn tốt cũng đồng nghĩa với việc chúng ta đang ứng dụng tốt "Best Coding Practice" mà trong đó phố biến nhất là Code Splitting và Maintainability (Tính khả năng bảo trì tốt).

Theo mình thì trên thực tế thì tuỳ thuộc vào độ lớn của "bài toán", thời gian sống của dự án, sản phẩm, trình độ nhân lực mà quyết định tới nhiều thứ, trong đó có cả cấu trúc mã nguồn, thứ mà mình đang nói. Nhưng vì bài này dành ra để học thuật, nên mình sẽ cố gắng làm mọi thứ phức tạp hơn, chính vì thế mình sẽ cho rằng "bài toán" mà chúng ta đang gặp phải có thể sẽ mở rộng thêm ở trong tương lai, cùng với đó là thời gian dài mà dự án này trong trạng thái hoạt đông.

Theo quan điểm của mình thì một dự án có thể mở rộng thêm các tính năng, cũng như là có thể triển khai ở nhiều nơi khác nhau, thì mình sẽ cần phải dùng tới tư duy trừu tượng hoá. Tại sao lại như thế? Ví dụ đơn giản, ở ngoài đường chúng ta thấy rất nhiều phương tiện di chuyển bằng 4 bánh, chở người và chúng ta đều gọi chung là Xe hơi, oto => chúng ta vừa mới trừu tượng khoá lớp phương tiện mà mỗi chiếc có thể đến từ nhiều hãng khác nhau, từ nhiều dòng khác nhau, từ nhiều phân phúc khác nhau, chúng ta bỏ qua rất nhiều các chi tiết khác biệt giữa chúng để đi đến được cái nhìn nhận chung nhất là: Xe hơi, oto bằng đặc điểm dễ nhận thấy nhất là các phương tiện này dùng 4 bành, trở người và có kích thước vừa đủ.

**INSERT IMAGE**

Như vậy, quay lại với câu chuyện xây dựng ứng dụng của chúng ta, mình sẽ không quan tâm việc thành phần này nó thực hiện như thế nào, mình sẽ quan tâm tới 3 thứ: Đầu vào, đầu ra và nhiệm vụ chính của nó. Ví dụ mình có một hàm dùng để đăng nhập, mình sẽ không quan tâm tới việc nó thực hiện công việc đó như thế nào, nhưng nó buộc phải đảm báo đúng dữ liệu đầu vào và đầu ra và làm đúng việc đăng nhập.

**INSERT IMAGE**

Và chúng ta cũng sẽ làm việc tương tự như thế với ứng dụng mà chúng ta sắp sửa build, thứ mà còn lớn hơn cả hàm đăng nhập mà mình đã ví dụ ở trên. Tới đây thì chúng ta sẽ chỉ cần quan tâm là ứng dụng này sẽ được chạy ở đâu đó, mà người dùng có thể truy cập được, xem nội dung, thực hiện các thao tác thức năng, xong. Với góc nhìn như này, thì về sau bài toán mở rộng thêm nhiều vấn đề, lúc này chúng ta vẫn có thể dễ dàng mở rộng và triển khai thêm các giải pháp mong muốn mà vẫn có thể tham khảo lại ý tưởng ban đầu một cách dễ dàng.

**INSERT IMAGE**

Để làm được như thế, thì mình cần phải thiết kế được mã nguồn có thể đảm bảo được các yếu tố trên. Giờ là lúc mà mình sẽ cần phải đi vào cụ thể hơn nhưng vẫn đủ trừu tượng để giữ mức trung lập nhất khi thiết kế mã nguồn:

- Ứng dụng có thể được triển khai ở đâu? Môi trường nào?
- Mình nên dùng công nghệ gì?
- Có phụ thuộc vào thành phần bên ngoài nào không?

Như thế, mã nguồn mình sẽ chia làm 3 phần trước tiên.

- **Runtimes**: phần mã được build để chạy trên một runtime đặc biệt. Mình sẽ chia làm 2 loại, một là Serverful và hai là Serverless.
- **Core**: phần mã nguồn chính, mã nguồn cốt lõi, chứa business logic của ứng dụng.
- **Utils**: để có thể có một số hàm hỗ trợ toàn cục thì mình sẽ thêm phần này vào trong mã nguồn.

**INSERT IMAGE**

Từ đây thì chúng ta có thể thấy được là: Entry point của ứng dụng nó sẽ nằm trong **Runtimes**. Việc mình nắm được Entry Point và Startup flow là việc rất quan trọng. Với kiến trúc này thì startup flow sẽ như sau:

**INSERT IMAGE**

Tới đây bạn đã nắm được ý tưởng rồi chứ? Mình cùng tóm ý lại nhé. Mục tiêu của chúng ta là xây dựng một cấu trúc mã nguồn mà có thể mở rộng ở trong tương lai, có thể dễ dàng thực hiện Unit Testing. Chính vì lý do này mà mình phải làm rõ các thành phần mã ở trong tổng thể mã nguồn: phần nào là phần mã chính, là phần chịu trách nhiệm xử lý kết quả và phản hồi => đây là phần mà chúng ta đặc biệt quan tâm; phần nào sẽ dùng được chạy để expose API cho ứng dụng bên ngoài xài, được chạy ở môi trường nào?

Chúng ta bắt đầu setup ứng dụng nào!

### Khởi tạo mã nguồn

Bạn có thể chọn 1 trong 2 hoặc là cả 2 để thực hiện bài thực hành này. Đầu tiên là vào trong máy ảo, mình sẽ dùng WSL. Đầu tiên thì chúng ta sẽ tạo một thư mục tới tên là `workspace` để làm nơi chứa mã nguồn của chúng ta.

```bash
mkdir workspace
cd workspace
```

![1.1.1](/images/1-preparation/1-1-setup-codebase/1.1.1-setup-codebase.png)

{{% notice note %}}
Nội dung của phần này có thể sẽ lặp lại ở cả 2 ngôn ngữ, vì mính muốn tái sử dụng lại nội dung, đồng thời là có thể nhiều bạn sẽ bỏ qua phần mà mình viết kĩ để setup luôn phần mà mình setup chưa kĩ.
{{% /notice %}}

Ấn [vào đây](#python) để tới thẳng phần setup với python.

#### Typescript

Trong mã nguồn với typescript, thì mình sẽ sử dụng Express, là thư viện xây dựng Web API rất nối tiếng, được nhiều anh em Developer lựa chọn khi bắt đầu một dự án với NodeJS. Chúng ta sẽ cài đặt những thứ này sau. Trước tiên thì trong workspace, mình sẽ cần phải tạo thêm một thư mục có thên là `cognito-example-ts`.

```bash
mkdir cognito-example-ts
cd cognito-example-ts
```

![1.1.2](/images/1-preparation/1-1-setup-codebase/1.1.2-setup-codebase.png)

Tiếp theo, chúng ta sẽ tạo 2 thư mục là `src` -> chứa mã nguồn chính của ứng dụng và `test` -> chứa các scripts test tính năng, chức năng của ứng dụng hoặc một số hàm hỗ trợ.

```bash
mkdir src test
```

![1.1.3](/images/1-preparation/1-1-setup-codebase/1.1.3-setup-codebase.png)

Thư mục `test` thì chúng ta sẽ để đó, vào trong thư mục `src` trước. Trong thư mục này thì chúng ta setup tiếp 3 thư mục là `core`, `runtimes` và `utils`.

```bash
cd src
mkdir core runtimes utils
```

![1.1.4](/images/1-preparation/1-1-setup-codebase/1.1.4-setup-codebase.png)

Trong `core` thì chúng ta sẽ setup một số thư mục.

```bash
cd ~/workspace/cognito-example-ts/src/core
mkdir context docs error modules validation
```

![1.1.5](/images/1-preparation/1-1-setup-codebase/1.1.5-setup-codebase.png)

Trong `runtimes` thì chúng ta sẽ setup một số thư mục.

```bash
cd ~/workspace/cognito-example-ts/src/runtimes
mkdir express lambda-functions
```

![1.1.6](/images/1-preparation/1-1-setup-codebase/1.1.6-setup-codebase.png)

Trong `utils` thì chúng ta sẽ setup một số thư mục.

```bash
cd ~/workspace/cognito-example-ts/src/utils
mkdir aws-clients configs constants crypto helpers
```

![1.1.7](/images/1-preparation/1-1-setup-codebase/1.1.7-setup-codebase.png)

{{% notice note %}}
Mình sẽ chú thích các thư mục và ý nghĩa của chúng ở bên dưới.
{{% /notice %}}

---

#### Python

Trong mã nguồn với python, thì mình sẽ sử dụng FastAPI, là thư viện xây dựng Web API cũng rất phố biến với python. Trong workspace, mình sẽ cần phải tạo thêm một thư mục có thên là `cognito-example-py`.

```bash
mkdir cognito-example-py
cd cognito-example-py
```

![1.1.8](/images/1-preparation/1-1-setup-codebase/1.1.8-setup-codebase.png)

Tiếp theo, chúng ta sẽ tạo 2 thư mục là `src` -> chứa mã nguồn chính của ứng dụng và `test` -> chứa các scripts test tính năng, chức năng của ứng dụng hoặc một số hàm hỗ trợ.

```bash
mkdir src test
```

![1.1.9](/images/1-preparation/1-1-setup-codebase/1.1.9-setup-codebase.png)

Thư mục `test` thì chúng ta sẽ để đó, vào trong thư mục `src` trước. Trong thư mục này thì chúng ta setup tiếp 3 thư mục là `core`, `runtimes` và `utils`.

```bash
cd src
mkdir core runtimes utils
```

![1.1.10](/images/1-preparation/1-1-setup-codebase/1.1.10-setup-codebase.png)

Trong `core` thì chúng ta sẽ setup một số thư mục.

```bash
cd ~/workspace/cognito-example-py/src/core
mkdir context docs error modules validation
```

![1.1.11](/images/1-preparation/1-1-setup-codebase/1.1.11-setup-codebase.png)

Trong `runtimes` thì chúng ta sẽ setup một số thư mục.

```bash
cd ~/workspace/cognito-example-py/src/runtimes
mkdir express lambda_functions
```

![1.1.12](/images/1-preparation/1-1-setup-codebase/1.1.12-setup-codebase.png)

Trong `utils` thì chúng ta sẽ setup một số thư mục.

```bash
cd ~/workspace/cognito-example-py/src/utils
mkdir aws_clients configs constants crypto helpers
```

![1.1.13](/images/1-preparation/1-1-setup-codebase/1.1.13-setup-codebase.png)

{{% notice note %}}
Mình sẽ chú thích các thư mục và ý nghĩa của chúng ở bên dưới.
{{% /notice %}}

Tổng thể thì cả Typescript và Python đều sẽ có cấu trúc thư mục trông như thế này:

![1.1.14](/images/1-preparation/1-1-setup-codebase/1.1.14-setup-codebase.png)

![1.1.15](/images/1-preparation/1-1-setup-codebase/1.1.15-setup-codebase.png)

Chú thích:

- Trong `core` thì chúng ta sẽ có:
  - `context`: là nơi định nghĩa ra ngữ cảnh thực thi. Có 2 ngữ cảnh chính: Runtime Context là context của **runtime** mà chúng ta sẽ sử dụng để chạy ứng dụng; Internal Context là context nội bộ trong **core**, nó tách biệt với context của Runtime. Thực chất thì context là các object mà mình tạo ra để nhằm chuẩn hoá dữ liệu đầu vào giữa các phần tử trong **core** và giữa **runtime** với **core**.
  - `docs`: là nơi viết tài liệu cho dự án, những tài liệu này mang tính application nhiều hơn, nghĩa là nó không phải là tài liệu văn bản thuần. Tài liệu mà chúng ta sẽ thiết lập sẽ là Swagger UI.
  - `error`: là nơi mà chúng ta sẽ định nghĩa lỗi chuẩn hoá cho mã nguồn. Lỗi có thể bắt nguồn từ rất nhiều nơi như là system, runtime, từ các thư viện bên ngoài, từ API của bên thứ 3 hoặc cũng là từ các ngoại lệ xảy ra trong một hàm. Chính vì thể mà mình sẽ cần gom chung lại thành các object trong mã nguồn và chuẩn hoá nó thành các nhóm, mã lỗi.
  - `modules`: là nơi sẽ chứa phần mã nguồn xử lý công việc chính của chúng ta, nơi chứa business logic. Hiện tại có 2 chức năng chính nên là chúng ta sẽ có 2 modules tương ứng.
  - `validation`: là nơi mà chúng ta sẽ setup thư viện xác minh dữ liệu. Đây là phần thuộc về Validation trong security, nên mình sẽ setup riêng cho nó. Cả typescript và python đều có rất nhiều thư viện để làm tác vụ này, nên mình sẽ làm một folder riêng.
- Trong `runtimes` thì chúng ta có:
  - `express` hoặc `fastapi`: là nơi mà chúng ta sẽ setup chạy API với các thư viện, lúc này thì chúng ta sẽ phải làm quen thêm với các khái niệm như là Middleware, Body Parser, HTTP Response and Request, Routing, ...
  - `lambda_function`: là nơi mà chúng ta sẽ thực hiện việc viết mã cho Lambda Function.
- Trong `utils` thì chúng ta có:
  - `aws_clients`: là nơi mà chúng ta cấu hình mọi thứ liên quan tới AWS để có thể giao tiếp với các dịch vụ AWS.
  - `configs`: là nơi chứa các cấu hình được khởi tạo trong quá trình build-time.
  - `constants`: là nơi chứa các giá trị hằng được khởi tạo trong quá trình viết mã. Không thay đổi trong built-time hoặc runtime.
  - `crypto`: là một module hỗ trợ cho việc mã hoá và giải mã (encoding - decoding; encrypting - decrypting).
  - `helpers`: là các module hỗ trợ riêng lẻ, nhỏ hơn.

Vậy là chúng ta đã tìm hiểu được ý nghĩa của mã nguồn và setup xong cấu trúc mã nguồn. Tiếp theo thì chúng ta sẽ tìm hiểu về AWS SDK và ý tưởng để tích hợp AWS SDK vào trong mã nguồn của chúng ta để thực hiện các chức năng đã đề ra.