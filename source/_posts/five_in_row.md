---
title: 五子棋之面对面下棋
date: 2019-10-23 23:13:46
catalogies: javascrpt
---

<!-- 2. 五子棋之人机大战 -->
五子棋，人人皆知，考验智力与耐心。但受限于一张纸和一盘棋子，因此，我试着开发了一个面对面五子棋，让智能手机/平板能够代替实物，冲破限制。
# 棋盘
首先需要生成一个棋盘，用canvas实现即可。
```html
<canvas id= "canvas"></canvas>
```
```js
const cv = document.querySelector("#canvas")
const ctx = cv.getContext("2d") //获取canvas上下文

const vars = {
	n: 10, // 棋盘行数、列数，为正方形
  sideLen: 50 // 每小格边长50px
}

for(let i = 0; i < vars.n; i++){
  for(let j = 0; j < vars.n; j++){
		ctx.strokeRect(i * vars.sideLen, j * vars.sideLen, vars.sideLen, vars.sideLen)
  }
}
// 还能画出各种花里胡哨的东西，比如城墙、方格纸..
```

# 点击生成棋子
主要有四步：
1. 判断点击位置是否有效
2. 确定落在哪个交叉点上
3. 生成棋子
4. 更新棋盘状态

```js
// 记录棋盘棋子状态，0：空，1：已有黑棋，2：已有白棋
// 作用：判断是否重复点击
const pieceCount = []
function init(){
	for(let i = 0; i <= vars.n; i++){
    for(let j = 0; j <= vars.n; j++){
      pieceCount[x][y] = 0 
    }
	}
}

init()

// 假设都生成黑棋，还未区分角色
cv.addEventListener('click', (e) => {
  // 相对于事件对象的偏移位置，也就是(0,0)
  const distanceX = e.offsetX
  const distanceY = e.offsetY

  const result = ifValid(distanceX, distanceY)
	if(!result){
    console.log('点击位置无效！')
  	return
  }
  printPiece(x * vars.sideLen, y * vars.sideLen)
	pieceCount[x][y] = 1
 })


// 有两种情况无效：1. 该位置上已有棋子； 2. 点击位置不在有效范围内
function ifValid(distanceX, distanceY){
  const validR = 30 //可以作为变量放入vars对象

  const x = Math.round(distanceX / vars.sideLen)
  const y = Math.round(distanceY / vars.sideLen)
  if(pieceCount[x][y]){
  	return 
  }

  const boardX = x * vars.sideLen
  const boardY = y * vars.sideLen

  if(Math.abs(distanceX - boardX) < validR && 
     Math.abs(distanceY - boardY) < validR){
    return {
    	x: x,
      y: y
    }
  }

}

// 画出一颗棋子
function printPiece(x, y){
  ctx.moveTo(x + vars.sideLen / 2, y)
  ctx.arc(x, y, vars.sideLen, 0, Math.PI * 2)
  ctx.fill()
}
```
# 区分角色
只有两种角色，用一个布尔值来区分
只要在上一步的最后增加更新角色状态即可
```js
// 只把需要更改的函数重写了一下
let isBlackTime = true

cv.addEventListener('click', (e) => {
  // 相对于事件对象的偏移位置，也就是(0,0)
  const distanceX = e.offsetX
  const distanceY = e.offsetY

  const result = ifValid(distanceX, distanceY)
	if(!result){
    console.log('点击位置无效！')
  	return
  }
  printPiece(x * vars.sideLen, y * vars.sideLen, isBlackTime ? 'black' : 'wheat')
	pieceCount[x][y] = isBlackTime ? 1 : 2
  isBlackTime = !isBlackTime
 })


// 画出一颗棋子
function printPiece(x, y, color){
	ctx.beginPath()
  ctx.moveTo(x + vars.sideLen / 2, y)
  ctx.arc(x, y, vars.sideLen, 0, Math.PI * 2)
  ctx.fillStyle = color
  ctx.fill()
  ctx.closePath()
}
```
# 获胜
有两种思路
- 不用记录，每次下完棋去判断棋子的横、竖、正斜、反斜四个方向上的9颗是否有相同且连续的5颗
- 需要记录，每颗棋子有八个方向，记录每个方向的拥有的棋子数，每生成一颗就更新，有5就结束

