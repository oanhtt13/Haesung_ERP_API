.. role:: red
    :class: red



.. _srsDocument:

SRS Document
========================

This document outlines the Software Requirements Specification (SRS) for the Haesung ERP API.

1. Introduction
-----------------------

1.1 Purpose
~~~~~~~~~~~~~~~

- Phát triển ứng dụng chatbot cho phép "Seller" truy xuất sản phẩm trong hệ thống, tạo báo giá và lên đơn hàng. (Khác với chatbot support Khách mua hàng đã làm tại phase 1).

- HBLab phát triển phần core AI giúp hỗ trợ Chatbot hiểu yêu cầu từ end-user, đồng thời đưa ra câu trả lời thích hợp cho yêu cầu đó.

1.2 Scope
~~~~~~~~~~~~~~~~~~~

**In-scope:**

Phát triển hệ thống core AI, cho phép nhận yêu cầu từ người dùng thông qua API Chatbot, kết quả trả về cũng qua API này.
Chatbot hỗ trợ tiếng Anh và tiếng Hàn.

Các tính năng chính:

* Tiếp nhận, đọc hiểu yêu cầu End-user
* Tìm kiếm sản phẩm
* Hỗ trợ mua hàng: tạo đơn hàng
* Tạo Báo giá
* Tạo Phiếu chứng nhận chất lượng
* Truy xuất thông tin trên hệ thống Email/Drive

**Out of scope:**

Không phát triển giao diện chatbot (FE).

Không phát triển API.

2. Overall Description
---------------------------------

2.1 Product Perspective
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Ứng dụng nhận thông tin yêu cầu từ người dùng cuối thông qua API.

- Truy xuất thông tin tại hệ thống ERP, NAS, Email.

- Trả về cho người thông tin cần thiết.

2.2 High-level user functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Người dùng tiến hành nhập yêu cầu : Tìm kiếm sản phẩm/Tạo báo giá/Tạo đơn hàng/Tạo Phiếu chứng nhận chất lượng. Gửi thông tin yêu cầu qua API.

- Truy xuất thông tin tại hệ thống ERP, NAS, Email.

- Trả về thông tin cho chatbot thông qua API.

- Ứng dụng hỗ trợ tiếng Anh/ Tiếng Hàn.

2.3 Constraints

- Hệ thống mail/drive sử dụng: :red:`Naverwork`.

- Build docker.

3. Specific Requirements
----------------------------------------

3.1 Functional Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

FR-Chatbot
^^^^^^^^^^^^

.. list-table:: **FR-Chatbot-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép chatbot có thể hiểu được các ngôn ngữ Tiếng Anh/Tiếng Hàn tự nhiên để truy xuất thông tin. Các ngôn ngữ khác là Optional.
   * - Input
     - Câu lệnh dạng text, được gửi đến core AI thông qua API Chatbot.
   * - Outpput
     - Trả về câu trả lời tương ứng thông qua API Chatbot.
   * - Trigger
     - End-user truy cập website khách hàng, chọn mục ``Chatbot``.

       Nhập câu hỏi mong muốn và nhấn ``Enter``.
   * - Preconditions
     - Server wesite hoạt động bình thường

       Đảm bảo kết nối mạng ổn định.
   * - Postconditionas
     - Trả lời câu hỏi của En-user đúng với mong muốn

.. list-table:: **Business Logic**
   :widths: 10 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Tiếp nhận yêu cầu của end-user
     - Tiếp nhận thông tin yêu cầu của end-user thông qua API đã có, thông tin được lưu vào body
     - API đã có sẵn, chứa thông tin: .....

       Định dạng message: Chuỗi kí tự (String)
   * - 2. Phân tích yêu cầu
     - Trích xuất yêu cầu của End-user, đọc hiểu yêu cầu End-user, trích xuất các thông tin cần thiết
     - Từ thông tin nhận từ API, parser đúng trường nội dung yêu cầu End-user

       Hiểu được ít nhất 10 dạng câu hỏi phổ biến về từng yêu cầu của End-user

       Xử lý lỗi chính tả nhẹ.
   * - 3. Thực thi yêu cầu End-user
     - Dựa trên yêu cầu End-user và các thông tin cần thiết, tiến hành call API hỗ trợ để lấy được thông tin cần thiết
     - Call đúng API
   * - 4. Trả về kết quả
     - Từ thông tin trả về từ các API hỗ trợ, tiến hành tạo respone cho API Chatbot.
     - Trả về theo form theo yêu cầu (thông tin form)

       Ngôn ngữ trả về tương thích với ngôn ngữ end-user nhập vào.

