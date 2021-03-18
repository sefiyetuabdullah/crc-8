# crc-8

This project implements a program that computes the CRC-8 error detection code and uses it for error detection. 

The program reads the data, calculates crc8, combine input and crc to generate the output file. Then the program writes the output and simulates a data link.
After this, the program reads the sender's frame, simulates data corruption and an error-free data link and writes the transmitted frame. Then the program simulates the receiver, reads the transmitted frame, checks CRC8 to detect errors and shows whether the frame has errors or not. 
