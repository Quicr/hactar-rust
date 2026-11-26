# bare-rust

Bare rust project for STM32

Read the [Architecture.md](Architecture.md) document for more
information.

# Instalations

```sh
rustup update
rustup target add thumbv6m-none-eabi
rustup target add thumbv7em-none-eabihf
```

```sh
brew install probe-rs-tools
or
cargo install probe-rs-tools 
```

# Run local

```sh
cargo run --bin ui --no-default-features --features bsp/board-sim,hal/stm32f405,hal/std,bsp/std,ui/std,ui/exit
```

# Build all

```sh
make
```


# Running on Board

Connect up the stlink to board.

```sh
make flash
```

or 

```sh
cd crates/ui; cargo flash --chip STM32F405RG --bin ui --no-default-features --features bsp/board-hactar12,hal/stm32f405 --target=thumbv7em-none-eabihf
````


In a separate window, on a mac, can monitor USB console with

```sh
screen /dev/tty.usbserial<SOOMETHING> 115200
```

To exit screen, type ^A^\


# Running with GDB

From in the main directory, start

```sh
openocd -f crates/ui/openocd.cfg
```

and leave running. Then in another window do

```sh
cd crates/ui; cargo run --bin ui --no-default-features --features bsp/board-hactar12,hal/stm32f405 --target=thumbv7em-none-eabihf
```

or for the MGMT chip

from in the main directory, start

```sh
openocd -f crates/mgmt/openocd.cfg
```

```shell
cd crates/mgmt; cargo run --bin mgmt  --no-default-features --features hal/stm32f072 --target=thumbv6m-none-eabi
```
this will bring you to gdb prompt where you can type "c" to continue.

Can flash the mgmt chip with (make sure openocd is not running)

```shell
cd crates/mgmt; cargo flash --chip STM32F072CB --bin mgmt --no-default-features --features hal/stm32f072 --target=thumbv6m-none-eabi --release
```



# Notes

Useful things to look at size of binary:

```aiignore
cd crates/ui
cargo build  --release  --no-default-features --features bsp/board-hactar12,hal/stm32f405 --target=thumbv7em-none-eabihf 
arm-none-eabi-size ../../target/thumbv7em-none-eabihf/release/ui 
cargo nm --release -- -a -n

cargo objdump --release  --bin ui --no-default-features --features bsp/board-hactar12,hal/stm32f405 --target=thumbv7em-none-eabihf  --  --disassemble-symbols="ui::fib::fib::h66fa5b0d20d9db18"  --source   --demangle

cargo objdump  --bin ui --no-default-features --features bsp/board-hactar12,hal/stm32f405 --target=thumbv7em-none-eabihf  --  --disassemble-symbols="ui::fib::fib::hc749cb1bc8cd917c"  --source   --demangle

```
