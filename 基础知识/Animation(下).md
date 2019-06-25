# Animation(下)

##### 案例：利用封装的代码，书写轮播图

```
//（一） 定时切换图片
//1.初始化索引
var index  = 0；
let timer = setInterval(function(){
	//2.设置定时器让索引递增，改变图片的top值为，为-图片大小*索引
    index++;
    //3.当索引大于等于图片数量时，将索引重置为0
    if(index >= ul.children.length){
        index = 0;
    }
    ul.style.top = -320 * index +'px';
}, 1000)
```

```
//（二） 利用封装animate实现动画
//1.初始化索引
var index  = 0；
let timer = setInterval(autoPlay, 1000)
lbt.onmouseenter = ()=>{
	clearInterval(timer);
}
lbt.onmouseleave = ()=>{
	timer = setInterval(autoPlay,1000);
}
function autoPlay(){
    //2.设置定时器让索引递增，将-图片大小*索引作为每次动画的目标值
    index++;
    //3.当索引大于等于图片数量时，将索引重置为0
    if(index >= ul.children.length){
        index = 0;
    }
    var target = -320 * index;
    animate(ul,"top",target);
}
```

##  完善animate封装


	function animate(ele,opt,callback){
		// 2.记录动画的数量，遍历时加1。执行后减1。动画结束后执行回调函数
		let timerLen = 0;
	    // 1.遍历opt，获取所有attr和target
	    for(var attr in opt){
	        timerLen++;
	        createTimer(attr);
	    }
	    function createTimer(attr){
	        let target = opt[attr];
	        // 3.1设置定时器的名字与attr关联
	        let timerName = attr + 'timer';
	        // 3.2清除定时器，避免多个定时器用作于一个效果
			clearInterval(ele[timerName]);
			ele[timerName] = setInterval(()=>{
	            let current = getCss(ele,attr);//100px,45deg,0.5(string)
	            let unit = current.match(/[a-z]+$/i);//[0:px,index:6,input:current],null
	            unit = unit ? unit[0] : '';
	            current = parseFloat(current);
	            let speed = (target-current)/10;
			   speed = speed>0 ? Math.ceil(speed) : Math.floor(speed);//1,-1
			   current += speed;
			   if(current === target ){
					clearInterval(ele[timerName]);
					// 每一个动画完成数量减一
					timerLen--;
					// 动画结束后执行回掉函数
	                if(timerLen === 0){
	                    typeof callback==='function' && callback();
	                }
	             }
	             ele.style[attr] = current + unit;
			},30);
		}
	}			

##### 案例：实现一个动画结束后再改变字体颜色之类的效果

##### 案例：链式动画

##### 案例：水平无缝滚动

	2）无缝滚动
	    * 把第一张复制到最后
	ul.appendChild(ul.children[0].cloneNode(true));
	1）获取ul子元素的长度设置ul宽度，达到水平排列的效果
	let len = ul.children.length;
	let imgWidth;
	ul.querySelector('img').onload = function(){
		imgWidth = this.offsetWidth;
		ul.style.width = imgWidth*len + 'px';
	}
	3）水平轮播效果
	let timer = setInterval(autoPlay,3000);
	function autoPlay(){
	    index++;
	    show();
	}
	function show(){
		//当show()执行到最后一张图片len-1时，代表索引为0的图片。
		//再次进入autofocus，index++后，其实要执行的下一次索引应该为1。所以要把当前位置改成0.
	    if(index >= len){//0,1,2,3,4
	        ul.style.left = 0;
	        index = 1;
	    }
	    animate(ul,{left:-imgWidth*index});
	    // 遍历所有页码，取消高亮，给点点被点击的页码添加高亮
	    for(let i=0;i<len-1;i++){//0,1,2,3
	        page.children[i].className = '';
	    }
	    if(index === len - 1){
	        page.children[0].className = 'active';
	    }else{
	        page.children[index].className = 'active'
	    }
	}
	4）移入移出，清除轮播效果
	focus.onmouseenter = ()=>{clearInterval(timer);}
	focus.onmouseleave = ()=>{timer = setInterval(autoPlay,3000);}
	5）添加分页效果，点击分页切换
	let page = setPage();
	function setPage(){
	    let page = document.createElement('div');
	    page.className = 'page';
	    for(let i=1;i<len;i++){
	        let span = document.createElement('span');
	        span.innerText = i;
	        if(i===1){
	            span.classList.add('active');
	        }
	        page.appendChild(span);
	    }
	    focus.appendChild(page);
	    return page;
	}
	focus.onclick = e=>{
	    if(e.target.parentNode.className === 'page'){
	        index = e.target.innerText-1;
	        show();
	    }
	}
	6）添加前后按钮，实现上一张、下一张的效果（待完成...）
案例：瀑布流

    (1)获取图片宽,计算当前容器能容纳多少列
    //	* 列数 = parseInt(容器宽度/图片宽度)
    let imgWidth = items[0].offsetWidth;
    let col = Math.floor((window.innerWidth-17)/imgWidth);//10.6
    //2）计算水平间隔
    //    * 间隔 = （容器宽度-17）%图片宽度/(列数+1)，17为滚动条宽度
    let gap = ((window.innerWidth-17)%imgWidth)/(col+1);
    // 3）创建一个数组pos
    //	  * 用来保存第一行每列图片左上角坐标(left,top)
    let pos = [];
    for(let i=0;i<col;i++){
        pos.push({
            left:(i+1)*gap + i*imgWidth,
            top:gap
        })
    }
    // 4）遍历所有图片，往容器里添加图片
        //  当前图片加载时，遍历数组pos，查找最小top值及对应的索引，给当前元素设置样式
        //  然后更新当前索引，即minIdx的top值,top = 容器高度（由图片高度跟上一次gap撑开） + gap 
    for(let i=0;i<items.length;i++){
        let currentImg = items[i].querySelector('img');
        // 等图片加载完成获取div高度
        currentImg.onload = function(){
            let minIdx = 0;
            let minTop = pos[minIdx].top;
            for(let j=1;j<pos.length;j++){
                if(pos[j].top < minTop){
                    minTop = pos[j].top;
                    // 更新最小索引值
                    minIdx = j;
                }
            }
            animate(items[i],{left:parseInt(pos[minIdx].left),top:parseInt(pos[minIdx].top)})
            // 然后更新当前top值
            pos[minIdx].top += gap + items[i].offsetHeight;
        }
    }             

