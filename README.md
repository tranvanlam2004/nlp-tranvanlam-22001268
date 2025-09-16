# Báo cáo Lab 1: Text Tokenization




## Các thành phần đã triển khai

### 1. Interface Tokenizer

- **Vị trí**: `src/core/interfaces.py`  
- **Ý nghĩa**: định nghĩa một lớp trừu tượng làm khung chuẩn cho các tokenizer khác.  
- **Chi tiết**:  
  - Lớp `Tokenizer` kế thừa từ `ABC`.  
  - Bắt buộc có phương thức:  
    ```python
    def tokenize(self, text: str) -> list[str]:
        ...
    ```
  - Nhờ đó, tất cả các tokenizer kế thừa đều có cấu trúc đồng nhất, dễ dàng thay thế khi cần.

---

### 2. SimpleTokenizer

- **File**: `src/preprocessing/simple_tokenizer.py`  
- **Nguyên tắc hoạt động**:
  1. Đưa toàn bộ câu về dạng chữ thường.  
  2. Chèn khoảng trắng trước và sau một số dấu câu cơ bản (`.`, `,`, `?`, `!`).  
  3. Sử dụng phương thức `.split()` để tách thành các token dựa trên khoảng trắng.  

- **Ví dụ**:  
  - Input  → `"Python, Java! C++?"`  
  - Output → `['python', ',', 'java', '!', 'c++', '?']`

---

### 3. RegexTokenizer

- **File**: `src/preprocessing/regex_tokenizer.py`  
- **Phương pháp**: áp dụng Regular Expression để kiểm soát tốt hơn việc cắt từ.  
- **Biểu thức regex sử dụng**:
  ```regex
  \w+|[^\w\s]
- **Ý nghĩa**:

    - \w+ → nhóm liên tiếp các ký tự chữ/số.

     -   [^\w\s] → giữ riêng các ký hiệu đặc biệt hoặc dấu câu.

- **Ví dụ**:

   - Input → "Email me at abc123@test.com!"

   - Output → ['email', 'me', 'at', 'abc123', '@', 'test', '.', 'com', '!']



 
# Báo cáo Lab 2 : Count Vectorization
##  Thành phần đã xây dựng

### 1. Interface Vectorizer  

**Vị trí:** `src/core/interfaces.py`  

Để thống nhất cách triển khai các mô hình biểu diễn văn bản, một interface trừu tượng mang tên **Vectorizer** đã được tạo ra.  

**Chi tiết:**  
- Đây là lớp trừu tượng kế thừa từ `ABC`.  
- Các phương thức bắt buộc có:  
  - `fit(self, corpus: list[str])`: trích xuất và ghi nhớ bộ từ vựng từ một tập văn bản.  
  - `transform(self, documents: list[str]) -> list[list[int]]`: ánh xạ mỗi văn bản thành vector số đếm dựa trên bộ từ vựng đã học.  
  - `fit_transform(self, corpus: list[str]) -> list[list[int]]`: thực hiện đồng thời hai bước trên.  

Nhờ đó, mọi vectorizer tuân theo interface này đều có API thống nhất, dễ dàng thay thế lẫn nhau.  

---


### 2. CountVectorizer  

**Vị trí:** `src/representations/count_vectorizer.py`  

Đây là hiện thực cụ thể của mô hình **Bag-of-Words**.  

**Đặc điểm chính:**  
- **Kế thừa:** từ interface `Vectorizer`.  
- **Thuộc tính chính:**  
  - `tokenizer`: một đối tượng `Tokenizer` (ví dụ `RegexTokenizer` từ Lab 1).  
  - `vocabulary_`: từ điển ánh xạ mỗi token duy nhất sang một chỉ số.  

**Nguyên tắc hoạt động:**  
- `fit`: duyệt qua toàn bộ corpus, tách từ bằng tokenizer, sau đó tập hợp và sắp xếp chúng để tạo `vocabulary_`.  
- `transform`: với mỗi văn bản, khởi tạo vector toàn 0 có độ dài bằng số từ vựng, sau đó tăng dần số đếm ở vị trí phù hợp mỗi khi gặp token.  
- `fit_transform`: thực hiện cả hai bước trên một cách gọn gàng.  

---

