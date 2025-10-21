

# Shamir's Secret Sharing / Shamir秘密共享算法

[English](#english) | [中文](#中文)

---

## English

### Overview

This project implements **Shamir's Secret Sharing** algorithm in C, which allows you to split a secret into multiple shares and reconstruct the original secret using a threshold number of shares. This is a cryptographic technique that provides security through distribution - no single share reveals any information about the secret.

### Features

- **Threshold Secret Sharing**: Split a secret into `n` shares with a threshold of `t` shares needed for reconstruction
- **Secure Implementation**: Uses finite field arithmetic over GF(257) for cryptographic security  
- **Flexible Input**: Supports both hexadecimal string and binary data input
- **Cross-platform**: Written in standard C, compatible with various operating systems

### Algorithm Details

- **Field**: Operations performed in Galois Field GF(257) using prime 257
- **Polynomial**: Uses Lagrange interpolation for secret reconstruction
- **Security**: Requires exactly `t` shares to reconstruct the secret; `t-1` shares provide no information

### Build Instructions

```bash
cd src
make
```

This will generate the `shamir_test` executable.

### Usage

#### Command Line Interface

```bash
./shamir_test <secret_in_hex>
```

**Example:**
```bash
./shamir_test 12345678
```

**Output:**
```
0103AA5D0D82692A9A2260
0203AA13CECD74B8520113  
0303AA54731355DE5FD552
0403AA1FFE560C9CC19C1C
0503AA756D959AF3775772
key::12345678
```

#### Programming Interface

Include the header file:
```c
#include "shamir.h"
```

**Generate Shares:**
```c
int GenerateShareKey(char *secret, int lenofsecret, int n, int t, char shares[][8192*2]);
```

**Combine Shares:**
```c
int CombineKey(char shares[][8192*2], int t, char *secret);
```

**Parameters:**
- `secret`: Input secret (hex string or binary data)
- `lenofsecret`: Length of the secret
- `n`: Total number of shares to generate
- `t`: Threshold number of shares needed for reconstruction
- `shares`: Output array to store the generated shares

### Example Code

```c
#include "shamir.h"
#include <stdio.h>
#include <string.h>

int main() {
    char secret[] = "12345678";
    int n = 5;  // Generate 5 shares
    int t = 3;  // Need 3 shares to reconstruct
    char shares[10][8192*2] = {0};
    
    // Generate shares
    GenerateShareKey(secret, strlen(secret), n, t, shares);
    
    // Print shares
    for(int i = 0; i < n; i++) {
        printf("Share %d: %s\n", i+1, shares[i]);
    }
    
    // Reconstruct secret using first 3 shares
    char reconstructed[64] = {0};
    CombineKey(shares, t, reconstructed);
    printf("Reconstructed: %s\n", reconstructed);
    
    return 0;
}
```

### File Structure

```
shamir/
├── src/
│   ├── shamir.h      # Header file with API declarations
│   ├── shamir.c      # Main implementation
│   ├── test.c        # Test program and examples
│   └── makefile      # Build configuration
├── README.md         # This file
├── LICENSE           # License information
└── .gitignore        # Git ignore rules
```

### License

See [LICENSE](LICENSE) file for details.

---

## 中文

### 概述

本项目使用C语言实现了**Shamir秘密共享算法**，可以将一个秘密分割成多个份额，并使用阈值数量的份额重构原始秘密。这是一种通过分布式存储提供安全性的密码学技术——单个份额不会泄露秘密的任何信息。

### 特性

- **阈值秘密共享**：将秘密分割成`n`个份额，需要`t`个份额才能重构
- **安全实现**：在GF(257)有限域上使用有限域算术确保密码学安全性
- **灵活输入**：支持十六进制字符串和二进制数据输入
- **跨平台**：使用标准C语言编写，兼容各种操作系统

### 算法详情

- **有限域**：在素数257的伽罗瓦域GF(257)中进行运算
- **多项式**：使用拉格朗日插值法进行秘密重构
- **安全性**：需要恰好`t`个份额才能重构秘密；`t-1`个份额不提供任何信息

### 编译说明

```bash
cd src
make
```

这将生成`shamir_test`可执行文件。

### 使用方法

#### 命令行界面

```bash
./shamir_test <十六进制秘密>
```

**示例：**
```bash
./shamir_test 12345678
```

**输出：**
```
0103AA5D0D82692A9A2260
0203AA13CECD74B8520113  
0303AA54731355DE5FD552
0403AA1FFE560C9CC19C1C
0503AA756D959AF3775772
key::12345678
```

#### 编程接口

包含头文件：
```c
#include "shamir.h"
```

**生成份额：**
```c
int GenerateShareKey(char *secret, int lenofsecret, int n, int t, char shares[][8192*2]);
```

**合并份额：**
```c
int CombineKey(char shares[][8192*2], int t, char *secret);
```

**参数说明：**
- `secret`：输入秘密（十六进制字符串或二进制数据）
- `lenofsecret`：秘密的长度
- `n`：要生成的份额总数
- `t`：重构所需的阈值份额数
- `shares`：存储生成份额的输出数组

### 示例代码

```c
#include "shamir.h"
#include <stdio.h>
#include <string.h>

int main() {
    char secret[] = "12345678";
    int n = 5;  // 生成5个份额
    int t = 3;  // 需要3个份额进行重构
    char shares[10][8192*2] = {0};
    
    // 生成份额
    GenerateShareKey(secret, strlen(secret), n, t, shares);
    
    // 打印份额
    for(int i = 0; i < n; i++) {
        printf("份额 %d: %s\n", i+1, shares[i]);
    }
    
    // 使用前3个份额重构秘密
    char reconstructed[64] = {0};
    CombineKey(shares, t, reconstructed);
    printf("重构结果: %s\n", reconstructed);
    
    return 0;
}
```

### 文件结构

```
shamir/
├── src/
│   ├── shamir.h      # 头文件，包含API声明
│   ├── shamir.c      # 主要实现代码
│   ├── test.c        # 测试程序和示例
│   └── makefile      # 编译配置
├── README.md         # 本文件
├── LICENSE           # 许可证信息
└── .gitignore        # Git忽略规则
```

### 许可证

详情请参见[LICENSE](LICENSE)文件。