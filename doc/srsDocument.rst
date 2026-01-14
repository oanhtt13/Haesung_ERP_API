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
* Tìm kiếm sản phẩm
* Tạo Báo giá
* Hỗ trợ mua hàng: tạo đơn hàng
* Tạo Phiếu chứng nhận chất lượng

**Out of scope:**

Không phát triển giao diện chatbot.

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

2.3 Constraints

- Hệ thống mail sử dụng: :red:`Naverwork`
- Build docker.
- 

3. Specific Requirements
----------------------------------------

3.1 Functional Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: **Find Product**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép en-user tìm kiếm sản phẩm bằng từ khóa.
   * - Input
     - Câu lệnh tìm kiếm sản phẩm của end-user
   * - Output
     - Danh sách các sản phẩm tương ứng
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
     - Khách hàng nhập thông tin từ 

3.1.2 Benchmarking Capabilities
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-   The tool shall support benchmarking of multiple LLMs.
-   The tool shall generate detailed performance reports.

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