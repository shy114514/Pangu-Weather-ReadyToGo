# Pangu-Weather-ReadyToGo
Unofficial demonstration of [Huawei's Pangu Weather Model](https://github.com/198808xc/Pangu-Weather). Implementing the entire process of data preparation for input, forecasting conversion of forecasted results, and visualization.

【非官方】华为盘古天气模型演示，含输入数据准备、预测结果转换及结果可视化全流程。[中文指南](#安装和准备工作)
![T2M 24h forecast](https://github.com/HaxyMoly/Pangu-Weather-ReadyToGo/raw/main/img/T2M_24h.png)

## Installation and Preparation
1. Register for an account at [Climate Data Store](https://cds.climate.copernicus.eu/user/register)
2. Copy the url and key displayed on [CDS API key](https://cds.climate.copernicus.eu/api-how-to) and add them to the` ~/.cdsapirc` file.
5. Clone this repo and install dependencies accordingly, depending on GPU availability.
```bash
git clone https://github.com/HaxyMoly/Pangu-Weather-ReadyToGo.git
cd Pangu-Weather-ReadyToGo

# GPU
pip install -r requirements_gpu.txt
# CPU
pip install -r requirements_cpu.txt

conda install -c conda-forge cartopy
```
4. Download four pre-trained weights from [Pangu-Weather](https://github.com/198808xc/Pangu-Weather/tree/main#global-weather-forecasting-inference-using-the-trained-models) and create a folder named `models` to put them in. Feel free to download only one of them for testing purposes.
```bash
mkdir models
```

## Forecasting
1. Modify the `date` and `time` of the initial field in `data_prepare.py`. You may check the data availability at a specific moment on [ERA5](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels).
```python
date = '2023-07-02'
time = '23:00'
```
2. Run `data_prepare.py` to download the initial field data and convert them to numpy array.
```bash
python data_prepare.py
```
3. Modify the following variables in `inference.py` according to your needs:
```python
# Enable GPU acceleration
use_GPU = True

# The date and time of the initial field
date = '2023-07-02'
time = '23:00'

# Uncomment the model to be used
model_used = 'models/pangu_weather_24.onnx' # 24h
# model_used = 'models/pangu_weather_6.onnx' # 6h
# model_used = 'models/pangu_weather_3.onnx' # 3h
# model_used = 'models/pangu_weather_1.onnx' # 1h
```
4. Execute `inference.py` to make forecast
```bash
python inference.py
```
5. Modify the date and time of the initial field in `forecast_decode.py`
```python
date = '2023-07-02'
time = '23:00'
```
6. After making the forecast, run `forecast_decode.py` to convert the numpy array back to NetCDF format
```bash
python forecast_decode.py
```
7. Navigate to the forecasting directory to visualize the results
```bash
cd forecasts/2023-07-02-23:00
# Visualize the land surface forecast
ncvue output_surface.nc
# Visualize the upper air forecast
ncvue output_upper.nc
```
Don't forget to select the variable to be visualized.
![ncvue demo](https://github.com/HaxyMoly/Pangu-Weather-ReadyToGo/raw/main/img/ncvue_demo.png)

## Acknowledgement
Thanks Huawei team for their amazing meteorological forecasting model [Pangu-Weather](https://github.com/198808xc/Pangu-Weather).  
Thanks mcuntz for his/her wonderful open-source NetCDF visualization project [ncvue](https://github.com/mcuntz/ncvue).


## Warning
I am a Bioinformatics student, not a meteorologist, so I cannot guarantee the accuracy of the code. Therefore, this project is only intended for reference and learning purposes. Additionally, this project is based on [Pangu-Weather](https://github.com/198808xc/Pangu-Weather/tree/main#global-weather-forecasting-inference-using-the-trained-models) and follows its BY-NC-SA 4.0 license, and should not be used for commercial purposes. Please cite the publication of Pangu-Weather.
```
@Article{Bi2023,
author={Bi, Kaifeng and Xie, Lingxi and Zhang, Hengheng and Chen, Xin and Gu, Xiaotao and Tian, Qi},
title={Accurate medium-range global weather forecasting with 3D neural networks},
journal={Nature},
doi={10.1038/s41586-023-06185-3},
}
```
## 安装和准备工作
1. 前往 [Climate Data Store](https://cds.climate.copernicus.eu/user/register) 注册一个账号
2. 前往 [CDS API key](https://cds.climate.copernicus.eu/api-how-to)，复制url和key，写入 `~/.cdsapirc` 文件
5. 克隆本仓库，根据是否有独显选择安装依赖
```bash
git clone https://github.com/HaxyMoly/Pangu-Weather-ReadyToGo.git
cd Pangu-Weather-ReadyToGo

# GPU
pip install -r requirements_gpu.txt
# CPU
pip install -r requirements_cpu.txt

conda install -c conda-forge cartopy
```
4. 在 [Pangu-Weather](https://github.com/198808xc/Pangu-Weather/tree/main#global-weather-forecasting-inference-using-the-trained-models) 下载4个预训练模型，创建一个名为 `models` 的文件夹，把它们放进去（也可以根据需要任意下载一个测试）
```bash
mkdir models
```

## 预测Demo
1. 修改 `data_prepare.py` 中初始场的 `date`和 `time`，某时刻数据可用性可在 [ERA5](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels) 查询
```python
date = '2023-07-02'
time = '23:00'
```
2. 执行 `data_prepare.py` 下载初始场数据并转换为npy格式
```bash
python data_prepare.py
```
3. 根据需要修改 `inference.py` 中以下变量
```python
# 是否启用GPU加速
use_GPU = True

# 初始场时刻
date = '2023-07-02'
time = '23:00'

# 用哪个模型就去掉哪个的注释
model_used = 'models/pangu_weather_24.onnx' # 24h
# model_used = 'models/pangu_weather_6.onnx' # 6h
# model_used = 'models/pangu_weather_3.onnx' # 3h
# model_used = 'models/pangu_weather_1.onnx' # 1h
```
4. 执行 `inference.py` 进行预测
```bash
python inference.py
```
5. 修改 `forecast_decode.py` 中初始场时刻
```python
date = '2023-07-02'
time = '23:00'
```
6. 预测完成后，执行 `forecast_decode.py` 将npy转换回NetCDF格式
```bash
python forecast_decode.py
```
7. 进入预测文件路径可视化结果
```bash
cd forecasts/2023-07-02-23:00
# 可视化预测地表数据
ncvue output_surface.nc
# 或可视化预测大气数据
ncvue output_upper.nc
```
记得选择要可视化的变量
![cvue demo](https://github.com/HaxyMoly/Pangu-Weather-ReadyToGo/raw/main/img/ncvue_demo.png)

## 感谢
华为团队开源的气象预测大模型 [Pangu-Weather](https://github.com/198808xc/Pangu-Weather)  
mcuntz开源的优秀NetCDF可视化项目 [ncvue](https://github.com/mcuntz/ncvue)

## 警告
本人专业为生物信息学，并非气象专业人士，无法保证代码完全准确，因此该项目仅供参考交流学习。另该项目系基于 [Pangu-Weather](https://github.com/198808xc/Pangu-Weather)，因此亦遵循原项目的BY-NC-SA 4.0开源许可证，切勿用于商业目的。使用本项目请引用原项目
```
@Article{Bi2023,
author={Bi, Kaifeng and Xie, Lingxi and Zhang, Hengheng and Chen, Xin and Gu, Xiaotao and Tian, Qi},
title={Accurate medium-range global weather forecasting with 3D neural networks},
journal={Nature},
doi={10.1038/s41586-023-06185-3},
}
```