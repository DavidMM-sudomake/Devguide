# 모듈 참고서: 명령어

## bl_update

소스 코드: [systemcmds/bl_update](https://github.com/PX4/Firmware/tree/master/src/systemcmds/bl_update)

파일에서 부트로더를 플래싱하는 유틸리티

### 사용법 {#bl_update_usage}

    bl_update [arguments...]
       setopt        Set option bits to unlock the FLASH (only needed if in locked
                     state)
    
       <file>        Bootloader bin file
    

## dumpfile

소스 코드: [systemcmds/dumpfile](https://github.com/PX4/Firmware/tree/master/src/systemcmds/dumpfile)

덤프 파일 유틸리티. 파일 크기와 내용을 표준 출력(터미널 창)에 바이너리 모드(LF를 CR LF로 바꾸지 않음)로 출력합니다.

### 사용법 {#dumpfile_usage}

    dumpfile [arguments...]
         <file>      File to dump
    

## dyn

소스 코드: [systemcmds/dyn](https://github.com/PX4/Firmware/tree/master/src/systemcmds/dyn)

### 설명

PX4 바이너리로 컴파일하지 않은 동적 PX4 모듈을 불러오고 실행합니다.

### 예시

    dyn ./hello.px4mod start
    

### 사용법 {#dyn_usage}

    dyn [arguments...]
         <file>      File containing the module
         [arguments...] Arguments to the module
    

## esc_calib

소스 코드: [systemcmds/esc_calib](https://github.com/PX4/Firmware/tree/master/src/systemcmds/esc_calib)

전동 변속기 보정 도구

보정 과정(명령을 실행하면 인터페이스 메시지로 안내해줌):

- 전동 변속기의 프로펠러를 제거하고 전원을 끄십시오
- 고도, 속도 컨트롤러의 동작을 중단하십시오: mc_rate_control stop, fw_att_control stop
- safety가 off인지 확인하십시오
- 이 명령어를 실행하십시오

### 사용법 {#esc_calib_usage}

    esc_calib [arguments...]
         [-d <val>]  Select PWM output device
                     values: <file:dev>, default: /dev/pwm_output0
         [-l <val>]  Low PWM value in us
                     default: 1000
         [-h <val>]  High PWM value in us
                     default: 2000
         [-c <val>]  select channels in the form: 1234 (1 digit per channel,
                     1=first)
         [-m <val>]  Select channels via bitmask (eg. 0xF, 3)
         [-a]        Select all channels
    

## failure

Source: [systemcmds/failure](https://github.com/PX4/Firmware/tree/master/src/systemcmds/failure)

### Description

Inject failures into system.

### Implementation

This system command sends a vehicle command over uORB to trigger failure.

### Examples

Test the GPS failsafe by stopping GPS:

failure gps off

### Usage {#failure_usage}

    failure [arguments...]
       help          Show this help text
    
       gps|...       Specify component
    
       ok|off|...    Specify failure type
    

## gpio

Source: [systemcmds/gpio](https://github.com/PX4/Firmware/tree/master/src/systemcmds/gpio)

This command is used to read and write GPIOs.

### Usage {#gpio_usage}

    gpio [arguments...]
       read
         <PORT> <PIN> GPIO port and pin
         [PULLDOWN|PULLUP] Pulldown/Pullup
         [--force]   Force (ignore board gpio list)
    
       write
         <PORT> <PIN> GPIO port and pin
         <VALUE>     Value to write
         [PULLDOWN|PULLUP] Pulldown/Pullup
         [--force]   Force (ignore board gpio list)
    

## hardfault_log

Source: [systemcmds/hardfault_log](https://github.com/PX4/Firmware/tree/master/src/systemcmds/hardfault_log)

Hardfault utility

Used in startup scripts to handle hardfaults

### Usage {#hardfault_log_usage}

    hardfault_log <command> [arguments...]
     Commands:
       check         Check if there's an uncommited hardfault
    
       rearm         Drop an uncommited hardfault
    
       fault         Generate a hardfault (this command crashes the system :)
         [0|1]       Hardfault type: 0=divide by 0, 1=Assertion (default=0)
    
       commit        Write uncommited hardfault to /fs/microsd/fault_%i.txt (and
                     rearm, but don't reset)
    
       count         Read the reboot counter, counts the number of reboots of an
                     uncommited hardfault (returned as the exit code of the program)
    
       reset         Reset the reboot counter
    

## i2cdetect

Source: [systemcmds/i2cdetect](https://github.com/PX4/Firmware/tree/master/src/systemcmds/i2cdetect)

Utility to scan for I2C devices on a particular bus.

### Usage {#i2cdetect_usage}

    i2cdetect [arguments...]
         [-b <val>]  I2C bus
                     default: 1
    

## led_control

Source: [systemcmds/led_control](https://github.com/PX4/Firmware/tree/master/src/systemcmds/led_control)

### 설명

Command-line tool to control & test the (external) LED's.

To use it make sure there's a driver running, which handles the led_control uorb topic.

There are different priorities, such that for example one module can set a color with low priority, and another module can blink N times with high priority, and the LED's automatically return to the lower priority state after the blinking. The `reset` command can also be used to return to a lower priority.

### Examples

Blink the first LED 5 times in blue:

    led_control blink -c blue -l 0 -n 5
    

### Usage {#led_control_usage}

    led_control <command> [arguments...]
     Commands:
       test          Run a test pattern
    
       on            Turn LED on
    
       off           Turn LED off
    
       reset         Reset LED priority
    
       blink         Blink LED N times
         [-n <val>]  Number of blinks
                     default: 3
         [-s <val>]  Set blinking speed
                     values: fast|normal|slow, default: normal
    
       breathe       Continuously fade LED in & out
    
       flash         Two fast blinks and then off with frequency of 1Hz
    
     The following arguments apply to all of the above commands except for 'test':
         [-c <val>]  color
                     values: red|blue|green|yellow|purple|amber|cyan|white, default:
                     white
         [-l <val>]  Which LED to control: 0, 1, 2, ... (default=all)
         [-p <val>]  Priority
                     default: 2
    

## listener

Source: [systemcmds/topic_listener](https://github.com/PX4/Firmware/tree/master/src/systemcmds/topic_listener)

Utility to listen on uORB topics and print the data to the console.

The listener can be exited any time by pressing Ctrl+C, Esc, or Q.

### Usage {#listener_usage}

    listener <command> [arguments...]
     Commands:
         <topic_name> uORB topic name
         [-i <val>]  Topic instance
                     default: 0
         [-n <val>]  Number of messages
                     default: 1
         [-r <val>]  Subscription rate (unlimited if 0)
                     default: 0
    

## mixer

Source: [systemcmds/mixer](https://github.com/PX4/Firmware/tree/master/src/systemcmds/mixer)

### Description

Load or append mixer files to the ESC driver.

Note that the driver must support the used ioctl's, which is the case on NuttX, but for example not on RPi.

### Usage {#mixer_usage}

    mixer <command> [arguments...]
     Commands:
       load
         <file:dev> <file> Output device (eg. /dev/pwm_output0) and mixer file
    
       append
         <file:dev> <file> Output device (eg. /dev/pwm_output0) and mixer file
    

## motor_ramp

Source: [systemcmds/motor_ramp](https://github.com/PX4/Firmware/tree/master/src/systemcmds/motor_ramp)

### Description

Application to test motor ramp up.

Before starting, make sure to stop any running attitude controller:

    mc_rate_control stop
    fw_att_control stop
    

When starting, a background task is started, runs for several seconds (as specified), then exits.

### Example

    motor_ramp sine -a 1100 -r 0.5
    

### Usage {#motor_ramp_usage}

    motor_ramp [arguments...]
         ramp|sine|square mode
         [-d <val>]  Pwm output device
                     default: /dev/pwm_output0
         -a <val>    Select minimum pwm duty cycle in usec
         [-b <val>]  Select maximum pwm duty cycle in usec
                     default: 2000
         [-r <val>]  Select motor ramp duration in sec
                     default: 1.0
    
     WARNING: motors will ramp up to full speed!
    

## motor_test

Source: [systemcmds/motor_test](https://github.com/PX4/Firmware/tree/master/src/systemcmds/motor_test)

Utility to test motors.

WARNING: remove all props before using this command.

### Usage {#motor_test_usage}

    motor_test <command> [arguments...]
     Commands:
       test          Set motor(s) to a specific output value
         [-m <val>]  Motor to test (1...8, all if not specified)
         [-p <val>]  Power (0...100)
                     default: 0
         [-t <val>]  Timeout in seconds (default=no timeout)
                     default: 0
         [-i <val>]  driver instance
                     default: 0
    
       stop          Stop all motors
    
       iterate       Iterate all motors starting and stopping one after the other
    

## mtd

Source: [systemcmds/mtd](https://github.com/PX4/Firmware/tree/master/src/systemcmds/mtd)

Utility to mount and test partitions (based on FRAM/EEPROM storage as defined by the board)

### Usage {#mtd_usage}

    mtd <command> [arguments...]
     Commands:
       status        Print status information
    
       start         Mount partitions
    
       readtest      Perform read test
    
       rwtest        Perform read-write test
    
       erase         Erase partition(s)
    
     The commands 'start', 'readtest', 'rwtest' and 'erase' have an optional
     parameter:
         [<partition_name1> [<partition_name2> ...]] Partition names (eg.
                     /fs/mtd_params), use system default if not provided
    

## nshterm

Source: [systemcmds/nshterm](https://github.com/PX4/Firmware/tree/master/src/systemcmds/nshterm)

Start an NSH shell on a given port.

This was previously used to start a shell on the USB serial port. Now there runs mavlink, and it is possible to use a shell over mavlink.

### Usage {#nshterm_usage}

    nshterm [arguments...]
         <file:dev>  Device on which to start the shell (eg. /dev/ttyACM0)
    

## param

Source: [systemcmds/param](https://github.com/PX4/Firmware/tree/master/src/systemcmds/param)

### 설명

Command to access and manipulate parameters via shell or script.

This is used for example in the startup script to set airframe-specific parameters.

Parameters are automatically saved when changed, eg. with `param set`. They are typically stored to FRAM or to the SD card. `param select` can be used to change the storage location for subsequent saves (this will need to be (re-)configured on every boot).

If the FLASH-based backend is enabled (which is done at compile time, e.g. for the Intel Aero or Omnibus), `param select` has no effect and the default is always the FLASH backend. However `param save/load <file>` can still be used to write to/read from files.

Each parameter has a 'used' flag, which is set when it's read during boot. It is used to only show relevant parameters to a ground control station.

### 예시

Change the airframe and make sure the airframe's default parameters are loaded:

    param set SYS_AUTOSTART 4001
    param set SYS_AUTOCONFIG 1
    reboot
    

### Usage {#param_usage}

    param <command> [arguments...]
     Commands:
       load          Load params from a file (overwrite all)
         [<file>]    File name (use default if not given)
    
       import        Import params from a file
         [<file>]    File name (use default if not given)
    
       save          Save params to a file
         [<file>]    File name (use default if not given)
    
       select        Select default file
         [<file>]    File name (use <root>/eeprom/parameters if not given)
    
       show          Show parameter values
         [-a]        Show all parameters (not just used)
         [-c]        Show only changed params (unused too)
         [-q]        quiet mode, print only param value (name needs to be exact)
         [<filter>]  Filter by param name (wildcard at end allowed, eg. sys_*)
    
       show-for-airframe Show changed params for airframe config
    
       status        Print status of parameter system
    
       set           Set parameter to a value
         <param_name> <value> Parameter name and value to set
         [fail]      If provided, let the command fail if param is not found
    
       compare       Compare a param with a value. Command will succeed if equal
         [-s]        If provided, silent errors if parameter doesn't exists
         <param_name> <value> Parameter name and value to compare
    
       greater       Compare a param with a value. Command will succeed if param is
                     greater than the value
         [-s]        If provided, silent errors if parameter doesn't exists
         <param_name> <value> Parameter name and value to compare
         <param_name> <value> Parameter name and value to compare
    
       touch         Mark a parameter as used
         [<param_name1> [<param_name2>]] Parameter name (one or more)
    
       reset         Reset only specified params to default
         [<param1> [<param2>]] Parameter names to reset (wildcard at end allowed)
    
       reset_all     Reset all params to default
         [<exclude1> [<exclude2>]] Do not reset matching params (wildcard at end
                     allowed)
    
       index         Show param for a given index
         <index>     Index: an integer >= 0
    
       index_used    Show used param for a given index
         <index>     Index: an integer >= 0
    
       find          Show index of a param
         <param>     param name
    

## perf

Source: [systemcmds/perf](https://github.com/PX4/Firmware/tree/master/src/systemcmds/perf)

Tool to print performance counters

### Usage {#perf_usage}

    perf [arguments...]
       reset         Reset all counters
    
       latency       Print HRT timer latency histogram
    
     Prints all performance counters if no arguments given
    

## pwm

Source: [systemcmds/pwm](https://github.com/PX4/Firmware/tree/master/src/systemcmds/pwm)

### Description

This command is used to configure PWM outputs for servo and ESC control.

The default device `/dev/pwm_output0` are the Main channels, AUX channels are on `/dev/pwm_output1` (`-d` parameter).

It is used in the startup script to make sure the PWM parameters (`PWM_*`) are applied (or the ones provided by the airframe config if specified). `pwm info` shows the current settings (the trim value is an offset and configured with `PWM_MAIN_TRIMx` and `PWM_AUX_TRIMx`).

The disarmed value should be set such that the motors don't spin (it's also used for the kill switch), at the minimum value they should spin.

Channels are assigned to a group. Due to hardware limitations, the update rate can only be set per group. Use `pwm info` to display the groups. If the `-c` argument is used, all channels of any included group must be included.

The parameters `-p` and `-r` can be set to a parameter instead of specifying an integer: use -p p:PWM_MIN for example.

Note that in OneShot mode, the PWM range [1000, 2000] is automatically mapped to [125, 250].

### Examples

Set the PWM rate for all channels to 400 Hz:

    pwm rate -a -r 400
    

Test the outputs of eg. channels 1 and 3, and set the PWM value to 1200 us:

    pwm arm
    pwm test -c 13 -p 1200
    

### Usage {#pwm_usage}

    pwm <command> [arguments...]
     Commands:
       arm           Arm output
    
       disarm        Disarm output
    
       info          Print current configuration of all channels
    
       forcefail     Force Failsafe mode. PWM outputs are set to failsafe values.
         on|off      Turn on or off
    
       terminatefail Enable Termination Failsafe mode. While this is true, any
                     failsafe that occurs will be unrecoverable (even if recovery
                     conditions are met).
         on|off      Turn on or off
    
       rate          Configure PWM rates
         -r <val>    PWM Rate in Hz (0 = Oneshot, otherwise 50 to 400Hz)
    
       oneshot       Configure Oneshot125 (rate is set to 0)
    
       failsafe      Set Failsafe PWM value
    
       disarmed      Set Disarmed PWM value
    
       min           Set Minimum PWM value
    
       max           Set Maximum PWM value
    
       test          Set Output to a specific value until 'q' or 'c' or 'ctrl-c'
                     pressed
    
       steps         Run 5 steps from 0 to 100%
    
     The commands 'failsafe', 'disarmed', 'min', 'max' and 'test' require a PWM
     value:
         -p <val>    PWM value (eg. 1100)
    
     The commands 'rate', 'oneshot', 'failsafe', 'disarmed', 'min', 'max', 'test'
     and 'steps' additionally require to specify the channels with one of the
     following commands:
         [-c <val>]  select channels in the form: 1234 (1 digit per channel,
                     1=first)
         [-m <val>]  Select channels via bitmask (eg. 0xF, 3)
         [-g <val>]  Select channels by group (eg. 0, 1, 2. use 'pwm info' to show
                     groups)
         [-a]        Select all channels
    
     These parameters apply to all commands:
         [-d <val>]  Select PWM output device
                     values: <file:dev>, default: /dev/pwm_output0
         [-v]        Verbose output
         [-e]        Exit with 1 instead of 0 on error
    

## reboot

Source: [systemcmds/reboot](https://github.com/PX4/Firmware/tree/master/src/systemcmds/reboot)

Reboot the system

### Usage {#reboot_usage}

    reboot [arguments...]
         [-b]        Reboot into bootloader
         [lock|unlock] Take/release the shutdown lock (for testing)
    

## sd_bench

Source: [systemcmds/sd_bench](https://github.com/PX4/Firmware/tree/master/src/systemcmds/sd_bench)

Test the speed of an SD Card

### Usage {#sd_bench_usage}

    sd_bench [arguments...]
         [-b <val>]  Block size for each read/write
                     default: 4096
         [-r <val>]  Number of runs
                     default: 5
         [-d <val>]  Duration of a run in ms
                     default: 2000
         [-s]        Call fsync after each block (default=at end of each run)
    

## system_time

Source: [systemcmds/system_time](https://github.com/PX4/Firmware/tree/master/src/systemcmds/system_time)

### Description

Command-line tool to set and get system time.

### Examples

Set the system time and read it back

    system_time set 1600775044
    system_time get
    

### Usage {#system_time_usage}

    system_time <command> [arguments...]
     Commands:
       set           Set the system time, provide time in unix epoch time format
    
       get           Get the system time
    

## top

Source: [systemcmds/top](https://github.com/PX4/Firmware/tree/master/src/systemcmds/top)

Monitor running processes and their CPU, stack usage, priority and state

### Usage {#top_usage}

    top [arguments...]
       once          print load only once
    

## usb_connected

Source: [systemcmds/usb_connected](https://github.com/PX4/Firmware/tree/master/src/systemcmds/usb_connected)

Utility to check if USB is connected. Was previously used in startup scripts. A return value of 0 means USB is connected, 1 otherwise.

### Usage {#usb_connected_usage}

    usb_connected [arguments...]
    

## ver

Source: [systemcmds/ver](https://github.com/PX4/Firmware/tree/master/src/systemcmds/ver)

Tool to print various version information

### Usage {#ver_usage}

    ver <command> [arguments...]
     Commands:
       hw            Hardware architecture
    
       mcu           MCU info
    
       git           git version information
    
       bdate         Build date and time
    
       gcc           Compiler info
    
       bdate         Build date and time
    
       px4guid       PX4 GUID
    
       uri           Build URI
    
       all           Print all versions
    
       hwcmp         Compare hardware version (returns 0 on match)
         <hw> [<hw2>] Hardware to compare against (eg. PX4_FMU_V4). An OR comparison
                     is used if multiple are specified
    
       hwtypecmp     Compare hardware type (returns 0 on match)
         <hwtype> [<hwtype2>] Hardware type to compare against (eg. V2). An OR
                     comparison is used if multiple are specified