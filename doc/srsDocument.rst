.. role:: red
    :class: red

.. _srsDocument:

SRS Document
========================

This document outlines the Software Requirements Specification (SRS) for the LLM Benchmark Tool.

1. Introduction
-----------------------

1.1 Purpose
~~~~~~~~~~~~~~~

Phát triển ứng dụng chatbot cho phép "Saler" truy xuất sản phẩm trong hệ thống, tạo báo giá và lên đơn hàng.

1.2 Scope
~~~~~~~~~~~~~~~~~~~

**In-scope:**

Phát triển hệ thống core, cho phép nhận yêu cầu từ người dùng thông qua API, trả về thông tin cũng qua API này.
Chatbot hỗ trơ tiếng Anh và tiếng Hàn.

Các tính năng chính:
* Tìm kiếm sản phẩm
* Tạo Báo giá
* Hỗ trợ mua hàng: tạo đơn hàng
* Tạo Phiếu chứng nhận chất lượng

**Out of scope:**

Không phát triển giao diện chatbot (FE).

2. Overall Description
---------------------------------

2.1 Product Perspective
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Ứng dụng nhận thông tin yêu cầu từ người dùng cuối thông qua API.

- Truy xuất thông tin tại hệ thống ERP, NAS, Email.

- Trả về cho người thông tin cần thiết

2.2 High-level user functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Người dùng tiến hành nhập yêu cầu : Tìm kiếm sản phẩm/Tạo báo giá/Tạo đơn hàng/Tạo Phiếu chứng nhận chất lượng. Gửi thông tin yêu cầu qua API.

- Truy xuất thông tin tại hệ thống ERP, NAS, Email.

- Trả về thông tin cho chatbot thông qua API.

- Ứng dụng hỗ trợ tiếng Anh/ Tiếng Hàn.

2.3 Constraints

- Hệ thống mail sử dụng: :red:`Naverwork`
- Build docker.

3. Specific Requirements
----------------------------------------

3.1 Functional Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

FR-Find Product
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table:: **FR-Find Product-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép en-user nhập vào một đoạn ``message`` yêu cầu tìm kiếm sản phẩm dựa theo ``keyword``

       ``Keyword`` có thể là: tên sản phẩm, mã sản phẩm, nguyên liệu, quy cách, đơn giá, mô tả, tồn kho (có thể mix)
   * - Input
     - Câu lệnh tìm kiếm sản phẩm của end-user
   * - Output
     - Danh sách các sản phẩm phù hợp với yêu cầu.

       Tất cả các thông tin có thể get được từ hệ thống theo từng sản phẩm.
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Thông tin sản phẩm cập nhật trước đó trên hệ thống (nếu không sẽ không tìm thấy sản phẩm)
   * - Postconditions
     - Trả về tất cả các sản phẩm có thông tin khớp từ khóa

.. list-table:: **Business Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Nhận lệnh từ end-user
     - Khách hàng nhập message yêu cầu truy xuất thông tin sản phẩm. 

       **Simple example:** Tôi muốn tìm sản phẩm có ``keyword`` ``condition``.

       Tôi muốn tìm sản phẩm có ``tên`` là ``chổi sơn tường``.

       **Compound Sentence:** Tôi muốn tìm sản phẩm có ``keyword1`` ``condition1`` và ``keyword2`` ``condition2``.

       Tôi muốn tìm sản phẩm có ``tên`` ``chổi sơn tường``, ``nguyên liệu`` ``gỗ``.
     - Tách được ``keyword`` và ``condition`` từ ``message``

       ``Keyword`` cần được nhập chính xác: `` mã ``, `` tên ``, `` nguyên liệu ``, `` quy cách ``, `` tồn kho ``, ``đơn giá ``, `` mô tả ``.
   * - 2. Tìm kiếm sản phẩm theo ``keyword`` và ``condition``
     - Tìm kiếm ``tên sản phẩm`` có được xuất hiện trên hệ thống ERP, Email hay NAS không.

     - **Nếu** ``keyword`` là tên sản phẩm. Tên này chỉ cần khớp một phần với bất kì tên sản phẩm nào đã có trên hệ thống **Thì** liệt kê sản phẩm đó vào danh sách tìm kiếm.

       **Nếu** ``keyword`` là mã sản phẩm. Mã sản phẩm cần khớp 100% mới mã sản phẩm trên hệ thống. Liệt kê các sản phẩm đó vào danh sách

       **Nếu** ``keyword`` là mã sản phẩm **thì** liệt kê thông tin sản phẩm tương ứng với mã sản phẩm đó.

.. list-table:: **Exception Logic**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Không tìm thấy tên sản phẩm trong hệ thống
     - Khách hàng để trống tên sản phẩm khi tìm kiếm theo tên hoặc không tìm thấy thông tin trong hệ thống
     - Phản hồi: không tìm thấy thông tin, yêu cầu nhập lại
   * - 2. Mã sản phẩm không có thông tin trên hệ thống
     - Mã sản phẩm để trống / không tìm thấy
     - Phản hồi: không tìm thấy thông tin, yêu cầu nhập lại

FR-Email
^^^^^^^^^^^^

FR-Email-1: Chatbot truy cập và lấy thông tin từ hệ thống Naverwork
***************************************************************************

.. list-table:: **FR-Email-1**
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
     - Dữ liệu được lưu trữ

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

       Thời gian phản hồi ........????

.. list-table:: **Exception Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Lỗi API Naverwork
     - API Naverwork trả vễ lỗi trong khi call
     - Trả về thông tin lỗi, các lần chạy sau vẫn hoạt động bình thường

FR-Email-2: Truy xuất "Chi tiết xuất kho" từ email
***********************************************************
.. list-table:: **FR-Email-2**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Đọc email trên hệ thống **Naverwork platform** (email, drive) từ đó hỗ trợ end-user truy xuất thông tin ``Chi tiết xuất kho``, ``Đơn giá ``, `` Báo giá ``.
   * - Input
     - Email được lưu trữ trên hệ thống **Naverwork platform**.
   * - Output
     - Tùy vào yêu cầu khách hàng, trả về các thông tin sau: ``Chi tiết xuất kho``, ``Đơn giá ``, `` Báo giá ``
   * - Trigger
     - End-user mở cửa sổ chatbot tại trang web

       Điền câu lệnh tìm kiếm sản phẩm
   * - Preconditions
     - Server, backend API, AI model đã hoạt động bình thường.

       UI chat hoạt động

       Chatbot được cấp permission để đọc email và file Drive.

       Hệ thống có quyền truy cập vào Naverwork.
   * - Postconditions
     - Trả về thông tin khách hàng mong muốn tại UI chatbot
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
   * - 2. Phân tích yêu cầu
     - Dựa trên yêu cầu của khách hàng, tiến hành phân tích yêu cầu của khách hàng 

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