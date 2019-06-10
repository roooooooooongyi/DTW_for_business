import pandas as pd
import numpy as np
from dtaidistance import dtw
from dtaidistance import dtw_visualisation as dtwvis
from scipy import stats
import seaborn as sns
import matplotlib as mpl
import matplotlib.pyplot as plt
import os


def read_data(filename):
    '''
    input: 文件名, 默认文件是csv格式，使用gb2312编码
    output: dataframe
    '''
    cur_path = os.getcwd()
    parent_path = os.path.dirname(cur_path) + '\\data\\'
    file_path = parent_path + filename +'.csv'
    data = pd.read_csv(file_path, encoding='gb2312')
    return data
    
def exec_dtw(data_for_dtw):
    '''
    input: dataframe 源数据
    output: 输出计算好矩阵的dtw距离   
    '''
    indicators = [i for i in data_for_dtw.columns if i not in 'date']
    array_for_dtw = data_for_dtw[indicators].values
    array_for_dtw_zscore = stats.zscore(array_for_dtw)
    array_for_dtw_zscore_T = array_for_dtw_zscore.T  # need transpose
    ds = dtw.distance_matrix(array_for_dtw_zscore_T)
   
    return pd.DataFrame(ds, index=indicators, columns=indicators)
    
def get_dtw_wrapping_path(data, col1, col2):
    '''
    input: 
        data: dataframe 源数据
        col1: 列名
        col2：列名
    output: 输出图像，上方是col1，下方是col2 
    '''
    indicators = [i for i in data.columns if i not in 'date']
    array_subset = data[indicators].values
    array_subset_zscore = stats.zscore(array_subset)
    array_subset_zscore_T = array_subset_zscore.T 
    x_idx = indicators.index(col1)
    y_idx = indicators.index(col2)
#     x = array_for_dtw_zscore_T[col1,:]
#     y = array_for_dtw_zscore_T[col2,:]
    x = array_subset_zscore_T[x_idx,:]
    y = array_subset_zscore_T[y_idx,:]
    path = dtw.warping_path(x, y)
    outname = col1 + 'vs' + col2
    ds_xy = dtw.distance(x, y)
    dtwvis.plot_warping(x, y, path, filename="D:/Pythoncode/JD_mart/operation_flow_distribution/DTW_for_business/results/%s.png" % outname)
    print("%s 和 %s 的DTW距离: %2.4f" % (col1, col2, ds_xy))


def get_plot_wrapping_paths(data, col1, col2):
    '''
    input: 
    data: dataframe 源数据
    col1: 列名
    col2：列名
    output: 输出图像  
    '''
    indicators = [i for i in data.columns if i not in 'date']
    array_subset = data[indicators].values
    array_subset_zscore = stats.zscore(array_subset)
    array_subset_zscore_T = array_subset_zscore.T 
    x_idx = indicators.index(col1)
    y_idx = indicators.index(col2)
    x = array_subset_zscore_T[x_idx,:]
    y = array_subset_zscore_T[y_idx,:]
    d, paths = dtw.warping_paths(x, y, window=25, psi=2)
    best_path = dtw.best_path(paths)
    dtwvis.plot_warpingpaths(x, y, paths, best_path)
 



dtw_matrix = exec_dtw(data)
get_wrapping_path(data, '直接流量737', '运营电脑')
get_plot_wrapping_paths(data, '直接流量737','整体737')   



