# Advance C
<h3>📕 SUMMARY </h3>
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

    > Dùng câu lệnh gcc -o program main.c utils.c

## II. MACRO và chỉ thị tiền xử lý
  ### 1. Macro:
 - Macro là một tính năng của compiler trong c, nó dùng để thay thế 1 giá trị hay đoạn mã được định nghĩa bằng #define ở preprocessing. Nó không tốn bộ nhớ hay thời gian chạy, do quá trình này diễn ra trước khi biên dịch
 - Syntax: #define TÊN_MACRO nội_dung_thay_thế
 - Macro không kiểm tra kiểu dữ liệu như hàm

  ### 2. Các chỉ thị tiền xử lý:
  #### 2.1. #include:
  - #include còn gọi là chỉ thị bao gồm tệp. Chỉ thị #include dùng để chèn nội dung của một file vào mã nguồn chương trình.
  - Có chức năng tái sử dụng mã nguồn và phân chia chương trình thành các phần nhỏ, giúp quản lý mã nguồn hiệu quả
  - #include dùng dấu < > (ví dụ: #include <stdio.h>) dùng để include 1 thư viện chuẩn của c
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
    #define MAX(x, y) ((x) > (y) ? (x) : (y))
    
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
                        GPIOA->BSRR = (1 << pin);  // Đặt bit tương ứng để thiết lập chân
                    } 
                    else {
                        GPIOA->BSRR = (1 << (pin + 16));  // Đặt bit tương ứng để reset chân
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
                if (numArgs > 2) {
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
                if (numArgs > 2) {
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

  > Output: Assertion failed: b != 0 && "Mau phai khac 0", file main.c, line 6.

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












  
