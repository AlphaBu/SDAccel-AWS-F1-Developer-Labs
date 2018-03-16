## Module 1: Introduction to the SDAccel Flow

<br>

#### Downloading the lab contents

Before starting this module, make sure that the lab contents are properly installed on your instance: 
1. Open a new terminal by right-clicking anywhere in the Desktop area and selecting **Open Terminal**, then run the following commands:
    ```bash  
    cd ~
    rm -rf SDAccel-AWS-F1-Developer-Labs
    git clone https://github.com/Xilinx/SDAccel-AWS-F1-Developer-Labs.git
    ```

#### Starting the module

This module is divided in 3 labs. Since building FPGA binaries is not instantaneous, all the modules of this Developer Lab will use precompiled FPGA binaries. It is recommended to complete each lab before proceeding to the next.

1. **Running the "Hello World" example** \
You will get familiar with the steps involved in running a simple application on the F1 instance. \
[**Start this lab.**](lab_01_helloworld.md)

1. **Introduction to the SDAccel development environment** \
You will use the SDAccel development environment to create and profile an F1 accelerator. The lab focuses on the Inverse Discrete Cosine Transform (IDCT), a compute intensive function used at the heart of all video codecs. \
[**Start this lab.**](lab_02_idct_introduction.md)

1. **Using the SDAccel GUI to optimize F1 applications** \
You will continue the IDCT project started in the previous lab and learn how you can use the various reports provided by SDAccel to guide the performance optimization process of your application. \
[**Start this lab.**](lab_03_idct_optimization.md)

After you complete the last lab, you will be guided to close your RDP session, stop your F1 instance and explore next steps to continue your experience with SDAccel on AWS. 

---------------------------------------

<p align="center"><b>
Start the first lab: <a href="lab_01_helloworld.md">Running the "Hello World" example</a>
</b></p>
