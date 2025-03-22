# Advance C/Cpp
## I. Compiler
- Compiler là một chương trình dịch mã nguồn (source code) thành ngôn ngữ máy để thực thi trên máy tính.
  ### Quá trình biên dịch:
          C source (.c)  -->  Preprocessing (.i)  -->  Compilation (.s)  -->  Assembly (.o)  -->  Linking (Executable)
  #### 1. Preprocessing (tiền xử lý):
    - Xử lý các chỉ thị tiền xử lý (#include, #define, #ifdef...).
    - Xóa comment
    - Expand Marco: là quá trình thay thế macro (định nghĩa bằng #define) bằng giá trị hoặc đoạn mã tương ứng trong giai đoạn tiền xử lý (Preprocessing) trước khi biên dịch.

    ---> Dùng câu lệnh gcc -E program.c -o program.i
  
  #### 2. Compilation (Biên dịch):
    - Dịch mã nguồn .i thành mã Assembly .s

    ---> Dùng câu lệnh gcc -S program.c -o program.s

  #### 3. Assembly (Dịch Assembly):
    - Dịch mã Assembly .s thành mã máy (Object code) .o

    ---> Dùng câu lệnh gcc -o program program.s
  #### 4. Linking
    - Ghép nhiều file object .o và thư viện để tạo ra file thực thi .exe hoặc .out

    ---> Dùng câu lệnh gcc -o program main.c utils.c

## II. MACRO và chỉ thị tiền xử lý
  ### 1. Macro:
 - Macro là một tính năng của compiler trong c, nó dùng để thay thế 1 giá trị hay đoạn mã được định nghĩa bằng #define ở preprocessing. Nó không tốn bộ nhớ hay thời gian chạy, do quá trình này diễn ra trước khi biên dịch
 -  Syntax: #define TÊN_MACRO nội_dung_thay_thế
 -  Macro không kiểm tra kiểu dữ liệu như hàm

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
      - Khi làm việc với macro và cấu hình (muốn bật tắt tính năng mà k phải sửa code nhiều)
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



  
