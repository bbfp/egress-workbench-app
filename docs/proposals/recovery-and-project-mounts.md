# Two proposals: connection recovery and additional project directories

These are requests for discussion, based on local experience with EWB 0.3.3. They are not instructions for, or claims about, the behavior of 0.5.2. See the [current connection and project workflow](../../README.md) for the shipped behavior.

## 1. Resume paused work after the same exit is verified again

When a Mac wakes or the network briefly drops, the project can remain paused waiting for another confirmation. Someone working remotely may only discover this hours later. Offer an opt-in on an already approved connection: **resume after fresh verification of the same exit**. Keep manual confirmation as the default.

| After an interruption | Expected result |
|---|---|
| The native gateway freshly verifies the same node, policy, exit IP, region and time zone; the transport is ready. | Resume only work paused by this interruption, through the existing approval flow. Do not start stopped projects or replay commands. |
| The node, policy or exit changed; verification is missing, expired or conflicting; or the reason is unknown. | Keep traffic blocked. Show why recovery did not happen and what needs review. |
| The user paused the connection, stopped the gateway or is switching nodes. | Preserve the user's action, even if the old exit becomes reachable again. |

## 2. Let each project share additional directories

A project may keep code in one folder and datasets or outputs elsewhere. Let users pick an existing Mac directory and grant **read-only** (default) or **read/write** access to it in one project. No extra directories should be shared by default.

- In project A, add a dataset read-only and an output directory read/write. The container can read the dataset but cannot modify it; a result written to the output directory appears on the Mac. Project B gets neither mount.
- Show the Mac path, container path and access mode before applying. Reject conflicting mounts or ones that would silently hide existing files. If the project container must be recreated, tell the user first and preserve project data.
- Remember grants across project restarts and supported upgrades. Removing a grant removes access through that mount without deleting its contents on the Mac. The environment check should report the actual access mode and any mismatch.

---

# 两项功能提案：连接恢复与项目附加目录

以下需求来自一套 0.3.3 部署的实际使用，不是 0.5.2 的功能说明，也没有在 0.5.2 上复测。已发布的行为见[当前使用说明](../../README.zh-CN.md)。

## 1. 同一出口重新核对通过后，恢复暂停的工作

Mac 睡眠唤醒或网络短暂中断后，项目有时会停在等待确认；远程用户可能几小时后才发现任务没继续。建议让用户对**已经批准的连接**选择“重新核对为同一出口后自动恢复”，默认仍由人确认。

| 中断后的情况 | 预期行为 |
|---|---|
| 原生网关重新核对成功，节点、策略、出口 IP、地区和时区都与上次批准的一致，传输已就绪。 | 走已有批准流程，只恢复本次中断暂停的工作；不启动本来就停止的项目，不重跑命令。 |
| 节点、策略或出口改变，验证结果缺失、过期或互相矛盾，或者无法判断中断原因。 | 继续阻断流量，显示为什么没有恢复、需要用户核对什么。 |
| 用户主动暂停、停止网关，或正在切换节点。 | 尊重用户操作；即使原出口恢复可用，也不自动恢复。 |

## 2. 每个项目可以添加多个目录，并分别设置访问权限

代码、数据集和实验输出经常不在同一目录。建议允许用户按项目选择额外的 Mac 目录，为每个目录单独授予**只读**（默认）或**读写**权限。初始不共享任何附加目录。

- 项目 A 添加只读数据集和读写输出目录：容器能读数据集但不能改写，写入的结果在 Mac 上可见。项目 B 不会得到这两个挂载。
- 应用前显示 Mac 路径、容器路径和访问模式；拒绝冲突或会静默遮住已有文件的挂载。确需重建容器时，先告知用户，并保留项目数据。
- 项目重启或受支持的升级后保留授权。撤销时只移除该挂载的访问权限，不删除 Mac 上的文件。“检查环境”应能显示实际访问模式及不一致之处。
