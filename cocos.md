# cocos

## 项目结构
`assets`：资源目录

`build`：构建目录（在构建某平台后会生成该目录）

`library`：导入的资源目录

`local`：日志文件目录

`profiles`：编辑器配置

`settings`：项目设置

`temp`：临时文件目录

`package.json`：项目配置

## 3D

### 创建3D节点
> 在左侧对应场景中右键点击创建

- 快速聚焦场景编辑器中的某个节点，在左侧层级管理器中选中对应节点，然后按f键就可以聚焦该节点

### 修改模型颜色
修改模型颜色即设置对应模型的Materiais（材质）
- 首先在资源管理器中的assets中创建Material文件夹，创建材质文件
- 将材质文件拖拽到指定模型上，或者在属性检查中的Materiais选择创建的材质文件


### 脚本生命周期函数

1. onLoad加载函数：脚本第一个执行的函数，一般用于开启监听事件
2. onDestroy销毁函数：当组件或者节点被销毁时，执行这个函数，一般用于关闭监听事件
3. start（默认）开始函数：脚本启动执行该函数
4. update（默认）帧函数（每帧都会运行，1s=60帧）循环函数
5. lateUpdate延迟函数：update函数执行后，执行这个函数
6. onDisable：节点被禁用/隐藏时 自动执行函数
7. onEnable：节点被启用/显示时 自动执行函数

#### 脚本的使用
```ts
import { _decorator, Component, Node } from "cc";
const { ccclass, property } = _decorator;

@ccclass("Player")
export class Player extends Component {
  // 给属性添加property装饰可以让该属性在编辑器中显示设置
  @property
  Player_Speed: number = 30;

  start() {
    console.log("游戏开始了");
  }

  update(deltaTime: number) {
    // this.node 代表节点自身
    // getPosition获取节点位置
    const pos = this.node.getPosition();
    // setPosition 设置节点位置
    // 由于个别设备帧率忽高忽低，会造成速度不对等，为了防止这样的情况，就需要帧时间补偿
    // 通过帧间隔时间乘以速度来确保速度的一致
    this.node.setPosition(pos.x, pos.y, pos.z - deltaTime * this.Player_Speed);
  }
}
```

### 监听事件
监听类型如下：
1. 鼠标监听
   1. `Input.EventType.MOUSE_DOWN`：鼠标按下
   2. `Input.EventType.MOUSE_MOVE`：鼠标移动
   3. `Input.EventType.MOUSE_UP`：鼠标抬起
   4. `Input.EventType.MOUSE_WHEEL`：鼠标滚轮
2. 触摸事件：
   1. Input.EventType.TOUCH_MOVE：
   2. Input.EventType.TOUCH_END
   3. Input.EventType.TOUCH_CANCEL
3. 键盘事件：
   1. `Input.EventType.KEY_DOWN`：键盘按下
   2. `Input.EventType.KEY_PRESSING`：键盘持续按下
   3. `Input.EventType.KEY_UP`：键盘释放

监听写法：input.on(监听类型, 触发后执行函数, this)
监听开启和关闭成对写（防止内存泄漏）

```ts
export class Player extends Component {
  // 给属性添加property装饰可以让该属性在编辑器中显示设置
  @property
  Player_Speed: number = 10;

  p = {
    a: false,
    d: false,
  };

  protected onLoad(): void {
    // 第一个参数事件类型，第二参数事件触发回调函数，第三个参数作用对象
    input.on(Input.EventType.KEY_DOWN, this.Key_Down, this);
    // 监听键盘抬起事件
    input.on(Input.EventType.KEY_UP, this.Key_Up, this);
  }
  // key是事件对象
  private Key_Down(key: EventKeyboard) {
    if (key.keyCode == 65) {
      this.p.a = true;
    } else if (key.keyCode == 68) {
      this.p.d = true;
    }
  }

  private Key_Up(key: EventKeyboard) {
    if (key.keyCode == 65) {
      this.p.a = false;
    } else if (key.keyCode == 68) {
      this.p.d = false;
    }
  }
  start() {
    console.log("游戏开始了");
  }

  update(deltaTime: number) {
    // this.node 代表节点自身
    // getPosition获取节点位置
    const pos = this.node.getPosition();
    // setPosition 设置节点位置
    // 由于个别设备帧率忽高忽低，会造成速度不对等，为了防止这样的情况，就需要帧时间补偿
    // 通过帧间隔时间乘以速度来确保速度的一致
    this.node.setPosition(pos.x, pos.y, pos.z - deltaTime * this.Player_Speed);
    // 设置横向移动
    if (this.p.a && !this.p.d) {
      this.node.setPosition(
        pos.x - deltaTime * this.Player_Speed,
        pos.y,
        pos.z - deltaTime * this.Player_Speed,
      );
    }
    if (!this.p.a && this.p.d) {
      this.node.setPosition(
        pos.x + deltaTime * this.Player_Speed,
        pos.y,
        pos.z - deltaTime * this.Player_Speed,
      );
    }
  }
}
```

### 在脚本中绑定节点
```ts
export class Player extends Component {
  // 脚本中绑定节点/组件
  // 在界面编辑器中将想要绑定的节点拖拽到C_Node上
  @property(Node)
  C_Node: Node = null;
}
```

### 3D碰撞
1. 为要设置碰撞的节点绑定刚体组件RigidBody（在右侧的属性检查器中的最下面有添加组件）
   1. > 刚体是组成物理世界的基本对象，它可以使游戏对象的运动方式受物理控制。
   2. > 什么时候该添加刚体
      1. 配置碰撞分组并让其生效。
      2. 物体需要具备运动学或动力学行为。
2. 为节点绑定碰撞组件（以盒碰撞体BoxCollider为例），可以设置碰撞区域