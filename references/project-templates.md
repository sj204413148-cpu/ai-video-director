# 项目与产物模板

按需创建项目产物，未开始的阶段不生成虚构审核或费用。

## 目录

```text
<project>/
  project.json
  requirements.md
  decisions.md
  costs.csv
  story.md
  storyboard.csv
  palette.md
  style-guide.md
  boards/
  assets/
  frames/<shot-id>/vNN/
  prompts/
  reviews/
  tasks/
  clips/<shot-id>/vNN/
  edits/vNN/
  delivery/
  lessons.md
```

## project.json

```json
{
  "name": "待填写",
  "stage": "brief",
  "duration_seconds": null,
  "aspect_ratio": null,
  "delivery": {"width": null, "height": null, "fps": null},
  "audio": {"music": null, "dialogue": null, "subtitles": null},
  "budget": {"currency": "CNY", "limit": null, "spent": 0, "reserved": 0},
  "approval_policy": "per_submission",
  "approvals": [],
  "pending_questions": []
}
```

`null` 表示未确认，不是允许自行选择。`reserved` 包括已提交尚未结算的任务。允许新任务之前检查 `spent + reserved + 本次估算上限 <= limit`；尚无可信报价或上限时暂停付费提交。实际费用未知不可记为零。

## requirements.md

每项一行：`ID | 用户原话/来源 | 规范化要求 | 验收方式 | 状态`。
状态：待确认、通过、失败、未验证。不得把偏好猜测写成用户要求。

## decisions.md

每项记录时间、阶段、决策、备选方案、依据、批准者、批准范围、关联文件版本。费用批准保存用户明确授权的来源；委托代理不能自我扩大权限。

## storyboard.csv 表头

```csv
shot_id,start_seconds,end_seconds,edit_seconds,generation_seconds,shot_size,camera_angle,camera_move,move_reason,screen_direction,action_order,expression,motion_subject,transition,palette,audio,risk,first_frame,last_frame,status
```

时间合计检查：`end-start=edit_seconds`，总片长匹配目标；重叠转场需要计入最终时长计算。生成秒数不能短于需要截取的时段，且必须符合当前服务档位。

## costs.csv 表头

```csv
record_id,shot_id,version,provider,model,task_id,generated_seconds,estimated_upper_bound,actual_cost,currency,status,approval_ref,created_at
```

一个付费任务一行。`actual_cost` 未知留空；失败可能仍计费，按账单核实。预算保留额直到费用结算后才能释放，不因请求超时直接释放。

## 镜头生成任务记录

```json
{
  "shot_id": "S001",
  "version": "v01",
  "model": null,
  "task_id": null,
  "status": "not_submitted",
  "prompt_file": "prompts/S001-v01.md",
  "first_frame": "frames/S001/v01/first.png",
  "last_frame": "frames/S001/v01/last.png",
  "preflight_review": "reviews/preflight-S001-v01.md",
  "approval_ref": null,
  "estimated_upper_bound": null,
  "actual_cost": null,
  "output_file": null
}
```

请求日志不保存 API 密钥、Authorization 头、可复用凭据或带敏感签名的 URL。按平台数据安全要求管理上传资产。

## palette.md / style-guide.md

- 主色、辅色、强调色、背景色及用途，颜色值。
- 每场明暗关系与主体分离；灰度检查结论与未验证项。
- 线条、材质、光照、镜头风格；样帧批准记录。
- 每个角色 ID、服饰、体貌、表情范围、固定特征位置；人物左右与画面左右分别描述。
- 道具 ID、数量、形状、大小、出入镜规则。
- 所有已批准参考资产的路径和版本。

## 交付清单

成片路径、尺寸、帧率、时长、编码、音轨、色彩空间；可完整解码检查结果；7 类制作资料（故事、分镜、设定、首尾帧、提示词、审核、费用）；授权风险；人工动态/声音验收结果；未解决问题；重现后期所需命令与输入列表。
