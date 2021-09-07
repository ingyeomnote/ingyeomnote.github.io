---
title: "Interactiv Developer - Wave"
excerpt: "finalProject에서 사용한 interactive한 Wave의 정리"

categories:
  - FinalProject
tags: 
  - Interactive
  - FinalProject
last_modified_at: 2021-07-25T19:26:00
---



## HTML5 Canvas Tutorial : 움직이는 웨이브 만들기

시작은 항상 html에서 시작한다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" type="text/css" src="style.css">
</head>
<body>

	<script type="module" src="app.js"></script>

</body>
</html>
```

css파일은 엘리먼트의 넓이와 높이만 지정해주는 역할을 한다.

```css
<!-- style.css -->

*{
    user-select: none;
    -ms-user-select: none;
    outline: 0;
    margin: 0;
    padding: 0;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

html{
    width: 100%;
    height: 100%;
}

body{
    width: 100%;
    height: 100%;
    overflow: hidden;
    background-color: #ffffff;
}

canvas{
    width: 100%;
    height: 100%;
}
```



```js
import{
    WaveGroup
} from "./wavegroup.js";

class App{
    constructor(){
        this.canvas = document.createElement('canvas'); // canvas 생성
        this.ctx = this.canvas.getContext('2d');
        document.body.appendChild(this.canvas);
        
        this.waveGroup = new waveGroup();
        
        window.addEventListener('resize', this.resize.bind(this), false);
        this.resize();
        
        requestAnimationFrame(this.animate.bind(this)); // 애니메이션 시작
    }
    
    resize(){
        this.stageWidth = document.body.clientWidth;
        this.stageHeight = document.body.clientHeight;
        
        this.canvas.width = this.stageWdith * 2;
        this.canvas.height = this.stageHeight * 2;
        this.ctx.scale(2, 2);
        // 캔버스를 더블 사이즈로 지정해서 레티나 디스플레이에서도 잘 보일 수 있게 해준다.
        
        this.waveGroup.resize(this.stageWidth, this.stageHeight);
    }
    
    animate(t){
        this.ctx.clearRect(0, 0, this.stageWidth, this.stageHeight); // canvas 클리어
        
        this.waveGroup.draw(this.ctx``);
        ``
        requestAnimationFrame(this.animate.bind(this));
    }
}

window.onload = () = > { // window가 로드되면 App을 생성 
    new App();
};
```



```js
export class Point{
    // wave를 그리기보단, 각각의 간격을 가진 좌표를 하나씩 만들어서
    // 그 좌표 값의 Y값을 아래 위로 이동시키고 각각의 좌표를 하나의 선으로 연결하는 것이다.
    constructor(x, y){
        this.x = x;
        this.y = y;
        this.fixedY = y;
        this.speed = 0.1;
        this.cur = index; // point에 index 값을 넘겨줘서 현재 point가 몇번째 point인지 정의
        this.max = Math.random() * 100 + 150; // 얼마만큼 움직일 것인가에 대한 max값
    }
    
    update(){
        this.cur += this.speed; // 현재값을 speed만큼 증가
        this.y = this.fixedY + (Math.sin(this.cur) * this.max); // sine 함수를 사용하여 아래위로 움직일 수 있도록 했다.
    }
}
```

```js
import{
    Point
} from "./point.js";

export class Wave{
    // point가 동시에 아래위로 움직이면 wave로 안보이고 하나의 선으로 보이기 때문의
    // 고유의 index 넘버를 넘겨줘서 wave가 약간의 차이를 뒀다.
    constructor(index, totalPoints, color){
        this.index = index;
        this.totalPoints = totalPoints;
        this.color = color;
        this.points = [];
    }
    
    resize(stageWidth, stageHeight){
    // 애니메이션을 할 때 가장 중요한 것은 내가 그리고자 하는 애니메이션의 좌표값을 가지고 오는 것이다.
    // 그러기 위해선 애니메이션의 크기를 알아야 한다. -> 스테이지의 넓이와 높이를 가져오는 게 중요
        this.stageWidth = stageWidth;
        this.stageHeight = stageHeight;
        
        // wave는 화면의 중간에 그려질 것이기 때문에 centerX와 centerY를 정한다.
        this.centerX = stageWidth / 2;
        this.centerY = stageHeight / 2;
        
        // point의 간격은 스테이지 넓이에서 totalPoints만큼을 나눈 값이다. -> pointGap(간격)
        this.pointGap = this.stageWidth / (this.totalPoints - 1);
        
        this.init();
    }
    
    init(){
        // resize 이벤트에서 정의했던 centerX와 centerY를 넘겨줘서 각각의 포인트가
        // 화면 중간에서 그려지도록 정의해준다.
       
        this.points = [];
        
        // 정해진 간격만큼 화면에 그려준다.
        for(let i = 0; i < this.totalPoints; i++){
            const point = new Point( 
            this.index + i,
            this.pointGap * i,
            this.centerY,
            );
            this.points[i] = point;
        }
    }
    
    // 실제로 canvas를 그리는 함수
    // update된 point 개수에 맞춰서 draw()함수도 업데이트
    draw(ctx){
        
        ctx.beginPath();
        ctx.fillStyle = this.color; // wave의 color 값
        
        // 처음 point와 마지막 point는 움직이지 않고, 가운데 point들만 아래위로 움직여서
        // wave의 움직임을 만든다. 
        let prevX = this.points[0].x; 
        let prevY = this.points[0].y;
        
        ctx.moveTo(prevX, prevY);
        
        // index가 0이거나 tatl -1 즉, 마지막 index면 update 함수를 실행시키지 않고
        // 그 외의 경우에만 update만 실행하여 아래 위로 움직임을 만든다.
        for(let i = 1 ; i < this.totalPoints; i++){
            if(i < this.totalPoinst - 1){
                this.points[i].update();
            }
            
            const cx = (prevX + this.points[i].x) /2;
            const cy = (prevY + this.points[i].y) /2;
            
            // ctx.lineTo(cx, cy); // 직선으로 긋기
            ctx.quadraticCurveTo(prevX, prevY, cx, cy); // 곡선
            
            // 우리가 만들려는 것은 곡선형 wave이기 때문에 현재 point의 x, y 좌표를
            // 그대로 적어주는 것이 아니라 이전 point의 x, y 좌표에 현재 point의 x, y 좌표를
            // 반으로 나눈 값 즉, 그 중간값을 lineTo에 적어준다.
            // 나중에 곡선 함수로 바꿨을 때 그렇게 하지 않고 곡선 함수를 사용하는데
            // 이전값과 현재값을 그대로 적게 되면 곡선이 아니라 직선이 그려지기 때문에
            // 이전값과 현재값의 중간 포인트로 잡아서 부드러운 곡선이 되게 한다.
            
            prevX = this.points[i].x;
            prevY = this.points[i].y;
        }
        
        ctx.lineTo(prevX, prevY);
        ctx.lineTo(this.stageWidth, this.stageHeight);
        ctx.lineTo(this.points[0].x, this.stageHeight);
        ctx.fill();
        ctx.closePath();
    }
}
```

```js
import{
    Wave
} from './wave.js';

export class WaveGroup{
    constructor(){
        this.totalWaves = 3; // total wave 수
        this.totalPoints = 6; // total point 수
        
        this.color = ['rgba(0, 199, 235, 0.4)', 'rgba(0, 146, 199, 0.4)', 'rgba(0, 87, 158, 0.4)']; // 각각의 wave에 다른 색을 넣기 위해 정의
        this.waves = [];
        
        for(let i = 0; i < this.totalWaves; i++){
            const wave = new Wave(
                i,
                this.totalPoints,
                this.color[i],
            );
            this.waves[i] = wave; 
        }
    }
    
    // 각각의 함수가 실행이 되면 WaveGroup 안에 있는 total wave 수 만큼 함수를 실행시켜준다.
    resize(stageWidth, stageHeight){
        for(let i = 0; i < this.totalWaves; i++){
            const wave = this.wave[i];
            wave.resize(stageWidth, stageHeight);
        }
    }
    
    draw(ctx){
        for(let i = 0; i < this.totalWaves; i++){
            const wave = this.waves[i];
            wave.draw(ctx);
        }
    }
}
```



출처 : [유투브 : Interactive Developer](https://www.youtube.com/watch?v=LLfhY4eVwDY)