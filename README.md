# Software UART

This is an asynchronus software UART library designed for ATTiny MCUs.  
Currently it's TX only.


## Functions

Transmit a single character (blocking until queued)
``` c
void uart_putc(const char c);
```

Try to transmit a single character, if UART is Busy, return UART_BUSY
```c
uart_error_t uart_try_queue(const char c);
```

Transmit a series of characters (blocking until queued)
```c
void uart_puts(const char *s, size_t len);
```

Try to transmit a string, if UART is Busy, return UART_BUSY
```c
uart_error_t uart_try_puts(const char *s, size_t len);
```

Initialize UART
```c
void uart_init();
```

## Example

``` c
#inlcude "software_uart/uart.h"

int main()
{
        uart_error_t ret;

        uart_init();

        /* Queue a message (this will block until the message was queued) */
        uart_puts("Hello, Universe!", 16);

retry:
        /* Queue a message if possible */
        ret = uart_try_puts("Hello, Universe?", 16);
        if(ret == UART_BUSY) {
                /* Do other stuff.
                 * Interrupts shall not be disabled at the time
                 * to ensure a valid transmission */
                goto retry;
        }

        return 0;
}
```

## Header configuration

The PORT register for UART
```c
#define UART_PORT PORTB
```

The DDR register for UART
```c
#define UART_DDR DDRB
```

The bit position for the UART tx pin
```c
#define UART_TX_BIT PB1
```

The UART baudrate (a sane value for 1MHz would be 4800 baud)
```c
#define UART_BAUDRATE 9600
```

Uncomment if parity bit shall be sent.
1 = odd parity
0 = even parity
```c
// #define UART_PARITY 0
```

Buffer size for outgoing bytes (number of queue slots)
``` c
#define UART_BUFFER_SIZE 64
```
