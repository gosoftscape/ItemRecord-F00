# ItemRecord-F00
## 特殊业务逻辑
1. 基础数据.
  1. 基础数据类型.
    - 工会机构.
    - 税局机构.
    - 基层单位.
    - 基层单位隶属.
    - 基层单位属地.
    - 分解办法.
  2. 基础数据包括增删改查基本功能,但是与银行的接口逻辑决定了,实际数据版本以银行发送的文件为准,所以增删改功能如何抉择还需要进一步确定.
  3. 每天凌晨从银行接收的文件中,包含当天税票涉及的基础数据,这些数据只是部分数据,对系统中的现有数据做增和改的操作.
2. 核心业务.
  1. 分解.
    1. 该业务存在3种计算关系.
      - 缴费金额计算.(此项非系统实现时需要关心的逻辑.)
      - 分解.即将一张税票的票面金额按分解办法中配置的7项比例分解为省总,州总,县总,全总,税局手续费,产业,基层留存7份.
      - 入账.即将上述7份资金按分解办法中配置的每项入账账户(划入省总为1,划入州总为2,划入县总为3),分别划入省总,州总,县总在银行开设的专户.
    2. 隶属包括省级,州级,县级,中央金融(中央15%),中央5%,农信,煤矿.
    3. 分解办法依据隶属和属地,查询出分解和入账两项关键信息.
    4. 隶属和属地编码格式为LZZXX(1位隶属,2位州编码,2位县编码),在银行系统中,州县编码存在通配符(00)和特殊编码(省本级99,市本级01)相关机制,匹配通配符时最精确者命中.
    5. 系统可能需要自行校验银行的分解(工会分解)和入账(入账分解)结果,具体未定,是否可以信任银行系统,以银行接口文件为主需要与工会人员沟通.
    6. 银行文件包括:
      - 批次数据.(银行端修改,最终需要包含批次号,工会分解总笔数,工会分解总金额(票面金额),入账分解总笔数,入账分解总金额(入账金额).)
      - 工会分解.(1张税票对应1条记录,包括7份资金的计算结果.)
      - 入账分解.(1张税票对应1-3条记录,包括分别划入省总专户,州总专户,县总专户的3笔资金.)
      - 税局机构.
      - 基层单位.
      - 分解办法.
      - 自动返拨.(暂时不需要关注.)
    7. 分解时由于乘百分数会导致7份资金相加不等于票面金额,出现误差的情况,考虑到省总的资金不可能为0%,所以采取先算出其余6份,再用票面金额减去6份总金额得出省总金额的策略.
    8. 导入批次状态采取允许部分成功,再次导入以流水号为唯一键去重,每次导入更新批次状态的策略.
    9. 业务数据表包括:
      - 批次信息.
      - 分解信息.
      - 入账信息.
    10. 系统组织架构和业务组织架构关系的相关讨论,涉及数据权限相关问题,此处暂不展开.
  2. 返拨.
3. 统计报表.

## 问题记录
### 开发阶段

### 测试阶段

### 维护阶段
