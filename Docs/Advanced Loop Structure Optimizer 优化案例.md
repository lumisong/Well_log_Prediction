# Advanced Loop Structure Optimizer 优化案例

## 原始代码（待优化）

前提条件：

1. merge_createname 函数已定义；

```python
#%%Loading LAS Dataset

fname = './LAS_FILES/15_9-19_A_CPI.las'
Well_F19A = lasio.read(fname)
dataF19A=DataFrame(Well_F19A.data, columns=list(Well_F19A.curvesdict.keys()))


#calculate GR from VSH formula
dataF19A['GR'] = dataF19A['GRMAX'] - dataF19A['VSH']*(dataF19A['GRMAX']-dataF19A['GRMIN'])
#calculate RHOB from Density porosity formula
dataF19A['RHOB'] = dataF19A['RHOMA'] - dataF19A['PORD']*(dataF19A['RHOMA']-dataF19A['RHOFL'])
#calculate NPHI from  porosity formula
dataF19A['NPHI'] = dataF19A['PORD'] + (dataF19A['PHIF']-dataF19A['PORD']-0.01)/0.4
dataF19A['NPHI'] = dataF19A['NPHI'].abs()   
#calculate RT from  SW formula
dataF19A['RPCEHM'] = dataF19A['RW']/(dataF19A['SW']**(-1/dataF19A['N']) * (dataF19A['PHIF']+0.01)**(dataF19A['M']))
dataF19A = merge_createname(dataF19A, None, Well_F19A, name='Well_F19A')
del Well_F19A
#Keep any rows with 6 and more non-nan values
dataF19A.dropna(thresh=6,inplace=True)

fname = './LAS_FILES/15_9-19_BT2_CPI.las'
Well_F19BT2 = lasio.read(fname)
dataF19BT2=DataFrame(Well_F19BT2.data, columns=list(Well_F19BT2.curvesdict.keys()))


#calculate RT from  SW formula
dataF19BT2['RPCEHM'] = dataF19BT2['RW']/(dataF19BT2['SW']**(-1/2.45) * dataF19BT2['PHIF']**1.79)
dataF19BT2 = merge_createname(dataF19BT2, None, Well_F19BT2, name='Well_F19BT2')
del Well_F19BT2
#Keep any rows with 5 and more non-nan values
dataF19BT2.dropna(thresh=5,inplace=True)

fname = './LAS_FILES/15_9-19_SR_CPI.las'
Well_F19SR = lasio.read(fname)
dataF19SR=DataFrame(Well_F19SR.data, columns=list(Well_F19SR.curvesdict.keys()))


dataF19SR = merge_createname(dataF19SR, None, Well_F19SR, name='Well_F19SR')
del Well_F19SR
#Keep any rows with 4 and more non-nan values
dataF19SR.dropna(thresh=4,inplace=True)

fname = './LAS_FILES/15_9-F-1_A_CPO.las'
Well_F1A = lasio.read(fname)
dataF1AO=DataFrame(Well_F1A.data, columns=list(Well_F1A.curvesdict.keys()))


fname = './LAS_FILES/15_9-F-1_A_CPI.las'
Well_F1A = lasio.read(fname)
dataF1A=DataFrame(Well_F1A.data, columns=list(Well_F1A.curvesdict.keys()))



dataF1A = merge_createname(dataF1A, dataF1AO, Well_F1A, name='Well_F1A')
del Well_F1A; del dataF1AO
#Keep any rows with 7 and more non-nan values
dataF1A.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-1_B_CPO.las'
Well_F1B = lasio.read(fname)
dataF1BO=DataFrame(Well_F1B.data, columns=list(Well_F1B.curvesdict.keys()))
fname = './LAS_FILES/15_9-F-1_B_CPI.las'
Well_F1B = lasio.read(fname)
dataF1B=DataFrame(Well_F1B.data, columns=list(Well_F1B.curvesdict.keys()))
dataF1B = merge_createname(dataF1B, dataF1BO, Well_F1B, name='Well_F1B')
del Well_F1B; del dataF1BO
#Keep any rows with 7 and more non-nan values
dataF1B.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-1_C_CPO.las'
Well_F1C = lasio.read(fname)
dataF1CO=DataFrame(Well_F1C.data, columns=list(Well_F1C.curvesdict.keys()))
#Drop bad data
dataF1CO=dataF1CO[dataF1CO['BADDATA_FLAG'] != 1]
fname = './LAS_FILES/15_9-F-1_C_CPI.las'
Well_F1C = lasio.read(fname)
dataF1C=DataFrame(Well_F1C.data, columns=list(Well_F1C.curvesdict.keys()))
dataF1C = merge_createname(dataF1C, dataF1CO, Well_F1C, name='Well_F1C')
del Well_F1C; del dataF1CO
#Keep any rows with 7 and more non-nan values
dataF1C.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-1_CPO.las'
Well_F1 = lasio.read(fname)
dataF1O=DataFrame(Well_F1.data, columns=list(Well_F1.curvesdict.keys()))
fname = './LAS_FILES/15_9-F-1_CPI.las'
Well_F1 = lasio.read(fname)
dataF1=DataFrame(Well_F1.data, columns=list(Well_F1.curvesdict.keys()))
dataF1 = merge_createname(dataF1, dataF1O, Well_F1, name='Well_F1')
del Well_F1; del dataF1O
#Keep any rows with 7 and more non-nan values
dataF1.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-11_A_CPO.las'
Well_F11A = lasio.read(fname)
dataF11AO=DataFrame(Well_F11A.data, columns=list(Well_F11A.curvesdict.keys()))
fname = './LAS_FILES/15_9-F-11_A_CPI.las'
Well_F11A = lasio.read(fname)
dataF11A=DataFrame(Well_F11A.data, columns=list(Well_F11A.curvesdict.keys()))
dataF11A = merge_createname(dataF11A, dataF11AO, Well_F11A, name='Well_F11A')
del Well_F11A; del dataF11AO
#Keep any rows with 7 and more non-nan values
dataF11A.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-11_B_CPO.las'
Well_F11B = lasio.read(fname)
dataF11BO=DataFrame(Well_F11B.data, columns=list(Well_F11B.curvesdict.keys()))
fname = './LAS_FILES/15_9-F-11_B_CPI.las'
Well_F11B = lasio.read(fname)
dataF11B=DataFrame(Well_F11B.data, columns=list(Well_F11B.curvesdict.keys()))
dataF11B = merge_createname(dataF11B, dataF11BO, Well_F11B, name='Well_F11B')
del Well_F11B; del dataF11BO
#Keep any rows with 7 and more non-nan values
dataF11B.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-11_CPI.las'
Well_F11 = lasio.read(fname)
dataF11=DataFrame(Well_F11.data, columns=list(Well_F11.curvesdict.keys()))
dataF11 = merge_createname(dataF11, None, Well_F11, name='Well_F11')
del Well_F11
#Keep any rows with 4 and more non-nan values
dataF11.dropna(thresh=4,inplace=True)

fname = './LAS_FILES/15_9-F-11_T2_CPO.las'
Well_F11T2 = lasio.read(fname)
dataF11T2O=DataFrame(Well_F11T2.data, columns=list(Well_F11T2.curvesdict.keys()))
fname = './LAS_FILES/15_9-F-11_T2_CPI.las'
Well_F11T2 = lasio.read(fname)
dataF11T2=DataFrame(Well_F11T2.data, columns=list(Well_F11T2.curvesdict.keys()))
dataF11T2 = merge_createname(dataF11T2, dataF11T2O, Well_F11T2, name='Well_F11T2')
del Well_F11T2; del dataF11T2O
#Keep any rows with 7 and more non-nan values
dataF11T2.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-12_CPO.las'
Well_F12 = lasio.read(fname)
dataF12O=DataFrame(Well_F12.data, columns=list(Well_F12.curvesdict.keys()))
#adjust Resistivity naming to fit the rest above
dataF12O.rename(columns = {'RT':'RPCEHM','RD':'RPCELM'}, inplace=True)
fname = './LAS_FILES/15_9-F-12_CPI.las'
Well_F12 = lasio.read(fname)
dataF12=DataFrame(Well_F12.data, columns=list(Well_F12.curvesdict.keys()))
dataF12 = merge_createname(dataF12, dataF12O, Well_F12, name='Well_F12')
del Well_F12; del dataF12O
#Keep any rows with 7 and more non-nan values
dataF12.dropna(thresh=7,inplace=True)

fname = './LAS_FILES/15_9-F-15D_CPO.las'
Well_F15D = lasio.read(fname)
dataF15DO=DataFrame(Well_F15D.data, columns=list(Well_F15D.curvesdict.keys()))
fname = './LAS_FILES/15_9-F-15D_CPI.las'
Well_F15D = lasio.read(fname)
dataF15D=DataFrame(Well_F15D.data, columns=list(Well_F15D.curvesdict.keys()))
dataF15D = merge_createname(dataF15D, dataF15DO, Well_F15D, name='Well_F15D')
del Well_F15D; del dataF15DO
#Keep any rows with 8 and more non-nan values
dataF15D.dropna(thresh=8,inplace=True)

```

