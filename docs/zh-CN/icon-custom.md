## IconCustom 扩展图标

按不同的分类扩展了一套常用的图标集合。

:::demo
```html
<template>
    
  <el-tabs v-model="activeName" tab-position="left">
    <el-tab-pane label="菜单" name="first">
        <p>菜单类图标共：{{$iconCustom.length}}个</p>
        <ul class="icon-list">
            <li v-for="name in $iconCustom" :key="name">
                <span>
                  <i :class="'el-icon-' + name"></i>
                  <span class="icon-name">{{'el-icon-' + name}}</span>
                </span>
            </li>
        </ul>
    </el-tab-pane>
    <el-tab-pane label="基础" name="second2">
        <p>基础类图标共：{{$iconBasics.length}}个</p>
      <ul class="icon-list">
          <li v-for="name in $iconBasics" :key="name">
            <span>
              <i :class="'el-icon-' + name"></i>
              <span class="icon-name">{{'el-icon-' + name}}</span>
            </span>
          </li>
      </ul>
    </el-tab-pane>
    <el-tab-pane label="登录" name="second">
        <p>登录类图标共：{{$iconLogin.length}}个</p>
        <ul class="icon-list">
            <li v-for="name in $iconLogin" :key="name">
                <span>
                  <i :class="'el-icon-' + name"></i>
                  <span class="icon-name">{{'el-icon-' + name}}</span>
                </span>
            </li>
        </ul>
    </el-tab-pane>
      <el-tab-pane label="表单" name="second3">
          <p>登录类图标共：{{$iconForm.length}}个</p>
          <ul class="icon-list">
              <li v-for="name in $iconForm" :key="name">
                <span>
                  <i :class="'el-icon-' + name"></i>
                  <span class="icon-name">{{'el-icon-' + name}}</span>
                </span>
              </li>
          </ul>
      </el-tab-pane>
    <el-tab-pane label="性别" name="third">
        <p>性别类图标共：{{$iconGender.length}}个</p>
        <ul class="icon-list">
            <li v-for="name in $iconGender" :key="name">
                <span>
                  <i :class="'el-icon-' + name"></i>
                  <span class="icon-name">{{'el-icon-' + name}}</span>
                </span>
            </li>
        </ul>
    </el-tab-pane>
    <el-tab-pane label="方向" name="third2">
        <p>方向类图标共：{{$iconDirection.length}}个</p>
      <ul class="icon-list">
          <li v-for="name in $iconDirection" :key="name">
            <span>
              <i :class="'el-icon-' + name"></i>
              <span class="icon-name">{{'el-icon-' + name}}</span>
            </span>
          </li>
      </ul>
    </el-tab-pane>
    <el-tab-pane label="天气" name="third3">
        <p>天气类图标共：{{$iconWeather.length}}个</p>
      <ul class="icon-list">
          <li v-for="name in $iconWeather" :key="name">
        <span>
          <i :class="'el-icon-' + name"></i>
          <span class="icon-name">{{'el-icon-' + name}}</span>
        </span>
          </li>
      </ul>
    </el-tab-pane>
  </el-tabs>
</template>
<script>
  export default {
    data() {
      return {
        activeName: 'first'
      };
    },
  };
</script>
```
:::
