# Hướng dẫn chi tiết thư viện JSON trong Python

## 1. Giới thiệu về JSON

JSON (JavaScript Object Notation) là một định dạng trao đổi dữ liệu phổ biến, dễ đọc và dễ viết. Python cung cấp module `json` tích hợp sẵn để làm việc với dữ liệu JSON.

## 2. Import thư viện

```python
import json
```

## 3. Các phương thức chính

### 3.1. json.dumps() - Chuyển Python object sang JSON string

Chuyển đổi một đối tượng Python thành chuỗi JSON.

**Cú pháp:**
```python
json.dumps(obj, indent=None, sort_keys=False, ensure_ascii=True)
```

**Ví dụ:**
```python
import json

# Dictionary Python
data = {
    "name": "Nguyen Van A",
    "age": 25,
    "city": "Ha Noi",
    "is_student": False,
    "courses": ["Python", "Java", "C++"]
}

# Chuyển sang JSON string
json_string = json.dumps(data)
print(json_string)
# Output: {"name": "Nguyen Van A", "age": 25, "city": "Ha Noi", ...}

# Với format đẹp hơn
json_formatted = json.dumps(data, indent=4, ensure_ascii=False)
print(json_formatted)
```

**Tham số quan trọng:**
- `indent`: Số khoảng trắng để format (thường dùng 2 hoặc 4)
- `sort_keys`: Sắp xếp keys theo thứ tự alphabet (True/False)
- `ensure_ascii`: Nếu False, cho phép ký tự Unicode

### 3.2. json.dump() - Ghi Python object vào file JSON

Ghi đối tượng Python vào file dưới dạng JSON.

**Cú pháp:**
```python
json.dump(obj, file_object, indent=None)
```

**Ví dụ:**
```python
import json

data = {
    "students": [
        {"id": 1, "name": "An", "score": 8.5},
        {"id": 2, "name": "Binh", "score": 9.0}
    ]
}

# Ghi vào file
with open('students.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=4, ensure_ascii=False)
```

### 3.3. json.loads() - Chuyển JSON string sang Python object

Chuyển đổi chuỗi JSON thành đối tượng Python.

**Cú pháp:**
```python
json.loads(json_string)
```

**Ví dụ:**
```python
import json

json_string = '{"name": "Tran Thi B", "age": 22, "scores": [8, 9, 7.5]}'

# Chuyển sang Python dictionary
data = json.loads(json_string)
print(data['name'])  # Output: Tran Thi B
print(data['scores'])  # Output: [8, 9, 7.5]
print(type(data))  # Output: <class 'dict'>
```

### 3.4. json.load() - Đọc file JSON thành Python object

Đọc dữ liệu từ file JSON và chuyển thành đối tượng Python.

**Cú pháp:**
```python
json.load(file_object)
```

**Ví dụ:**
```python
import json

# Đọc từ file
with open('students.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
    print(data)
    print(data['students'][0]['name'])
```

## 4. Bảng chuyển đổi kiểu dữ liệu

### Python → JSON
| Python | JSON |
|--------|------|
| dict | object |
| list, tuple | array |
| str | string |
| int, float | number |
| True | true |
| False | false |
| None | null |

### JSON → Python
| JSON | Python |
|------|--------|
| object | dict |
| array | list |
| string | str |
| number (int) | int |
| number (real) | float |
| true | True |
| false | False |
| null | None |

## 5. Ví dụ thực tế

### 5.1. Làm việc với API

```python
import json
import urllib.request

# Giả sử gọi API (ví dụ minh họa)
response = '{"status": "success", "data": {"temperature": 25, "humidity": 60}}'
data = json.loads(response)

print(f"Nhiệt độ: {data['data']['temperature']}°C")
print(f"Độ ẩm: {data['data']['humidity']}%")
```

### 5.2. Lưu cấu hình ứng dụng

