# Advance C
<h2>📕 SUMMARY </h2>

<details>
  <summary><font size="10"><b>📑 COMPILER - MACRO </b></front></summary>

  ---

## I. Compiler
- Compiler là một chương trình dịch mã nguồn (source code) thành ngôn ngữ máy để thực thi trên máy tính.
  ### Quá trình biên dịch:
          C source (.c)  -->  Preprocessing (.i)  -->  Compilation (.s)  -->  Assembly (.o)  -->  Linking (Executable)
  #### 1. Preprocessing (tiền xử lý):
    - Xử lý các chỉ thị tiền xử lý (#include, #define, #ifdef...).
    - Xóa comment
    - Expand Marco: là quá trình thay thế macro (định nghĩa bằng #define) bằng giá trị hoặc đoạn mã tương ứng trong giai đoạn tiền xử lý (Preprocessing) trước khi biên dịch.

    > Dùng câu lệnh gcc -E program.c -o program.i
  
  #### 2. Compilation (Biên dịch):
    - Dịch mã nguồn .i thành mã Assembly .s

    > Dùng câu lệnh gcc -S program.i -o program.s

  #### 3. Assembly (Dịch Assembly):
    - Dịch mã Assembly .s thành mã máy (Object code) .o

    > Dùng câu lệnh gcc -c program.s -O program.o
  #### 4. Linking
    - Ghép nhiều file object .o và thư viện để tạo ra file thực thi .exe hoặc .out

     Dùng câu lệnh gcc -o program main.c utils.c

## II. MACRO và chỉ thị tiền xử lý
  ### 1. Macro:
 - Macro là một tính năng của compiler trong c, nó dùng để thay thế 1 giá trị hay đoạn mã được định nghĩa bằng #define ở preprocessing. Nó không tốn bộ nhớ hay thời gian chạy, do quá trình này diễn ra trước khi biên dịch
 - Syntax: #define TÊN_MACRO nội_dung_thay_thế
 - Macro không kiểm tra kiểu dữ liệu như hàm

  ### 2. Các chỉ thị tiền xử lý:
  #### 2.1. #include:
  - #include còn gọi là chỉ thị bao gồm tệp. Chỉ thị #include dùng để chèn nội dung của một file vào mã nguồn chương trình.
  - Có chức năng tái sử dụng mã nguồn và phân chia chương trình thành các phần nhỏ, giúp quản lý mã nguồn hiệu quả
  - #include dùng dấu <  (ví dụ: #include <stdio.h) dùng để include 1 thư viện chuẩn của c
  - #include dùng dấu " " (ví dụ: #include "utilities.h") dùng để include 1 file tự viết trong thư mục hiện tại

  #### 2.2. #define:
  - #define là chỉ thị định nghĩa, dùng để định nghĩa tên để thay thế cho giá trị, hàm, mảng, ...
  
  _Ex: dùng #define cho 1 value_
  ```c
    #include <stdio.h>
    // Định nghĩa hằng số Pi sử dụng #define
    #define PI 3.14
    int main() {
        // Sử dụng hằng số Pi trong chương trình
        double radius = 5.0;
        double area = PI * radius * radius;
    
        printf("Radius: %.2f\n", radius);
        printf("Area of the circle: %.2f\n", area);
    
        return 0;
    }
  ```

  _Ex: dùng #define cho 1 hàm_
  ```c
    #include <stdio.h>
    
    // Macro để tính bình phương của một số
    #define SQUARE(x) ((x) * (x))
    
    int main() {
        // Sử dụng macro để tính bình phương của num
        int result = SQUARE(5);
    
        printf("Result is: %d\n", result);
    
        return 0;
    }
  ```

  - Cần đặt dấu () để tránh lỗi toán tử
  ```c
    #include <stdio.h>
    
    // Định nghĩa macro để tìm số lớn hơn giữa hai số
    #define MAX(x, y) ((x)  (y) ? (x) : (y))
    
    int main() {
        int a = 10, b = 20;
        
        // Sử dụng macro để tìm số lớn hơn giữa a và b
        int maxNumber = MAX(a, b);
    
        printf("The bigger number between %d and %d is: %d\n", a, b, maxNumber);
    
        return 0;
    }
  ```

  - Đổi với #define cần nhiều hàng thì dùng kí tự '\' ở cuối dòng
  ```c
    #include <stdio.h>

    #define CREATE_FUNC(name, cmd) \
    void name()                    \
    {                              \
        print(cmd)                 \
    }
    
    int main() {
        CREATE_FUNC(test1, "this is ....\n");
        test1();
        return 0;
    }
  ```

  #### 2.3. #undef:
  - Chỉ thị #undef dùng để hủy định nghĩa của một macro đã được định nghĩa trước đó bằng #define

  _Ex:_
  ```c
    #include <stdio.h>
    
    // Định nghĩa SENSOR_DATA 
    #define SENSOR_DATA 42
    
    int main() {
        printf("Value of MY_MACRO: %d\n", MY_MACRO);
    
        // Hủy định nghĩa SENSOR_DATA 
        #undef SENSOR_DATA 
        // Định nghĩa SENSOR_DATA 
        #define SENSOR_DATA 50
    
        printf("Value of MY_MACRO: %d\n", MY_MACRO);
    
        return 0;
    }
  ```

  #### 2.4. #if, #elif, #else:
  - **#if** sử dụng để bắt đầu một điều kiện tiền xử lý.
  - Nếu điều kiện trong **#if** là đúng, các dòng mã nguồn sau **#if** sẽ được biên dịch
  - Nếu sai, các dòng mã nguồn sẽ bị bỏ qua đến khi gặp **#endif**
  - #**elif** dùng để thêm một điều kiện mới khi điều kiện trước đó trong **#if** hoặc **#elif** là sai
  - **#else** dùng khi không có điều kiện nào ở trên đúng.
  - Dùng **#if, #elif, #else** khi:
      - Muốn trình biên dịch có điều kiện (ví dụ muốn chạy trên Win hay Linux)
      - Khi làm việc với macro và cấu hình (muốn bật tắt tính năng mà k phải sửa code nhiều lần)
      - Khi tối ưu hóa code để chạy trên nhiều môi trường khác nhau (x86 hoặc ARM)

  _Ex:_
  ```c
    #include <stdio.h>
    
    typedef enum
    {
        GPIOA,
        GPIOB,
        GPIOC
    } Ports;
    
    typedef enum
    {
        PIN1,
        PIN2,
        PIN3,
        PIN4,
        PIN5,
        PIN6,
        PIN7,
    } Pins;
    
    typedef enum
    {
        HIGH,
        LOW
    } Status;
    
    #define STM32 0
    #define ATMEGA 1
    #define PIC 2
    
    #define MCU STM32
    
    #if MCU == STM32
    void daoTrangThaiDen(Ports port, Pins pin, Status status)
    {
        if (status == HIGH)
        {
            HAL_GPIO_WritePin(port, pin, LOW);
        }
        else
        {
            HAL_GPIO_WritePin(port, pin, HIGH);
        }  
    }
    #elif MCU == ATMEGA
    void daoTrangThaiDen(Pins pin, Status status)
    {
        if (status == HIGH)
        {
            digitalWrite(pin, LOW);
        }
        else
        {
            digitalWrite(pin, HIGH);
        }  
    }
    
    #endif
    
    void delay(int ms)
    {
    
    }
    
    
    int main()
    {
        while(1)
        {
            daoTrangThaiDen(GPIOA,13,HIGH);
            delay(1000);
        }
    
        return 0;
    }
  ```

  - Dùng chỉ thị #if, #elif, #else trong hàm main, không dùng if, elif và else trong hàm main trong trường hợp này do:
    - Dùng #if giúp chỉ biên dịch phần code cần thiết, tránh dư thừa, tối ưu chương trình.
    - Dùng if sẽ làm chương trình chậm hơn, nặng hơn do vẫn giữ tất cả mã nguồn trong file biên dịch.

  _Ex:_
  ```c
    #include <stdio.h>
    
    typedef enum{
        LOW,
        HIGH
    } Status;
    
    typedef enum{
        PIN0,
        PIN1,
        PIN2,
        PIN3,
        PIN4,
        PIN5,
        PIN6,
        PIN7,
    } Pin;
    
    #define ESP32      1
    #define STM32      2
    #define ATmega324  3
    
    #define MCU STM32
    
    int main(int argc, char const *argv[])
    {
        while(1){
            #if MCU == STM32
                void digitalWrite(Pin pin, Status state) {
                    if (state == HIGH){
                        GPIOA-BSRR = (1 << pin);  // Đặt bit tương ứng để thiết lập chân
                    } 
                    else {
                        GPIOA-BSRR = (1 << (pin + 16));  // Đặt bit tương ứng để reset chân
                    }
                }
    
            #elif MCU == ESP32
                void digitalWrite(Pin pin, Status state) {
                    if (state == HIGH) {
                        GPIO.out_w1ts = (1 << pin);  // Đặt bit tương ứng để thiết lập chân HIGH
                    } 
                    else {
                        GPIO.out_w1tc = (1 << pin);  // Đặt bit tương ứng để reset chân LOW
                    }
                }
    
            #else
                void digitalWrite(Pin pin, Status state) {
                    if (state == HIGH) {
                        PORTA |= (1 << pin);  // Đặt bit tương ứng để thiết lập chân HIGH
                    } 
                    else {
                        PORTA &= ~(1 << pin);  // Xóa bit tương ứng để reset chân LOW
                    }
                }
                
            #endif
        }
        return 0;
    }
  ```

  #### 2.5. #ifdef, #ifndef:
  - #ifdef dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro đã được định nghĩa thì mã nguồn sau #ifdef sẽ được biên dịch.
  - #ifndef dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro chưa được định nghĩa thì mã nguồn sau #ifndef sẽ được biên dịch
  - Dùng #ifdef cũng để tránh trường hợp khi 1 file #include nhiều lần gây ra lỗi biên dịch như ví dụ sau sẽ gặp lỗi define nhiều lần
  
  _Ex:_

  file abc.txt:

  ```c
      #ifndef __ABC_H
      #define __ABC_H
      
      int a = 10;
      
      #endif
  ```

  file main.c:

  ```c
    #include <stdio.h>
    
    #include "abc.txt"
    #include "abc.txt"
    #include "abc.txt"
    
    
    int main()
    {
        printf("Hello \n");
        
        return 0;
    }
  ```

  _Ex: kiểm tra file include nhiều lần bằng **Header Guard**_

  ```c
    #ifndef TEST_H
    #define TEST_H ...
  ```

  _Có cách đơn giản hơn là dùng #pragma once_

  ### 3. Các toán tử tiền xử lý:

  #### 3.1. Toán tử stringize "#":toán tử này cho phép chuyển đổi các tham số thành chuỗi

  _Ex:_

  ```c
    #include <stdio.h>
    
    #define STRINGIZE(x) #x
    #define DATA 40
    
    int main() {
    
        // Sử dụng toán tử #
        printf("The value is: %s\n", STRINGIZE(DATA));
    
        return 0;
    }
  ```

  #### 3.2. Toán tử token pasting "##" : toán tử nối 2 token lại với nhau

  _Ex:_

  ```c
    #include <stdio.h>
    
    #define CREATE_VAR(name, num) int name##num = num;
    
    int main() {
        CREATE_VAR(var, 1)  // Tạo ra int var1 = 1;
        CREATE_VAR(var, 2)  // Tạo ra int var2 = 2;
    
        printf("%d, %d\n", var1, var2);  //output: 1, 2
        return 0;
    }
  ```

  #### 3.3. Toán tử variadic: Dùng cho hàm chưa biết số lượng tham số truyền vào

  - Syntax: #define MACRO_NAME(...) macro_expansion(__VA_ARGS__)
    ... đại diện danh sách đối số
    __VA_ARGS__ đại diện cho tất cả các tham số truyền vào sau dấu ...

  ```c
    #include <stdio.h>
    
    #define LOG(fmt, ...) printf("[LOG] " fmt "\n", __VA_ARGS__)
    
    int main() {
        LOG("Hello, %s!", "World");  // printf("[LOG] Hello, %s!\n", "World");
        LOG("Sum: %d + %d = %d", 5, 10, 5 + 10);
        return 0;
    }
  ```

  - fmt: Chuỗi format (ví dụ: "[LOG] " fmt "\n").
  - __VA_ARGS__: Các tham số còn lại truyền vào printf.


  _##__VA_ARGS__ Variadic Macro không cần đối số:  Dấu ##__VA_ARGS__ giúp tránh lỗi nếu không có tham số nào truyền vào._
  
  ```c
    #include <stdio.h>
    
    // Định nghĩa macro DEBUG_PRINT với __VA_ARGS__
    #define DEBUG_PRINT(fmt, ...) printf("[DEBUG] " fmt "\n", ##__VA_ARGS__)
    
    int main() {
        int x = 10, y = 20;
    
        // In chuỗi đơn giản
        DEBUG_PRINT("Program started");
    
        // In biến với format string
        DEBUG_PRINT("Value of x: %d", x);
        DEBUG_PRINT("Sum of x and y: %d + %d = %d", x, y, x + y);
    
        return 0;
    }
  ```
    
</details>

  ---

<details>
  <summary><font size="10"><b>📑 STDARG - ASSERT </b></front></summary>
  
  ---

## I. Thư viện stdarg

  - Cung cấp các phương thức để làm việc với các hàm có số lượng input parameter không cố định (như printf, scanf, ...)
  - Các phương thức như:
    | **Macro**                           | **Mô tả** |
    |-------------------------------------|-----------|
    | `va_list`                           | Kiểu dữ liệu khai báo một biến cho list các đối số |
    | `va_start(va_list, last_fixed_arg)`       | Khởi tạo danh sách đối số, nhận vào 2 tham số là biến **va_list** được khai báo ở trên và **last_fixed_arg** là tên của đối số cuối cùng có kiểu cố định trước danh sách đối số không cố định |
    | `va_arg(va_list, type)`             | Lấy giá trị của đối số tiếp theo trong danh sách, có kiểu type |
    | `va_end(va_list)`                   | Kết thúc việc sử dụng list đối số biến đổi (cần gọi trước khi kết thúc hàm) |
    | `va_copy(arg2, arg1)`               | Dùng để copy dữ liệu cùng kiểu va_list (copy arg1 gán cho arg2)  |

  _Ex:_

  ```c
    #include <stdarg.h>
    #include <stdio.h>
    
    // Hàm tính tổng các số
    int sum(int count, ...) {  //count dùng để xác định số lượng tham số
        va_list args;  // Khai báo biến danh sách đối số
        va_start(args, count);  // Khởi tạo danh sách, count là đối số cuối cùng có kiểu cố định giúp xác định vị trí của danh sách đối số biến đổi.
        int total = 0;
    
        for (int i = 0; i < count; i++) {
            total += va_arg(args, int);  // Lấy từng đối số và cộng vào tổng
        }
    
        va_end(args);  // Kết thúc danh sách đối số
        return total;
    }
    
    int main() {
        printf("Tổng: %d\n", sum(3, 10, 20, 30)); // Kết quả: 60
        printf("Tổng: %d\n", sum(5, 1, 2, 3, 4, 5)); // Kết quả: 15
        return 0;
    }
  ```

  _Ex: kiểu struct_

  ```c
    #include <stdio.h>
    #include <stdarg.h>
    
    
    typedef struct Data
    {
        int x;
        double y;
    } Data;
    
    void display(int count, ...) {
    
        va_list args;
    
        va_start(args, count);
    
        int result = 0;
    
        for (int i = 0; i < count; i++)
        {
            Data tmp = va_arg(args,Data);
            printf("Data.x at %d is: %d\n", i,tmp.x);
            printf("Data.y at %d is: %f\n", i,tmp.y);
        }
       
    
        va_end(args);
    
    
    }
    
    int main() {
    
    
        display(3, (Data){2,5.0} , (Data){10,57.0}, (Data){29,36.0});
        return 0;
    }
  ```

  _Ex: không có số lượng tham số truyền vào như ở ví dụ trên_

  ```c
    #include <stdio.h>
    #include <stdarg.h>
    
    typedef enum {
        TEMPERATURE_SENSOR,
        PRESSURE_SENSOR
    } SensorType;
    
    void processSensorData(SensorType type, ...) {  //SensorType type là tham số cố định để va_start hoạt động, nó không nhất thiết phải là int count
        va_list args;
        va_start(args, type);
    
        switch (type) {
            case TEMPERATURE_SENSOR: {
                int numArgs = va_arg(args, int);
                int sensorId = va_arg(args, int);
                float temperature = va_arg(args, double); // float được promote thành double
                printf("Temperature Sensor ID: %d, Reading: %.2f degrees\n", sensorId, temperature);
                if (numArgs  2) {
                    // Xử lý thêm tham số nếu có
                    char* additionalInfo = va_arg(args, char*);
                    printf("Additional Info: %s\n", additionalInfo);
                }
                break;
            }
            case PRESSURE_SENSOR: {
                int numArgs = va_arg(args, int);
                int sensorId = va_arg(args, int);
                int pressure = va_arg(args, int);
                printf("Pressure Sensor ID: %d, Reading: %d Pa\n", sensorId, pressure);
                if (numArgs  2) {
                    // Xử lý thêm tham số nếu có
                    char* unit = va_arg(args, char*);
                    printf("Unit: %s\n", unit);
                }
                break;
            }
        }
    
        va_end(args);
    }
    
    int main() {
        processSensorData(TEMPERATURE_SENSOR, 2, 1, 36.5, "Room Temperature");
        processSensorData(PRESSURE_SENSOR, 2, 2, 101325);
        return 0;
    }
  ```

  **NOTE:**

  - Các tham số truyền vào phải có cùng kiểu dữ liệu, nếu không có thể gây lỗi undefined behavior
  - Có thể không cần truyền tham số xác định số lượng đối số cần truyền vào nếu biết được chính xác số lượng cần truyền là bao nhiêu

## II. Thư viện assert
  - Cung cấp macro assert để kiểm tra một điều kiện. 
  - Nếu điều kiện đúng (true), không có gì xảy ra và chương trình tiếp tục thực thi.
  - Nếu điều kiện sai (false), chương trình dừng lại và thông báo một thông điệp lỗi.
  - Dùng trong debug, dùng **#define NDEBUG** để tắt debug

  _Ex:_

  ```c
    #include <stdio.h>
    #include <assert.h>
    
    void divide(int a, int b) {
        assert(b != 0 && "Mau phai khac 0");  // Kiểm tra b có khác 0 không
        printf("Result: %d\n", a / b);
    }
    
    int main() {
        int x = 10, y = 0;
        divide(x, 2);  // Hợp lệ, in kết quả
        divide(x, y);  // Lỗi: assert(b != 0) sẽ kích hoạt lỗi và dừng chương trình
    
        return 0;
    }
  ```

   Output: Assertion failed: b != 0 && "Mau phai khac 0", file main.c, line 6.

  - Hoặc có thể dùng #define như sau:

  ```c
    #include <stdio.h>
    #include <assert.h>

    #define LOG(condition, cmd) assert(condition && #cmd)  // '#' dùng để biến thành chuỗi
    
    void divide(int a, int b) {
        LOG(b != 0, Mau phai khac 0);  // Kiểm tra b có khác 0 không
        printf("Result: %d\n", a / b);
    }
  ```

</details>

  ---

<details>
  <summary><font size="10"><b>📑 BITMASK </b></front></summary>
  
  ---

</details>

  ---

<details>
  <summary><font size="10"><b>📑 POINTER </b></front></summary>
  
  ---

## I. Khái niệm Pointer
  - Con trỏ (pointer) là một biến chứa địa chỉ bộ nhớ của một đối tượng khác (biến, mảng, hàm). 
  - Sử dụng con trỏ để thao tác trên bộ nhớ linh hoạt hơn.
  - Kích thước của con trỏ sẽ phụ thuộc vào kiến trúc máy tính, trình biên dịch hoặc kiến trúc vi xử lý (máy tính 64bit thì kích thước con trỏ là 8 byte)

## II. Cách lưu trữ của con trỏ
  - Trong hệ thống máy tính, dữ liệu được lưu trữ theo dạng bit và byte
  - LSB (Least Significant Bit) và MSB (Most Significant Bit):
    - LSB: Bit có trọng số nhỏ nhất (ít quan trọng nhất), thường là bit ngoài cùng bên phải trong hệ thống số nhị phân.
    - MSB: Bit có trọng số lớn nhất (quan trọng nhất), thường là bit ngoài cùng bên trái trong hệ thống số nhị phân.
  
    _Ex:_
      0b1011 0101 ====== ở đây LSB sẽ là bit 0 (giá trị là 0b1), MSB là bit 8 (giá trị là 0b1)
  - Endianness gồm có:
    - Little-Endian: LSB (byte) được lưu ở địa chỉ thấp nhất (phổ biến hiện nay).

    _Ex:_
    
      | **Address**  | **Giá trị (Hex)** | **Ghi chú** |
      |-----------|------------------|------------|
      | `0x1000`  | `78`             | *(LSB - Byte ít quan trọng nhất)* |
      | `0x1001`  | `56`             | |
      | `0x1002`  | `34`             | |
      | `0x1003`  | `12`             | *(MSB - Byte quan trọng nhất)* |

    - Big-Ediant: MSB (byte) được lưu ở địa chỉ thấp nhất.

    _Ex:_

      | **Address**  | **Giá trị (Hex)** | **Ghi chú** |
      |-----------|------------------|------------|
      | `0x1000`  | `12`             | *(MSB - Byte quan trọng nhất)* |
      | `0x1001`  | `34`             | |
      | `0x1002`  | `56`             | |
      | `0x1003`  | `78`             | *(LSB - Byte ít quan trọng nhất)* |

  int var = 10 ===> có kích thước bộ nhớ 4 byte (Address: **0x01 0x02 0x03 0x04**)
  
  int* ptr = &var ===> có kích thước 8 byte (Win 64bit), ví dụ như:
  
      Address:  0xc1 0xc2 0xc3 0xc4 0xc5 ... 0xc8
      
      Value:    0x01 0x02 0x03 0x04 0x00 ... 0x00 (4 byte còn lại không có giá trị lưu nên là 0x00)

  ## III. Cách sử dụng con trỏ
  
  _Ex: truyền con trỏ vào 1 hàm_

  ```c
    #include <stdio.h
    
    void swap(int* a, int* b)
    {
      int tmp = *a;
      *a = *b;
      *b = tmp;
    }
    
    int main()
    {
      int a = 10, b = 20;
      swap(&a, &b);
      printf("valuw a is: %d\n", a);
      printf("valuw b is: %d\n", b);
      return 0;
    }
  ```

  - Nếu muốn thay đổi giá trị thông quan 1 hàm thì phải sử dụng con trỏ, vì khi truyền vào hàm là 1 biến thông thường thì       nó sẽ sao chép giá trị của biến (nghĩa là sẽ tạo ra 1 địa chỉ khác). Do đó để thay đổi giá trị biến thông qua hàm phải      dùng con trỏ để thao tác trên dịa chỉ của biến đó

   _Ex: dùng con trỏ thao tác với mảng_

  ```c
    #include <stdio.h
    
    int main()
    {
      int arr[] = {1, 2, 3, 4, 5};
      int n = (sizeof(arr)/sizeof(arr[0]));  //lấy số lượng phần tử trong mảng
      int* ptr = arr;  //arr chính là &arr[0]
    
      for(int i = 0; i < n; i++)
      {
        printf("Giá trị của arr[%d] là: %d, ở địa chỉ: %p\n", i, arr[i], ptr+i);  //ptr + i có nghĩa là ptr + i.sizeof(data_type)
      }
    }
  ```

  ## III. Các loại con trỏ đặc biệt

  ### 1. Void Pointer:
  
  - Void pointer là con trỏ dùng để trỏ tới địa chỉ mà tại đó không cần biết kiểu dữ liệu của giá trị mà địa chỉ đó đang lưu trữ
  - Void pointer giúp viết code linh hoạt hơn, phù hợp với lập trình tổng quát và xử lý dữ liệu động.
  - Void pointer còn dùng để tối ưu hóa bộ nhớ (vì dùng int*, hay float* sẽ bị phình bộ nhớ)
  - Dùng void pointer khi lấy giá trị phải ép kiểu
  - Syntax: **void* ptr**

  _Ex:_

  ```c
    #include <stdio.h
    
    int main()
    {
      void* ptr;
      int a = 10;
      double b = 6.5;

      ptr = &a;
      printf("Địa chỉ: %p - Value: %d\n", ptr, *(int*)ptr);  //phải ép về kiểu *int

      ptr = &b;
      printf("Địa chỉ: %p - Value: %f\n", ptr, *(double*)ptr);

      char arr[] = "Hello World";

      //Mảng con trỏ void
      void* ptr1[] = {&a, &b, arr};
      printf("Địa chỉ: %p - Value: %d\n", ptr1[0], *(int*)ptr1[0]);
      printf("Địa chỉ: %p - Value: %f\n", ptr1[1], *(double*)ptr1[1]);
      printf("Địa chỉ: %p - Value: %c\n", ptr1[2], *(char*)ptr1[2]+1);     

    return 0;
    }
  ```

  ### 2. Function Pointer:

  - Con trỏ hàm là một biến lưu địa chỉ của 1 hàm.
  - Con trỏ hàm cho phép truyền một hàm như đối số cho hàm khác, lưu địa chỉ hàm trong một cấu trúc dữ liệu, hoặc truyền hàm như một giá trị trả về từ hàm khác.
  - Syntax:

    >  <return_type> (*func_poiter)(<data_type_1>, <data_type_2>);

    >  func_point = name_func (hoặc &name_func)  //gán địa chỉ hàm cho con trỏ hàm

  - Để gọi hàm từ con trỏ hàm có thể dùng

    >  func_point()

    > hoặc (*func_point)()

  _Ex1:_

  ```c
    #include <stdio.h>
    
    void greetEnglish(){ printf("Hello!\n"); }
    void greetFrench(){ printf("Bonjour!\n"); }
    
    int main()
    {
        // Khai báo con trỏ hàm
        void (*ptrToGreet)();
    
        // Gán địa chỉ của hàm greetEnglish cho con trỏ hàm
        ptrToGreet = greetEnglish;
    
        // Gọi hàm thông qua con trỏ hàm
        ptrToGreet();  // In ra: Hello!
    
        // Gán địa chỉ của hàm greetFrench cho con trỏ hàm
        ptrToGreet = greetFrench;
    
        // Gọi hàm thông qua con trỏ hàm
        (*ptrToGreet)();  // In ra: Bonjour!
        return 0;
    }

  ```

  _Ex2:_

  ```c
    #include <stdio.h>
    
    void sum(int a, int b) { return a+b; }
    void subtract(int a, int b) { return a-b; }
    void multiple(int a, int b) { return a*b; }
    void devide(int a, int b) { return a/b; }
    
    ===========================Cách 1================================
    int main()
    {
      void (*calc)(int, int);
    
      calc = sum;
      calc(2,3);
    
      calc = subtract;
      calc(2,3);
    
      ...
    
      return 0;
    }
    
    ===========================Cách 2==================================
    int main()
    {
      void (*calc[])(int, int) = {sum, subtract, multiple, devide};
      calc[0](2,3);
      calc[1](2,3);
      ...
    
      return 0;
    }
    
    ===========================Cách 3==================================
    void calculate(void (*calc)(int, int), int a, int b)
    {
      calc(a, b);
    }
    
    int main()
    {
      calculate(sum, 2, 3);
      ...
    
      return 0;
    }
  ```

  - So sánh giữa việc gọi hàm bằng con trỏ hàm và gọi hàm thông thường:

  * Giống nhau:
    
    - Trong máy tính có thanh ghi program counter (PC). Khi ta khai báo 1 biến hay 1 hàm thì giá trị nó sẽ được gán cho 1 địa chỉ, ngoài ra         câu lệnh đó còn được gán cho 1 địa chỉ nằm trong thanh ghi PC. Do đó khi gọi hàm thông thường hay gọi hàm bằng con trỏ hàm đều gọi tại        địa chỉ trong PC
   
  * Khác nhau:

    - Khi gọi hàm bằng con trỏ hàm sẽ linh hoạt hơn so với gọi hàm thông thường, do gọi hàm bằng con trỏ hàm sẽ có thế thay đổi mục đích của         hàm như ví dụ 2 cách 3.

  ### 3. Pointer to Constant (con trỏ hằng):

  - Con trỏ hằng là con trỏ **không thể thay đổi giá trị** tại địa chỉ mà nó trỏ tới thông qua phép giải tham chiếu dereference (*) nhưng giá      trị tại địa chỉ đó có thể thay đổi

  - Syntax:

    > `<type>` const *ptr_const = &value;

    > hay const `<type>` *ptr_const = &value;

  - Ứng dụng để giữ lại dữ liệu trước đó mà không muốn thay đổi nó trong quá trình xử lý.

  _Ex:_

  ```c
    #include <stdio.h>
    
    int main()
    {
      int value = 5;
      int test = 8;
      int const *ptr_const = &value;
    
      printf("value: %d\n", *ptr_const);
    
      value = 9;
    
      printf("value: %d\n", *ptr_const);
    
      return 0;
    }
  ```

  ### 3. Constant Pointer (hằng con trỏ)

  - Hằng con trỏ là con trỏ trỏ đến địa chỉ không thể thay đổi. Nhưng giá trị tại đó có thể thay đổi được thông qua dereference.

  - Syntax:

    > <type> *const const_ptr = &value;

  - Ứng dụng khi chỉ muốn thao tác tại 1 địa chỉ cố định, hay 1 vị trí cố định

  _Ex:_
  ```c
    #include <stdio.h>
    
    int main()
    {
      int value = 5;
      int test = 10;
      int *const const_ptr  = &value;
    
      printf("value: %d\n", *const_ptr);
      *const_ptr = test;
      printf("value: %d\n", *const_ptr);
    
      **const_ptr = &test;  //wrong**
    
      return 0;
    }
  ```

  - Có thể vừa dùng con trỏ hằng và hằng con trỏ như sau:

    >   const int *const const_ptr  = &value;

  ### 4. NULL Pointer

  - Là con trỏ **không trỏ đến bất kì đối tượng** hoặc vùng nhớ cụ thể nào

  - Dùng để khai báo khi chưa sử dụng con trỏ đó ngay lập tức (tránh bị trình biên dịch gán cho 1 địa chỉ random)

  _Ex:_

  ```c
    #include <stdio.h>
    
    int main()
    {
        int *ptr = NULL;  // Gán giá trị NULL cho con trỏ 0x0000000
    
        if (ptr == NULL)
        {
            printf("Pointer is NULL\n");
        }
        else
        {
            printf("Pointer is not NULL\n");
        }
    
        int score_game = 5;
        if (ptr == NULL)
        {
            ptr = &score_game;
            *ptr = 30;
            ptr = NULL;
        }
        return 0;
    }

  ```

  ### 5. Pointer to Pointer

  - Pointer to Pointer (hay còn gọi là con trỏ cấp cao) là một kiểu dữ liệu mà cho phép lưu địa chỉ của 1 con trỏ khác.

  - Ví dụ:

    > int test = 5;              Address = 0x01; Value = 5

    > int *ptr = &test;          Address = &0xc2; Value = 0x01

    > int **ptp = &ptr;          Address = 0xee; Value = &0xc2

  - Ứng dụng trong kiểu dữ liệu JSON hay cấu trức dữ liệu danh sách liên kết
  
  _Ex:_
  ```c
    #include <stdio.h>
    
    int main()
    {
      int value = 41;
      int *ptr = &value;
      int **ptp = &ptr;
    
      /*
        **ptp = &ptr
        ptp = &ptr
        *ptp = ptr = &value
        **ptp = *ptr = value
    
        printf("address of value: %p\n", &value);
        printf("value of ptr: %p\n", ptr);
    
        printf("address of ptr: %p\n", &ptr);
        printf("value of ptrp: %p\n", ptp);
    
        printf("dereference ptp first time: %p\n", *ptp);
        printf("dereference ptp second time: %d\n", **ptp);
    
        return 0;
    }

  ```

</details>

  ---

<details>
  <summary><font size="10"><b>📑 STORAGE CLASS </b></front></summary>
  
  ---

</details>

  ---

<details>
  <summary><font size="10"><b>📑 GOTO - setjmp.h </b></front></summary>
  
  ---

  ## I. Goto

  - Goto là một từ khóa trong C, cho phép chương trình nhảy đến 1 label đã được đặt trước trong **cùng một hàm**.

  - Goto giúp kiểm soát flow của chương trình, nhưng nó làm cho source code trở nên khó đọc và bảo trì.

  _Ex:_

  ```c

#include <stdio.h>

int main(()
{
  int i = 0;

  //Đặt nhãn
  start:
    if (i >= 5)
    {
        goto end;  // chuyển control đến nhãn "end"
    }

    printf("%d", i);
    i++;

    goto start;  // chuyển control đến nhãn "start"

  end:
    printf("/n");

  return 0;
}
  ```

  *Kỹ thuật quét LED Matrix:

  - Vi điều khiển sẽ quét từng hàng trong LED Matrix, và sẽ bật những LED cần sáng trong hàng đó, sau đó nó sẽ tắt đi và quét hàng tiếp theo. Vì tốc độ xử lý của VĐK rất nhanh nên ta có cảm giác như nó sáng liên tục.

  ## II. Thư viện setjmp.h

  - setjmp.h là một thư viện trong C, gồm 2 hàm chính là setjmp và longjmp:

      - setjmp(jmp_buf env): Đánh dấu vị trí để có thể quay lại bằng longjmp

        > trả về 0 khi gọi lần đầu, và giá trị khác 0 khi quay lại từ longjmp

        - Cách thức hoạt động: biến jmp_buf là biến đặc biệt dùng để lưu giá trị là địa chỉ câu lệnh đặt setjmp do PC cung           cấp.

    - longjmp(jmp_buf env, int value): nhảy về vị trí hiện tại của setjmp và tiếp tục thực thi từ đó

      > Biến value là giá trị mà setjmp sẽ trả về ở lần gọi tiếp theo.
          
  - Dùng để thực hiện xử lý ngoại lệ trong C.

  _Ex:_
  
  ```c
  
    #include <stdio.h>
    #include <setjmp.h>
    
    jmp_buf buf;
    
    int exception = 0;
    
    void func2()
    {
        printf("This is function 2\n");
        longjmp(buf, 2);
    }
    
    void func3()
    {
        printf("This is function 3\n");
        longjmp(buf, 3);
    }
    
    void func1()
    {
        exception = setjmp(buf);
        if (exception == 0)
        {
            printf("This is function 1\n");
            printf("exception = %d\n", exception);
            func2();
        }
        else if (exception == 2)
        {
            printf("exception = %d\n", exception);
            func3();
        }
        else if (exception == 3)
        {
            printf("exception = %d\n", exception);
        }
    }
    
    int main(int argc, char const *argv[])
    {
        func1();
        return 0;
    }
  
  ```

  ## III. Exception Handling

  - Xử lý ngoại lệ giúp phát hiện và xử lý các lỗi hoặc tình huống bất thường xảy ra trong quá trình thực thi chương trình.

  #### 3.1. Exception

  - Ngoại lệ là những lỗi hoặc tình huống không mong muốn xảy ra, như:

    - Chia một số cho 0 (division by zero).

    - Truy cập mảng ngoài phạm vi (out of bounds array access).

    - Truy xuất con trỏ null (null pointer dereference).

    - Lỗi khi mở hoặc đọc tập tin (file not found).

    - Lỗi cấp phát bộ nhớ (bad allocation).
   
  #### 3.2. Cơ chế xử lý Exception

  - Hầu hết các ngôn ngữ lập trình hiện đại như C++, Java, Python, C# đều hỗ trợ xử lý ngoại lệ thông qua các từ khóa như:

    - **try:** Định nghĩa một khối lệnh có thể phát sinh lỗi.
   
    - **catch:** Xử lý ngoại lệ nếu có lỗi xảy ra.

    - **throw:** Ném ra một ngoại lệ khi xảy ra lỗi.
   
  - Nhưng trong C không có cơ chế này, do đó cần dùng thư viện setjmp.h

  _Ex:_

  ```c

    #include <stdio.h>
    #include <setjmp.h>
    
    jmp_buf buf;
    
    int exception_code;
    
    typedef enum
    {
        NO_ERROR,
        NO_EXIT,
        DIVIDE_BY_0
    } ErrorCodes;  
    
    #define TRY if ((exception_code = setjmp(buf)) == 0)
    #define CATCH(x) else if (exception_code == x)
    #define THROW(x) longjmp(buf, x)
    
    double divide(int a, int b)
    {
        if (a == 0 && b == 0)
        {
            THROW(NO_EXIT);
        }
        else if (b == 0)
        {
            THROW(DIVIDE_BY_0);
        }
    
        return (double)a/b;
    }
    
    int main(int argc, char const *argv[])
    {
        exception_code = NO_ERROR;
    
        TRY
        {
            printf("Ket qua: %0.3f\n", divide(0,0));
        }
        CATCH(NO_EXIT)
        {
            printf("ERROR! Không tồn tại\n");
        }
        CATCH(DIVIDE_BY_0)
        {
            printf("ERROR! Chia cho 0\n");
        }
    
        // thêm code ở đây
        printf("Hello world\n");
        return 0;
    }

  ```

  - So sánh Exception Handling and ASSERT:

    - ASSERT sẽ dừng chương trình ngay lập tức khi điều kiện sai
   
    - Exception Handling sẽ không dừng chương trình mà chỉ đưa ra báo lỗi và chạy tiếp

</details>








  
