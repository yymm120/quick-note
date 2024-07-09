## Rust和WebAssembly两大用例

- 整个  Web 应用都基于 Rust 开发！  
- 在JavaScript 前端中使用 Rust。



### 依赖wasm-bindgen

```toml
wasm-bindgen = "0.2"
```

`wasm-bindgen` 来提供  JavaScript 和 Rust 类型之间的桥梁。它允许 JavaScript 使用字符串调用 Rust API，或调用 Rust 函数来捕获  JavaScript 异常。



### wasm构建工具 wasm-pack

```bash
cargo install wasm-pack
```

wasm-pack用于将rust代码编译成wasm, 同时将wasm包装为npm包。



### rust调用JS

```rust
extern crate wasm_bindgen;
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
extern {
    pub fn alert(s: &str);
}
```

- 第一行, 声明一个外部库`wasm_bindgen` 
- 第二行, 导入这个外部库`wasm_bindgen`的所有`prelude`模块。
- 第四行, 在 `#[]` 中的内容叫做  "属性。
- 第五行, 提供一个函数签名。`extern`告诉`rust`这个函数来自外部。

```bash
# 编译成wasm, 同时将wasm包装为npm包。
wasm-pack build --scope mynpmusername
```



### JS调用rust

```rust
extern crate wasm_bindgen;
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn greet(name: &str) {
    alert(&format!("Hello, {}!", name));
}
```

- 第一行, 声明一个外部库`wasm_bindgen` 
- 第二行, 导入这个外部库`wasm_bindgen`的所有`prelude`模块。

- 第四行, 在 `#[]` 中的内容叫做  "属性。
- 第五行, 提供一个`pub`函数, 它将提供给外部(非rust)语言使用。

```bash
# 编译成wasm, 同时将wasm包装为npm包。
wasm-pack build --scope mynpmusername
```



### wasm-pack build命令做了什么

```bash
# 编译成wasm, 同时将wasm包装为npm包。
wasm-pack build --scope mynpmusername
```

- 这个命令将rust编译成wasm。
- 生成一个js文件包装wasm, 以便npm可以识别。
- 创建一个pkg文件夹, 将js和wasm移入其中。
- 读取 `Cargo.toml` 并生成相应的 `package.json`。 
- 复制 `README.md` (如果有的话)  到文件夹中。



### 发布wasm到npm上

第一步, 添加一个npm账号。如果没有, 需要在官网注册。

```bash
npm addusesr
```

第二步, 使用wasm-pack构建一个wasm, 同时将wasm包装为npm包。

```bash
wasm-pack build --scope mynpmusername
```

第三步, 使用`npm publish`命令发布到npm, 供所有人使用。

```bash
cd pkg
npm publish --access=public
```









