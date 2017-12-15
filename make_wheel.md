### BMP文件的的保存

4个blocks：FILEHEADER, INFOHEADER, 调色板，数据

两个header的定义：
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

两个header的填充，以8位灰度图为案例：

```cpp
int init_bmp_header(FILEHEADER *fh, INFOHEADER *ih, int width, int height)
{
	ih->size = sizeof(INFOHEADER);
	ih->width = width;
	ih->height = -height;	// 负数表示从左上角为存储起始地址，bmp标准
	ih->planes = 1;
	ih->bits = 8;
	ih->compression = 0; 
	ih->imagesize = width*height;
	ih->xresolution = 0;
	ih->yresolution = 0;
	ih->ncolours = 0;
	ih->importantcolours = 0;


	fh->type = 0x4d42;
	fh->size = sizeof(FILEHEADER) + sizeof(INFOHEADER) + 1024 + width*height;
	fh->offset = sizeof(FILEHEADER) + sizeof(INFOHEADER) + 1024;
	fh->reserved1 = 0;
	fh->reserved2 = 0;

	return 0;
}
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
