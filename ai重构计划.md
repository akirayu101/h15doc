### AI重构计划memo

#####目前实现方法的问题
1. ai有些是动态添加进入entity的，对于entity的ai管理不便，并且所有entity初始化ai方式都不一样
2. 由于ai动态添加，英雄释放自身技能，需要走doSkill，createNpc， npc的ai是给英雄一个ai，然后在运行结束后remove掉英雄的ai状态，逻辑和调用上都比较绕
3. ai的每个instance都是deepcopy的，在系统上表现为ai数量巨大。实际上ai的参数可以绑定到entity，并且，一类的entity的参数也是共有，使用deepcopy是浪费资源的。
4. 序列执行ai的过程，都是给出delaytime，其问题在于游戏的timetick不是非常准确，这样对于序列执行的scheduleScript来说造成对时不准。
5. 其他，ai中一些函数的低效

#####改进方法


1. 增加ai的enable状态，标识ai是否在作用中，entity初始化时获取所有ai，设定其enable状态
3. ai的proto实现，实现entity参数来injector到ai的proto来实现降低ai数量
4. ai中一些基础函数效率低下的改进
5. 改进技能ai的方式，不要走skill->npc->npc给英雄ai这个路线，对于不同的skillId进行区分，也就是召唤物类型，和自身ai设定类型。重构doSkill
6. 增加ai的序列执行方法，方便策划后期自由组合ai的方法，并且保证ai的执行是前一个ai完全结束后下一个ai开始进行。在ai的视线中去除所有的schedule方法。


#####预估影响面

1. 策划导表时候的ai填写部分。
2. 导表工具
3. obj层面的ai读取以及heartbeart的方法。