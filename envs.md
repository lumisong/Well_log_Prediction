# 环境设置

需要安装的包和库：

- skrlearn
- pandas
- numpy
- matplotlib
- seaborn
- scipy
- lasio
- dlisio

## 手动安装

先conda进行大包管理，然后使用pip进行小包管理。

```bash
conda install -c conda-forge pandas
conda install -c conda-forge matplotlib
conda install -c conda-forge scikit-learn
conda install seaborn -c conda-forge

conda update --all
```

```bash
pip install lasio
pip install dlisio
```

## 安装脚本

pip 方案：

```bash
pip install -r requirements.txt
```

conda 方案：

```bash
conda install --file requirements.txt
```

1. 希望使用conda来管理安装，可以创建一个conda环境文件（`.yml`文件），其中列出了所有需要安装的包和库。这样，可以使用`conda env create -f environment.yml`命令来创建一个新的conda环境，并在其中安装所有列出的包。这种方法的优点是它可以确保所有依赖项都是兼容的，并且可以轻松地在不同的系统和环境中重现。

2. 根据在`envs.md`文件中列出的包，`requirements.txt`文件应该包含以下内容：

```text
scikit-learn
pandas
numpy
matplotlib
seaborn
scipy
lasio
dlisio
```

注意，`scikit-learn`是`skrlearn`的正确拼写。如果您的项目确实需要`skrlearn`，那么可能是一个拼写错误，或者您可能需要查找一个名为`skrlearn`的Python库。如果是拼写错误，请使用正确的库名`scikit-learn`。

创建一个conda环境文件来替代`requirements.txt`，您可以创建一个名为`environment.yml`的文件，内容如下：

```yaml
name: well_log_prediction
channels:
 - defaults
dependencies:
 - scikit-learn
 - pandas
 - numpy
 - matplotlib
 - seaborn
 - scipy
 - lasio
 - dlisio
```

然后，可以使用以下命令来创建并激活新的conda环境：

```bash
conda env create -f environment.yml
conda activate well_log_prediction
```

这将创建一个名为`well_log_prediction`的新环境，并在其中安装所有列出的依赖项
