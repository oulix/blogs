# 软件代码编码规范
## 代码风格和规范
保持代码简洁易懂，及时重构和优化代码
## 代码风格
- 合理组织代码，代码目录结构力求简洁明了  
- 多采用MVC等模式考虑问题，控制类和类之间的关联度  
举例如下：
```
Project
  ├─Project.pro
  ├─CMakeLists.txt
  └─main.cpp
    ├─bin
    ├─Common
    ├─Model
    ├─Controller
    ├─Resources
    │├─icon
    || |_app.ico
    │└─qss
    │└─psblack.css
    │├─Testing
    │  ├─UnitTest
    │  |_IntergrationTest
    └─View
        ├─MainWIndow.h
        ├─MainWindow.cpp
        ├─ToolBar.h
        ├─MainWindow.cpp
        ├─ToolBar.h
```
- 非必要不在一个 .h 中写多个类
- 复杂功能函数需要及时重构为多个单一功能的函数  
- 魔术值或者经验值不直接使用，用易懂的别名定义  
 如经验参数 0.55
```
const int EXPERIENCED_VAL=0.55;
if( ret==EXPERIENCED_VAL) 
{
    ...
}
```
- 用一个很小的小数判断大于还是小于，如 >1e-10  
- 所有头文件使用 #define 防止头文件被多重包含，不使用#pragma   once  
 注：能依赖声明的就不要依赖定义。如
```
#ifndefWINDOW_H
#defineWINDOW_H
​
#include<QWidget>
​
QT_BEGIN_NAMESPACE
classQCalendarWidget;
classQCheckBox;
classQComboBox;
classQDate;
classQDateEdit;
classQGridLayout;
classQGroupBox;
classQLabel;
QT_END_NAMESPACE
​
//! [0]
classWindow: publicQWidget
{ ...
```
- 只有当函数只有 10 行甚至更少时才会将其定义为内联函数（inline function） 
- 定义函数时，参数顺序为：输入参数在前，输出参数在后  
- 头文件包含顺序，系统、库、第三方库在前，其他头文件在后   
举例来说，fooserver.cpp的包括次序如下：
```
#include"foo/public/fooserver.h"// 自身头文件最优先
​
#include<sys/types.h>
#include<unistd.h>
​
#include<hash_map>
#include<vector>
​
#include"base/basictypes.h"
#include"base/commandlineflags.h"
#include"foo/public/bar.h"
```
-  各组之间留空行， 函数和变量各自放一起，空行不要多于两行  
```
classWindow: publicQWidget
{
    Q_OBJECT
​
public:
    Window();   
​
protected:
​
privateslots:
    voidlocaleChanged(int index);
​
private:
    voidsetWidth(size_t);
​
    int m_width;
    int m_windowTitle;
```
-  允许内建类型全局变量；禁止class 类型的全局变量，如必须使用，请用单件模式。
注意静态成员变量视作全局变量，所以，也不能是 class 类型
-  不要声明命名空间 std 下的任何内容，包括标准库类的前置声明。声明标准库下的实体，需要包括对应的头文件
-  为了方便代码管理，不建议使用异常
-  使用 C++风格的类型转换，除单元测试外不要使用 dynamic_cast
-  能用 ++i 不用 i++
-  const 能用则用，提倡 const 在前
-  除字符串化、尽量避免使用宏
-  整数用0, 实数用0.0,指针用nullptr(C++11),字符（串）用 '\0'
-  用sizeof(变量名) 代替 sizeof(类型)
命名约定
-  函数命名、变量命名、文件命名应具有描述性，不要过度缩写，类型和变量应该是名词，函数名可以用“命令性”动词。
-  文件名要全部首字母大写，可以包括下划线  
可接受文件名: 
```
My_Useful_Class.cpp
MyUsefulClass.cc
UrlTable.h  // The class declaration. 
UrlTable.cc // The class definition
```
-  类型命名每个单词以大写字母开头，不包括下划线。 所有类型命名：类、结构体、类型定义（typedef）、枚举, 均使用相同约定  
```
MyExcitingClass
MyExcitingEnum
​
// classes and structs
classUrlTable{ ...
classUrlTableTester{ 
...structUrlTableProperties{ ...
​
// typedefs
typedefhash_map<UrlTableProperties*, string>PropertiesMap;
​
// enums
enumUrlTableErrors{ ...
```
-  建议对基本类型只设私有成员变量，需要的话提供set/get函数
```
public:
    MyWigetmyExcitingLocalVariable;
    MyWindow*m_MyExcitingMemberVariable;   
​
private：
    int m_classPrivateVariable;
    bool m_windowVisiable;
```
-  函数名：首字母小写，单词间首字母大写，无空格
```
enableWindow() 
resizeWindowToolBar()
```
-  全局变量用g开始  
## 格式和注释 
基本原则，保持代码简洁易读
-  如果注释过长，使用多行注释或者换行
-  除非确实需要，如果有超过4级以上的缩进，考虑重构下代码和逻辑
-  采用 Doxygen 注释风格
-  class头文件必须有注释，函数注释在头文件中
-  命名明确的，功能简单的，简短的函数可不注释
-  函数实现代码不易理解的地方要加注释
-  适当对齐排版易于阅读理解
-  避免同行注释，单行不要超过100个字符
-  使用自动化格式化工具clang-format,将.clang-format文件放在工程的根目录下
-  文件头必须公司包含版权信息
## Git 分支与消息
- 使用本地分支管理开发 
- 为每个feature， bug， issue单独建立分支  
- 不要为每个分支单独建立一个目录  
- 分支名用小写和短横，分支名含义清晰  
- 无其他要求可以branch_开头，如 branch_test_module_a  
- 为分支起一个简单易懂的名字  
- 如果有feature, task, bug 等ID，用ID作为分支名
- bug修复分支使用fix_bugid,提交信息的第二行注明修复了什么
- 提交信息要明确提交的类型:  
标记	标记说明
```
feat:	    功能特性实现
fix:	    bug修复
ui:	        与界面相关的功能和更新
test:	    与测试相关的提交
docs：	    与文档相关的提交
chore：	    普通的代码维护
refactor:	重构代码库的特定部分
```
