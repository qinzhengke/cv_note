### BMP文件的的保存

4个blocks：FILEHEADER, INFOHEADER, 调色板，数据

两个header：
```cpp
# pragma pack(push)
# pragma pack(1)
typedef struct {
	/* type : Magic identifier,一般為BM(0x42,0x4d) */
	unsigned short int type;
	unsigned int size;/* File size in bytes,全部的檔案大小 */
	unsigned short int reserved1, reserved2; /* 保留欄位 */
	unsigned int offset;/* Offset to image data, bytes */
} FILEHEADER;

typedef struct {
	unsigned int size;/* Info Header size in bytes */
	int width, height;/* Width and height of image */
	unsigned short planes;/* Number of colour planes */
	unsigned short bits; /* Bits per pixel */
	unsigned int compression; /* Compression type */
	unsigned int imagesize; /* Image size in bytes */
	int xresolution, yresolution; /* Pixels per meter */
	unsigned int ncolours; /* Number of colours */
	unsigned int importantcolours; /* Important colours */
} INFOHEADER;
#pragma pack(pop)
```

调色板：B+G+R+Aplha，（bmp也有alpha？从来没见过半透明的bmp图）

```cpp
  // 8bit灰度图调色板
	uchar palette[1024];
	for (int i = 0; i < 256; i++)
	{
		palette[i*4] = i;
		palette[i*4+1] = i;
		palette[i*4+2] = i;
		palette[i*4+3] = 0;
	}
```

最后的写文件：

```cpp
	file.write((char*)(&fh), sizeof(FILEHEADER));
	file.write((char*)(&ih), sizeof(INFOHEADER));
	file.write((char*)palette, 1024);
	file.write((char*)buf, ih.imagesize);
```
