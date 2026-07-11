# 2D

## 界面

> 下面这个紫色的框是用户手机屏显示区域，这个区域宽高可以在`项目/项目设置/显示/窗口`里设置

<img src="./pic/godot/屏幕截图 2026-01-25 154801.png">

## 场景与节点

<img src="./pic/godot/屏幕截图 2026-01-25 155749.png">

<img src="./pic/godot/屏幕截图 2026-01-25 155922.png">

## 常用操作与快捷键

## 继承关系

<img src="./pic/godot/屏幕截图 2026-01-25 190105.png">

## 脚本生命周期方法

> 脚本中的`this`指向绑定的元素

### _EnterTree 方法

> 一个节点会经历被创建，被挂载到节点树上两个步骤
> 
> 节点添加到节点树中的时候会调用`_EnterTree`，如果有多个节点加入节点树执行顺序是`先加载先执行`（即入队顺序）

```csharp
// 节点添加到节点数中的时候会调用
public override void _EnterTree()
{
    base._EnterTree();
}
```

> 节点添加到节点树中后会调用`_Ready`，如果有多个节点加入节点树执行顺序是`后加载先执行`（即栈顺序）

```cs
// 节点添加到节点树中后会调用
public override void _Ready()
{
    // 逻辑
}
```

> 节点离开节点树(销毁时)调用`_ExitTree`

```cs
public override void _ExitTree()
{
    base._ExitTree();
}
```

> 帧方法（每个帧都会调用一次）`_Process`，有个参数是两个帧的间隔时机

```cs
public override void _Process(double delta)
{
}
```

> 每次物理系统计算，会调用一次`_PhysicsProcess`，参数是物理系统计算时间间隔

```cs
public override void _PhysicsProcess(double delta)
{
    base._PhysicsProcess(delta);
}
```

## 键盘和鼠标的输入

> 在脚本中使用方法`_Input`

```cs
public override void _Input(InputEvent @event)
{
    base._Input(@event);
    // 如果是键盘事件,转成键盘事件
    if (@event is InputEventKey)
    {
        // 转成键盘事件
        var key = @event as InputEventKey;
        // 判断是否按下了v键
        if (key.Keycode == Key.V)
        {
            // 判断当前是否持续按压
            if (key.IsEcho())
            {
                GD.Print("持续按压");
            }
            // 判断当前是否是按下瞬间
            else if (key.IsPressed()) {
                GD.Print("按下瞬间");
            }
            // 判断当前是否是抬起瞬间
            else if (key.IsReleased())
            {
                GD.Print("抬起瞬间");
            }
        }
    }
    // 如果是鼠标事件，转成鼠标事件
    if (@event is InputEventMouse)
    {
        var mouse = @event as InputEventMouse;
        if (mouse.IsPressed())
        {
            // 打印鼠标位置
            GD.Print(mouse.Position);
            // 
            GD.Print(mouse.ButtonMask);
        }
    }
}
```

> 也可以先在`项目>项目设置>输入映射`绑定按键，然后在帧方法中调用

```cs
public override void _Process(double delta)
{
    // IsActionJustPressed按下一次返回一次true，不会因为随着帧刷新而多次返回
    if (Input.IsActionJustPressed("跳跃"))
    {
        GD.Print("跳跃");
    }
    // 获取按键力度，按下返回1，不按返回0
    float s = Input.GetActionStrength("跳跃");
    // 获取一个向量（0，0）
    Vector2 vector2 = Input.GetVector("左", "右", "上", "下");
}
```

## 节点操作

```cs
// 获取当前场景的根节点
Node root = this.GetTree().CurrentScene;
// 寻找Test节点
Node test = root.FindChild("Test");
// 尝试删除节点
test?.QueueFree();
// 把test节点从节点树上拿出来
root.RemoveChild(test);
// 把test节点挂载到当前节点上
this.AddChild(test);
// 创建一个新节点
Node2D node2D = new Node2D();
// 获取父节点,并断言类型
Node2D node = this.GetParent() as Node2D;
```

## 场景切换

<img src="./pic/godot/屏幕截图 2026-01-26 130914.png">

```cs
// 获取场景树
SceneTree sceneTree = this.GetTree();
// 方法一
// 跳转场景2
sceneTree.ChangeSceneToFile("res://game2.tscn");
```

## 全局脚本 --- 每个场景都会自动加载的脚本

> 先定义一个要全局挂载的脚本，然后在`项目/项目设置/全局`里添加脚本

## 向量

<img src="./pic/godot/屏幕截图 2026-01-26 140330.png">

### 向量运算

#### 加法 与 减法

> (x, y) + (w, z) = (x+w, y+z)

==在几何上，A - B 等价于在B处指向A处(在游戏中有用)==

> (x, y) - (w, z) = (x-w, y-z)

#### 乘法

==点乘==
A·B ，可以获取向量夹角

> (x, y) · (w, z) = xw + yz = |a||b||cos&|

## 节点常用方法

### 手动修改

> 这里指的是在godot界面右侧显示的内容

#### Node2D

> 修改位置和大小，旋转

#### CanvasItem

