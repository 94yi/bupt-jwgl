# bupt-jwgl

北邮新版教务系统学生评教自动化脚本。

脚本当前适配 `https://jwgl.bupt.edu.cn` 的强智教务评教流程，会自动登录、查找待评价课程、保存评价结果，并可选择是否在保存完成后统一提交。

## 功能

- 可以自动评教。

## 环境要求

- Python 3
- 能访问北邮教务系统的网络环境
- Windows、Linux、macOS 均可运行

如果不在校园网环境中，请先连接可访问教务系统的网络或 VPN。

## 使用方法

在项目目录运行：

```bash
python auto_evalued
```

按提示输入：

```text
ID: 学号
PASSWORD: 教务系统密码
COMMENT: 主观评价文字
degree(1-5, 1 is the best): 评分档位
submit after saving all? (y/N): 是否保存后统一提交
```

示例：

```text
ID: 2024210000
PASSWORD: ********
COMMENT: 老师认真负责，讲解清晰，课程内容安排合理。
degree(1-5, 1 is the best): 1
submit after saving all? (y/N): n
```

## 参数说明

`degree` 为评分档位，范围是 `1-5`：

- `1`：最高评价
- `2`：较高评价
- `3`：中等评价
- `4`：较低评价
- `5`：最低评价

新版教务系统不允许所有题目选择完全相同的选项。脚本会以你输入的档位为主，并自动将最后一题错开一档，以通过页面校验。

`submit after saving all?`：

- 输入 `y`：保存所有课程评价后统一提交。
- 直接回车或输入 `n`：只保存，不最终提交，可之后手动进入教务系统检查并提交。

建议第一次使用时选择 `n`，确认保存结果无误后再手动提交。

## 自定义教务地址

默认地址为：

```text
https://jwgl.bupt.edu.cn
```

如果教务地址发生变化，可以通过环境变量覆盖：

PowerShell：

```powershell
$env:BUPTJW_BASE_URL="https://example.edu.cn"
python auto_evalued
```

Bash：

```bash
BUPTJW_BASE_URL="https://example.edu.cn" python auto_evalued
```

## 常见问题

### `cannot connect to jwgl.bupt.edu.cn:443`

当前网络无法连接教务系统。请确认已经连接校园网或可访问教务的 VPN。

### `login failed: 用户名/密码/验证码`

脚本没有成功登录。请先确认同一账号密码可以在浏览器正常登录新版教务系统。

如果浏览器登录时需要验证码、短信、统一认证跳转或其他二次验证，脚本无法自动处理。

### `no evaluation batch found`

当前账号没有可评价批次，或评教入口暂未开放。

也可能是教务系统页面结构更新，需要重新适配。

### GitHub 推送受代理影响

如果 Git 操作报 `127.0.0.1:9` 或连接重置，检查代理环境变量：

```powershell
Get-ChildItem Env: | Where-Object { $_.Name -match 'proxy' }
```

临时清除后再推送：

```powershell
Remove-Item Env:HTTP_PROXY,Env:HTTPS_PROXY,Env:ALL_PROXY,Env:GIT_HTTP_PROXY,Env:GIT_HTTPS_PROXY -ErrorAction SilentlyContinue
git push
```

## 注意事项

- 本项目仅用于学习和个人自动化实践。
- 脚本会使用你的教务账号和密码登录，请只在可信环境运行。
- 最终提交后通常不能修改，提交前请确认评价内容。
- 教务系统页面结构可能变化，若运行异常，需要根据最新 HTML 重新适配。