.. list-table:: **Extent Logic**
   :widths: 10 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. End-user nhập nội dung không liên quan
     - End-user nhập nội dung không thể đọc hiểu, sai chính tả quá nhiều.
     - Thông báo không hiểu, yêu cầu End-user nhập lại thông tin.

       Có cần đưa ra một vài gợi ý câu hỏi thích hợp không?
   * - 2. End-user nhập sai ngôn ngữ
     - End-user nhập nội dung câu hỏi ngôn ngữ không hỗ trợ
     - Thông báo cho End-user không hỗ trợ ngôn ngữ này, chỉ hỗ trợ tiếng Anh/tiếng Hàn

       Yêu cầu nhập lại
   * - 3. End-user nhập yêu cầu thiếu/sai thông tin
     - End-user nhập yêu cầu thiếu các thông tin cung cấp tối thiểu để có thể thực hiện yêu cầu
     - Thông báo cho End-user yêu cầu thiếu thông tin

       Gợi ý các thông tin end-user có thể bổ sung để lệnh có thể chạy được.


FR-FR: Find Product
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FR-FP-1: Find Product
*******************************

.. list-table:: **FR-Find Product-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Chatbot cho phép người dùng tìm sản phẩm bằng tiếng Anh/Hàn tự nhiên và trả về mọi thông tin liên quan.
   * - Input
     - Câu lệnh: "Tìm sản phẩm có tên là A", "Cho tôi thông tin sản phẩm có nguyên liệu là ABC", ....

       ``Keywors`` về sản phẩm: tên sản phẩm, mã sản phẩm, nguyên liệu, quy cách, đơn giá, mô tả, tồn kho (có thể mix)
   * - Output
     - Danh sách các sản phẩm phù hợp với yêu cầu.

       Mỗi sản phẩm kèm tất cả thông tin có lấy được từ hệ thống ERP.
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Thông tin sản phẩm cập nhật trước đó trên hệ thống (nếu không sẽ không tìm thấy sản phẩm)
   * - Postconditions
     - Trả về tất cả các sản phẩm có thông tin khớp từ khóa
       
       Chatbot ghi nhớ sản phẩm đã được chọn (để phục vụ các bước tiếp theo)

.. list-table:: **Business Logic**
   :widths: 10 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Nhận lệnh từ end-user
     - User gửi yêu cầu tìm sản phẩm
     - Tiếp nhận yêu cầu thông qua API Chatbot
   * - 2. Phân tích câu hỏi và trích xuất từ khóa
     - Sử dụng AI đọc hiểu yêu cầu của User
     - Trích xuất từ khóa (Các từ khóa cần thiết để call API find-product)
   * - 3. Truy vấn thông tin
     - Sử dụng các từ khóa đã trích xuất được, tiến hàng call API Find product để lấy thông tin
     - Get thành công toàn bộ thông tin sản phẩm

       Có thể lấy nhiều thông tin sản phẩm một lúc

.. list-table:: **Exception Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Không tìm thấy tên sản phẩm trong hệ thống
     - End-user để trống tên sản phẩm khi tìm kiếm theo tên hoặc không tìm thấy thông tin trong hệ thống
     - Phản hồi: không tìm thấy thông tin, yêu cầu nhập lại
   * - 2. Mã sản phẩm không có thông tin trên hệ thống
     - Mã sản phẩm để trống / không tìm thấy
     - Phản hồi: không tìm thấy thông tin, yêu cầu nhập lại

FR-Quotation
^^^^^^^^^^^^^^^^^^^

FR-Quotation-1
************************

.. list-table:: **FR-Quotation-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Dựa theo thông tin trên hệ thống eCount, hỗ trợ tạo báo giá cho sản phẩm theo yêu cầu End-user
   * - Input
     - Yêu cầu của End-user nhập tại cửa sổ chatbot
   * - Output
     - File báo giá cho sản phẩm theo form có sẵn dựa trên thông tin đã cung cấp.
   * - Preconditions
     - Hệ thống đã được update thông tin đầy đủ
   * - Postconditions
     - Trả về url có chứa `` Báo giá ``

