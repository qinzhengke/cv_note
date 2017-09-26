### color map

自己设计的color map丑的一比：

![out](E:\gitbook\cv_note\files\out.bmp)

看一看别人的。

![yJCDd](E:\gitbook\cv_note\files\yJCDd.png)



照着别人的图写了一遍代码，并且把绿色部分给压缩了一下，得到的结果完美！

```cpp
int cvt2color(float *img, uint32_t W, uint32_t H, uint8_t **img_out)
{
    const int32_t N = 500;
    uint8_t color_map[N][3];
    float r = 0.4;
    int32_t s1=(int32_t)((float)N*r), s2 = N/2, s3=(int32_t)((float)N*(1.0-r));
    float k1,k2,k3,k4;
    k1 = 255.0 / r /(float)N;
    k2 = -255.0/(0.5-r)/(float)N;
    k3 = 255.0/(0.5-r)/(float)N;
    k4 = -255.0 / r /(float)N;
    for(int32_t i=0; i<N; i++)
    {
        if(i<s1)
        {
            color_map[i][0] = 255;
            float pix = k1 *(float)i;
            if(pix<0.0) pix = 0.0;
            else if(pix>255.0) pix = 255.0;
            color_map[i][1] = (uint8_t)(pix);
            color_map[i][2] = 0;
        }
        else if(i<s2)
        {
            float pix = k2*(float)(i-s2);
            if(pix<0.0) pix = 0.0;
            else if(pix>255.0) pix = 255.0;
            color_map[i][0] = (uint8_t)(pix);
            color_map[i][1] = 255;
            color_map[i][2] = 0;
        }
        else if(i<s3)
        {
            color_map[i][0] = 0;
            color_map[i][1] = 255;
            float pix = k3*(float)(i-s2);
            if(pix<0.0) pix = 0.0;
            else if(pix>255.0) pix = 255.0;
            color_map[i][2] = (uint8_t)(pix);
        }
        else
        {
            color_map[i][0] = 0;
            float pix = k4 *(float)(i-N);
            if(pix<0.0) pix = 0.0;
            else if(pix>255.0) pix = 255.0;
            color_map[i][1] =  (uint8_t)(pix);
            color_map[i][2] = 255;
        }
    }

    *img_out = new uint8_t[3*W*H];
    for(uint32_t r=0; r<H; r++)
        for(uint32_t c=0; c<W; c++)
        {
            int32_t loc = (int32_t)img[r*W+c] + N/2;
            for(int k=0; k<3; k++)
            {
                if(loc < 0)
                    loc = 0;
                else if(loc > N-1)
                    loc = N-1;
                (*img_out)[r*3*W+3*c+k] = color_map[(uint32_t)loc][k];
            }
        }

    return 0;
}
```

结果：（改变k可以调节绿色部分的宽度）

![color_perfect](E:\gitbook\cv_note\files\color_perfect.bmp)

