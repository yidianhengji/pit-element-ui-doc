<script>
const arrTextList = [
    "天",
    "地",
    "玄",
    "黄",
    "宇",
    "宙",
    "洪",
    "荒",
    "A",
    "B",
    "C",
    "D",
    "E",
    "F",
    "G",
    "a",
    "b",
    "c",
    "d",
    "e",
    "f",
    "g",
    "0",
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "7",
    "8",
    "9",
    ";",
    "；",
    ",",
    "，",
    "。",
];
export default {
    data() {
      return {
        arrTextList: arrTextList,
        textLength: arrTextList.length
      }
    },
  }
</script>

## FontFamily 字体

### 不同的字体在中文、英文、数字下的展示效果。

<div class="demo-font-family">
    <p class="family1 font14">思源黑体 14像素</p>
    <div class="family1 font14 demo-font-family-list">
        <div v-for="(item,index) in arrTextList" :key="index">{{ item }}</div>
    </div>
    <br />
    <br />
    <p class="family1 font16">思源黑体 16像素</p>
    <div class="family1 font16 demo-font-family-list">
        <div v-for="(item,index) in arrTextList" :key="index">{{ item }}</div>
    </div>
    <br />
    <br />
    <p class="family1 font18">思源黑体 18像素</p>
    <div class="family1 font18 demo-font-family-list">
        <div v-for="(item,index) in arrTextList" :key="index">{{ item }}</div>
    </div>
    <br>
    <br>
    <p class="family1 font14 fontWeight">思源黑体 14像素 加粗</p>
    <div class="family1 font14 fontWeight demo-font-family-list">
        <div v-for="(item,index) in arrTextList" :key="index">{{ item }}</div>
    </div>
    <br />
    <br />
    <p class="family1 font16 fontWeight">思源黑体 16像素 加粗</p>
    <div class="family1 font16 fontWeight demo-font-family-list">
        <div v-for="(item,index) in arrTextList" :key="index">{{ item }}</div>
    </div>
    <br />
    <br />
    <p class="family1 font18 fontWeight">思源黑体 18像素 加粗</p>
    <div class="family1 font18 fontWeight demo-font-family-list">
        <div v-for="(item,index) in arrTextList" :key="index">{{ item }}</div>
    </div>
</div>
