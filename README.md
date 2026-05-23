# NGuyenTuanAnh_BT4_PTUDVMNM
# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
## Lớp: 58KTPM
## Bài tập 04:
# KHAI THÁC N8N ĐỂ TỰ ĐỘNG ĐĂNG BÀI LÊN WORDPRESS 
## SỬ DỤNG KẾT QUẢ ĐÃ LÀM Ở BÀI TẬP 3, BỔ SUNG VÀO DOCKER COMPOSE ĐỂ CÓ THÊM SERVICE 8N8:

1. SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ TẠO 1 file docker-compose.yml chứa:
   - Mariadb: sử dụng image: mariadb:latest để làm hệ quản trị csdl cho wordpress, thêm các biến môi trường: TZ: "Asia/Ho_Chi_Minh", MARIADB_ROOT_PASSWORD, MARIADB_DATABASE, MARIADB_USER, MARIADB_PASSWORD (giá trị tuỳ ý)
   - Phpmyadmin: sử dụng image: phpmyadmin:latest để đăng nhập vào mariadb rồi tạo csdl trống (chỉ để xem, ko cần tạo bảng từ đây, wordpress sẽ làm hết), khai báo biến môi trường: PMA_HOST: <tên service mariadb>, PMA_ARBITRARY: 1
   - WordPress: sử dụng image: wordpress:latest, truyền các tham số môi trường cho wordpress là các thông tin truy cập csdl mariadb, tạo bởi Phpmyadmin, khai báo biến môi trường: WORDPRESS_DB_HOST: <tên service mariadb>, WORDPRESS_DB_NAME, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD (giá trị theo mariadb đã khai báo)
   - Cloudflared: sử dụng image: cloudflare/cloudflared:latest , full command và token lấy từ dashboard của cloudflare, dùng AI chuyển sang dạng docker compose
N8n : sử dụng image: n8nio/n8n:latest, nhớ truyền biến môi trường WEBHOOK_URL theo sub-domain đã add router cho cloudflared tunnel

<img width="961" height="541" alt="image" src="https://github.com/user-attachments/assets/f65c5200-1e9b-44c1-8bc5-842b1715c600" />
<img width="966" height="544" alt="image" src="https://github.com/user-attachments/assets/c21d6f8a-5c38-4374-90c1-dbec5a9ccdd0" />

Truy cập sub-domain2 để quan sát xem cơ sở dữ liệu chưa có bảng nào:
- Container mariadb và phpmyadmin đã khởi chạy thành công, kết nối được với nhau thông qua mạng nội bộ của Docker (Docker Network).
  
-Việc phân quyền user và khởi tạo database ban đầu hoàn toàn chính xác.

<img width="1635" height="866" alt="image" src="https://github.com/user-attachments/assets/822206e4-d857-4607-829a-6fca3d9826f0" />

- em đã đăng nhập thành công vào bên trong hệ thống với tư cách là Quản trị viên: -> Truy cập sub-domain1 để cài đặt wordpress (làm theo hướng dẫn của wordpress)

- mã nguồn WordPress đã kết nối thành công tới database MariaDB ở các bước trước để tự khởi tạo cấu trúc hệ thống của nó -> Hệ thống WordPress đã tự động chạy các đoạn script tạo bảng dữ liệu ngầm.
<img width="966" height="462" alt="image" src="https://github.com/user-attachments/assets/8bf488fa-a2fc-4780-b649-f46a7555face" />

- Truy cập sub-domain2 để quan sát xem cơ sở dữ liệu có những bảng dữ liệu nào sau khi cài wp.

- Luồng kết nối cơ sở dữ liệu giữa WordPress container và MariaDB container hoạt động hoàn hảo 100%. WordPress đã chiếm quyền và tự khởi tạo hạ tầng lưu trữ thành công
<img width="1638" height="797" alt="image" src="https://github.com/user-attachments/assets/06847160-cddf-43ae-9bd5-b9ec80c47b69" />

- WordPress hiển thị trên tên miền wp.teumagic.io.vn

- Container cloudflared (Cloudflare Tunnel) trong file docker-compose đã kết nối thành công về Dashboard Cloudflare
<img width="1616" height="880" alt="image" src="https://github.com/user-attachments/assets/e5f56f89-110e-49ac-97bc-9e693b513d63" />

- Cấu hình cloudflare tunnel add router để public Phpmyadmin lên sub-domain2 (dùng để truy cập phpmyadmin)

- Định tuyến Tunnel chính xác: Container cloudflared đã nhận diện và thiết lập một đường hầm bảo mật từ bên trong máy ảo của bạn lên hệ thống Cloudflare Cloud

- Giao diện đăng nhập hiển thị đầy đủ thông tin máy chủ MariaDB và máy chủ Web Apache
<img width="1603" height="871" alt="image" src="https://github.com/user-attachments/assets/54f764b2-2e0d-4861-991d-163b0e728ef8" />

- Cấu hình cloudflare tunnel add router để public Phpmyadmin lên sub-domain2 (dùng để truy cập phpmyadmin)