这里因为后续有人机对战，所以采用了第二种方法
```js
// 记录每个棋子不同方向的棋子数
// -2：为对方棋子 -1：该方向越界了 1：该方向上没有别的棋子，只有刚下的自己...
const process = {}

let gameover = false
let blackSuc = false

function success(x, y) {
  const curPiece = piecesCount[x][y]; // 当前位置 0: 空, 1: 黑棋, 2: 白棋
  const key = "" + curPiece + "_" + x + "_" + y;
  process[key] = process[key] || {};
  const dirs = process[key];
  
   // 左上，上，右上，左，右，左下，下，右下
  const tempDir = ["lup", "up", "rup", "l", "r", "lbt", "bt", "rbt"];

  // 按照tempDir的顺序 计算该棋子每个方向的相同棋子数
  for (let jj = y - 1; jj <= y + 1; jj++) {
    for (let ii = x - 1; ii <= x + 1; ii++) {
      // 避免多余的循环
      if (gameover) {
        return true;
      }

      // 该位置本身不参与记录
      if (jj === y && ii === x) {
        continue;
      }
      
      const curDir = tempDir.shift();
      // 表示该方向越界了
      if (jj < 0 || ii < 0 || jj > rows.rowNum || ii > rows.rowNum) {
        dirs[curDir] = -1;
        continue;
      }

      dirs[curDir] = getDirCount(ii, jj, curDir, curPiece);
    }
  }
  process[key] = dirs;
  if (gameover) {
    return true;
  }
}

function getDirCount(x, y, curDir, curPiece) {
  const targetPiece = piecesCount[x][y]; // 该方向上的第一个棋子
  const index = dirs.findIndex(dir => dir === curDir);
  const revDir = dirs[dirs.length - index - 1];
  let tarCount = 1;

  // 如果第一颗棋子为空，返回1，表示只有它本身
  if (!targetPiece) {
    return tarCount;
  } else if (targetPiece === curPiece) {
    // 1. 目标count = 当前count + 1
    const ii = dirOprs[curDir][0];
    const jj = dirOprs[curDir][1];
    const curCount =
      process["" + targetPiece + "_" + x + "_" + y][curDir] > 0
        ? process["" + targetPiece + "_" + x + "_" + y][curDir]
        : 1; // 小于0表示没有一样的棋子，不能用该值参与计算
    tarCount = curCount + 1;

    // 2. 当前方向上所有的棋子反方向的count 更新
    // 需判断新棋子的反方向有无自家棋子,有则需要全部重新赋值
    const realCount =
      process["" + targetPiece + "_" + (x - ii) + "_" + (y - jj)][revDir];
    const revCount = realCount > 1 ? realCount : 1;// 大于1表示有
    
    for (let k = 0; k < curCount; k++) {
      process["" + targetPiece + "_" + (x + ii * k) + "_" + (y + jj * k)][
        revDir
      ] += revCount;
    }

    // 11011情况
    if (revCount > 1) {
      for (let k = 2; k <= revCount; k++) {
        let rp = (process[
          "" + targetPiece + "_" + (x - ii * k) + "_" + (y - jj * k)
        ][curDir] += tarCount - 1); // 减去新棋的重复
        if (rp >= 5) {
          gameover = true;
          blackSuc = curPiece === 1 ? true : false;
          return;
        }
      }
    }
  } else {
    // 表示第一颗棋为对方棋子, 不仅要返回-2, 还要设置对方棋反方向的count = -2
    tarCount = -2;
    process["" + targetPiece + "_" + x + "_" + y][revDir] = -2;
    return tarCount;
  }
  if (tarCount >= 5) {
    gameover = true;
    blackSuc = curPiece === 1 ? true : false;
  }
  return tarCount;
}
```


以上只是把重要的代码整理了出来，全部代码参考[GitHub源码](https://github.com/LiZhaji/five_in_row)

如有错误/理解不当/更好建议，欢迎Issue或联系我！