.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Tiếp nhận yêu cầu từ end-user
     - Tiếp nhận yêu cầu từ phía End-user

       Mẫu yêu cầu của End-user như thế nào? En-user có thể cung cấp các thông tin gì để yêu cầu tạo Phiếu?
     - Đọc hiểu nội dung yêu cầu của End-user
   * - 2. Xác nhận yêu cầu của End-user
     - Tổng hợp thông tin, yêu cầu End-user xác nhận yêu cầu
     - Chỉ xác định End-user đồng ý tạo ``Phiếu chứng nhận chất lượng`` khi có những từ ngữ mang ý nghĩa chấp nhận.

       Nếu không, cần hỏi lại End-user để tiếp nhận yêu cầu
   * - 3. Yêu cầu tạo báo giá
     - Gửi yêu cầu tạo báo giá
     - Yêu cầu phải đầy đủ thông tin: ``Thông tin sản phẩm, số lượng``
   * - 4. Trả về thông tin
     - Trả về báo giá đã được tạo
     - Nhận được link url chứa ``Phiếu chứng nhận chất lượng``

FR-POC: Purchase Order Creation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FR-POC-1: Purchase Order Creation
****************************************

.. list-table:: **FR-POC-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép User tạo yêu cầu tạo ``Purchase Order`` dựa trên các sản phẩm mà người dùng đã tìm kiếm hoặc đã chọn trong phiên hội thoại

       Chức năng này hỗ trợ quy trình mua hàng: chọn sản phẩm -> Tạo PO -> gửi PO cho tính năng mua hàng.
   * - Input
     - Câu lệnh:

       * Tạo đơn mua hàng cho sản phẩm này.

       * Tôi muốn mua sản phẩm tên/mã A.

       * Lập cho tôi PO cho 3 sản phẩm tôi đã xem.
       
       Thông tin cần thiết (chatbot sẽ hỏi lại User nếu thiếu): (Yêu cầu cung cấp)
   * - Output
     - File Purchase Oder trong url trả về.
   * - Trigger
     - Khi khách hàng nhập yêu cầu tại UI chatbot
   * - Preconditions
     - Người dùng đã thực hiện tìm kiếm hoặc chỉ định ít nhất 1 sản phẩm trong phiên chat

       Chatbot có quyền truy cập dữ liệu sản phẩm và NCC thông qua API

       API hỗ trợ tạo PO trả về đúng thông tin yêu cầu
   * - Postconditions
     - PO được tạo thành công, trả về cho AI core url dẫn đến trang mua hàng với thông tin đúng theo khách hàng yêu cầu


.. list-table:: **Business Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Tiếp nhận yêu cầu từ khách hàng
     - Tiếp nhận yêu cầu của khách hàng và gửi thông tin cho core AI thông qua API Chatbot
     - Gửi thành công
   * - 2. Phân tích yêu cầu
     - Phân tích yêu cầu User, xác định danh sách sản phẩm đã được tìm trong session
     - Liệt kê toàn bộ các sản phẩm User quan tâm
   * - 3. Thu thập thông tin bổ sung
     - Yêu cầu User cung cấp các thông tin cần thiết để có thể tiến hành tạo đơn hàng
     - Chỉ hỏi những thông tin bị thiếu
   * - 4. Tạo yêu cầu
     - Tổng hợp thông tin cần thiết, call API hỗ trợ yêu cầu tạo PO
     - Chuẩn bị các thông tin đầy đủ, call thành công API

       API trả về url chưa link dẫn đến trang thanh toán
   * - 5. Phản hồi khách hàng
     - Core AI gửi lại url dẫn đến trang mua hàng cho phía FE.

       Có gửi câu trả lời mẫu cho FE không?

.. list-table:: **Exception Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Không có sản phẩm nào được chọn trước đó
     - User chưa có lịch sử tìm kiếm sản phẩm được ghi nhận
     - Chatbot nhắc người dùng cần cung cấp thông tin trước
   * - 2. Không cung cấp đầy đủ các thông tin cần thiết
     - User cung cấp thiếu thông tin
     - Thông báo cho User, yêu cầu cung cấp thông tin
   * - 3. Sản phẩm không còn hàng
     - Sản phẩm không còn hàng hoặc không đủ số lượng yêu cầu
     - Thông báo cho User, vẫn tạo order

FR-RAG
^^^^^^^^^^^^

FR-RAG-1: Chatbot truy cập và lấy thông tin từ hệ thống Naverwork
***************************************************************************