- Truy cập sub-domain2 để quan sát xem cơ sở dữ liệu có những bảng dữ liệu nào sau khi cài wp
<img width="1694" height="751" alt="image" src="https://github.com/user-attachments/assets/3da048b2-f90b-4e02-91cd-d3f3504f37ed" />

"Truy cập sub-domain3 để cấu hình n8n: tạo tài khoản admin..."
"Send me a Licence key... check email để lấy KEY"
"Activate License key: vào trang chủ => SETTING (góc dưới trái) => Usage and plan => Enter activation key: paste key từ email vào đây"
<img width="1681" height="877" alt="image" src="https://github.com/user-attachments/assets/d291c55e-d07e-455c-8a52-c85a095b99cc" />

- Sau khi kích hoạt License thành công ở bước trước, em đã bấm vào nút "Create workflow" để mở ra một giao diện lập trình trực quan trống. Tại đây, em đã thực hiện thao tác tìm kiếm và thêm vào node đầu tiên: Telegram Trigger.

- "Create workflow (home page => overview => Create workflow)"
  
- "Add trigger node: tìm node: Telegram => OnMessage"

- Cấu hình Credential cho Telegram: Đảm bảo bạn đã click đúp vào node này, nhập Access Token lấy từ @BotFather sau khi tạo bot mới trên ứng dụng Telegram
<img width="1690" height="858" alt="image" src="https://github.com/user-attachments/assets/b127ebfc-f04d-447e-9d13-270c2c98020b" />

- "Cần chát với bot @BotFather để đẻ ra bot mới của riêng mình. bot này sẽ là nơi nhận lệnh (promt) để AI sinh html => n8n sẽ dùng html này để đăng bài lên wp"

- "Sau khi tạo bot mới cần copy lấy Token..."

- Gửi lệnh /newbot để yêu cầu tạo bot mới.

- Đặt tên hiển thị cho bot theo yêu cầu của hệ thống: teumagic_bot.

- Thiết lập tên người dùng duy nhất (Username) kết thúc bằng chữ "bot": teumagic_ai_bot.

<img width="1694" height="819" alt="image" src="https://github.com/user-attachments/assets/059cbfbf-7dfa-4d84-87f9-39d631cb482c" />

- em đã click đúp vào Node AI để thiết lập các thông số hoạt động cho mô hình ngôn ngữ lớn. Nền tảng được chọn ở đây là Google Gemini, và em đang mở tab Parameters (Tham số cấu hình) để thiết lập cách thức mà AI sẽ tiếp nhận câu lệnh (Prompt) từ người dùng.

- "Add (nối tiếp vào sau node Telegram Trigger) node: AI Google Gemini => Message a model"

- "kéo thả nội dung đã chát với bot của telegram (phía bên trái) vào nội dung phần PROMPT kết quả được {{ $json.message.text }}"

- "cần gõ thêm vào sau {{ $json.message.text }} để promt dài hơn : vd ({{ $json.message.text }}. Kết quả sinh ra ở định dạng HTML+CSS để tôi dùng HTML+CSS này tạo bài viết cho wordpress.)"
<img width="1547" height="866" alt="image" src="https://github.com/user-attachments/assets/2ceb90f1-1ab7-43b7-b2a4-751f0d32a3c1" />

- Em đang mở cửa sổ thiết lập thông tin xác thực tên Google Gemini(PaLM) Api account. Tại đây, em đã dán mã khóa bí mật vào trường bắt buộc là API Key để cấp quyền gọi mô hình AI.

- "Add (nối tiếp vào sau node Telegram Trigger) node: AI Google Gemini => Message a model => Set up Credential => cần Nhập API KEY".

<img width="1696" height="870" alt="image" src="https://github.com/user-attachments/assets/1630c0ec-5782-4c9d-8ccd-9d1869cbd53c" />

- Em đang mở giao diện cấu hình và kiểm thử chi tiết của node Telegram Trigger.

- "Sau khi tạo bot mới cần copy lấy Token, và chát lần đầu với bot mới này, nội dung bất kỳ (bước này quan trọng!)"
  
-"bấm nút Execute previous nodes để thấy trường giá trị của node trước trả về..."

- cấu trúc gói tin JSON thực tế từ Telegram: Hệ thống hiển thị rõ ràng một cây dữ liệu có cấu trúc phục vụ cho các bước sau:

- message_id: ID của tin nhắn là 3.

- from: Thông tin người gửi tin nhắn là tài khoản cá nhân của em (first_name: Nguyen, last_name: Anh, mã ngôn ngữ vi - Tiếng Việt, ID người dùng 6544384539).

- chat: Trạng thái cuộc hội thoại là chat riêng tư (type: private).

- text: giới thiệu bản thân
<img width="1261" height="884" alt="image" src="https://github.com/user-attachments/assets/562ebf37-f555-40ce-a637-cbeec62cd5fe" />

