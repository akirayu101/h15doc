### AI重构计划memo

1. 增加ai状态，标识ai是否在作用中
2. 初始化所有entity都要有ai列表，英雄应该持有其技能ai，在运行过程中，不动态的添加ai
3. ai的proto实现，实现entity参数来injector到ai的proto来实现降低ai数量
4. ai中一些基础函数效率低下的改进
5. 改进技能ai的方式，不要走skill->npc->npc给英雄ai这个路线，对于不同的skillId进行区分，也就是召唤物类型，和自身ai设定类型
6. 增加ai的序列执行方法，方便策划后期自由组合ai的方法，并且保证ai的执行是前一个ai完全结束后下一个ai开始进行
7. state的time方法进行时间累计再考虑下
8. 替换为下场状态时候，需要终止目前的schedule。考虑去除各种schedule的非同步update方法，来保证程序的重入性和随时撤销。schedule方法可以用ai的状态判断，一次性解决问题。主要去掉ccaction的异步问题。
9. 其他