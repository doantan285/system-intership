# Markdown DOCS

## 1. Markdown là gì?

Markdown là một ngôn ngữ đánh dấu đơn giản, dễ đọc, dễ viết, được sử dụng rộng rãi để tạo tài liệu văn bản (như README, blog, ghi chú). Markdown chuyển đổi cú pháp đơn giản thành HTML hoặc các định dạng văn bản khác.

## 2. Lợi ích Markdown

1. **Đơn giản**: Không cần học cú pháp phức tạp.
2. **Đa nền tảng**: Markdown hoạt động tốt trên GitHub, VS Code, các hệ thống quản lý nội dung (CMS), v.v.
3. **Khả năng mở rộng**: có thể bổ sung các tính năng nâng cao như biểu đồ, công thức toán học.
4. **Dễ chuyển đổi**: Markdown có thể xuất ra HTML, PDF, và các định dạng khác.

## 3. Cú pháp Markdown cơ bản

### 3.1. Tiêu đề

Sử dụng dấu `#` để tạo tiêu đề, số lượng dấu `#` tương đương với cấp tiêu đề của nó.
Cú pháp:

```markdown
# Heading1
## Heading2
### Heading3
#### Heading4
##### Heading5
```

Kết quả tương tự:
# Heading1
## Heading2
### Heading3
#### Heading4
##### Heading5

### 3.2. Đoạn văn bản

Để viết đoạn văn, chỉ cần nhập nội dung bình thường.

Ví dụ nội dung nhập: `Đây là một đoạn văn bản.`

Kết quả: Đây là một đoạn văn bản.

### 3.3. In đậm, in nghiêng, gạch ngang

**In đậm**: dùng `**` hoặc `__` bao quanh cụm từ.

*In nghiêng*: dùng `*` hoặc `_` bao quanh cụm từ.

~~Gạch ngang~~: dùng `~~` bao quanh cụm từ.

Cú pháp:

``` markdown
**In đậm**, *In nghiêng*, ~~Gạch ngang~~.
```

Kết quả: **In đậm**, *In nghiêng*, ~~Gạch ngang~~

### 3.4. Danh sách

#### 3.4.1. Danh sách không có thứ tự

Dùng -, * hoặc +:

Cú pháp:

```markdown
- Mục 1
- Mục 2
  - Mục con
  - Mục con khác
```

Kết quả:

- Mục 1
- Mục 2
  - Mục con
  - Mục con khác

#### 3.4.2. Danh sách có thứ tự

Dùng số và dấu chấm 1., 2.:

Cú pháp:

```markdown
1. Mục 1
2. Mục 2
  1. Mục con
  2. Mục con khác

```

Kết quả:

1. Mục 1
2. Mục 2
   1. Mục con
   2. Mục con khác

### 3.5. Liên kết

Gồm có liên kết văn bản `[Tên hiển thị](https://example.com)` và liên kết trần `<https://example.com>`.

#### 3.5.1 Liên kết văn bản

Cú pháp:

```markdown
[Cú pháp Markdown](https://www.markdownguide.org/basic-syntax/)
```

Kết quả:

[Cú pháp Markdown](https://www.markdownguide.org/basic-syntax/)

#### 3.5.2 Liên kết trần (URL tự động)

cú pháp:

```markdown
<https://www.markdownguide.org/basic-syntax/>
```

Kết quả:

<https://www.markdownguide.org/basic-syntax/>

### 3.6. Hình ảnh

Cú pháp:

```markdown
![Tên ảnh](đường_dẫn_ảnh)
```

Ví dụ:

```markdown
![Logo](https://upload.wikimedia.org/wikipedia/commons/4/48/Markdown-mark.svg)
hoặc
![Logo](../images/Markdown-mark.svg)
```

Kết quả:

![Logo](../images/Markdown-mark.svg)

### 3.7. Chèn mã (code)

#### 3.7.1 Chèn mã nội dòng

Dùng dấu ` bao quanh nội dung

Ví dụ:

```javascript
`console.log("Hello World");`
```

Kết quả:

`console.log("Hello World");`

#### 3.7.2. Chèn khối mã

Dùng dấu ``` (ba dấu backtick)

Ví dụ:

``` markdown
```javascript
// In ra dòng chữ "Hello World!"
console.log("Hello World!");
```

Kết quả:

```javascript
// In ra dòng chữ "Hello World!"
console.log("Hello World!");
```

#### 3.7.3. Trích dẫn

Dùng dấu `>` để trích dẫn.

Ví dụ:

```markdown
> Đây là một trích dẫn.
```

Kết quả:

> Đây là một trích dẫn.

### 3.8. Tạo bảng

Sử dụng Sử dụng | và - để tạo bảng.

Ví dụ:

`| Cột 1     | Cột 2       | Cột 3   |`

`|-----------|-------------|---------|`

`| Nội dung 1| Nội dung 2  | Nội dung 3 |`

Kết quả:

| Cột 1 | Cột 2 | Cột 3 |
|-----------|-------------|---------|
| Nội dung 1 | Nội dung 2 | Nội dung 3 |

### 3.9. Chèn đường kẻ ngang

Dùng `---`, `***` hoặc `___`.

Ví dụ:

```markdown
---
```

Kết quả:

```markdown
________________________________________________
```

## 4. Cú pháp Markdown nâng cao

### 4.1. Chèn công thức toán học

Dùng `$` xung quanh với công thức inline hoặc `$$` xung quanh với công thức khối.

Ví dụ 1 (inline):

```markdown
$E = mc^2$
```

Kết quả:

$E = mc^2$

Ví dụ 2 (khối):

```markdown
$$
E = mc^2
$$
```

Kết quả:
$$
E = mc^2
$$
