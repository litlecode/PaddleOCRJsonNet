# PaddleOCRJsonNet
将OCR开源项目 PaddleOCR-JSON（ https://github.com/hiroi-sora/PaddleOCR-json ）封装为.NET环境调用的开源库，支持 .NET Framework和.NET Core，运行环境必须是x64.

### 1. 添加引用
项目——引用——将PaddleOCRJsonDotNet.dll引用到项目中。
<img width="1197" height="590" alt="image" src="https://github.com/user-attachments/assets/3185e23a-6c37-474b-823e-9ee3e8da7acc" />


### 2.模型路径和名称，路径可以按照自己的需要修改：
```
string modelPath = $"{AppDomain.CurrentDomain.BaseDirectory}\\models"; // 替换为你的模型根目录（包含det/rec/cls子文件夹）
string det_Name = "ch_PP-OCRv3_det_infer"; // det文件夹名
string rec_Name = "ch_PP-OCRv3_rec_infer"; // det文件夹名
string cls_Name = "ch_ppocr_mobile_v2.0_cls_infer"; // det文件夹名
string dict_Name = "dict_chinese.txt"; // det文件夹名
```

### 3.如果要识别本地路径的图片，可以按照下列方式调用：
```
private void ReadImg()
{
    string testImagePath = $"{AppDomain.CurrentDomain.BaseDirectory}test1.png"; // 替换为你的测试图片路径（绝对路径或相对路径）

    PaddleOCRJsonDotNet.PaddleOCR pocr = new PaddleOCRJsonDotNet.PaddleOCR();
    string status = pocr.InitPaddleOCR(modelPath, det_Name, rec_Name, cls_Name, dict_Name);
    if (status != "初始化成功！")
    {
        MessageBox.Show(status);
        return;
    }

    //根据路径识别图片
    string result = pocr.ImageRecognize(testImagePath);

    pocr.Close();
}
```

### 4.如果你读取的内存中的图片，可以根据图片的字节数组直接识别内容：
```
private void ReadImg()
{
    PaddleOCRJsonDotNet.PaddleOCR pocr = new PaddleOCRJsonDotNet.PaddleOCR();
    string status = pocr.InitPaddleOCR(modelPath, det_Name, rec_Name, cls_Name, dict_Name);
    if (status != "初始化成功！")
    {
        MessageBox.Show(status);
        return;
    }

    //根据图片字节数组识别内容
    byte[] imgBytes = null;//图片的字节数组
    string resultByBytes = pocr.ImageRecognize(imgBytes, imgBytes.Length);
    pocr.Close();
}
```

### 5.如果需要批量识别，可以使用下列方法：
```
private void ReadMoreImgs(List<string> imgList)
{
    string testImagePath = $"{AppDomain.CurrentDomain.BaseDirectory}test1.png"; // 替换为你的测试图片路径（绝对路径或相对路径）

    PaddleOCRJsonDotNet.PaddleOCR pocr = new PaddleOCRJsonDotNet.PaddleOCR();
    string status = pocr.InitPaddleOCR(modelPath, det_Name, rec_Name, cls_Name, dict_Name);
    if (status != "初始化成功！")
    {
        MessageBox.Show(status);
        return;
    }

    foreach(string f in imgList)
    {
        string result = pocr.ImageRecognize(f);
    }
    
    pocr.Close();
}
```



