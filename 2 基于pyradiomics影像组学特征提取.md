

<a name="rMmOR"></a>
# FQA：
主要使用pyradiomics提取影像组学特征：

<a name="cJKz6"></a>
# 特征提取：
在这里，我们使用pyradiomics来提取影像组学特征。首先，我们先介绍pyradiomica工具包，然后我们进行特征的提取。
<a name="hgkiB"></a>
## 1 pyradiomics的使用：
PyRadiomics的官方文档:[https://pyradiomics.readthedocs.io/en/latest/](https://pyradiomics.readthedocs.io/en/latest/)<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/38497976/1705118001124-74ee84df-ba95-4e12-949b-2fbe9020192a.png#averageHue=%23c6c3c2&clientId=u742953c3-c837-4&from=paste&height=595&id=u6c6e24bc&originHeight=744&originWidth=1362&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=206489&status=done&style=none&taskId=u7c152d36-a80a-4ce8-acf9-9201a1e9016&title=&width=1089.6)

<a name="LIcmm"></a>
### 1.1，在python环境下安装pyradiomics:

```python
pip install pyradiomics
```

<a name="IXZJW"></a>
### 1.2,设置特征提取器，获得想要特征：
**通过自定义特征提取器，可以根据自己的需求来设置并提取特征。**<br />其实，需要设置两个方面：**1，图像类型；2，所要提取的特征；3，提取器设置**

**第一步：图像类型：**首先，设置提取特征的**图像类型**，可以指定用于提取特征的图像类型。在pyradiomics包中为我们提供了许多可以使用的滤波器，所以我们可以使用原始图像及经过各种滤波器之后的图像，如下方表格。<br />具体可以参考官方：[https://pyradiomics.readthedocs.io/en/latest/customization.html#image-types](https://pyradiomics.readthedocs.io/en/latest/customization.html#image-types)

| 图像类型 | 解释 |
| --- | --- |
| Original | 原始图像 |
| Wavelet | 小波变换。产生在三个维度中每个维度分别使用高通、低通滤波器的所有组合（LLH、LHL、LHH、HLL、HLH、HHL、HHH、LLL） |
| LoG | Laplacian of Gaussian filter高斯滤波器的拉普拉斯算子，是一种边缘增强滤波器。使用它需要指定参数sigma，低 sigma 强调精细纹理，高 sigma 值强调粗糙纹理 |
| Square | 平方。取原始像素的平方并将它们线性缩放回原始范围 |
| SquareRoot | 平方根。取绝对图像强度的平方根并将它们缩放回原始范围 |
| Logarithm | 对数。取绝对强度 + 1 的对数，值缩放到原始范围 |
| Exponential | 指数。采用e^(绝对强度)获取强度的指数值，值被缩放到原始范围 |
| Gradient | 梯度。返回局部梯度的大小 |
| LocalBinaryPattern2D | 在每一片中进行的本地二进制模式 |
| LocalBinaryPattern3D | 在3d中进行的本地二进制模式 |


如何使用：<br />指定方式：设置特征提取器后可以在下边指定<br />可以直接使用：**enableAllImageTypes()** 启用所有类型<br />也可以使用：enableImageTypeByName( imageType , enabled=True , customArgs=None )启用你想用的类型<br />如：
```python
# 所有类型
extractor.enableAllFeatures()
# 指定使用LoG和Wavelet滤波器
extractor.enableImageTypeByName('LoG')
extractor.enableImageTypeByName('Wavelet')
```

**第二步：目标特征设置**：<br />pyradiomics包也为我们提供了很多种可选的特征，如下表格所示：<br />具体可参考资料：[https://pyradiomics.readthedocs.io/en/latest/features.html](https://pyradiomics.readthedocs.io/en/latest/features.html)<br />这些特征主要包含：

- 一阶特征 First Order Statistics (19 features)
- 3D形状特征  Shape-based (3D) (16 features)
- 2D形状特征 Shape-based (2D) (10 features)
- 灰度级共生矩阵 Gray Level Co-occurrence Matrix (24 features)
- 灰度级游程矩阵 Gray Level Run Length Matrix (16 features)
- 灰度大小区域矩阵 Gray Level Size Zone Matrix (16 features)
- 相邻灰度色调差异矩阵 Neighbouring Gray Tone Difference Matrix (5 features)
- 灰度依赖矩阵 Gray Level Dependence Matrix (14 features)

下面展示一阶特征及其解释，更多的可以参考官方文档。

| 特征类型 | 特征 | 解释 |
| --- | --- | --- |
| First Order Features（共19个） | Energy | 能量 |
|  | Total Energy | 总能量 |
|  | Entropy | 熵 |
|  | Minimum | 最小特征值 |
|  | 10Percentile | 特征值的百分之10的值 |
|  | 90Percentile | 特征值得百分之90的值 |
|  | Maximum | 最大特征值 |
|  | Mean | 均值 |
|  | Median | 中位数 |
|  | InterquartileRange | 四分位距离 |
|  | Range | 灰度值范围 |
|  | MeanAbsoluteDeviation | (MAD)平均绝对误差 |
|  | RobustMeanAbsoluteDeviation | (rMAD) 鲁棒平均绝对偏差 |
|  | RootMeanSquared | (RMS)均方根误差 |
|  | StandardDeviation | 标准差。测量平均值的变化或离散量，默认不启用，因为与方差相关 |
|  | Skewness | 偏度。测量值的分布关于平均值的不对称性 |
|  | Kurtosis | 峰度。是图像 ROI 中值分布的“峰值”的量度 |
|  | Variance | 方差。是每个强度值与平均值的平方距离的平均值 |
|  | Uniformity | 均匀度。是每个强度值的平方和 |


**注意：除了形状特征类外，其他特征都可以在原始图像和滤波后的图像上进行计算。**

具体的指定方法：<br />可以直接使用：enableAllFeatures( )启用所有类型<br />也可以使用：enableFeatureClassByName(featureClass, enabled=True)启用你想用的类型<br />例如：
```python
# 设置一阶特征
extractor.enableFeatureClassByName('firstorder')
# 设置只提取一阶特征的'Mean'和'Skewness'
extractor.enableFeaturesByName(firstorder=['Mean', 'Skewness'])

```


**第三步：特征提取器设置：**<br />是否对原图归一化、是否重采样。<br />图像归一化：

- normalize：默认为false。设置为true时进行图像归一化。
- normalizeScale：确定图像归一化后的比例。默认为1。
- removeOutliers：定义要从图像中删除的异常值。默认为0。

图像/mask重采样：

- resampledPixelSpacing：设置重采样时的体素大小。默认无。
- interpolator：设置用于重采样的插值器。仅适用于重采样图像，sitkNearestNeighbor始终用于重采样掩码以保留标签值。可选的插值器：

![](https://cdn.nlark.com/yuque/0/2024/png/38497976/1705199481969-5d227e8a-11a8-47d9-b9c1-b75f4da09545.png#averageHue=%23f7f4f2&clientId=u4dd278b8-cae9-4&from=paste&id=uf89578a4&originHeight=262&originWidth=291&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=ud800022a-6ce3-4e14-981f-4c23a07f408&title=)

- padDistance：设置重采样期间裁剪肿瘤体时的体素补充数量。

例如：
```python
settings = {}
settings['![binWidth](https://img-blog.csdnimg.cn/c9b0896a5eea4eaf8217d0ed7f23e92b.png)
'] = 25
settings['resampledPixelSpacing'] = [3,3,3]  # [3,3,3] is an example for defining resampling (voxels with size 3x3x3mm)
settings['interpolator'] = sitk.sitkBSpline

```

还有其他的一些设置，根据自己的需要修改即可。

<a name="lwdtr"></a>
# 2 代码示例;
下面是一个CT肺部特征提取特征的代码示例：

特征提取块设置：当然，你可以把这一块写成函数：
```python
import radiomics
from radiomics import featureextractor

# 定义特征提取设置
settings = {}
settings['binWidth'] = 25
settings['sigma'] = [3, 5]
settings['resampledPixelSpacing'] = [1,1,1] # 3,3,3
settings['voxelArrayShift'] = 1000 # 300
settings['normalize'] = True
settings['normalizeScale'] = 100

# 实例化特征提取器
extractor = featureextractor.RadiomicsFeatureExtractor(**settings)

# 指定使用 LoG 和 Wavelet 滤波器
extractor.enableImageTypeByName('LoG')
extractor.enableImageTypeByName('Wavelet')
# 所有类型
extractor.enableAllFeatures()
extractor.enableFeaturesByName(firstorder=['Energy', 'TotalEnergy', 'Entropy','Minimum', '10Percentile', '90Percentile',
                                                 'Maximum', 'Mean', 'Median', 'InterquartileRange', 'Range',
                                                 'MeanAbsoluteDeviation', 'RobustMeanAbsoluteDeviation','RootMeanSquared',
                                                 'StandardDeviation', 'Skewness', 'Kurtosis', 'Variance', 'Uniformity'])
extractor.enableFeaturesByName(shape=['VoxelVolume', 'MeshVolume', 'SurfaceArea', 'SurfaceVolumeRatio', 'Compactness1', 'Compactness2', 
                                            'Sphericity', 'SphericalDisproportion',  'Maximum3DDiameter', 'Maximum2DDiameterSlice', 
                                            'Maximum2DDiameterColumn', 'Maximum2DDiameterRow', 
                                            'MajorAxisLength', 'MinorAxisLength', 'LeastAxisLength', 'Elongation', 'Flatness'])
```

将输出特征保存：<br />我们使用单例数据进行测试，当然，你可以在此基础上写个循环对整个文件夹进行测试：

```python
import pandas as pd
import numpy as np

# Get the testCase
nii_Path = './test/Image/'
seg_Path = './test/Mask/'

features_dict = dict()
df = pd.DataFrame()


imagePath = nii_Path + 'sub-strokecase0001_ses-0001_dwi_reg_norm.nii.gz'
maskPath = seg_Path + 'sub-strokecase0001_ses-0001_msk_reg.nii.gz'
print(imagePath)
features = extractor.execute(imagePath, maskPath)  # 抽取特征

for key, value in features.items():  # 输出特征
    features_dict[key] = value

df = df._append(pd.DataFrame.from_dict(features_dict.values()).T, ignore_index=True)

df.columns = features_dict.keys()
df.to_csv('Radiomics-Features.csv', index=0)
print('Done')

```



excel表：<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/38497976/1705201756258-96fd9abd-8cf1-4efc-a489-bee4c4e67b66.png#averageHue=%23fbfaf9&clientId=u4dd278b8-cae9-4&from=paste&height=352&id=ud19e4793&originHeight=440&originWidth=1604&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=16925&status=done&style=none&taskId=ufa51a800-47c1-41a6-ac19-d89e2eeadb1&title=&width=1283.2)






<a name="LfjKZ"></a>
# 参考：

[1]  [https://pyradiomics.readthedocs.io/en/latest/index.html](https://pyradiomics.readthedocs.io/en/latest/index.html)<br />[2] [https://blog.csdn.net/weixin_46428351/article/details/123592586](https://blog.csdn.net/weixin_46428351/article/details/123592586)




