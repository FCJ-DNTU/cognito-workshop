+++
title = "Thiết lập git repository"
date = 2025
weight = 4
chapter = false
+++

{{% notice note %}}
Bạn có thể bỏ qua bước này nếu như không muốn upload code lên trên Github.
{{% /notice %}}

### Create a repository on Github

Trong bước này thì chúng ta sẽ tạo một repository trên github để có thể lưu lại code. Như đã nói ở trên thì bước này là tuỳ chọn, nó sẽ không ảnh hưởng quả nhiều tới mục tiêu và kết quả. Nhưng để cho đầy đủ thì mình vẫn sẽ thiết lập một repository để đẩy code lên đó.

Vào trong giao diện chính của Github.

![1.4.1](/images/1-preparation/1-4-setup-git-repository/1.4.1-setup-git-repository.png)

Ấn tạo một repository mới, đặt tên là `cognito-example-ts`. Mô tả để gì cũng được hoặc là: `Đây là mã nguồn ứng dụng của bài workshop Authentication & Authorization with Cognito` và ấn tạo repository mới.

![1.4.2](/images/1-preparation/1-4-setup-git-repository/1.4.2-setup-git-repository.png)

![1.4.3](/images/1-preparation/1-4-setup-git-repository/1.4.3-setup-git-repository.png)

Tương tự với python, mình chỉ cần đổi tên repo là `cognito-example-py`.

![1.4.4](/images/1-preparation/1-4-setup-git-repository/1.4.4-setup-git-repository.png)

![1.4.5](/images/1-preparation/1-4-setup-git-repository/1.4.5-setup-git-repository.png)

{{% notice note %}}
Bạn có thể đặt bất kì cái tên nào, miễn sao là dễ nhớ. Mình đặt như thế để cho mình dễ phân biệt.
{{% /notice %}}

### Setup local Git

Tiếp theo, giờ chúng ta sẽ setup local git và có origin là repository mà chúng ta đã tạo ở trước đó.

#### Typescript

Vào trong thư mục chứa mã nguồn typescript trước và tạo.

```bash
cd ~/workspace/cognito-example-ts
git init
```

![1.4.6](/images/1-preparation/1-4-setup-git-repository/1.4.6-setup-git-repository.png)

Tiếp theo là chúng ta sẽ thêm Remote cho git repository này ở dưới local.

```bash
# Nếu bạn dùng SSH Key thì xài đường dẫn này
git remote add origin git@github.com:NguyenAnhTuan1912/cognito-example-ts.git

# Nếu dùng authentication bình thường thì dùng HTTPs
git remote add origin https://github.com/NguyenAnhTuan1912/cognito-example-ts.git

# Kiểm tra kết quả
git remote -v
```

![1.4.7](/images/1-preparation/1-4-setup-git-repository/1.4.7-setup-git-repository.png)

#### Python

Vào trong thư mục chứa mã nguồn python trước và tạo.

```bash
cd ~/workspace/cognito-example-py
git init
```

![1.4.8](/images/1-preparation/1-4-setup-git-repository/1.4.8-setup-git-repository.png)

Tiếp theo là chúng ta sẽ thêm Remote cho git repository này ở dưới local.

```bash
# Nếu bạn dùng SSH Key thì xài đường dẫn này
git remote add origin git@github.com:NguyenAnhTuan1912/cognito-example-py.git

# Nếu dùng authentication bình thường thì dùng HTTPs
git remote add origin https://github.com/NguyenAnhTuan1912/cognito-example-py.git

# Kiểm tra kết quả
git remote -v
```

![1.4.9](/images/1-preparation/1-4-setup-git-repository/1.4.9-setup-git-repository.png)

Xong ! Ở phần tiếp theo thì chúng ta sẽ tiến hành tạo một User Pool cho bài workshop.