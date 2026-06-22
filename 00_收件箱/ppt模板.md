
Agent使用分享


1. 点出核心论点
		我认为ai agent是革命性的生产力工具，目前处于成长期，已经有了比较好的工具，主要是创新者（geek）使用，还没有进入利润高爆发气（特别想2000年pc兴起的时候，大家在寻找新的赛道时候，现在投资人看到agent创业者，眼里是有光的）
	1. 个人生产力 = 需求*（模型能力*agent框架能力）*token数
		1. 需求就是工作和生活中想做，但是因为技能不够开始成本很高
		2.  模型能力就是模型的推理、agentic等基础能力（备选是 anthropic/gpt/deepseek/kimi）
		3. agnet框架能力：工具：claude code 和codex；需要自己定义skill/hook等，我认为skill也是⚙️重要的，它的本质给agent搜索解决路径它把你已经总结好的
		4. token 数：包括了价格和api稳定性
	所以，我介绍两个事：
		1.怎么以低价获得稳定的anthropic和gpt的api？
		2 怎么配置skill，更好地满足需求？
	然后，先介绍api的获取途径：1 chatgpt/anthropic 包月/包年 plan，每天限额，会不定期遇到封号（特别是antropic）2. 代理api，是有专人搞到便宜的api token（例如gravity的免费额度），通过中转分发给大家；如果没有封号，稳定性约等于原生plan（还有客服，好评），价格便宜，按需购买
	
	因为我可能会在短时间内大量消耗token，会超过原生plan的天限额，并且代理api目前价格也比较便宜，所以我选择代理api
		
	然后介绍例子，我会用orsidian+codex+orbitOS（skill） 来管理我的日/周/月包，以及 长周期任务的推进情况，并且把他们互相关联起来 

		第二个例子是superpowers（skill）+codex，我觉得可以把我代码开发、debug和试验的工作量从1个月-->1周