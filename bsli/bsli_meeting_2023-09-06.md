# BSLI Meeting 2023-09-06

## Context
- we have a good plan for the flight computer architecture
- still need to decide:
    - how arming/disarming will work
    - how saving sensor data to an SD card will work
    - how the camera will work
    - how to do unit testing

## Conventions

### Code conventions
- prefixing things with `fc_`
- try to keep code outside of generated files
- every function should be documented
- NASA's Power of 10
    - use `assert()`
    - keep it simple, this isn't code golf
    - avoid macros/preprocessor
    - hard upper limit on loops
- 

### Git/GitHub workflow
- feature branches
- pull requests and code reviews
- branch protections
- many small commits > one big commit
- extended commit messages
- we might want to try moving to a fork-and-pr workflow instead of a branch-and-pr workflow
    - then we can set the default repo permissions to "read" instead of "write"

### STM32CubeIDE use
- don't change project settings without documenting what you do
    - check `git status` to make sure you didn't accidentally change something

#### Lab: Blinking an LED
1. install STM32CubeIDE
2. create new project:
    1. select correct chip (commercial part number: STM32F302R8T6)
3. configure chip (no FreeRTOS for now):
    1. configure pins (right click to give then names):
        1. `PC13` as `GPIO_Input` ("`B1`")
        2. `PB13` as `GPIO_Output` ("`LD2`")
4. in the while loop:
    1. `if (HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin) == GPIO_PIN_RESET) {}`
        1. follow definition of `HAL_GPIO_ReadPin()` to figure out what `PinState` is.
    2. `HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_SET);`
5. build + run

#### Lab: Sending and receiving UART data
1. install PuTTY
2. configure chip:
    1. configure pins:
        1. `PA2` as `USART2_TX` ("`USART2_TX`")
        2. `PA3` as `USART2_RX` ("`USART2_RX`")
        3. (they should be orange, because we haven't configured the `USART2` peripheral yet)
    2. configure peripherals:
        1. set `Connectivity -> USART2 -> Mode` to `Asynchronous`
        2. set `Connectivity -> USART2 -> Parameter Settings -> Basic Parameters -> Baud Rate` to `9600`
        3. check the `Connectivity -> USART2 -> NVIC Settings -> USART2 global interrupt -> Enabled` checkbox
3. write code:
4. configure PuTTY:
    1. set `Session -> Connection type` to `Serial`
    2. set `Session -> Speed` to `9600`
    3. set `Session -> Serial line` to the serial port
        1. on linux it is `/dev/ttyACM0`
5. run and connect

```c
/* USER CODE BEGIN 4 */
uint8_t uart2_tx_buffer[1];
uint8_t uart2_rx_buffer[1];
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) {
    if (huart->Instance == USART2) {
        HAL_GPIO_TogglePin(LD2_GPIO_Port, LD2_Pin);
        /* TODO: Copy rx_buffer contents into external buffer */
        // HAL_UART_Transmit_IT(&huart2, uart2_rx_buffer, 1); /* echo back to sender (loopback mode) */
        
        HAL_UART_Receive_IT(&huart2, uart2_rx_buffer, 1);
    }
}
/* USER CODE END 4 */
```

#### Lab: FreeRTOS setup
1. configure chip:
    1. configure peripherals:
        1. set `System Core -> SYS -> Timebase Source` to `TIM6`
        2. set `Middleware and Software -> FREERTOS -> Interface` to `CMSIS_V2`
    2. configure tasks:
        1. configure `Middleware and Software -> FREERTOS -> Tasks and Queues`
2. you should see generated functions for your tasks

#### Lab: Reading from UART
- 

## Conclusion
- send my system 1 notes + other resources for learning C
- same for git
