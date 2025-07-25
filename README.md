# AD-PDILM: 感知决策一体化大模型

AD-PDILM 是一个感知决策一体化大模型，通过创新的对象级场景表示和基于Transformer的大型语言模型架构，实现完全的感知与决策一体化。模型从视觉大模型提取高质量对象特征，并通过模仿学习方法，利用大规模专家驾驶数据，将场景表征直接映射到未来轨迹。在模拟实验中，AD-PDILM表现出色，达到了目标感知准确率99.74%、水平运动控制误差0.11m、航向控制误差0.14°、路径规划误差0.06m、模型平均响应时间27ms、变道超车成功率99.92%、匝道通过成功率99.95%、在平均车速80km/h下连续自动驾驶2小时等性能指标。

# 内容
* [训练](#训练)
* [评估](#评估)
* [感知AD-PDILM](#感知ad-pdilm)




## 训练
要在数据集上运行AD-PDILM训练，请运行：
```bash
python Model/lit_train.py model=AD-PDILM
```
要更改任何超参数，请查看`Model/AD-PDILM.yaml`。对于一般训练设置（例如，GPU数量），请查看`Model/config.yaml`。

## 评估
这将在指定的基准（默认：longest6）上评估AD-PDILM模型。配置在`carla_agent_files/config`文件夹中指定。

启动Carla服务器（参见[数据生成](#数据生成)）。  
当服务器运行时，启动评估：
```bash
python leaderboard/scripts/run_evaluation.py experiments=AD-PDILMmedium3x eval=longest6
```
你可以在模型文件夹内新创建的评估文件夹中找到评估结果。如果你想要一个（非常简约的）可视化，你可以设置`viz`标志（即，`python leaderboard/scripts/run_evaluation.py user=$USER experiments=AD-PDILMmedium3x eval=longest6 viz=1`）。


## 感知AD-PDILM
我们发布了两个适用于CARLA Leaderboard的AD-PDILM代理。对于SENSORS轨道，我们用感知模块预测路线。在MAP轨道模型中，我们从地图获取路线信息。代码取自[TransFuser (PAMI 2022) repo](https://github.com/autonomousvision/transfuser)并为我们的用例进行了调整。配置在`carla_agent_files/config`文件夹中指定。感知模型的配置在`model/config.py`中。

### SENSORS
启动Carla服务器。  

当服务器运行时，启动评估：
```bash
python leaderboard/scripts/run_evaluation.py experiments=AD-PDILMSubmission track=SENSORS eval=longest6 save_path=SENSORSagent
```
可视化可以通过`viz`标志激活，TransFuser repo中的解锁可以通过`experiments.unblock`标志激活。

### MAP
启动Carla服务器。  

当服务器运行时，启动评估：
```bash
python leaderboard/scripts/run_evaluation.py experiments=AD-PDILMSubmissionMap track=MAP eval=longest6 save_path=MAPagent
```
可视化可以通过`viz`标志激活，TransFuser repo中的解锁可以通过`experiments.unblock`标志激活。
