# homework3



## 新建模块rust代码

```
cd linux/sample/rust
vim rust_helloworld.rs
```

rust_helloworld.rs

```
// SPDX-License-Identifier: GPL-2.0
//! Rust minimal sample.
      
use kernel::prelude::*;
      
module! {
  type: RustHelloWorld,
  name: "rust_helloworld",
  author: "whocare",
  description: "hello world module in rust",
  license: "GPL",
}
      
struct RustHelloWorld {}
      
impl kernel::Module for RustHelloWorld {
  fn init(_name: &'static CStr, _module: &'static ThisModule) -> Result<Self> {
      pr_info!("Hello World from Rust module\r\n");
      Ok(RustHelloWorld {})
  }
}
```



## 修改 Makefile

```
obj-$(CONFIG_SAMPLE_RUST_HELLOWORLD)            += rust_helloworld.o
```

![image-20231109213333261](image/homework3/image-20231109213333261.png)



## 修改 Kconfig



```
config SAMPLE_RUST_HELLOWORLD
        tristate "Print Helloworld in Rust"
        help
          This option builds the hello world for Rust.

          If unsure, say N.
```

![image-20231109213317757](image/homework3/image-20231109213317757.png)



## 编译内核

```
# 回到 linux 目录下
cd ../../
make LLVM=1 distclean
make x86_64_defconfig
make LLVM=1 menuconfig
```

Kernel hacking

  ---> Sample Kernel code

​      ---> Rust samples

​              ---> <M>Print Helloworld in Rust (NEW)

下图位置中按 y 选中，并回车

![image-20231109213538128](image/homework3/image-20231109213538128.png)

下图位置中按 y 选中，并回车

![image-20231109213635693](image/homework3/image-20231109213635693.png)

按M选中，并保存退出。注意这里是按的 M。

![image-20231109224344826](image/homework3/image-20231109224344826.png)

 编译

```
make LLVM=1 -j8
```



![image-20231109225253549](image/homework3/image-20231109225253549.png)



## 仿真运行



```
cd ../src_e1000
cp ../linux/samples/rust/rust_helloworld.ko rootfs
./build_image.sh

insmod rust_helloworld.ko
```



![image-20231109230618410](image/homework3/image-20231109230618410.png)

