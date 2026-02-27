## IconCustom 扩展图标

### 线性图标集合

<ul class="icon-list">
  <li v-for="name in $iconLineCustom" :key="name">
    <span>
      <i :class="'el-icon-' + name"></i>
      <span class="icon-name">{{'el-icon-' + name}}</span>
    </span>
  </li>
</ul>
