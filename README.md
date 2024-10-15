# CZDB Searcher

CZDB Searcher 是一个用于高效 IP 地理位置查询的 Python 库，它使用紧凑的数据库格式和二叉树搜索算法，提供快速准确的 IP 查找功能。


# 感谢

纯真(CZ88.NET)自2005年起一直为广大社区用户提供社区版IP地址库，只要获得纯真的授权就能免费使用，并不断获取后续更新的版本。如果有需要免费版IP库的朋友可以前往纯真的官网进行申请。

纯真除了免费的社区版IP库外，还提供数据更加准确、服务更加周全的商业版IP地址查询数据。纯真围绕IP地址，基于 网络空间拓扑测绘 + 移动位置大数据 方案，对IP地址定位、IP网络风险、IP使用场景、IP网络类型、秒拨侦测、VPN侦测、代理侦测、爬虫侦测、真人度等均有近20年丰富的数据沉淀。

## 特点

- IP 地理位置查询
- 支持 IPv4 和 IPv6 地址
- 简单易用的 API

## 性能

请尽量使用单例来查询，避免每次查询都初始化DbSearcher，这会带来性能瓶颈。**同时要注意BTREE模式查询时不是线程安全的**。

## 安装

在项目目录下运行以下命令来安装 CZDB Searcher 所需的第三方库：

```bash
pip install msgpack
pip install pycryptodome
```

## 使用方法
以下是一个快速开始的示例：
    
```python
import sys
from czdb.db_searcher import DbSearcher

database_path = "/path/to/your/database.czdb"
query_type = "BTREE"
key = "YourEncryptionKey"
ip = "8.8.8.8"

db_searcher = DbSearcher(database_path, query_type, key)

try:
    region = db_searcher.search(ip)
    print("搜索结果：")
    print(region)
except Exception as e:
    print(f"An error occurred during the search: {e}")

db_searcher.close()
```

请将 database_path 和 key 替换为您项目中实际的数据库路径和加密密钥。

## 配置

`DbSearcher` 构造函数接受以下参数：

- `databasePath`：您的 CZDB 数据库文件路径。
- `searchMode`：搜索模式（例如，"BTREE" 或者 "MEMORY"）。
- `encryptionKey`：密钥。

数据库文件和密钥可以从 [www.cz88.net](https://cz88.net/geo-public) 获取。

## 模式选择

- **批量查询**：对于批量查询，建议使用 "MEMORY" 模式。这是因为 "MEMORY" 模式会将数据库加载到内存中，从而提高查询速度，尤其是在处理大量查询时。虽然这会增加内存的使用，但可��显著提高批量处理的效率。

- **少量查询**：如果每个请求只查询少量的 IP 地址，那么使用 "BTREE" 模式可能更合适。"BTREE" 模式不需要预先加载整个数据库到内存中，适用于处理较少量的查询请求，可以减少内存的使用，同时保持良好的查询性能。

## 许可证

该项目在 Apache-2.0 许可下授权 - 详情见 LICENSE 文件。
