---
layout: default
title: 引导阶段
permalink: /timeline/bootstrap/
group: timeline
---

<!-- Reviewed at c23493d7a33a82d559d5bd9d289486795cf6592f -->

# 引导阶段

在卡尔达诺测试网阶段和卡尔达诺结算层发布之后，该网络会在『引导模式』下运行一段时期，称为引导阶段。当购买 Ada 的人兑换他们的币时，股权将自动被委派给维护网络的可信节点池。在此期间，不会发放任何区块奖励 - 我们将保持网络在线。这是必要的，因为为了协议功能正常，拥有大部分期权的一些权益所有人必须在线，而在网络运行的头几个月，情况并非如此。

引导阶段将慢慢进入[奖励阶段](/timeline/reward)，在这期间更新协议将被发布，将为大宗权益所有人提供在云服务器运行的方便选项。


## 股权锁定

引导阶段存在于卡尔达诺结算层存在的时期，它只允许固定的预定义用户对系统进行控制。这些用户集（引导阶段权益所有人）以及他们各自控制的总权益的比例，在创始区块中定义。

引导阶段的目的是为了解决在主网开始的时候，大部分股权可能会脱机（开始时违反协议）的担忧。引导阶段将在网络稳定，并且大部分股权在线时结束。

Bootstrap 之后的下一个阶段被称为[奖励阶段](https://cncardanodocs.com/timeline/reward/)，奖励阶段实际上是卡尔达诺结算层作为 PoS 加密货币的『正常』运行模式。


### 要求

1. 在引导阶段，卡尔达诺的股权应当被有效地委派给一组固定的密钥 `S`。
2. `S` = 7
3. 股权应该在 `s` ∈  `S`
4. 在引导阶段结束时应该解开股权
    1. Ada 买家能够自己参与协议（或将其权利委派给某个代表 `S`)。
    2. 每个 Ada 买方都应该明确声明自己想要控制的股份。
        * 否则，一旦奖励阶段开始，很容易导致在少于大多数股权在线的情况
    3. 在撤销股权行动之前，股权应该仍然由 `S` 节点控制。

### 提案

现在让我们来看看引导阶段的解决方案：

1. 初始 `utxo` 包含引导阶段股权所有人的所有股权。初始 `utxo` 由 `(txIn, txOut)` 组成，并且每个 `txOut` 都有一个存有股权分发的地址。所以我们只是以一种将所有币发送给所有股权所有的方式设置分配。
2. 引导阶段开始时，用户可以发送更改初始 `utxo` 的请求。我们为每个交易输出设定股权分配，以将权益分配给引导阶段权益相关者。这有效得使得利益分配是系统不变的。
3. 重量级代表团有成因状态。它包含对 `(Issuer, ProxySK)`，其中 `Issuer` 是被委托的利益相关者的标识符，`ProxySK` 是委派的代理密钥。请注意：
    * 代表必须与发行人不同，即不允许撤销；
    * 委派人不能成为发行人，即不支持过渡性委派。
4. 当引导阶段结束后，我们禁用股权分配的限制。股权所有人将投票让引导阶段结束：将形成特殊的更新提案，其中一个特定的常量将被适当设置，以触发引导阶段结束的更新提案获得通过。系统的运行方式与引导阶段相同，但用户需要明确说明自己的股份所有权，能承担处理节点的责任。为了获取他的股权，用户应该发送一个交易，指定股权分配的代理密钥（s）。它可能是用户自己拥有的密钥，也可能是某个关键代表（可能是引导阶段股权所有人中的一个或几个）。