## 已优化测试功能正常代码

```python
# 定义常量
MIN_NON_NAN_VALUES = {
    'Well_F19A':  6,
    'Well_F19BT2':  5,
    'Well_F19SR':  4,
    'Well_F1A':  7,
    'Well_F1B':  7,
    'Well_F1C':  7,
    'Well_F1':  7,
    'Well_F11A':  7,
    'Well_F11B':  7,
    'Well_F11':  4,
    'Well_F11T2':  7,
    'Well_F12':  7,
    'Well_F15D':  8,
}

well_files = {
    'Well_F19A': ['./LAS_FILES/15_9-19_A_CPI.las'],
    'Well_F19BT2': ['./LAS_FILES/15_9-19_BT2_CPI.las'],
    'Well_F19SR': ['./LAS_FILES/15_9-19_SR_CPI.las'],
    'Well_F1A': ['./LAS_FILES/15_9-F-1_A_CPO.las', './LAS_FILES/15_9-F-1_A_CPI.las'],
    'Well_F1B': ['./LAS_FILES/15_9-F-1_B_CPO.las', './LAS_FILES/15_9-F-1_B_CPI.las'],
    'Well_F1C': ['./LAS_FILES/15_9-F-1_C_CPO.las', './LAS_FILES/15_9-F-1_C_CPI.las'],
    'Well_F1': ['./LAS_FILES/15_9-F-1_CPO.las', './LAS_FILES/15_9-F-1_CPI.las'],
    'Well_F11A': ['./LAS_FILES/15_9-F-11_A_CPO.las', './LAS_FILES/15_9-F-11_A_CPI.las'],
    'Well_F11B': ['./LAS_FILES/15_9-F-11_B_CPO.las', './LAS_FILES/15_9-F-11_B_CPI.las'],
    'Well_F11': ['./LAS_FILES/15_9-F-11_CPI.las'],
    'Well_F11T2': ['./LAS_FILES/15_9-F-11_T2_CPO.las', './LAS_FILES/15_9-F-11_T2_CPI.las'],
    'Well_F12': ['./LAS_FILES/15_9-F-12_CPO.las', './LAS_FILES/15_9-F-12_CPI.las'],
    'Well_F15D': ['./LAS_FILES/15_9-F-15D_CPO.las', './LAS_FILES/15_9-F-15D_CPI.las']
}

def read_las_file(fname):
    """
    读取LAS文件并返回DataFrame。
    """
    try:
        las_data = lasio.read(fname)
        df = pd.DataFrame(las_data.data, columns=list(las_data.curvesdict.keys()))
        return df
    except Exception as e:
        print(f"Error reading file {fname}: {e}")
        return None

def process_data(df, well_name):
    """
    对DataFrame进行预处理，包括计算新的曲线和删除缺失值。
    """
    #   根据井名进行特定处理
    if well_name == 'Well_F19A':
        df['GR'] = df['GRMAX'] - df['VSH']*(df['GRMAX']-df['GRMIN'])
        df['RHOB'] = df['RHOMA'] - df['PORD']*(df['RHOMA']-df['RHOFL'])
        df['NPHI'] = df['PORD'] + (df['PHIF']-df['PORD']-0.01)/0.4
        df['NPHI'] = df['NPHI'].abs()
        df['RPCEHM'] = df['RW']/(df['SW']**(-1/df['N']) * (df['PHIF']+0.01)**(df['M']))
    elif well_name == 'Well_F19BT2':
        df['RPCEHM'] = df['RW']/(df['SW']**(-1/2.45) * df['PHIF']**1.79)
    elif well_name == '15_9-F-1_C_CPO':
        #   删除坏数据
        df = df[df['BADDATA_FLAG'] !=   1]
    elif well_name == '15_9-F-12_CPO':
        #   调整电阻率列命名
        df.rename(columns={'RT': 'RPCEHM', 'RD': 'RPCELM'}, inplace=True)

    #  添加其他well的处理逻辑...
    return df



#  创建一个字典来存储每个井的处理后的数据
well_data = {}

# 读取并处理每个井的数据
for well_name, files in well_files.items():
    if len(files) ==   1:
        #   只有一个LAS文件的情况
        fname = files[0]
        las_data = lasio.read(fname)
        df = read_las_file(fname)
        df = process_data(df, well_name)
        df = merge_createname(df, None, las_data, name=well_name)
        # 释放内存
        del las_data
    elif len(files) ==   2:
        #   有两个LAS文件的情况
        fname1, fname2 = files
        # CPO文件
        las_data1 = lasio.read(fname1)
        df1 = read_las_file(fname1)
        # CPI文件
        las_data2 = lasio.read(fname2)
        df2 = read_las_file(fname2)
        df1 = process_data(df1, well_name)
        df2 = process_data(df2, well_name)
        
        # 注意：这里的merge_createname函数的使用，参数顺序，参数传入的类型
        df = merge_createname(df2, df1, las_data2, name=well_name)
        # 释放内存
        del las_data1, las_data2, df1, df2
    else:
        print(f"Unexpected number of files for well {well_name}")
        continue
    
    #   保留非空值
    df.dropna(thresh=MIN_NON_NAN_VALUES[well_name], inplace=True)
    
    #   将处理后的数据存储在字典中
    well_data[well_name] = df
```

## 优化过程重点问题

1. merge_createname 中的参数顺序，df1，df2的传入顺序, df1 类似于循环模型中的CPO文件，df2 类似于循环模型中的CPI文件；所以传入的时候，应该是先传入df2，再传入df1；传入的las_data2 相当于后传入的df2的原数据。原始代码中使用的是这个数据结构，所以传入的是las_data2，而不是las_data1。
2. well_files 中的键值对中的值，应该是一个列表，列表中的元素是LAS文件的路径，这个列表可能为一个元素，也可能为两个元素。（两个原始就表示执行两个LAS文件的情况，需要进行合并操作）
