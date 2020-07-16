# FPGA-AES-encryptor
AES processor impelmented by Verilog. This processor can run at the frequency of 100MHz and take 10 cycles to encrypt an 128-bit plain text.The processor uses several simple commands and state bits to input, encrypt and output the data.
## Signal description
|PIN|Width(bit)|Direction|Description|
|:---:|:---:|:---:|:---:|
|CLK|1|input|The clock signal of the processor|
|RST_|1|input|The reset signal of the processor. Low active|
|DIN|8|input|Data input. Sampled at the positive edge of clock. The cyber and initial key are input through DIN.  One byte each time, lower bytes first. |
|CMD|2|input|Command input. Sampled at the positive edge of clock.|
|DOUT|8|output|Data output|
|READY|1|output|Chip ready signal. The chip is available only when READY=1|
|OK|1|output|Output ready signal. The DOUT is valid only when OK=1|

## Command description
|CMD|Descripion|
|:---:|:---:|
|CMD_ID|Standby state. No operation.|
|CMD_ST|Begin encryption using the key and plaintext.|
|CMD_SK|Input the original key. Reset the key on chip.|
|CMD_SP|Input the plaintext. Reset the plaintext on chip.|

## Timing description
The operations of the coprocessor are decided by the CMD input signal. The CMD input signal is sampled at each positive edge of the clock. If the coprocessor is ready (READY=1), the system would send commands to the coprocessor to input data through DIN. Both the key and the plaintext are input through DIN according to the corresponding command. The READY signal keeps low until the current encryption procedure is finished. Then, the coprocessor is ready for new commands. Each time the encryption is accomplished, OK=1 and the cipher text is output through DOUT. After the output of the cipher text, OK is reset to 0.
