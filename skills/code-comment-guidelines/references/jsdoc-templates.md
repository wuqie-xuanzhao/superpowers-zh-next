# JSDoc 模板参考

**加载时机：** 为导出函数、类、接口编写 JSDoc 时参考。

## 函数

### 基本函数

```typescript
/**
 * 将摄氏温度转换为华氏温度
 *
 * @param celsius - 摄氏温度值
 * @returns 华氏温度值
 */
export function celsiusToFahrenheit(celsius: number): number {
  return celsius * 9 / 5 + 32;
}
```

### 带异常的函数

```typescript
/**
 * 解析用户输入的日期字符串
 *
 * @param input - 用户输入的日期字符串，支持 YYYY-MM-DD 和 MM/DD/YYYY 格式
 * @returns 解析后的 Date 对象
 * @throws {SyntaxError} 输入格式无法识别时抛出
 * @throws {RangeError} 日期值超出有效范围时抛出
 *
 * @example
 * parseUserDate('2024-03-15')     // Date(2024, 2, 15)
 * parseUserDate('03/15/2024')     // Date(2024, 2, 15)
 */
export function parseUserDate(input: string): Date {
  // ...
}
```

### 泛型函数

```typescript
/**
 * 从数组中查找第一个满足条件的元素，找不到则返回默认值
 *
 * @typeParam T - 数组元素的类型
 * @param items - 待搜索的数组
 * @param predicate - 查找条件函数
 * @param defaultValue - 未找到时的默认返回值
 * @returns 第一个满足条件的元素，或默认值
 */
export function findOrDefault<T>(
  items: T[],
  predicate: (item: T) => boolean,
  defaultValue: T,
): T {
  return items.find(predicate) ?? defaultValue;
}
```

### 异步函数

```typescript
/**
 * 从远程服务获取用户资料
 *
 * @param userId - 用户唯一标识符
 * @param options - 请求选项
 * @param options.timeout - 请求超时时间（毫秒），默认 5000
 * @param options.retries - 失败重试次数，默认 3
 * @returns 用户资料对象
 * @throws {NetworkError} 网络不可达或超时时抛出
 * @throws {AuthError} 凭证过期或无效时抛出
 */
export async function fetchUserProfile(
  userId: string,
  options?: { timeout?: number; retries?: number },
): Promise<UserProfile> {
  // ...
}
```

## 类

### 基本类

```typescript
/**
 * 管理客户端与 WebSocket 服务器的连接生命周期
 *
 * @example
 * ```typescript
 * const ws = new WebSocketConnection('wss://api.example.com');
 * await ws.connect();
 * ws.on('message', (data) => console.log(data));
 * await ws.disconnect();
 * ```
 */
export class WebSocketConnection {
  /**
   * 创建 WebSocket 连接实例
   *
   * @param url - WebSocket 服务器地址
   * @param protocols - 子协议列表（可选）
   */
  constructor(url: string, protocols?: string[]) { /* ... */ }

  /**
   * 建立连接
   *
   * @returns 连接建立完成后的 Promise
   * @throws {ConnectionError} 连接失败时抛出
   */
  async connect(): Promise<void> { /* ... */ }

  /**
   * 断开连接并清理资源
   */
  async disconnect(): Promise<void> { /* ... */ }
}
```

## React 组件

### Props 接口

```typescript
/**
 * 用户头像组件的属性
 */
export interface AvatarProps {
  /** 用户头像 URL，为空时显示默认头像 */
  src?: string;
  /** 用户显示名称，用于 alt 文本和无头像时的占位 */
  name: string;
  /** 头像尺寸 */
  size?: 'sm' | 'md' | 'lg';
  /** 点击头像时的回调 */
  onClick?: (userId: string) => void;
}
```

### 组件

```typescript
/**
 * 用户头像——显示用户图片或首字母占位
 *
 * @example
 * ```tsx
 * <Avatar name="Alice" src="/avatars/alice.jpg" size="md" />
 * <Avatar name="Bob" size="sm" />
 * ```
 */
export function Avatar({ src, name, size = 'md', onClick }: AvatarProps) {
  // ...
}
```

### Hook

```typescript
/**
 * 管理分页状态的自定义 Hook
 *
 * @param totalItems - 总条目数
 * @param itemsPerPage - 每页条目数，默认 20
 * @returns 分页状态和操作方法
 * @returns currentPage - 当前页码（从 1 开始）
 * @returns totalPages - 总页数
 * @returns goToPage - 跳转到指定页
 * @returns nextPage - 下一页
 * @returns prevPage - 上一页
 *
 * @example
 * ```tsx
 * const { currentPage, totalPages, nextPage } = usePagination(100, 20);
 * ```
 */
export function usePagination(totalItems: number, itemsPerPage?: number) {
  // ...
}
```

## 工具类型

### 联合类型

```typescript
/**
 * 订单状态流转
 *
 * - `pending` - 待支付（初始状态）
 * - `paid` - 已支付，等待发货
 * - `shipped` - 已发货，在途
 * - `delivered` - 已签收
 * - `cancelled` - 已取消（终态）
 * - `refunded` - 已退款（终态）
 */
export type OrderStatus =
  | 'pending'
  | 'paid'
  | 'shipped'
  | 'delivered'
  | 'cancelled'
  | 'refunded';
```

### 工具类型

```typescript
/**
 * 将 T 的所有属性变为可选，并允许嵌套的深层部分可选
 *
 * @typeParam T - 目标类型
 */
export type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};
```

## 常量/配置

```typescript
/**
 * HTTP 请求默认配置
 *
 * @property timeout - 请求超时时间（毫秒）
 * @property retries - 失败重试次数
 * @property baseURL - API 基础路径
 */
export const DEFAULT_HTTP_CONFIG = {
  timeout: 5000,
  retries: 3,
  baseURL: '/api/v1',
} as const;
```

## 写作规范

| 规范 | 好的 | 差的 |
|------|------|------|
| 第一行 | 简短摘要（祈使句） | 详细描述 |
| 参数描述 | 说明含义和格式 | 重复参数名 |
| @returns | 描述返回内容 | 只写"返回值" |
| @throws | 列出所有可能的异常 | 只写"可能抛异常" |
| @example | 展示典型用法 | 空的或无意义的示例 |
| 语言 | 中文描述，英文代码 | 中英混搭不一致 |