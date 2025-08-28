+++
title = "Cài đặt các công cụ cần thiết"
date = 2025
weight = 3
chapter = false
+++

Bước tiếp theo là chúng ta sẽ cài đặt một số các công cụ cần thiết trong quá trình phát triển ứng dụng.

### Cài đặt AWS CLI

Chúng ta sẽ cài đặt AWS CLI từ trang này: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

```bash
sudo apt update -y
```

Cài với lệnh sau:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

![1.3.1](/images/1-preparation/1-3-install-tools/1.3.1-install-tools.png)

Cài xong thì chúng ta sẽ kiểm tra thử:

![1.3.2](/images/1-preparation/1-3-install-tools/1.3.2-install-tools.png)

Tiếp theo thì chúng ta sẽ cần phải thiết lập profile để dùng CLI. Việc này sẽ không cần thiết với một số dịch vụ ở trên AWS, nhưng sẽ cần thiết khi mà chúng ta đang phát triển ứng dụng với AWS ở trong local. Giờ thì chúng ta sẽ cấu hình profile mới với tên là `cognito-workshop`.

```bash
# Flag --profile sẽ cho chúng ta biết là chúng ta đang cấu hình
# profile nào, giống như việc đặt tên và chỉ điểm để cấu hình.
aws configure --profile cognito-workshop
```

Ở bước này thì chúng ta sẽ tạm để 2 cấu hình:

- Access key id và Secrect access key sẽ cấu hình sau.
- Region: `ap-southeast-1`.
- Output format: `json`.

![1.3.3](/images/1-preparation/1-3-install-tools/1.3.3-install-tools.png)

Như vậy là chúng ta đã hoàn tất việc cài đặt AWS CLI vào trong máy.

### Cài đặt NodeJS

Tiếp theo thì chúng ta sẽ tiết hành cài đặt NodeJS, khi cài đặt NodeJS xong thì NPM (Node Package Manager) cũng sẽ được cài đặt theo. Ngoài ra thì các bạn cũng có thể dùng PNPM để có thể tái sử dụng các thư viện đã tải mà không cần phải tải lại mới toàn bộ như NPM.

Đầu tiên là tải bộ cài NVM (Node Version Manager) trước

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

![1.3.4](/images/1-preparation/1-3-install-tools/1.3.4-install-tools.png)

Reset shell để apply biến môi trường mới được setup

```bash
\. "$HOME/.nvm/nvm.sh"
```

Tiến hành cài đặt Node 22.18 (phiên bản này sẽ được cài đặt vì nó được đánh dấu là LTS)

```bash
nvm install 22
```

![1.3.5](/images/1-preparation/1-3-install-tools/1.3.5-install-tools.png)

Kiểm tra Node và Npm

```bash
node -v
npm -v
```

![1.3.6](/images/1-preparation/1-3-install-tools/1.3.6-install-tools.png)

Như vậy là chúng ta đã cài đặt xong các công cụ cần thiết để phát triển ứng dụng với NodeJS.

### Cài đặt Python

Giờ thì sẽ là bước cài đặt Python. Trong bài này thì chúng ta sẽ cài đặt Python 3.12.

```bash
sudo apt update -y
sudo apt install python3.12
```

![1.3.7](/images/1-preparation/1-3-install-tools/1.3.7-install-tools.png)

Tiếp theo là pip và venv (dùng để tạo môi trường ảo cho python).

```bash
sudo apt install python3-pip python3-venv
```

![1.3.8](/images/1-preparation/1-3-install-tools/1.3.8-install-tools.png)

Ok, như vậy thì mình đã cài đặt xong các công cụ cần thiết để phát triển ứng dụng python rồi. Trong phần tiếp theo thì chúng ta sẽ cùng đi thiết lập repository để chứa mã nguồn của chúng ta.

{{% notice note %}}
Vì mình đã cài ở trong máy của mình rồi cho nên là nó sẽ hiện ra kết quả khác khi so với bạn làm, nhưng chúng ta vẫn sẽ kiểm tra xem các công cụ này đã cài đặt hay chưa bằng nhiều lệnh khác nhau.
{{% /notice %}}