.. list-table:: **FR-RAG-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Chatbot phải kết nối được với hệ thống Naverwork (Email / Drive) để đọc nội dung email và tài liệu liên quan.
   * - Input
     - API cho phép truy cập vào hệ thống **Naverwork**.
   * - Output
     - Nội dung mail được lưu lại để sử dụng khi cần thiết.
   * - Trigger
     - Thực hiện đọc dữ liệu liên tục vì nội dung email cần được update mới nhất.
   * - Preconditions
     - User có quyền truy cập email Naverwork.

       Hệ thống Naverwork API khả dụng.

       Chatbot được cấp token/permission để đọc email và file Drive.
   * - Postconditions
     - Dữ liệu được lưu trữ, luôn luôn update dữ liệu mới nhất từ hệ thống Email/Drive

       Log truy vấn

.. list-table:: **Business Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Kết nối với hệ thống **Naverwork**
     - Sử dung API đăng nhập vào hệ thống Naverwork
     - Đăng nhập thành công
   * - 2. Hệ thống truy xuất email
     - Hệ thống truy xuất toàn bộ thông tin trong mail, phân tích nội dung email.
     - Đọc được email hợp lệ khi API hoạt động đúng
   * - 3. Lưu email
     - Lưu toàn bộ nội dung email
     - Có thể dễ dang truy cập, trích xuất nội dung email khi cần thiết

       Các định dạng file cần xử lý: ``.doc``, ``.csv``, ``.xlsx``, ``.pdf``, ``.hwp``, ``.pptx``

.. list-table:: **Exception Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Lỗi API Naverwork
     - API Naverwork trả vễ lỗi trong khi call
     - Trả về thông tin lỗi, các lần chạy sau vẫn hoạt động bình thường

FR-RAG-2: Truy xuất "Chi tiết xuất kho" từ email
***********************************************************
.. list-table:: **FR-RAG-2**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Đọc email trên hệ thống **Naverwork platform** (email, drive) từ đó hỗ trợ end-user truy xuất thông tin ``Chi tiết xuất kho``.
   * - Input
     - Email được lưu trữ trên hệ thống **Naverwork platform**.
   * - Output
     - Tùy vào yêu cầu End-user, trả về các thông tin sau: ``Chi tiết xuất kho``
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Thông tin xuất kho tồn tại trong email
   * - Postconditions
     - Trả về thông tin End-user mong muốn thông qua API đã dựng sẵn. Từ đó sẽ hiển thị lên màn hình chatbot

       Nếu không tìm được thông tin theo yêu cầu trên hệ thống, thông báo end-user.

.. list-table:: **Exception Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Nhập lệnh yêu cầu
     - End-user nhập message yêu cầu truy xuất thông tin tại một email chỉ định.
     - 
   * - 2. Tìm kiếm thông tin
     - Dựa trên yêu cầu của End-user, tiến hành tìm kiếm email có chứa thông tin xuất kho
     - Thời gian truy xuất thông tin?
   * - 3. Truy xuất thông tin xuất kho
     - Truy xuất thông tin xuất kho (thông tin này là dạng gì? text?, file pdf?, ...)
     - Trích xuất thông tin thành công, giữ nguyên tình trạng file?
   * - 4. Trả kết quả cho user
     - Trả kết quả tìm kiếm được thông qua API
     - Thông tin trả về yêu cầu như thế nào?

FR-RAG-3: Truy xuất ``Đơn giá``
****************************************

.. list-table:: **FR-RAG-3**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép truy xuất thông tin ``Đơn giá `` từ email
   * - Input
     - Email được lưu trữ trên hệ thống **Naverwork platform**.
   * - Output
     - Tùy vào yêu cầu End-user, trả về các thông tin sau: ``Đơn giá``
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Thông tin xuất kho tồn tại trong email
   * - Postconditions
     - Trả về thông tin End-user mong muốn thông qua API đã dựng sẵn. Từ đó sẽ hiển thị lên màn hình chatbot
     - Nếu không tìm được thông tin theo yêu cầu trên hệ thống, thông báo end-user.

.. list-table:: **Exception Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Nhập lệnh yêu cầu
     - End-user nhập message yêu cầu truy xuất thông tin tại một email chỉ định.
     - 
   * - 2. Tìm kiếm thông tin
     - Dựa trên yêu cầu của End-user, tiến hành tìm kiếm email có chứa thông tin ``đơn giá``
     - Thời gian truy xuất thông tin?
   * - 3. Truy xuất thông tin xuất kho
     - Truy xuất thông tin xuất kho (thông tin này là dạng gì? text?, file pdf?, ...)
     - Trích xuất thông tin thành công, giữ nguyên tình trạng file?
   * - 4. Trả kết quả cho user
     - Trả kết quả tìm kiếm được thông qua API
     - Thông tin trả về yêu cầu như thế nào?