- Kích hoạt chạy thử toàn bộ luồng tự động hóa (Execute Workflow) và nhận về gói kết quả xử lý hoàn chỉnh từ Trí tuệ nhân tạo (Google Gemini API).

- "kéo thả nội dung đã chát với bot của telegram vào nội dung phần PROMPT... gõ thêm vào sau để prompt dài hơn... Trả về kết quả dưới dạng JSON"
  
- "bấm nút Execute previous nodes để thấy trường giá trị của node trước trả về..."

- Vòng tròn tích xanh ✓ cạnh chữ OUTPUT (1) và thời gian xử lý hiển thị ở góc dưới node chứng minh kết nối API Key bạn thiết lập hoàn toàn hợp lệ, không bị dính lỗi vượt quá hạn mức (Quota) hay lỗi cú pháp lệnh.
<img width="1690" height="795" alt="image" src="https://github.com/user-attachments/assets/ea8ee8bb-2b48-426e-bb2d-4c018bda9145" />

- Cấu hình thành công Node Code trong JavaScript để bóc tách và định hình lại cấu trúc dữ liệu.

- Em đang mở giao diện cấu hình chi tiết của Node có tên là Code in JavaScript (bản chất là node Code của n8n chuyển sang chế độ biên dịch ngôn ngữ JavaScript).

- Add (nối tiếp vào sau node Message a model) node: Code in JavaScript. Code js ở dạng này, có thể phải thay đổi tuỳ theo json AI trả về.

- Trích xuất chuỗi text từ vị trí content.parts[0].text.

- Dùng hàm JSON.parse() để biên dịch chuỗi text thô đó thành một Object JavaScript có cấu trúc.

- Trả về một Object mới tinh gọn chỉ gồm đúng 2 trường dữ liệu cốt lõi: title và content.
<img width="1672" height="841" alt="image" src="https://github.com/user-attachments/assets/124388ef-687d-4086-8fa5-5ea097bbf82a" />

- Khởi tạo Mật khẩu ứng dụng (Application Password) trên WordPress

- "vào mục Tài Khoản => chọn user đã tạo lúc setup wordpress => Mật khẩu ứng dụng => Nhập n8n và bấm 'Thêm mật khẩu ứng dụng' => copy chuỗi 24 kí tự"
<img width="1698" height="842" alt="image" src="https://github.com/user-attachments/assets/3095c257-ec65-463c-8a85-86c623ac4562" />

- Node WordPress (Create a Post). Đây là bước em thiết lập các tham số để n8n truyền dữ liệu đã được xử lý sang website WordPress và tiến hành đăng bài viết một cách tự động.

- Em đã thêm thành công Node WordPress vào cuối luồng xử lý (ngay sau Node Code in JavaScript ) và chọn hành động (Operation) là Create a Post (Tạo một bài viết mới).

- Add (nối tiếp vào sau node Code in JavaScript) node: WordPress => Create a Post"

-"Wordpress URL: điền giá trị https://sub-domain1/"

-"Ignore SSL Issues (Insecure): TURN ON"

-"Cấu hình node Create a Post: ... kéo nội dung phần title (bên trái) vào trường title, tương tự kéo nội dung content vào content"

-"Add field (Thêm thuộc tính): Status == Publish"
<img width="1680" height="880" alt="image" src="https://github.com/user-attachments/assets/75c27f6f-ff40-4f10-825f-2356825f15a8" />

- kích hoạt thành công lệnh đăng bài viết tự động từ n8n sang website WordPress.

- Hoàn tất thiết lập quyền kết nối (Setup Credential) cho WordPress API
<img width="1692" height="684" alt="image" src="https://github.com/user-attachments/assets/0bee847b-4e19-4ac8-9782-0b7dd77e65f2" />

- Giao diện sơ đồ luồng công việc (Workflow) hoàn chỉnh, khép kín và đã được kích hoạt chạy tự động thành công trên n8n!

- Em đang ở màn hình thiết kế chính (Editor Canvas) của n8n tại địa chỉ tên miền công khai qua Tunnel là k58-n8n.teumagic.io.vn.

- Nối dây liên kết tuần tự cả 4 node lại với nhau để tạo thành một chuỗi cung ứng dữ liệu (Data Pipeline) tự động.

- Kích hoạt xuất bản luồng chạy (Publish/Active workflow) để hệ thống chuyển sang chế độ giám sát thời gian thực.

- "PUBLISH flow (góc trên phải) Nút này thực hiện việc xuất bản flow <=> flow sẽ tự động thực thi khi thoả mãn điều kiện trigger"
  
- "Kết quả cuối cùng cần đặt được: từ điện thoại, chát với telegram bot => tự động gửi tới node Telegram trigger => Gửi tới Google Gemini Message a model => Gửi

-   sang node Code in JavaScript để tách tiêu đề và nội dung => gửi đến node WordPress để Create a Post"
<img width="1673" height="866" alt="image" src="https://github.com/user-attachments/assets/6c4a7035-d5d5-4271-b34b-b9fa7df88d65" />





















