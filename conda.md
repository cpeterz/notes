#conda

##1. 常用的conda命令有

```bash
source activate // 切换到base环境（默认环境）
conda env list // 列出conda管理的所有环境
conda create -n learn python=3.5 // 创建一个名为learn的环境并指定python版本
source activate learn //激活当前环境
activate learn // 切换到learn环境
conda env create -f environment.yaml // 用配置文件创建新的虚拟环境
```
##2.常用conda对包的管理命令有

```bash
conda list // 列出当前环境的所有包
conda install requests 安装requests包
conda remove requeremosts 卸载requets包
conda remove -n learn --all // 删除learn环境及下属所有包
conda update requests 更新requests包
conda env export > environment.yaml // 导出当前环境的包信息
```

