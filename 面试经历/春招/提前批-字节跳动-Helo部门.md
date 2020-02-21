# 提前批-字节跳动-Helo部门



* iOS中有哪些property属性
* assgin和weak的区别
* copy关键字，介绍一个拷贝的机制
* 深拷贝和浅拷贝的区别
* NSString 使用copy关键字会怎么样



* `nil`、`Nil`、`NULL`和`NSNull`的理解



* UIView的父类？
* frame和bounds的区别？
* 介绍CALayer？



* 操作系统的段式 和 页式存储 相关问题



* ipv4 和 ipv6的区别



算法：

* c语言 实现 `char *strcpy(char *src, char *dest)`

  ```c++
  char *strcpy(const char *str, char *dest) {
      assert(str != NULL && dest != NULL);
      char *res = dest;
      while (*src != '\0') {
          *(destCopy++) = *(src++);
      }
      *dest = '\0';   // 字符串末尾为'\0'字符
      return res;
  }
  ```

  

* 二叉树，判断二叉树是否是完全二叉树

