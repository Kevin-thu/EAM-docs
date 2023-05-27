# 前端模块设计
前端整体采用 react 作为基本框架实现，使用 AntDesign / AntDesign Pro / AntDesign mobile 等组件库辅助 UI 设计，使用 next.js 和 react router 管理路由，使用 react context 管理全局组件。整体来看，我们奉行分层解耦合的设计理念，分为 UI 层、逻辑层与网络层三部分：UI 层主要是面向用户的组件渲染呈现，逻辑层主要负责全局变量的管理与数据流的管理，网络层主要负责与后端进行通信。

下面介绍我们源代码（ `/src` 目录下）中各部分模块设计。

## 页面模块 `/pages`
在 `/pages` 目录下放有我们各个功能对应的主页面 UI 模块。

### `_app.tsx`
对应组件 `App`，是所有组件的根节点。除了部分无需登录即可访问的界面（登陆界面、资产信息扫描界面、审批单界面）直接渲染外，其他界面如未登录都会渲染主页登录界面；在验证登录后除了渲染相应界面还会同时渲染门户旁栏。

### `index.tsx`
对应组件 `Homepage`，使用 Cookies 和 `token` 状态判断用户是否登录，若未登录则渲染登录、注册界面，若已登录则跳转至主页界面。 

### `welcome.tsx`
对应组件 `WelUI`，渲染登录后的主页界面。

### `user.tsx`
对应组件 `UserUI`，渲染用户列表界面。

### `entity.tsx`
对应组件 `EntityUI`，渲染实体列表界面。

### `dept.tsx`
对应组件 `DeptUI`，渲染部门树与部门列表界面。

### `datacenter.tsx`
对应组件 `DataCenterUI`，获取最新日志并渲染日志界面。

### `stat.tsx`
对应组件 `StatUI`，渲染资产统计信息界面。

### `filetask.tsx`
对应组件 `Filetask`，渲染异步任务列表界面。

### `3siderurl.tsx`
对应组件 `URLUI`，渲染第三方 URL 应用管理界面。

### `scan.tsx`
对应组件 `AssetScanInfo`，渲染移动端扫码后看到的资产信息界面。

### `task.tsx`
对应组件 `TaskInfo`，渲染审批单界面。

### `assets/index.tsx`
对应组件 `AssetsDeftypeUI`，渲染资产分类定义树结构界面。

### `assets/defattr.tsx`
对应组件 `AssetsDefattrUI`，渲染资产属性定义界面。

### `assets/deflabel.tsx`
对应组件 `AssetsDeflabelUI`，渲染资产标签格式定义界面。

### `assets/[id].tsx`
对应组件 `AssetUI`，渲染资产详情界面。

### `userasset/index.tsx`
对应组件 `CheckUI`，渲染个人资产界面。

### `userasset/apply.tsx`
对应组件 `ApplyUI`，渲染资产申请与审批界面。

## 组件模块 `/components`
在 `/components` 下放了页面渲染中需要用到的所有组件，并按照上述分类进行了目录划分，在此仅选择若干有代表性的组件进行讲解。

### `UserTableUI`
用户列表对应组件，基于 ProTable 实现。从后端获取当前用户所在实体用户信息列表（若为超级管理员会获取所有用户信息），以表格形式展示在前端。若为系统管理员，还可以对成员进行操作。获取的用户信息形式为：

属性名 | 类型 | 功能或含义
---------|----------|---------
 `user` | `string` | 用户名
 `user_type` | `{name: string}[]` | 用户类型
 `entity_name` | `string` | 实体名
 `dept_name` | `string` | 部门名
 `valid` | `boolean` | 是否激活

### `UserModal`
创建或修改用户对应的弹框表单，基于 ModalForm 实现。属性包括：

属性名 | 类型 | 功能或含义
---------|----------|---------
 `isCreate` | `boolean` | 创建还是修改
 `user` | `string` | 用户名
 `privilege` | `DefaultOptionType[]` | 用户权限（label为权限名称）
 `entity_name` | `string` | 实体名
 `dept_name` | `string` | 部门名
 `valid` | `boolean` | 是否激活
 `handleRefresh` | `() => void` | 处理自动刷新

若为超级管理员，会从后端获取实体列表供选择；并根据所选实体/企业系统管理员所在实体获取部门列表供选择。选择完用户属性后，会发送给后端进行创建/修改。