1. Visibility -- 显示（是否显示）
   - Modulate：设置颜色，会影响子节点
   - self Modulate：设置颜色，不会影响子节点
2. Ordering -- 排序（在z轴上的显示顺序）

### 脚本修改

```cs
// CanvasItem设置
// 设置可见性
this.Visible = true;
// z轴顺序
this.ZIndex = 10;
// z轴顺序是否相对于父元素
this.ZAsRelative = false;

// Node2D常用属性
// 位置
this.Position = new Vector2(500, 300);
// 设置旋转 -- 弧度
this.Rotation = 0.1f;
// 设置旋转 -- 角度
this.RotationDegrees = 30;
// 设置缩放
this.Scale = new Vector2(2, 2);
// 倾斜
this.Skew = 30;

// 获取鼠标位置
var pos = GetGlobalMousePosition();
// 看向某个点
LookAt(pos);
```

## 精灵节点的常用面板属性

> 主要用于显示图片

1. offset -- 偏移

2. region -- 裁剪
   只显示设定区域内的精灵节点的内容，x y为设置，w y为显示窗口

3. animation -- 动画
   
   > 当精灵图是由多个连续的图片组合成一个图片时（例如由4个图片组成），可以将Hframes设为2（意味着将图片左右对半分），Vframes设为2（意味着将图片上下对半分）此时只显示左上角1/4的图片，设置Frame为0，1，2，3分别显示1/4左上，右上，左下，右下图片（即用一张图片来展示一个动画，和前端精灵图一样的意义）

### 用脚本操作精灵节点常用属性

```cs
public partial class Sprite2dTest : Sprite2D
{
    // 这种方式也可以设置纹理
    // [Export]
    // public Texture2D texture2D;
    public override void _Ready()
    {
        // 脚本加载纹理（即图片）
        this.Texture = GD.Load<Texture2D>("res://icon.svg");

        // 设置精灵节点中心点为左上角，false为节点左上角，true为节点中心
        this.Centered = false;
        // 设置偏移
        this.Offset = new Vector2(0,0);
        // 翻转
        this.FlipH = true;    // 水平翻转
        this.FlipV = true;    // 垂直翻转

        // 动画    -- 和前端精灵图做法类似
        this.Hframes = 2;    // 设置水平方向切成几分
        this.Vframes = 2;    // 设置垂直方向切成几分
        this.Frame = 0;        // 显示第几份图片
    }
}
```

## 物品分组

> 有时想对多个节点做同一个类型的操作（如：销毁所有敌人），可以把多个节点纳入同一个分组中
> 
> 在godot编辑页面右侧工具栏有个节点工具，里面有个分组可以创建不同的组，可以将多个节点分入创建的组中，可以组内的节点统一操作

### 用脚本操作分组

```cs
public partial class Player : Sprite2D
{
    public override void _Ready()
    {
    }

    // Called every frame. 'delta' is the elapsed time since the previous frame.
    public override void _Process(double delta)
    {
        if (Input.IsActionJustPressed("右"))
        {
            // 获取分组中的节点，获取名为敌人的分组
            var enemys = this.GetTree().GetNodesInGroup("敌人");
            // 遍历敌人
            foreach (var enemy in enemys)
            {
                // 销毁敌人
                enemy.QueueFree();
            }
        }
    }
}
```

### 用脚本操作分组内节点挂载的脚本方法

```cs
// 在敌人分组内所有节点脚本都有一个Test方法
public partial class Enemy : Sprite2D
{
    // Test方法
    public void Test()
    {
        GD.Print("我是敌人");
    }
}

// 通过分组来操作Test方法
public partial class Player : Sprite2D
{
    public override void _Process(double delta)
    {
        if (Input.IsActionJustPressed("左"))
        {
            // 第一个参数是分组名，第二个参数是方法名
            this.GetTree().CallGroup("敌人", "Test");
        }
    }
}
```

### 用脚本将节点添加到分组中

```cs
public partial class Player : Sprite2D
{
    public override void _Process(double delta)
    {
        if (Input.IsActionJustPressed("上"))
        {
            // 将当前节点添加到敌人分组
            this.AddToGroup("敌人");
        }
    }
}
```

## 信号

> 有点像事件，以一个按钮节点添加按下事件(pressed)为例

步骤：

- 首先给按钮节点创建一个脚本，再在脚本中创建一个方法用于事件的回调函数
  
  ```cs
  public partial class MyButton : Button
  {
      // 回调方法
      // 找到精灵节点并删除
      public void ButtonClick()
      {
          Node2D node = GetNode<Node2D>("/root/Node2D/player");
          node.QueueFree();
      }
  }
  
  ```

- 将方法绑定到事件上
  
  - 手动绑定：
    
    > 选中button按钮节点，在右侧选中节点，选择信号，选择信号（事件），右键连接（==注意脚本是cs时要先编译一下==）
  
  - 脚本绑定
    
    ```cs
    public partial class MyButton : Button
    {
      public override void _Ready()
      {
          // 脚本手动添加
          this.Connect("pressed", new Callable(this, "ButtonClick"));
          // 或者采用下面更简单的方式添加
          this.Pressed += ButtonClick;
      }
    
      // 找到精灵节点并删除
      public void ButtonClick()
      {
          Node2D node = GetNode<Node2D>("/root/Node2D/player");
          node.QueueFree();
      }
    }
    ```

