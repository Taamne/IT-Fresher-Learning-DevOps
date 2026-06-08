Shell scripts (Linux)
Introduction
Một chương trình máy tính được thiết kế để chạy trên Unix shell (trình thông dịch dòng lệnh)
Wrapper script: Là loại script thiết lập môi trường, chạy chương trình và thực hiện các bước dọn dẹp hoặc ghi log cần thiết.1
1. Math calculation

Shell hỗ trợ nhiều cách để thực hiện tính toán:

Lệnh let: Sử dụng cho các phép tính và toán tử tăng/giảm

Ví dụ: let RESULT=NUMBER+5, let NUMBER++

Cú pháp (()) và []:

RESULT=$((NUMBER+5))

RESULT=$[NUMBER+5]

Lệnh expr: RESULT=$(expr $NUMBER + 5)

3. Cấu trúc điều khiển (Decision Making)

Câu lệnh if:

Cú pháp cơ bản: if [ condition ]; then ... elif [ condition ]; then ... else ... fi

Toán tử logic:

AND: if [ cond1 ] && [ cond2 ]; then

OR: if [ cond1 ] || [ cond2 ]; then

Phủ định: if [ ! condition ]; then

Các phép so sánh (Comparisons)

Loại so sánh

Toán tử / Ví dụ

Số học: -eq (bằng), -ne (khác), -gt (>), -lt (<), -ge (>=), -le (<=)

Chuỗi:  = (bằng), != (khác). Lưu ý luôn sử dụng khoảng trắng trong dấu ngoặc.

Kiểm tra chuỗi:  -z (chuỗi rỗng), -n (chuỗi không rỗng).

Hệ thống tệp  -e (tồn tại), -f (tệp thường), -d (thư mục), -x (có quyền thực thi), -w (có quyền ghi).

5. Ký tự đại diện (Wildcards) và Biểu thức chính quy (Regex)

Wildcards: ? (một ký tự), * (nhiều ký tự), [ ] (phạm vi ký tự), { } (danh sách các cụm từ).

Regex cơ bản: ^ (bắt đầu dòng), $ (kết thúc dòng), . (ký tự bất kỳ), + (một hoặc nhiều lần).

6. Vòng lặp (Loops)

Vòng lặp for:

Duyệt danh sách: for arg in [list]; do ... done

ví dụ: for i in a b c d ; do useradd $i done

Duyệt phạm vi số: for NUMBER in {1..10}

Duyệt tệp tin: for FILE in *.txt

Vòng lặp while:

Chạy khi điều kiện đúng: while [ condition ]; do ... done

Đọc tệp tin theo từng dòng: while read line; do ... done < "$FILENAME"

Case:

7. Mảng (Arrays) và Hàm (Functions)

Mảng:

Khai báo: ARRAY=(value1 value2)

Truy cập: ${ARRAY[0]} (phần tử đầu). ${ARRAY[1]} ( phần tử thứ 1),…, ${ARRAY[@]} (tất cả phần tử), ${#ARRAY[@]} (số lượng phần tử), ${ARRAY[*] (tất cả phần tử ), ${!ARRAY[@]} (tất cả chỉ số trong mảng (@/*))

Hàm:

Định nghĩa: function_name() { ... } hoặc function function_name { ... }

Tham số: Sử dụng $1, $2 để nhận giá trị truyền vào

Biến cục bộ: Sử dụng từ khóa local để giới hạn phạm vi biến trong hà

8. Công cụ xử lý văn bản (Awk & Sed)

Awk: Dùng để tìm kiếm và xử lý các dòng văn bản theo mẫu

Cấu trúc: awk 'BEGIN{...} {...} END{...}' input-file

Hỗ trợ các biến như NR (số dòng), FS (dấu phân cách trường), $1, $2 (trường cụ thể)

Sed: Trình chỉnh sửa luồng văn bản

Cú pháp: sed [addr]X[option]

Lệnh phổ biến: d (xóa), p (in), s/regexp/replacement/ (thay thế)

Cờ g (global) dùng để thay thế tất cả các lần xuất hiện trên một dòng

9. Arguments

passing into scripts: để chạy được chương trình, truyền vào giá trị để chạy được scripts

passing into function:

ví dụ: ./hello.sh 1 2 3

$0: scripts name

$1: first argument

$2

$n:

“$#”:  đếm số lượng arguments

“$*”

|: pipe

10. Redirection and Piping

STDIN(0)

STDOUT(1)

STDERR(2)

ví dụ:

cat file1.txt > output_from_cat.txt

cat file1.txt 1> output_from_cat.txt

cat file1.txt 2> output_from_cat.txt

11. filesystem relate test



12. gán giá trị

FILE=tam.txt #gán file

road FILE #đọc file từ đầu vào


echo $FILE

CURRENT_FOLDER=$(pwd) hoặc CURRENT_FOLDER=’pwd’ # lấy output từ cmd

Các cách chạy chương trình Shell Scripts

./filename.sh


bash filename.sh

filename.sh (đặt PATH)



sed snd awk

awk

advand fillter

data validation

custom tranformation

formatted report

matheratical function

linux shell scripts: conidtional branding, loop, variable , arrays

### Basic awk syntax

awk <option 1>… <option n>… `/pattern/ {awk commands}' <target file1>...<target file n>

awk <option 1>… <option n>… -f {awk commands_file}' <target file1>...<target file n>

định nghĩa: Awk là 1 chương trình , có 1/nhiều mệnh đề (pattern), tương ứng với pattern có action tương ứng. đọc từng dòng một, datastream cho đến khi tìm được đúng pattern

khung chuwogn trình

!image.png

### built-in  variable

FS: field separator. default value “\t”

record: Dinh Thi Thanh Tam

mỗi 1 dòng là 1 record

$0: dòng

$1: field đầu tiên của dòng 

NF: number of field =4

NR

RS: record sepatator

#### functions for numbers, strings, I/O

- print(”..”)
- length()
- substr()
- gsub()
- index()
- tolower()
- toupper()

if (condition) command_statement; else onother_command_statement

`BEGIN { FS = ",", IGNORECASE = 1 } { if($0 ~ "<search text>"} print $2 } ' <target file>  |  sed 's/search text /replace text/'

while (condition) command_statement 

do command _statement  while (condition)

Ví dụ awk:

```bash
awk 'BEGIN { print("Hello world !") }'
```