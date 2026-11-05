# Meshcore Custom code for ESP32 Classic Wroom and C3 SuperMini
Download visual studio code on windows from https://code.visualstudio.com/ 

Add extension **platformio** as well as **c++** microsoft.



<img width="684" height="429" alt="image" src="https://github.com/user-attachments/assets/bb08f419-4f36-4a85-99cc-a205d50bad69" />



Download the zip **MeshCore_SX1278_variants_clean.zip** OR **Meshcore-ajm.zip** and unzip to C or D drive as per your PC. 

Open folder in explorer



<img width="419" height="374" alt="image" src="https://github.com/user-attachments/assets/cbe5f376-f8f3-475b-87d0-c4d3ef978faf" />



open a new terminal from platformio extension (alien logo on left side) run 



<img width="743" height="430" alt="image" src="https://github.com/user-attachments/assets/c71a98ae-c5c0-44f3-b005-8e54b19ad207" />




make sure your root folder is showing up in terminal e.g. **C:\Meshcore>** or **D:\Meshcore>** as per your unzipped folder location.



<img width="892" height="877" alt="image" src="https://github.com/user-attachments/assets/fd039a54-ce69-4879-8093-569288445c93" />




Connect your esp32 to PC.

For Classic ESP32 type the following;

"pio run -e my_esp32_sx1278_companion_ble -t upload"

For ESP32 C3 Supermini type the following;

"pio run -e my_esp32c3_sx1278_companion_ble -t upload"
