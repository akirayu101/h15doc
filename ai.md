####ai过程

##### 1.autocode产生

autocode是用xmindfiles产生的，产生的lua重有如下几个字段

1. STATE_MAP
2. TRIGGER_MAP
3. getStateMap()
4. getTriggerMap()

以move为例子

其中STATE_MAP中有两个状态，以MovingRight为例子，包含以下字段

1. Trigger = {[1] = 12，}，
2. ParamMapFunc = “$speed1”
3. tags = {[1] = "1",},
4. StateClassType = "runningLeft"
5. name = "MovingRight"

在此FSM中，触发到达最右的状态则向左移动，触发到达最右的状态则向右移动，TriggerMap中以到达最右为例。包含以下字段

1. TriggerClassType = "isRightMost"
2. tags = {[1] = "12",},
3. name = “到达最右”
4. AddStatusType = {[1] = 2}

##### 2.parser 

产生xmind到autocode的过程，不用看，知道用法即可。

##### 3.FSMFactory

######3.1 TransitionFactory

transitionFactory就是管理变换状态的，其中重要的是判断状态切换条件的triggerFunc。其具体的所有视线在TransitionLogic.lua里面

函数原型为

	function(status, entity_status, ...)

##### 4.其他文件

tools.lua为ai工具库，包括

1. findPAraValueInTable是table中找para，返回为bool,value
2. entityDisapear为控制消失
3. excludeFromTable返回一个exclude某个element后的table
4. outputStatus参数为entity, target, stateId来对target输出状态
5. doSkill参数为state, entity_status, skillID, delay。调用fuben的doSkill，支持delay操作
6. removeAI。在aiList中remove掉同名的ai

time.lua为游戏中计时模块，实现较为简单，不赘述

ai.lua提供了ai的listener model。具体包括

1. deepcopy， 对table进行recursive的copy
2. emptyFunc， 空函数，无操作
3. Listener原型，对listener进行初始化操作，其中包含paramList,beginFunc,updateFunc,endFunc
4. State原型，包含参数，name，listener，transitionList，time，maintain，counter
5. FSM原型，重要的就是run的时候带有的transition变换状态，其中maintain没有看到在哪里用，FSM为多状态的切换。正常不需要FSM得可以直接只包含一个状态不断update即可，在transition的state中有一个isTrigger的方法。这个需要后面看一下。
6. entiti_status对应一个实体的状态，这个目前看起来应该是一个包装好的spirite对象，后期验证下。