### 自定义信号

> 不使用官方的信号（事件）自定义信号

==创建自定义信号==

> 注意创建完后编译一下，可以在godot界面的信号中看到自定义信号
> 信号的回调函数绑定参照上面的步骤（可以手动，也可以脚本绑定）

```cs
public partial class A : Sprite2D
{
    // 声明委托
    [Signal]
    public delegate void MySignalEventHandler();
    public override void _Ready()
    {
    }

    public override void _Process(double delta)
    {
        if (Input.IsActionJustPressed("左"))
        {
            //发射信号（触发事件），信号名取MySignalEventHandler前面部分
            EmitSignal("MySignal");
        }
    }
}
```

## AnimatedSprite2D -- 用于替代sprite2d动画选项的

> 使用sprite2d动画选项来进行播放动画不太方便，可以使用AnimatedSprite2D，这是一个专门播放动画的节点，

如何给AnimatedSprite2D添加帧不在这里说了

<mark>用脚本控制动画的播放</mark>

```cs
// 继承于AnimatedSprite2D
public partial class MyAnimatedSprite2d : AnimatedSprite2D
{
    public override void _Ready()
    {
    }

    public override void _Process(double delta)
    {
        if (Input.IsActionJustPressed("left"))
        {
            // 播放动画，idle名字是动画名
            this.Play("idle");
        }
        if (Input.IsActionJustPressed("right"))
        {
            this.Play("attack");
        }
    }
}
```

## AnimationPlayer -- 更为强大的动画节点

## 光照

### DirectionalLight2D 平行光源 -- 全局光源

> 可以覆盖整个场景的光源

### PointLight2D 点光源 -- 局部光源

> 只会对目标区域进行光照

## 声音 AudioStreamPlayer

> AudioStreamPlayer有2d的和3d，两者的区别在于3的会随着位置的变化而产生声音大小的变化

==用脚本控制生成一个节点，并自动播放音乐==

```cs
public partial class MyNode2d : Node2D
{
    AudioStreamPlayer audioStreamPlayer = new AudioStreamPlayer();
    public override void _Ready()
    {
        // 加载资源
        var audioStream = GD.Load<AudioStream>("res://2dres/bgm/bg.ogg");
        // 把资源挂载到节点上
        audioStreamPlayer.Stream = audioStream;
        // 把节点添加到节点树上
        this.AddChild(audioStreamPlayer);
        // 自动播放音乐
        audioStreamPlayer.Play();
    }

    public override void _Process(double delta)
    {
    }
}
```

## 碰撞

> 一般做法是给要检测碰撞的节点挂载Area2D节点，再给Area2D节点挂载CollisionShape2D节点，通过CollisionShape2D节点绘制形状，当其它CollisionShape2D节点进入后，会被Area2D检测到

### 区域 --- Area2D

> 可以检测到其它`CollisionObject2D`的进入或退出

==信号会调函数==

```cs
public partial class PlayerArea2d : Area2D
{
    // 可以绑定到Area2D的enter信号
    // 参数是发生碰撞的另一个节点
    public void OnAreaEnter(Area2D area2D)
    {
        GD.Print("我发生碰撞了");
    }
}
```

### 射线形状检测

> 2D空间中的射线，用于查找第一个相交的碰撞物体

==用脚本检测碰撞==

```cs
public partial class MyRayCast2d : RayCast2D
{
    public override void _Process(double delta)
    {
    }
    public override void _PhysicsProcess(double delta)
    {
        base._PhysicsProcess(delta);
        // 按下左键拿到当前射线的碰撞检测结果
        if (Input.IsActionJustPressed("left"))
        {
            // 如果碰撞了
            if (this.IsColliding())
            {
                // 拿到碰撞区域
                var area = this.GetCollider() as Area2D;
                GD.Print("检测到了：" + area.Name);
            } else
            {
                GD.Print("没有检测到!");
            }
        }
    }

}
```

## 物理

> 可以使元素具有比较真实的物理效果

### StaticBody2D 节点 --- 静态2D物理节点

> 静态 2D 物理体。无法因外力或接触而移动，但可以通过代码移动，手动移动时不会影响路径上的其他物体。
> 
> 通常用作墙壁地板等

### RigidBody2D 节点 --- 刚体物理节点

> 由物理仿真进行移动的 2D 物理体。RigidBody2D 实现了完整的 2D 物理。这个物理体无法直接控制，必须对其施加力（重力、冲量等），物理仿真将计算由此产生的移动、旋转、对碰撞的反应以及对沿路其他物理体的影响等。
> 
> 也就是这个物体会具有重力等真实物理属性

## CharacterBody2D -- 角色控制节点

## UI界面

> control是所有ui的基类

### UI图像与文字

> `TextureRect`用于显示纹理（图像）的节点
> 
> `Label`用于显示文字的节点

### 按钮 button

#### 图片按钮 TextureButton

#### 链接按钮 LinkButton

#### 标准按钮 Button

#### 复选框 CheckBox
