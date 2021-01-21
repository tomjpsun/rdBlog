title: Fix Virtual Maching Manager Problem
slug: 2021-01-21-fix-virtual-machine-manager-problem
date: 2021-01-21 15:06:19 UTC+08:00
type: text
Category: Setup

使用 Virtual Machine Manager 創建新的 VM 時,
遇到 "無法新增貯藏儲區" 的現象.
應該是權限跑掉了，GUI 不許我們新增 disk image.

以實際環境說明, 新增的 disk image, 我都放在 /dev/3Tlv 的 Logical Volumn 裡面,
所以把 /dev/3Tlv 的 pool 移除, 然後重新加一次即可解決,

因為原本的 pool 還有其他 disk image, 我們在 VMM GUI
裡面做 移除 動作, 實驗後發現不是真的移除 /dev/3Tlv 這個 node,
只有 GUI 看不到而已, 重新加入同名的 pool 後, 全部都回來啦！
並且 pool 也可以再新增 disk image.

蠻 tricky 的 bug, 特此紀念一下！
