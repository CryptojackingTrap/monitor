# cryptojackingtrap-monitor 

# Purpose
This project is a module of the Cryptojackingtrap solution, which is responsible for monitoring the suspicious executable file or live process. We notice the application memory read attempts in this monitoring process and make logs of the memory contents. This log file is used in the detector module of Cryptojackingtrap to determine whether this application is mining cryptocurrencies.

# Technical points
This project is a C++ project developed using the "PIN" tool, a binary instrumentation tool. Hence it is necessary to download this tool for building the current project (cryptojackingtrap-monitor). 

# Tested environment
We built this application on "Windows 11 pro" using Visual Studio, version 2022. This project has been tested with the last version of the PIN tool, "Pin 3.22," released on "February 28, 2022". 

However, the PIN debugger is supported for Windows, Linux, and Mac; Consequently, the cryptojackingtrap-monitor can be built on these platforms.

# Build
For building the current project, you should do the following steps:
1- Download the PIN tool currently available on the following link: (tested version: 3.22)
https://www.intel.com/content/www/us/en/developer/articles/tool/pin-a-binary-instrumentation-tool-downloads.html
let's call the downloaded path as "${PinPath}" in the rest of this text. for example ${PinPath} can be "C://pin-3.22"

2- For the Windows environment, install Visual Studio (tested version: 2022)

3- Download the latest version of the cryptojackingtrap-monitor project.

4- Place the cryptojackingtrap-monitor folder in "${PinPath}\source\tools" 

5- Open the project we would have in "${PinPath}\source\tools\cryptojackingtrap-monitor" by the visual studio and then build it. As a result it is expected to find CryptojackingtrapMonitor.dll file based on your platform. For example, in X64 you will find it in x64\Debug\

# Execution
For running the application you can use command prompt to debug an executable file or an existing process. Their commands are as mentioned in the following respectively:

1) pin.exe -t "${PinPath}\source\tools\cryptojackingtrap-monitor\x64\Debug\CryptojackingtrapMonitor.dll" -support_jit_api -o fileName.out -- "App-path\application.exe"

2) pin.exe -pid 36999 -t "${PinPath}\source\tools\cryptojackingtrap-monitor\x64\Debug\CryptojackingtrapMonitor.dll" -support_jit_api -o fileName.out

In the first command, we use the <b>absolute</b> path of application.exe to debug, and in the second command, we use -pid to mention to an existing process ID, which is 36999 in this example.

Any operating system assigns a PID (Process ID) to each process. This ID can be accessible in "Task Manager / Details" in Windows.

In both cases, we use -o to specify the output file name or output file path. But please note that their base directory is different.

<b>Important Note - File Path:</b>

In the first case (executable file), the base directory is "${PinPath}", the path that you execute the pin command. It means that if you just use " -o fileName.out" as the option, this file would be created in the "${PinPath}/fileName.out".

In contrast for the second case (process id), it will be written relative to the current directory of the instrumented process, not the directory you started the Pin command from. So it is changed based on your process type.

In both ways of calling PIN, if we do not use this option -o to specify the output path, the default file name is cryptojackingtrapMonitorLog.out in their relative path that is above explained.

So in both cases especially in the second one we recommend using an absolute path for fileName.out.

<b>Important Note - Privileges:</b>

Please note that if you don't open the command prompt as an administrator in the second command format that uses PID, the output will not be created. So for Windows OS, simply right-click on the command prompt and select "Run as administrator."

<b>Important Note - Network Access:</b>

Please be aware that many public networks, including those at universities, restrict cryptocurrency mining traffic. Ensure that you are connected to a suitable network connection to facilitate the detection of suspicious applications attempting cryptocurrency network access, as part of our cryptojacking detection approach.

# Development

This file is developed based on the original PIn example available in source/tools/SimpleExamples/pinatrace.cpp. The output is all memory content read from memory in hex format in addition to its event timestamp in "%Y/%m/%d %X" format

for example, each line can be like the following lines:
2022/04/26 15:47:00     0x7ffd5b272bce
2022/04/26 15:47:00 0xffffffffffffffff
