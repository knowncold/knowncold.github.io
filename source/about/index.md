---
title: 关于大排面
date: 2020-05-10 21:14:53
---

- 二十出头的北漂小伙 ∈ 阿萍
- 写后端也画前端，学安全也整硬件
- [听友](/2019/12/14/never-stop-lijian/)，业余羽毛球爱好者，初级烹饪师
- 手游八赛季王者，LPL云玩家


### 我喜欢
<div id="like"></div>

### 不喜欢
<div id="dislike"></div>

<script
  src="https://code.jquery.com/jquery-3.4.1.slim.min.js"
  integrity="sha256-pasqAKBDmFT4eHoN2ndd6lN370kFiGUFyTiUHWhU7k8="
  crossorigin="anonymous"></script>
<script type="text/javascript">
$(document).ready(function(){
    const likes = [
        "周杰伦",
        "五月天",
        "哥德堡变奏曲",
        "Green Day",
        "一路向北",
        "田馥甄",
        "裴秀智",
        "西野七濑",
        "大排面",
        "烹饪",
        "折纸",
        "李健"];
  $("#like").append(likes.sort(() => Math.random() - 0.5).map(e => {
        var rand = parseInt(Math.random() * 15 + 10);
        return `<span style="font-size: ${rand}px">${e}</span>`;
      }).join("、"));
    const dislikes = [
        "华晨宇",
        "华为",
        // "花粥",
        "饭圈",
        // "欧阳娜娜",
        "郑爽",
        // "谢娜",
        // "杜海涛",
        // "王源",
        "肖战",
        "张艺兴"];
  $("#dislike").append(dislikes.sort(() => Math.random() - 0.5).map(e => {
        var rand = parseInt(Math.random() * 15 + 10);
        return `<span style="font-size: ${rand}px">${e}</span>`;
      }).join("、"));
});
</script>