```python
import json

# Tạo cấu hình
config = {
    "database": {
        "host": "localhost",
        "port": 5432,
        "username": "admin"
    },
    "debug_mode": True,
    "max_connections": 100
}

# Lưu cấu hình
with open('config.json', 'w') as f:
    json.dump(config, f, indent=4)

# Đọc cấu hình
with open('config.json', 'r') as f:
    loaded_config = json.load(f)
    print(loaded_config['database']['host'])
```

### 5.3. Xử lý dữ liệu phức tạp

```python
import json

data = {
    "company": "Tech Corp",
    "employees": [
        {
            "id": 1,
            "name": "Le Van C",
            "position": "Developer",
            "skills": ["Python", "JavaScript"],
            "salary": 2000
        },
        {
            "id": 2,
            "name": "Pham Thi D",
            "position": "Designer",
            "skills": ["Photoshop", "Illustrator"],
            "salary": 1800
        }
    ]
}

# Ghi vào file với format đẹp
with open('company.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=2, ensure_ascii=False, sort_keys=True)

# Đọc và xử lý
with open('company.json', 'r', encoding='utf-8') as f:
    company_data = json.load(f)
    
    # Tính tổng lương
    total_salary = sum(emp['salary'] for emp in company_data['employees'])
    print(f"Tổng lương: ${total_salary}")
```

## 6. Xử lý lỗi (Error Handling)

```python
import json

# Xử lý JSON không hợp lệ
try:
    invalid_json = '{"name": "Test", "age": }'  # Thiếu giá trị
    data = json.loads(invalid_json)
except json.JSONDecodeError as e:
    print(f"Lỗi JSON: {e}")

# Xử lý file không tồn tại
try:
    with open('nonexistent.json', 'r') as f:
        data = json.load(f)
except FileNotFoundError:
    print("File không tồn tại!")
except json.JSONDecodeError:
    print("File không phải định dạng JSON hợp lệ!")
```

## 7. JSON với Custom Objects

```python
import json
from datetime import datetime

class Student:
    def __init__(self, name, age, enrollment_date):
        self.name = name
        self.age = age
        self.enrollment_date = enrollment_date

# Custom encoder
class StudentEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Student):
            return {
                'name': obj.name,
                'age': obj.age,
                'enrollment_date': obj.enrollment_date.isoformat()
            }
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

# Sử dụng
student = Student("Hoang Van E", 20, datetime.now())
json_string = json.dumps(student, cls=StudentEncoder, indent=2)
print(json_string)
```

## 8. Tips và Best Practices

1. **Luôn sử dụng `encoding='utf-8'`** khi làm việc với file có ký tự Unicode
2. **Sử dụng `ensure_ascii=False`** để giữ ký tự tiếng Việt
3. **Dùng `indent`** để format JSON dễ đọc khi debug
4. **Luôn xử lý ngoại lệ** khi parse JSON từ nguồn bên ngoài
5. **Sử dụng context manager (`with`)** khi làm việc với file
6. **Validate dữ liệu** trước khi chuyển đổi sang JSON

## 9. Tổng kết các phương thức

| Phương thức | Chức năng | Input | Output |
|-------------|-----------|-------|--------|
| `json.dumps()` | Python → JSON string | Python object | String |
| `json.dump()` | Python → JSON file | Python object, file | None |
| `json.loads()` | JSON string → Python | String | Python object |
| `json.load()` | JSON file → Python | File | Python object |

## 10. Ví dụ tổng hợp

```python
import json

# 1. Tạo dữ liệu
data = {
    "title": "Danh sách sinh viên",
    "year": 2024,
    "students": [
        {"id": 1, "name": "An", "gpa": 3.5},
        {"id": 2, "name": "Bình", "gpa": 3.8}
    ]
}

# 2. Ghi vào file
with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=4, ensure_ascii=False)

# 3. Đọc từ file
with open('data.json', 'r', encoding='utf-8') as f:
    loaded_data = json.load(f)

# 4. Chuyển sang string
json_str = json.dumps(loaded_data, ensure_ascii=False)

# 5. Parse string
parsed_data = json.loads(json_str)

print(f"Có {len(parsed_data['students'])} sinh viên")
```

---

**Lưu ý:** Thư viện `json` là built-in trong Python, không cần cài đặt thêm!
