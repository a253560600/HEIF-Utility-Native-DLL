# HEIF-Utility-Native-DLL
HUD.dll is a part of <a href="https://github.com/liuziangexit/HEIF-Utility">HEIF-Utility</a>.<br>
It can convert Apple HEIF to JPEG with EXIF metadata and Color Profile.The output quality can be custom.<br>
<a href="https://github.com/liuziangexit/HEIF-Utility-Native-DLL/blob/master/Srcs/HUD/main.cpp">Main Source Code</a><br>
If you don't understand how to build this project pls view: https://www.youtube.com/watch?v=F-4jwy8rV24&feature=youtu.be<br>

<h2>Overview</h2>
This project generates HUD.dll, which is part of the HEIF Utility.<br>

<h2>Interface</h2>
void heif2jpg(const char heif_bin[], int input_buffer_size, const int jpg_quality, char output_buffer[], int output_buffer_size, const char* input_temp_filename, int* copysize, bool include_exif, bool color_profile, const char icc_bin[], int icc_size)<br><br>
Effect: Convert Apple HEIF to JPEG<br>
Calling Conventions: Cdecl<br>
Parameter heif_bin: Binary data of HEIF file<br>
Parameter input_buffer_size: Size of heif_bin<br>
Parameter jpg_quality: Set the quality of the output JPEG image. The value range is 0-100, Higher is better<br>
Parameter output_buffer: Where to out put the JPEG image<br>
Parameter output_buffer_size: Size of output_buffer<br>
Parameter input_temp_filename: Set temporary file name(when this function finish, you can delete this file)<br>
Parameter copysize: the content of this pointer will store the output jpg image's real size<br>
Parameter include_exif: if this == true, then the output jpg image will have the EXIF metadata<br>
Parameter color_profile: if this == true, this function will embed ICC Color Profile to output jpg image<br>
Parameter icc_bin: the binary data of the ICC Profile(Display P3's ICC Profile can be found in here: https://github.com/liuziangexit/EmbedICCProfile/tree/master/icc-profile) . if the parameter "color_profile"==false,icc_bin can be NULL.<br>
Parameter icc_size:size of icc_bin<br>
Exception: None<br>

<h2>How does the Apple HEIF store a picture?</h2>
Apple HEIF divides the image into small tiles, tile resolution is generally 512 * 512. And save the tiles to the HEIF image sequence.<br>
The angle of the tile may be inconsistent with the angle of the photo, but HEIF also records the angle of the tile relative to the photo.<br>
Some of the edges of the block may contain a black fill area, because the resolution of the photo is not necessarily a multiple of 512 * 512, so the extra part is filled with black.<br>

<h2>Demo</h2>
For a typical image with a resolution of 4032 * 3024 taken by iPhone 7, Apple HEIF will store it like this:<br>
The picture is divided into 8 * 6 blocks, each row 8, a total of 6 lines.<br>
The extra part is filled with black.<br><br>
<img src="/Images/img1.jpg"><br>
