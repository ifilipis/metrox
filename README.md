# Revopoint MetroX tools
Last updated for Revoscan 5.6.2
Scripts and reverse engineering for Revopoint MetroX

# Patched Revoscan .exe that unlocks fusion distance limit
![image](https://github.com/user-attachments/assets/8dc76e15-0fca-46d0-be84-cc8182fe99a2)

[Link](https://github.com/ifilipis/metrox/raw/refs/heads/main/RevoScan5MetroX_patched_5.6.2.exe)

Usage: place and run it from Revoscan folder in Program Files. Optionally, original exe can be replaced with the patched one to always use this version

Fusion step is now set to a minimum of 0.01mm. However, the true accuracy of laser mode is around 0.08-0.12mm, so anything below those values wouldn't help (apart from getting a different scanner)

For structured light mode (continuous and single shot), results stop improving at right about 0.15mm due to lack of accuracy and low capture resolution

Auto turntable was not tested yet

 

# Parallel & cross laser raw data extraction
![image](https://github.com/user-attachments/assets/014a9fd2-81f9-45fe-ad1a-4ba409fd3cd7)

[Colab Link](https://colab.research.google.com/drive/1YHmjcKTnzMG80ej8_0_MHOYLJblzS1Fs)

Usage: you'll be prompted to upload frames.dataset from the {project folder}/cache

The tool can extract single frames as well as a complete point cloud. Note the subsampling slider when extracting point cloud - it defines the percentage of points that will be kept (1.0 means 100% will be kept)

 

# Structured light raw data extraction
![image](https://github.com/user-attachments/assets/50eb3280-52ee-49ab-bc6c-b9b154084e70)

[Colab Link](https://colab.research.google.com/drive/1DwiqnBmaMUU5qXByF6CON_3EOUxDNilw)

Usage: upload property.rvproj from the project folder, Pl.bin from {project folder}/params and frames.dataset from {project folder}/cache

If available, global_register_pose.pose can be uploaded - this file contains updated camera poses for better frame alignment. It appears after you click Optimize in Fusion in Revoscan

The tool can extract individual frames (including color data, but it hasn't been validated), individual frame point clouds in world coordinates and a complete point cloud.

Note the subsampling slider

Due to lack of tracking accuracy and low resolution of MetroX, extracted point cloud is very noisy. Revoscan has an optional step to perform fine registration of individual frames using surface features, so in case you want to increase the accuracy of raw data, the frames should be extracted individually and then aligned in your software of choice, such as CloudCompare

 

# Single frame depth upscaling using upscaled surface normals
![image](https://github.com/user-attachments/assets/7480edd9-7d87-4135-b31f-34d2954c4a27)

[Colab Link](https://colab.research.google.com/drive/1QHjJilEoRjJZEYjfA3Bb9Deql10p3j_2)

This is a proof of concept to retreive as much information from the stock depth stream data. It does not improve accuracy, but produces more accurate surface normals for the features that it can recognize.

The algorithm uses ESRGAN to upscale surface normals, then create a high-res depth map using upscaled normals information. At 800x600 pixels for the initial depth frame, detail limit is around 2 pixels (0.5mm, sometimes even more because of noise). Anything smaller than this size would require either more pixels, or more frames.

On a T4 GPU, it takes around one minute per frame for all four steps

Usage: upload property.rvproj from the project folder, Pl.bin from {project folder}/params and an extracted depth_frame.tiff from [step 1 here](https://colab.research.google.com/drive/1DwiqnBmaMUU5qXByF6CON_3EOUxDNilw)
