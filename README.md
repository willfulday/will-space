# SAP 接口调试与电子发票资产库

用 VS Code 管理 SAP 接口调试报文与业务 Mapping 文档的工程模板。

- 接口请求用 `REST Client` 插件直接在编辑器内发起，取代 Postman
- 业务 Mapping 用 Markdown 维护（Docs as Code）
- 每一次报文与文档变更都用 Git / GitHub 留痕、可回溯

## 目录结构

```
sap-interface-lab/
├── README.md               # 项目主说明文档（本文件）
├── docs/
│   └── invoice-mapping.md  # 业务字段 Mapping 规范
└── requests/
    └── test-api.http       # 接口调试请求文件
```

## 环境要求

| 组件 | 说明 |
| :--- | :--- |
| VS Code | 1.80 及以上 |
| REST Client (`humao.rest-client`) | 在编辑器内点 `Send Request` 发请求 |
| Markdown All in One (`yzhang.markdown-all-in-one`) | `Cmd + K, V` 实时预览 Markdown |

> 注意：若企业网络拦截了 VS Code 扩展市场，可从 Open VSX 下载 `.vsix` 后用
> `code --install-extension <file>.vsix` 离线安装。

## 使用方式

1. 打开 `requests/test-api.http`，在每个 `###` 请求块上方点击 `Send Request`，右侧即显示状态码、Header 与响应正文。
2. 打开 `docs/invoice-mapping.md`，按 `Cmd + K, V` 打开预览，边写边看排版。
3. 改完报文或文档后，用 VS Code 侧边栏的 Source Control 面板提交，写清变更原因（如：调整 `<Divisa>` 取值来源）。

## 切换到真实 S/4HANA 系统

`requests/test-api.http` 默认指向 `httpbin.org` 做无风险演练。接入真实系统时：

1. 在请求文件中使用变量，避免把主机名与凭证散落在正文里：
   ```
   @host = https://<your-s4-host>:44300
   @client = 100
   ```
2. 凭证不要写进仓库。建议在 VS Code 用户设置 `rest-client.environmentVariables` 中维护，
   或使用 `.env` / 系统 Keychain，并把敏感文件加入 `.gitignore`。
3. OData 常用路径：
   - `GET {{host}}/sap/opu/odata/sap/API_BILLING_DOCUMENT_SRV/A_BillingDocument?$top=5`
   - `GET {{host}}/sap/opu/odata/sap/API_SUPPLIERINVOICE_PROCESS_SRV/A_SupplierInvoice?$top=5`

## 变更记录

- 2026-11-24: 初始化工程，建立接口调试与发票 Mapping 规范骨架。
