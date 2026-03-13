# 边缘函数站点切换相关测试例-61036167-EdgeFunctionSiteCoverage
tags: cdn, ncdn, edgeFunction, changeEeRegion, trigger, bindDomain

获取当前节点vip并保存在data_store中
* query edge compute vip & save to datastore key is "edge_compute_vip"

获取站点初始区域
* get "Coverage", edge compute common dcdn openapi with request params is "{'Action': 'GetSite', 'SiteId':site-name-id }", request method is "GET";response status code is "200", expect "" in response , "" not in response

## 1.创建函数并绑定站点
调用openapi创建边缘函数
* edge compute common dcdn openapi with request params is "{'Action':'CreateRoutine','Name':'test-routine-changesiteregion','Description':'autotest', 'SpecName':'100ms'}", request method is "POST";response status code is "200", expect "['\"status\": \"ok\"']" in response , "" not in response

通过ListUserRoutines能够查到创建的routine列表同时能够查到创建的函数
* edge compute common dcdn openapi with request params is "{'Action':'ListUserRoutines','SearchKeyWord':'test-routine-changesiteregion','PageNumber':1, 'PageSize':20}", request method is "GET";response status code is "200", expect "test-routine-changesiteregion" in response , "" not in response

发布ER代码至测试环境
* upload es code to staging env with es function name is "test-routine-changesiteregion", js code path is "scripts/cdn_script/er_test_js/addroute1.js"

等待
* create rule with sleep time is "10" seconds

生成正式版本
* edge compute publish version with request param is "{'Action':'CommitRoutineStagingCode','Name':'test-routine-changesiteregion', 'CodeDescription':'test'}", request method is "POST"; expect status code is "200", expect "" in response && save CodeVersion to data store "version_id"

将正式版本发布到测试环境