FR-RAG-4: Truy xuất :red:`Báo giá`
****************************************

.. list-table:: **FR-RAG-4**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép truy xuất thông tin ``Báo giá `` từ email
   * - Input
     - Email được lưu trữ trên hệ thống **Naverwork platform**.
   * - Output
     - Tùy vào yêu cầu End-user, trả về các thông tin sau: ``Báo giá``
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Thông tin xuất kho tồn tại trong email
   * - Postconditions
     - Trả về thông tin End-user mong muốn thông qua API đã dựng sẵn. Từ đó sẽ hiển thị lên màn hình chatbot
     - Nếu không tìm được thông tin theo yêu cầu trên hệ thống, thông báo end-user.

.. list-table:: **Exception Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Nhập lệnh yêu cầu
     - End-user nhập message yêu cầu truy xuất thông tin tại một email chỉ định.
     - 
   * - 2. Tìm kiếm thông tin
     - Dựa trên yêu cầu của End-user, tiến hành tìm kiếm email có chứa thông tin ``Báo giá``
     - Thời gian truy xuất thông tin?
   * - 3. Truy xuất thông tin xuất kho
     - Truy xuất thông tin xuất kho (thông tin này là dạng gì? text?, file pdf?, ...)
     - Trích xuất thông tin thành công, giữ nguyên tình trạng file?
   * - 4. Trả kết quả cho user
     - Trả kết quả tìm kiếm được thông qua API
     - Thông tin trả về yêu cầu như thế nào?

FR-Quality_Certificate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FR-Quality_Certificate-1: Tạo phiếu chứng nhận chất lượng
***************************************************************

.. list-table:: **FR-Quality_Certificate-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Hỗ trợ tạo Phiếu chứng nhận chất lượng sau khi hàng đã được xuất kho
   * - Input
     - End-user nhập message yêu cầu tạo ``Phiếu chứng nhận chất lượng``
   * - Output
     - Tạo thành công
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Kết nối thành công đến hệ thống ERP

       Đảm bảo các thông tin cần thiết đã được update trên hệ thống ERP

       Đơn hàng đã xuất kho
   * - Postconditions
     - Trả về các thông tin cần thiết cho ``Phiếu chứng nhận chất lượng``

.. list-table:: **Exception Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Tiếp nhận yêu cầu từ end-user
     - Tiếp nhận yêu cầu từ phía End-user

       Mẫu yêu cầu của End-user như thế nào? En-user có thể cung cấp các thông tin gì để yêu cầu tạo Phiếu?
     - Đọc hiểu nội dung yêu cầu của End-user
   * - 2. Xác nhận yêu cầu của End-user
     - Tổng hợp thông tin, yêu cầu End-user xác nhận yêu cầu
     - Chỉ xác định End-user đồng ý tạo ``Phiếu chứng nhận chất lượng`` khi có những từ ngữ mang ý nghĩa chấp nhận.

       Nếu không, cần hỏi lại End-user để tiếp nhận yêu cầu
   * - 3. Yêu cầu tạo báo giá
     - Gửi yêu cầu tạo báo giá
     - Yêu cầu phải đầy đủ thông tin: ``Thông tin sản phẩm, số lượng``
   * - 4. Trả về thông tin
     - Trả về báo giá đã được tạo
     - Nhận được link url chứa ``Phiếu chứng nhận chất lượng``



3.2 Non-functional Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3.2.1 Performance Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-   The tool shall execute benchmarks within a reasonable time frame.
-   The tool shall provide real-time feedback during benchmark execution.

3.2.2 Usability Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-   The tool shall be easy to use for both technical and non-technical users.
-   The tool shall provide clear instructions and documentation.

4. External Interface Requirements
--------------------------------------------

4.1 User Interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sử dụng giao diện chatbot do phía HBU phát triển

4.2 Hardware Interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

None

4.3 Software Interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: API List
   :widths: 15 10 30
   :header-rows: 1

   * - GET 

5. System Features and Use Cases
---------------------------------

5.1 Feature: Configure Benchmark Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Users can configure parameters such as input text length, number of iterations, and model selection.

5.2 Feature: Execute Benchmarks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Users can initiate benchmark runs with their configured settings.

5.3 Feature: View Results and Reports
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Users can view detailed results in tabular format or export them as CSV files.