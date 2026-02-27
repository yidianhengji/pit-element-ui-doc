## BaseTabs 用于FormBase选项卡切换

用于FormBase选项卡切换，用于点击切换不同功能状态。

:::demo 定义`value`属性，它接受`Number`或者`String`。

```html
<el-base-tabs
    style="height: 500px;"
    v-model="activeName"
    :tab-list-data="tabListData">1</el-base-tabs>

<script>
    export default {
        data() {
            return {
                activeName: 'loginLog',
                tabListData: [
                    {
                        label: "登录日志",
                        name: 'loginLog',
                    },
                    {
                        label: "请求日志",
                        name: "requestLog",
                    },
                    {
                        label: "异常日志",
                        name: 'errorLog',
                    },
                ],
            };
        }
    };
</script>
```
:::


:::demo 定义`value`属性，它接受`Number`或者`String`。

```html
<el-base-tabs
    style="height: 500px;"
    v-model="activeName"
    noCard
    :tab-list-data="tabListData">1</el-base-tabs>

<script>
    export default {
        data() {
            return {
                activeName: 'loginLog',
                tabListData: [
                    {
                        label: "登录日志",
                        name: 'loginLog',
                    },
                    {
                        label: "请求日志",
                        name: "requestLog",
                    },
                    {
                        label: "异常日志",
                        name: 'errorLog',
                    },
                ],
            };
        }
    };
</script>
```
:::
