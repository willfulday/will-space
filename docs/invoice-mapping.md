# 意大利电子发票字段 Mapping 规范

> 适用范围：SAP 销售/开票凭证 → FatturaPA XML（意大利电子发票）
> 维护方式：任何人修改映射前先在本文件留痕，再改代码或配置。

## 1. 抬头映射

| SAP 字段 (VBRK/BKPF) | FatturaPA XML 节点 | 说明 | 示例值 |
| :--- | :--- | :--- | :--- |
| `BUKRS` | `<IdFiscaleIVA>` | 公司税号 | `IT99` |
| `BLART` (如 DR) | `<TipoDocumento>` | 单据类型代码 | `TD02` (预付款发票) |
| `WAERK` | `<Divisa>` | 交易币种 | `EUR` |
| `BELNR` | `<Numero>` | 发票号 | `5100000123` |
| `ERDAT` | `<Data>` | 发票日期（YYYY-MM-DD） | `2026-11-24` |
| `KUNRG` | `<CessionarioCommittente>` | 收票方客户 | `0000100234` |

## 2. 行项目映射

| SAP 字段 (VBAP) | FatturaPA XML 节点 | 说明 | 示例值 |
| :--- | :--- | :--- | :--- |
| `POSNR` | `<NumeroLinea>` | 行号（从 1 起，左补零到 4 位） | `0001` |
| `ARKTX` | `<Descrizione>` | 行项目描述 | `Consulenza SAP` |
| `KWMENG` | `<Quantita>` | 数量（小数用 `.` 分隔） | `1.00` |
| `NETWR` | `<PrezzoUnitario>` | 单价（不含税） | `2500.00` |
| `MWSBP` | `<AliquotaIVA>` | 税率（按行计算得出） | `22.00` |

## 3. 校验规则

1. `WAERK` 必须为 ISO 4217 三位币种码，`EUR` 之外需单独确认是否允许跨境开票。
2. 金额一律保留 2 位小数，禁止出现千分位分隔符。
3. `<IdFiscaleIVA>` 需带国家前缀（如 `IT`），格式为 `IT` + 11 位数字。
4. 客户主数据缺失 Codice Fiscale / P.IVA 时，接口应报错而非回退默认值。

## 4. 变更记录

- 2026-11-24: 初始化意大利预付款发票 Mapping 规则。
