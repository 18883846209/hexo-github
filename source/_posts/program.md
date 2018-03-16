---
# title: 前端开发小技巧
title: ''
---
<!-- 这里是平时做项目的时候踩过的一些坑，希望能帮到大家吧！ -->
<br/>
#### 给html元素添加点击状态
给元素添加伪类:active
css设置如下

``` css
:active {
	background-color: #eee; // 设置自己需要的颜色
}

```

有些iOS和Android上的链接或者按钮被点击的时候都会忽略这个属性。为了使用这个active状态，你需要使用JavaScript给页面添加一个简单的事件:

```javascript
document.body.addEventListener('touchstart', function(){}, true);
```

#### 文字输入限制问题（输入完成后才做出判断）

当输入汉字时必然会是非直接输入，需要我们点选才能正式输入。
当我们字数限制为16个字，需要实时检查是否到16字。输入文字时，当有非直接的文字输入时，监听keydown事件和input事件都会直接触发判断字数逻辑，会截断我们正在输入的文字。
解决办法：监听compositionend（当直接的文字输入时触发）这时，当没选中文字的时候不会进行字数判断。

```javascript
$('#input').on('compositionend', () => {
	let len = $('#input').val().length;
	if (len > 5) {
		// 提示超出字数
		$.toast().reset('all');
		$.toast('不能超过5个字');
	}
});
```

#### 判断滚动方向(pc)

```javascript
scrollFunc () { // 判断滚动方向
	if (typeof scrollAction.x === 'undefined') {
		scrollAction.x = window.pageXOffset;
		scrollAction.y = window.pageYOffset;
	}
	let diffX = scrollAction.x - window.pageXOffset;
	let diffY = scrollAction.y - window.pageYOffset;
	if (diffX < 0) {
		// Scroll right
		// scrollDirection = 'right';
	} else if (diffX > 0) {
		// Scroll left
		// scrollDirection = 'left';
	} else if (diffY < 0) {
		// Scroll down
		// scrollDirection = 'down';
	} else if (diffY > 0) {
		// Scroll up
		// scrollDirection = 'up';
	} else {
		// First scroll event
		// scrollDirection = 'noscroll';
	}
	scrollAction.x = window.pageXOffset;
	scrollAction.y = window.pageYOffset;
}

调用
window.onscroll = function () {
	scrollFunc();
	// 然后通过scrollDirection的值获取方向
}
```

#### 滚动到底部的判断(在滚动事件中判断)

```javascript
if (window.pageYOffset + window.innerHeight >= document.body.scrollHeight) {
	console.log('到底了');
}
其中window.pageYOffset为滚动条的坐标（距离顶部的距离） window.innerHeight为屏幕的高度  document.body.scrollHeight为网页内容的高度
```

#### 判断滑动方向（移动端）

```javascript
//返回角度
function GetSlideAngle(dx, dy) {
	return (Math.atan2(dy, dx) * 180) / Math.PI;
}

	//根据起点和终点返回方向 up：向上，down：向下，left：向左，right：向右,0：未滑动
	function GetSlideDirection(startX, startY, endX, endY) {
		let dy = startY - endY;
		let dx = endX - startX;
		let result = 0;

		//如果滑动距离太短
		if (Math.abs(dx) < 2 && Math.abs(dy) < 2) {
			return result;
		}
		let angle = GetSlideAngle(dx, dy);
		if (angle >= -45 && angle < 45) {
			result = 'right';
		}
		else if (angle >= 45 && angle < 135) {
			result = 'up';
		}
		else if (angle >= -135 && angle < -45) {
			result = 'down';
		}
		else if ((angle >= 135 && angle <= 180) || (angle >= -180 && angle < -135)) {
			result = 'left';
		}
		return result;
	}

	function handleSlide(dir) {
		switch (dir) {
			case 0:
				// alert('没滑动');
				break;
			case 'up':
				// alert('向上');
				break;
			case 'down':
				// alert('向下');
				break;
			case 'left':
				// alert('向左');
				break;
			case 'right':
				// alert('向右');
				break;
			default:
				// alert('nomove');
		}
	}
	//滑动处理
	let startX;
	let startY;
	let moveX;
	let moveY;
	let endX;
	let endY;
	document.addEventListener('touchstart', (ev) => {
		startX = ev.touches[0].pageX;
		startY = ev.touches[0].pageY;
	}, false);
	document.addEventListener('touchmove', (ev) => {
		moveX = ev.changedTouches[0].pageX;
		moveY = ev.changedTouches[0].pageY;
		let direction = GetSlideDirection(startX, startY, moveX, moveY);
		handleSlide(direction);
	}, false);
	document.addEventListener('touchend', (ev) => {
		endX = ev.changedTouches[0].pageX;
		endY = ev.changedTouches[0].pageY;
		let direction = GetSlideDirection(startX, startY, endX, endY);
		handleSlide(direction);
	}, false);
```
