# 县域人口队列与学位压力分析

cohorts 校验人口单元并推进年龄队列；forecast 提供递进率估计、情景与历史回看；school_pressure 将叶子需求聚合为学段压力；releases 保存输入版本与运行结果。

同一年龄单元的地区与城乡键需要一致。情景假设不等于概率区间，零实际人数不能当作百分比误差的正常分母。演练使用合成人口和学校容量，仅作为规划分析示例。

## 运行

Python 3.11，使用标准库及内置 SQLite，无第三方依赖。无需安装第三方包。 在项目根目录执行，命令不访问公网。演示在临时目录内创建数据，结束后自行清理，不需要预置凭据或另启服务。

```sh
python3 cohort_cli.py demo --fixture fixtures/county.json
```

## 开发检查

测试覆盖当前产品的核心计算、边界输入、事务或版本恢复及异常路径。编译或语法检查可单独运行。

```sh
python3 -m unittest discover -s tests -v
python3 -m compileall -q cohorts forecast school_pressure releases cohort_cli.py
```

## 容器复现

先在有网络的准备阶段取得运行镜像。之后可在断网容器中复制项目到临时工作区运行，避免向源目录写入缓存。

```sh
docker build -t local/analysis-product .
docker run --rm --network none local/analysis-product
```

具体接口参数可通过主入口的帮助命令查看。数据库、归档和导出文件应存入调用者指定的工作目录，不提交到源码仓库。
