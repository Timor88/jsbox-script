// @widget
// @version 1.1.0
// @name 挂刀指数小组件
// @desc 获取iflow.work挂刀指数，动态变色，每10分钟刷新

const url = "https://iflow.work";

// 顏色插值函數
function interpolateColor(color1, color2, t) {
  // color: "#rrggbb"
  let c1 = color1.match(/\w\w/g).map(x => parseInt(x, 16));
  let c2 = color2.match(/\w\w/g).map(x => parseInt(x, 16));
  let c = c1.map((v, i) => Math.round(v + (c2[i] - v) * t));
  return `#${c.map(x => x.toString(16).padStart(2, "0")).join("")}`;
}

// 根據index獲取顏色
function getIndexColor(index) {
  // 綠→藍→紅
  const green = "27ae60";
  const blue = "2980ef";
  const red = "e74c3c";
  if (index <= 0.8) {
    // 0.6~0.8 綠到藍
    let t = (index - 0.6) / 0.2;
    return interpolateColor(green, blue, t);
  } else {
    // 0.8~1.0 藍到紅
    let t = (index - 0.8) / 0.2;
    return interpolateColor(blue, red, t);
  }
}

// 获取指数的函数
async function fetchIndex() {
  try {
    let resp = await $http.get({ url });
    // 用正則抓取 <b> 標籤內的數值
    let match = resp.data.match(/当前挂刀指数：<b>([\d.]+)<\/b>/);
    if (match) {
      return match[1];
    } else {
      return "未找到";
    }
  } catch (e) {
    return "獲取失敗";
  }
}

async function run() {
  let indexStr = await fetchIndex();
  let index = parseFloat(indexStr);
  console.log(index);
  let color = "#888888"; // 預設灰色
  if (!isNaN(index) && index >= 0.6 && index <= 1.0) {
    color = getIndexColor(index);
    console.log(color);
  }
  $widget.setTimeline({
    render: ctx => {
      return {
        type: "vstack",
        props: {
          spacing: 8
        },
        views: [
          {
            type: "text",
            props: {
              text: "掛刀指數",
              font: $font("bold", 18)
            }
          },
          {
            type: "text",
            props: {
              text: indexStr,
              font: $font(32),
              color: $color(color)
            }
          },
          {
            type: "text",
            props: {
              text: "每10分鐘自動更新",
              font: $font(10),
              color: $color("gray")
            }
          }
        ]
      }
    },
    interval: 10 * 60 // 10分鐘
  });
}

run();

// $http.get({
//   url: url,
//   handler: function(resp) {
//     console.log(resp.data); // 這裡會彈出原始碼
//   }
// });
