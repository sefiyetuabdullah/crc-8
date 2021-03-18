import java.io.*;
import java.nio.*;
import java.nio.file.*;
import java.util.Random;

public class CRC8 {
  public static void printByteCodes(byte[] data) {
    int n = 0;
    for (byte b : data) {
      String st = String.format("0x%02x ", b);
      System.out.print(st);
      if(n % 16 == 7)
        System.out.print(" ");
      if(n % 16 == 15)
        System.out.println();
      n++;
    }
  }
  
  public static byte crc8(byte[] data){
    byte crcInit = 0;
    byte poly = 0x07;
    byte crc;
    byte polynom;
    int i;

    crc = crcInit;
    for (byte b : data) {
      crc = (byte)(b ^ crc);
      for (int j = 0; j < 8; j++) {
        if ((crc & 0x80) != 0) {
          crc = (byte)((crc << 1) ^ poly);
        } else {
          crc <<= 1;
        }
      }
      crc &= 0xFF;
    }
    return crc;
  }

  public static int uint(byte v) {
    return v & 0xFF;
  }

  // Task 1. Simulate sender
  public static byte[] sender(String inputPath, String outputPath){
      byte[] data = {};
      byte[] frame = {};
      
      System.out.println("Data: "+inputPath);
      
      // Task 1.1: Read data
      try{
        data = Files.readAllBytes(Paths.get(inputPath));
      } catch (IOException e) {
        e.printStackTrace();
      }

      // Task 1.2: Calculate crc8
     byte crc = crc8(data);

      // Task 1.3: Combine input and crc to generate output file
    frame = new byte[data.length+1];
      for (int i = 0; i < data.length; i++) 
         frame[i] = data[i];
      frame[data.length] = crc;

      // Task 1.4: Write output
    try{
        Files.write(Paths.get(outputPath), frame, StandardOpenOption.CREATE);
      } catch (IOException e) {
        e.printStackTrace();
      }


      return frame;
  }

  // Task 2. Simulate data link
  public static byte[] link(String inputPath, String outputPath){
    byte[] inFrame = {};
    byte[] outFrame = {};

    System.out.println("Sender's Frame: "+inputPath);
    // Task 2.1: Read Sender's frame
    try{
        inFrame = Files.readAllBytes(Paths.get(inputPath));
      } catch (IOException e) {
        e.printStackTrace();
      }

    // data link simulation
    outFrame = inFrame.clone(); // make a copy
    
    Random rand = new Random();
    int ep = rand.nextInt(100);
    if(ep < 20){
      System.out.println("This link is unstable");
      // Task 2.2: simulate data corruption
      int ri = rand.nextInt(outFrame.length);
      outFrame[ri]++;
    }
    else {
      System.out.println("This link is stable");
      // Task 2.3: simulate error free data link
    }

    // Task 2.4: write the transmitted frame
     try{
        Files.write(Paths.get(outputPath), outFrame, StandardOpenOption.CREATE);
      } catch (IOException e) {
        e.printStackTrace();
      }

    return outFrame;
  }

  // Task 3. Simulate receiver
  public static boolean receiver (String inputPath){
    byte[] frame = {};

    System.out.println("Transmitted Frame: "+inputPath);
    // Task 3.1: Read the transmitted frame
    try{
        frame = Files.readAllBytes(Paths.get(inputPath));
      } catch (IOException e) {
        e.printStackTrace();
      }

    // Task 3.2: Check CRC8 to detect errors
    byte crc_check = crc8(frame);

    // Task 3.3: Show message whether the frame has errors or not
    if (crc_check == 0x00){
    System.out.println("\ncrc check = " + String.format("0x%02x ", crc_check));
    }
    else {
      System.out.println("\ncrc check with error = " + String.format("0x%02x ", crc_check));
      return true;
    }  
    
    return false;
  }

  public static void main(String[] args){
    if(args.length < 2)
      System.out.println("Usage: java CRC8 [input path] [output path]");
    else{
      String inputPath = args[0];
      String outputPath = args[1];

      byte[] data = {};
      byte[] data_w_crc = {};
      
      // Read data
      try{
        data = Files.readAllBytes(Paths.get(inputPath));
      } catch (IOException e) {
        e.printStackTrace();
      }
      System.out.println("\nData");
      printByteCodes(data);

      // Calculate crc8
      byte crc = crc8(data);
      System.out.println("\ncrc = " + String.format("0x%02x ", crc));

      // Combine input and crc to generate output file
      data_w_crc = new byte[data.length+1];
      for (int i = 0; i < data.length; i++) 
         data_w_crc[i] = data[i];
      data_w_crc[data.length] = crc;
      
      System.out.println("\nOutput");
      printByteCodes(data_w_crc);

      // Write output
      try{
        Files.write(Paths.get(outputPath), data_w_crc, StandardOpenOption.CREATE);
      } catch (IOException e) {
        e.printStackTrace();
      }

      // crc check
      byte crc_check = crc8(data_w_crc);
      System.out.println("\ncrc check = " + String.format("0x%02x ", crc_check));
      
      // simulate data corruption
      byte[] data_w_crc_err = data_w_crc;
      Random rand = new Random();
      int ri = rand.nextInt(data_w_crc_err.length);
      data_w_crc_err[ri]++;

      // crc check
      byte crc_check_err = crc8(data_w_crc_err);
      System.out.println("\ncrc check with error = " + String.format("0x%02x ", crc_check_err));

      // unstable data link simulations
      for( int i = 0; i < 10; i++){
        System.out.println("\nSimulation "+i);
        String outputPath1 = outputPath.replace(".txt", "_link"+i+".txt");
        sender(inputPath, outputPath);
        link(outputPath, outputPath1);
        receiver(outputPath1);
      }
    }
  }
}
