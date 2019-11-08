# conda命令的使用

```python
# 激活、退出虚拟环境
1.  conda activate learn
2.  conda deactivate
# 创建虚拟环境
1.  conda create -n learn
    conda create -n py3 python=3
    conda create -n py2 python=2
    conda env export > environment.yaml # 共享环境
    conda env create -f environment.yaml # 通过环境文件创建环境
    conda env remove -n env_name
```